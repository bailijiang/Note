<!-- MarkdownTOC -->

- [TCP server](#tcp-server)
    - [一、echo源码1如下，main.c](#一、echo源码1如下，mainc)
    - [二、echo源码2如下，main.c](#二、echo源码2如下，mainc)
    - [三、客户端测试](#三、客户端测试)
    - [四、常用终端命令](#四、常用终端命令)
    - [五、TCP连接状态详解](#五、tcp连接状态详解)
    - [六、Linux网络参数](#六、linux网络参数)
- [TCP client，压力测试代码](#tcp-client，压力测试代码)

<!-- /MarkdownTOC -->


<a id="tcp-server"></a>
#### TCP server

守护进程daemonize的源码可以借鉴redis的：

void daemonize(void) { //come from /redis/server.c/daemonize()
    int fd;
 
    if (fork() != 0) exit(0); /* parent exits */
    setsid(); /* create a new session */
 
    /* Every output goes to /dev/null. If Redis is daemonized but
     * the 'logfile' is set to 'stdout' in the configuration file
     * it will not log at all. */
    if ((fd = open("/dev/null", O_RDWR, 0)) != -1) {
        dup2(fd, STDIN_FILENO);
        dup2(fd, STDOUT_FILENO);
        dup2(fd, STDERR_FILENO);
        if (fd > STDERR_FILENO) close(fd);
    }
}


<a id="一、echo源码1如下，mainc"></a>
##### 一、echo源码1如下，main.c
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>
 
#include <netinet/in.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <sys/epoll.h>
#include <unistd.h>
#include <sys/types.h>
 
#include <sys/resource.h>    /*setrlimit */
#include <signal.h>
#include <fcntl.h>
 
#define bool  int    //linux C中没有bool类型
#define false 0      //linux C中没有bool类型
#define true  1      //linux C中没有bool类型
#define IPADDRESS   "127.0.0.1"
#define PORT        1883
#define MAXSIZE     1024
#define LISTENQ     512
#define FDSIZE      1024
#define EPOLLEVENTS 60000
#define MAXCONN     60000
 
//函数声明
//创建套接字并进行绑定
static int socket_bind(const char* ip,int port);
//IO多路复用epoll
static void do_epoll(int listenfd);
//事件处理函数
static void handle_events(int epollfd,struct epoll_event *events,int num,int listenfd,char *buf);
//处理接收到的连接
static void handle_accpet(int epollfd,int listenfd);
//读处理
static void do_read(int epollfd,int fd,char *buf);
//写处理
static void do_write(int epollfd,int fd,char *buf);
//添加事件
static void add_event(int epollfd,int fd,int state);
//修改事件
static void modify_event(int epollfd,int fd,int state);
//删除事件
static void delete_event(int epollfd,int fd,int state);
 
void init_signal(void)//设置信号处理,SIG_IGN表示忽略信号,SIG_DFL表示使用信号的默认处理方式
{
    signal(SIGCHLD, SIG_DFL);
    signal(SIGPIPE, SIG_IGN);
}
 
int set_fdlimit()
{
    //设置每个进程允许打开的最大文件数
    //这项功能等价于linux终端命令 "ulimit -n 102400"
    struct rlimit rt;
    rt.rlim_max = rt.rlim_cur = MAXCONN;
    if (setrlimit(RLIMIT_NOFILE, &rt) == -1)
    {
        perror("setrlimit error");
        return -1;
    }
 
    return 0;
}
 
void daemon_run_method1()//来自https://github.com/baloonwj/flamingo
{
    int pid;
    signal(SIGCHLD, SIG_IGN);
    //1）在父进程中，fork返回新创建子进程的进程ID；
    //2）在子进程中，fork返回0；
    //3）如果出现错误，fork返回一个负值；
    pid = fork();
    if (pid < 0)
    {
        //std::cout << "fork error" << std::endl;
        exit(-1);
    }
    //父进程退出，子进程独立运行
    else if (pid > 0)
    {
        exit(0);
    }
 
    //之前parent和child运行在同一个session里,parent是会话（session）的领头进程,
    //parent进程作为会话的领头进程，如果exit结束执行的话，那么子进程会成为孤儿进程，并被init收养。
    //执行setsid()之后,child将重新获得一个新的会话(session)id。
    //这时parent退出之后,将不会影响到child了。
    setsid();
    int fd;
    fd = open("/dev/null", O_RDWR, 0);
    if (fd != -1)
    {
        dup2(fd, STDIN_FILENO);
        dup2(fd, STDOUT_FILENO);
        dup2(fd, STDERR_FILENO);
    }
 
    if (fd > 2)
    {
        close(fd);
    }
}
 
bool daemon_run_method2() //Linux高性能服务器编程.pdf,游双
{
    //创建子进程，关闭父进程，这样可以使程序在后台进行
    pid_t pid = fork();
    if ( pid < 0 )
    {
        return false;
    }
    else if ( pid > 0 )
    {
        exit( 0 );
    }
 
    //设置文件权限掩码。当进程创建新文件时，文件的权限将是mode & 0777
    umask( 0 );
 
    //创建新的会话，设置本进程为进程组的首领
    pid_t sid = setsid();
    if ( sid < 0 )
    {
        return false;
    }
    //切换工作目录
    if ( ( chdir( "/" ) ) < 0 )
    {
        /* Log the failure */
        return false;
    }
    //关闭标准输入设备、标准输出设备和标准错误输出设备
    close( STDIN_FILENO );
    close( STDOUT_FILENO );
    close( STDERR_FILENO );
    //将标准输入、输出和错误输出都定向到/dev/null文件
    open( "/dev/null", O_RDONLY );
    open( "/dev/null", O_RDWR );
    open( "/dev/null", O_RDWR );
    return true;
}
int main(int argc,char *argv[])
{
    //设置信号处理
    init_signal();
    //设置每个进程允许打开的最大文件数,socket
    if (set_fdlimit() < 0)
    {
        return -1;
    }
    //守护者进程
    bool bdaemon = false;
    if (bdaemon)
    {
        daemon_run_method1();
    }
    int  listenfd;
    listenfd = socket_bind(IPADDRESS,PORT);
    listen(listenfd,LISTENQ);
    do_epoll(listenfd);
    return 0;
}
static int socket_bind(const char* ip,int port)
{
    int  listenfd;
    struct sockaddr_in servaddr;
    listenfd = socket(AF_INET,SOCK_STREAM,0);
    if (listenfd == -1)
    {
        perror("socket error:");
        exit(1);
    }
    //一个端口释放后会等待两分钟之后才能再被使用，SO_REUSEADDR是让端口释放后立即就可以被再次使用。
    int reuse_addr = 1;
    if (setsockopt(listenfd, SOL_SOCKET, SO_REUSEADDR, &reuse_addr, sizeof(reuse_addr)) == -1)
    {
        return -1;
    }
    bzero(&servaddr,sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    //inet_pton(AF_INET,ip,&servaddr.sin_addr);
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);//绑定所有网卡所有IP
    //servaddr.sin_addr.s_addr = inet_addr("172.16.6.178");
    //servaddr.sin_addr.s_addr = inet_addr("127.0.0.1");//这样写指代不明，当服务器有多网卡时，不知道绑定哪个IP，导致连接失败
    servaddr.sin_port = htons(port);
    if (bind(listenfd,(struct sockaddr*)&servaddr,sizeof(servaddr)) == -1)
    {
        perror("bind error: ");
        exit(1);
    }
    printf("listen on: %d,listenfd=%d\n",PORT,listenfd);
    return listenfd;
}
static void do_epoll(int listenfd)
{
    int epollfd;
    struct epoll_event events[EPOLLEVENTS];
    int ret;
    char buf[MAXSIZE];
    memset(buf,0,MAXSIZE);
    //创建一个描述符
    epollfd = epoll_create(FDSIZE);
    //添加监听描述符事件
    add_event(epollfd,listenfd,EPOLLIN);
    for ( ; ; )
    {
        //获取已经准备好的描述符事件
        ret = epoll_wait(epollfd,events,EPOLLEVENTS,-1);
        handle_events(epollfd,events,ret,listenfd,buf);
    }
    close(epollfd);
}
static void handle_events(int epollfd,struct epoll_event *events,int num,int listenfd,char *buf)
{
    int i;
    int fd;
    //进行选好遍历
    for (i = 0;i < num;i++)
    {
        fd = events[i].data.fd;
        //根据描述符的类型和事件类型进行处理
        if ((fd == listenfd) &&(events[i].events & EPOLLIN))
            handle_accpet(epollfd,listenfd);
        else if (events[i].events & EPOLLIN)
            do_read(epollfd,fd,buf);
        else if (events[i].events & EPOLLOUT)
            do_write(epollfd,fd,buf);
    }
}
static void handle_accpet(int epollfd,int listenfd)
{
    int clifd;
    struct sockaddr_in cliaddr;
    socklen_t  cliaddrlen = sizeof(cliaddr);
    clifd = accept(listenfd,(struct sockaddr*)&cliaddr,&cliaddrlen);
    if (clifd == -1)
        perror("accpet error:");
    else
    {
        printf("accept a new client: %s:%d,fd=%d\n",inet_ntoa(cliaddr.sin_addr),cliaddr.sin_port,clifd);
        //添加一个客户描述符和事件
        add_event(epollfd,clifd,EPOLLIN);
    }
}
static void do_read(int epollfd,int fd,char *buf)
{
    int nread;
    nread = read(fd,buf,MAXSIZE);
    if (nread == -1)
    {
        perror("read error:");
        close(fd);
        delete_event(epollfd,fd,EPOLLIN);
    }
    else if (nread == 0)
    {
        fprintf(stderr,"client close,fd=%d\n",fd);
        close(fd);
        delete_event(epollfd,fd,EPOLLIN);
    }
    else
    {
        printf("read message is: %s,fd=%d\n",buf,fd);
        //修改描述符对应的事件，由读改为写
        modify_event(epollfd,fd,EPOLLOUT);
    }
}
static void do_write(int epollfd,int fd,char *buf)
{
    int nwrite;
    nwrite = write(fd,buf,strlen(buf));
    if (nwrite == -1)
    {
        perror("write error:");
        close(fd);
        delete_event(epollfd,fd,EPOLLOUT);
    }
    else
        modify_event(epollfd,fd,EPOLLIN);
    memset(buf,0,MAXSIZE);
}
static void add_event(int epollfd,int fd,int state)
{
    struct epoll_event ev;
    ev.events = state;//LT
    ev.data.fd = fd;
    epoll_ctl(epollfd,EPOLL_CTL_ADD,fd,&ev);
}
static void delete_event(int epollfd,int fd,int state)
{
    struct epoll_event ev;
    ev.events = state;
    ev.data.fd = fd;
    epoll_ctl(epollfd,EPOLL_CTL_DEL,fd,&ev);
}
static void modify_event(int epollfd,int fd,int state)
{
    struct epoll_event ev;
    ev.events = state;
    ev.data.fd = fd;
    epoll_ctl(epollfd,EPOLL_CTL_MOD,fd,&ev);
}
```

<a id="二、echo源码2如下，mainc"></a>
##### 二、echo源码2如下，main.c
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/epoll.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <netinet/in.h>
#include <sys/resource.h>    /*setrlimit */
 
#define ECHO_SERVER_PORT        1883
#define LISTEN_BACKLOG          16
#define MAX_EVENT_COUNT         32
#define BUF_SIZE                2048
#define MAXCONN                 60000
 
int set_fdlimit()
{
    //设置每个进程允许打开的最大文件数
    //这项功能等价于linux终端命令 "ulimit -n 102400"
    struct rlimit rt;
    rt.rlim_max = rt.rlim_cur = MAXCONN;
    if (setrlimit(RLIMIT_NOFILE, &rt) == -1)
    {
        perror("setrlimit error");
        return -1;
    }
 
    return 0;
}
 
int main() {
 
    //设置每个进程允许打开的最大文件数,socket
    if (set_fdlimit() < 0)
    {
        return -1;
    }
    int ret, i;
    int server_fd, client_fd, epoll_fd;
    int ready_count;
    struct sockaddr_in server_addr;
    struct sockaddr_in client_addr;
    socklen_t addr_len;
    struct epoll_event event;
    struct epoll_event* event_array;
    char* buf;
    event_array = (struct epoll_event*)
                    malloc(sizeof(struct epoll_event)*MAX_EVENT_COUNT);
    buf = (char*)malloc(sizeof(char)*BUF_SIZE);
    memset(&server_addr, 0, sizeof(server_addr));
    server_addr.sin_family = AF_INET;
    server_addr.sin_addr.s_addr = htonl(INADDR_ANY);
    server_addr.sin_port = htons(ECHO_SERVER_PORT);
    server_fd = socket(AF_INET, SOCK_STREAM, 0);
    if(server_fd == -1) {
        perror("create socket failed.\n");
        return 1;
    }
    //一个端口释放后会等待两分钟之后才能再被使用，SO_REUSEADDR是让端口释放后立即就可以被再次使用。
    int reuse_addr = 1;
    if (setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR, &reuse_addr, sizeof(reuse_addr)) == -1)
    {
        return -1;
    }
    ret = bind(server_fd, (struct sockaddr*)&server_addr, sizeof(server_addr));
    if(ret == -1) {
        perror("bind failed.\n");
        return 1;
    }
    ret = listen(server_fd, LISTEN_BACKLOG);
    if(ret == -1) {
        perror("listen failed.\n");
        return 1;
    }
    fprintf(stderr,"listen on,fd=%d\n",server_fd);
    epoll_fd = epoll_create(1);
    if(epoll_fd == -1) {
        perror("epoll_create failed.\n");
        return 1;
    }
    event.events = EPOLLIN;
    event.data.fd = server_fd;
    ret = epoll_ctl(epoll_fd, EPOLL_CTL_ADD, server_fd, &event);
    if(ret == -1) {
        perror("epoll_ctl failed.\n");
        return 1;
    }
    while(1) {
        ready_count = epoll_wait(epoll_fd, event_array, MAX_EVENT_COUNT, -1);
        if(ready_count == -1) {
            perror("epoll_wait failed.\n");
            return 1;
        }
        for(i = 0; i < ready_count; i++) {
            if(event_array[i].data.fd == server_fd) {
                client_fd = accept(server_fd,
                            (struct sockaddr*)&client_addr, &addr_len);
                if(client_fd == -1) {
                    perror("accept failed.\n");
                    return 1;
                }
                event.events = EPOLLIN;
                event.data.fd = client_fd;
                ret = epoll_ctl(epoll_fd, EPOLL_CTL_ADD, client_fd, &event);
                if(ret == -1) {
                    perror("epoll_ctl failed.\n");
                    return 1;
                }
                fprintf(stderr,"accept,fd=%d\n",client_fd);
            }
            else {
                ret = recv(event_array[i].data.fd, buf, BUF_SIZE, 0);
                if(ret <= 0) {
                    close(event_array[i].data.fd);
                    epoll_ctl(epoll_fd, EPOLL_CTL_DEL,
                              event_array[i].data.fd, &event);
                    continue;
                }
                ret = send(event_array[i].data.fd, buf, (size_t)ret, 0);
                if(ret == -1) {
                    perror("send failed.\n");
                }
            }
        } // for each event
    } // while(1)
    close(epoll_fd);
    close(server_fd);
    free(event_array);
    free(buf);
    return 0;
}
```

<a id="三、客户端测试"></a>
##### 三、客户端测试

* 1、工具介绍

强大的TcpServer压力测试工具源码（附突破连接限制的方法和工具）

TCP_UDP_PerformanceTest

TCPCOPY：https://github.com/session-replay-tools/tcpcopy

Apache Bench：https://httpd.apache.org/docs/2.4/programs/ab.html -- ab

Apache jmeter：https://github.com/apache/jmeter

webbench：http://home.tiscali.cz/~cz210552/webbench.html

★Windows解决端口号限制方法，修改两个注册表： （必须修改，否则测试工具作为客户端发起TCP连接，数目上不去）

HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters\MaxUserPort 如果没有，则手动创建 DWord（32位） ”数值数据“改为十进制65534 或者认为适当的值。 此值表示 用户最大使用的端口数量，默认为5000。 

HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters\TCPTimedWaitDelay 如果没有，则手动创建 DWord（32位） ”数值数据“改为十进制30 或者你认为适当的值。 此值表示一个关闭后的端口等待多久之后可以重新使用，默认为120秒，也就是2分钟才可以重新使用。

* 2、实践体会
* ![](image\server_test_tool_1.PNG)

★Server Test Tool这个工具，测试完成需要按“Stop“，界面会暂时卡死，没关系，请耐心等待，千万不要强行kill进程，否则会导致资源无法正常释放。如果资源无法正常释放，会出现连接数上不去的情况，此时只能通过重启电脑可以解决。

我在公司办公电脑，使用如图配置，每1000ms发起1000个tcp连接，测试通过，可以达到ConnectTotal的数值；

但是我在家里电脑，使用如图配置，tcp连接数却怎么都上不去，始终不过万，后来我调整为OnHeartbeat模式，每1000ms发起100个tcp连接，方可达成。因为OnHeartbeat比OnTimer更省流量，另外发起连接数的频次也降下来了。看来参数很重要，与路由器带宽等的承受能力有关。不妨多尝试，找出性价比最好的参数组合。

★如果使用SSH远程访问的方式，服务器程序不能是控制台带打印信息的方式，否则客户端连接会出问题，一来TCP客户端连接数很难上的去，二来SSH软件网络很容易中断。结论，服务器程序应该使用守护进程，在后台运行。

* ![](image\server_test_tool_2.PNG)



<a id="四、常用终端命令"></a>
##### 四、常用终端命令

查看文件描述符使用情况的命令是：

lsof -i -n -P
lsof -i -n -P | grep :1883

查看1883端口的连接情况,观察TCP状态图
netstat -nalp|grep 1883

查看1883端口的客户端连接数

netstat -nalp|grep 1883|wc -l 

查看已连接的数目

netstat -na|grep ESTABLISHED|wc -l

修改当前进程的最大文件数
ulimit -n 102400 

查看进程是否在，指定进程名

netstat -lnpt | grep epltest

查看进程是否在，指定端口号

netstat -tunlp|egrep "(1883|1982)"
netstat -tunlp|egrep "1883"
netstat -tunlp|egrep 1883

终止进程

kill -9 <nginx进程号>

查看日志

`tail -f log\*.log`



<a id="五、tcp连接状态详解"></a>
##### 五、TCP连接状态详解 
LISTEN： 侦听来自远方的TCP端口的连接请求
SYN-SENT： 再发送连接请求后等待匹配的连接请求
SYN-RECEIVED：再收到和发送一个连接请求后等待对方对连接请求的确认
ESTABLISHED： 代表一个打开的连接
FIN-WAIT-1： 等待远程TCP连接中断请求，或先前的连接中断请求的确认
FIN-WAIT-2： 从远程TCP等待连接中断请求
CLOSE-WAIT： 等待从本地用户发来的连接中断请求
CLOSING： 等待远程TCP对连接中断的确认
LAST-ACK： 等待原来的发向远程TCP的连接中断请求的确认
TIME-WAIT： 等待足够的时间以确保远程TCP接收到连接中断请求的确认
CLOSED： 没有任何连接状态



<a id="六、linux网络参数"></a>
##### 六、Linux网络参数

Linux参数调优，修改文件 /etc/sysctl.conf ，在末尾追加这些文字

fs.file-max = 2097152
fs.nr_open = 2097152

net.core.somaxconn = 65535
net.core.rmem_default = 65535
net.core.wmem_default = 65535
net.core.rmem_max = 8388608
net.core.wmem_max = 83886080
net.core.optmem_max = 40960
net.ipv4.tcp_rmem = 4096 87380 83886080
net.ipv4.tcp_wmem = 4096 65535 83886080
net.ipv4.tcp_mem = 8388608 8388608 83886080

net.ipv4.ip_local_port_range = 1025 65000
net.ipv4.tcp_max_syn_backlog = 16384
net.core.netdev_max_backlog = 16384
net.ipv4.tcp_fin_timeout = 15

修改完，输入终端命令，使能生效 sysctl -p

允许当前会话/进程打开文件句柄数

ulimit -n 1048576



<a id="tcp-client，压力测试代码"></a>
#### TCP client，压力测试代码
下载https://download.csdn.net/download/libaineu2004/10468728

//参考了Linux高性能服务器编程,chapter-16,16-4stress_client.cpp
``` 
#include <stdlib.h>
#include <stdio.h>
#include <assert.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/epoll.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <string.h>
#include <errno.h>
#include <signal.h>
#include <sys/resource.h>    /*setrlimit */
#include <fcntl.h> //daemonize
 
#define CONFIG_MIN_RESERVED_FDS 32
#define CONFIG_FDSET_INCR       (CONFIG_MIN_RESERVED_FDS+96)
 
//./mystressclient 172.16.6.161 8011 1000 1
//./mystressclient 172.16.6.161 1884 10 1
 
//请注意，http的内容主体，必须含有两次的换行，即\r\n\r\n
//static const char* request = "GET http://localhost/index.html HTTP/1.1\r\nConnection: keep-alive\r\n\r\n";
//static const char* request = "hello world\r\n";
static const char* request = "GET /msg_server HTTP/1.1\r\nConnection: keep-alive\r\n\r\n";//GET
//static const char* request = "POST /msg_server HTTP/1.1\r\nConnection: keep-alive\r\n\r\n";//POST
int stop = 0;
 
int setnonblocking( int fd )
{
    int old_option = fcntl( fd, F_GETFL );
    int new_option = old_option | O_NONBLOCK;
    fcntl( fd, F_SETFL, new_option );
    return old_option;
}
 
void addfd( int epoll_fd, int fd )
{
    epoll_event event;
    event.data.fd = fd;
    //event.events = EPOLLOUT | EPOLLET | EPOLLERR;
    event.events = EPOLLOUT | EPOLLERR;
    epoll_ctl( epoll_fd, EPOLL_CTL_ADD, fd, &event );
    setnonblocking( fd );
}
 
bool write_nbytes( int sockfd, const char* buffer, int len )
{
    int bytes_write = 0;
    printf( "write out %d bytes to socket %d\n", len, sockfd );
    while( 1 )
    {
        bytes_write = send( sockfd, buffer, len, 0 );
        if ( bytes_write == -1 )
        {
            return false;
        }
        else if ( bytes_write == 0 )
        {
            return false;
        }
 
        len -= bytes_write;
        buffer = buffer + bytes_write;
        if ( len <= 0 )
        {
            return true;
        }
    }
}
bool read_once( int sockfd, char* buffer, int len )
{
    int bytes_read = 0;
    memset( buffer, '\0', len );
    bytes_read = recv( sockfd, buffer, len, 0 );
    if ( bytes_read == -1 )
    {
        return false;
    }
    else if ( bytes_read == 0 )
    {
        return false;
    }
    printf( "read in %d bytes from socket %d with content: %s\n", bytes_read, sockfd, buffer );
    return true;
}
void start_conn( int epoll_fd, int num, const char* ip, int port, int space )
{
    if (num <= 0 || port <= 0 || space <= 0)
    {
        exit(0);
    }
    struct sockaddr_in address;
    bzero( &address, sizeof( address ) );
    address.sin_family = AF_INET;
    inet_pton( AF_INET, ip, &address.sin_addr );
    address.sin_port = htons( port );
    for ( int i = 0; i < num; ++i )
    {
        if ((i % space) == 0)
        {
            sleep( 1 );//1s
        }
        int sockfd = socket( PF_INET, SOCK_STREAM, 0 );
        if ( sockfd < 0 )
        {
            continue;
        }
        if (  connect( sockfd, ( struct sockaddr* )&address, sizeof( address ) ) == 0  )
        {
            printf( "build connection %d\n", i );
            addfd( epoll_fd, sockfd );
        }
        else
        {
            printf( "create fail\n" );
        }
    }
}
void close_conn( int epoll_fd, int sockfd )
{
    epoll_ctl( epoll_fd, EPOLL_CTL_DEL, sockfd, 0 );
    close( sockfd );
}
void daemonize(void) { //come from /redis/server.c/daemonize()
    int fd;
    if (fork() != 0) exit(0); /* parent exits */
    setsid(); /* create a new session */
    /* Every output goes to /dev/null. If Redis is daemonized but
     * the 'logfile' is set to 'stdout' in the configuration file
     * it will not log at all. */
    if ((fd = open("/dev/null", O_RDWR, 0)) != -1) {
        dup2(fd, STDIN_FILENO);
        dup2(fd, STDOUT_FILENO);
        dup2(fd, STDERR_FILENO);
        if (fd > STDERR_FILENO) close(fd);
    }
}
 
/* This function will try to raise the max number of open files accordingly to
 * the configured max number of clients. It also reserves a number of file
 * descriptors (CONFIG_MIN_RESERVED_FDS) for extra operations of
 * persistence, listening sockets, log files and so forth.
 *
 * If it will not be possible to set the limit accordingly to the configured
 * max number of clients, the function will do the reverse setting
 * server.maxclients to the value that we can actually handle. */
void adjustOpenFilesLimit(int maxclients) { //come from /redis/server.c/adjustOpenFilesLimit()
    rlim_t maxfiles = maxclients+CONFIG_MIN_RESERVED_FDS;
    struct rlimit limit;
 
    if (getrlimit(RLIMIT_NOFILE,&limit) == -1) {
        printf("Unable to obtain the current NOFILE limit (%s), assuming 1024 and setting the max clients configuration accordingly.",
               strerror(errno));
        maxclients = 1024-CONFIG_MIN_RESERVED_FDS;
    } else {
        rlim_t oldlimit = limit.rlim_cur;
 
        /* Set the max number of files if the current limit is not enough
         * for our needs. */
        if (oldlimit < maxfiles) {
            rlim_t bestlimit;
            int setrlimit_error = 0;
 
            /* Try to set the file limit to match 'maxfiles' or at least
             * to the higher value supported less than maxfiles. */
            bestlimit = maxfiles;
            while(bestlimit > oldlimit) {
                rlim_t decr_step = 16;
 
                limit.rlim_cur = bestlimit;
                limit.rlim_max = bestlimit;
                if (setrlimit(RLIMIT_NOFILE,&limit) != -1) break;
                setrlimit_error = errno;
 
                /* We failed to set file limit to 'bestlimit'. Try with a
                 * smaller limit decrementing by a few FDs per iteration. */
                if (bestlimit < decr_step) break;
                bestlimit -= decr_step;
            }
            /* Assume that the limit we get initially is still valid if
             * our last try was even lower. */
            if (bestlimit < oldlimit) bestlimit = oldlimit;
            if (bestlimit < maxfiles) {
                unsigned int old_maxclients = maxclients;
                maxclients = bestlimit-CONFIG_MIN_RESERVED_FDS;
                /* maxclients is unsigned so may overflow: in order
                 * to check if maxclients is now logically less than 1
                 * we test indirectly via bestlimit. */
                if (bestlimit <= CONFIG_MIN_RESERVED_FDS) {
                    printf("Your current 'ulimit -n' "
                           "of %llu is not enough for the server to start. "
                           "Please increase your open file limit to at least "
                           "%llu. Exiting.",
                           (unsigned long long) oldlimit,
                           (unsigned long long) maxfiles);
                    exit(1);
                }
                printf("You requested maxclients of %d "
                       "requiring at least %llu max file descriptors.",
                       old_maxclients,
                       (unsigned long long) maxfiles);
                printf("Server can't set maximum open files "
                       "to %llu because of OS error: %s.",
                       (unsigned long long) maxfiles, strerror(setrlimit_error));
                printf("Current maximum open files is %llu. "
                       "maxclients has been reduced to %d to compensate for "
                       "low ulimit. "
                       "If you need higher maxclients increase 'ulimit -n'.",
                       (unsigned long long) bestlimit, maxclients);
            } else {
                printf("Increased maximum number of open files "
                       "to %llu (it was originally set to %llu).",
                       (unsigned long long) maxfiles,
                       (unsigned long long) oldlimit);
            }
        }
    }
}
void signal_exit_func(int signo)
{
    printf("exit sig is %d\n", signo);
    stop = 1;
}
void signal_exit_handler()
{
    struct sigaction sa;
    memset(&sa, 0, sizeof(sa));
    sa.sa_handler = signal_exit_func;
    sigaction(SIGINT, &sa, NULL);//当按下ctrl+c时，它的效果就是发送SIGINT信号
    sigaction(SIGTERM, &sa, NULL);//kill pid
    sigaction(SIGQUIT, &sa, NULL);//ctrl+\代表退出SIGQUIT
    //SIGSTOP和SIGKILL信号是不可捕获的,所以下面两句话写了等于没有写
    sigaction(SIGKILL, &sa, NULL);//kill -9 pid
    sigaction(SIGSTOP, &sa, NULL);//ctrl+z代表停止
    //#define    SIGTERM        15
    //#define    SIGKILL        9
    //kill和kill -9，两个命令在linux中都有杀死进程的效果，然而两命令的执行过程却大有不同，在程序中如果用错了，可能会造成莫名其妙的现象。
    //执行kill pid命令，系统会发送一个SIGTERM信号给对应的程序。
    //执行kill -9 pid命令，系统给对应程序发送的信号是SIGKILL，即exit。exit信号不会被系统阻塞，所以kill -9能顺利杀掉进程。
}
//./mystressclient 172.16.6.161 8011 1000 1
//argv[1] 表示服务器IP地址
//argv[2] 表示服务器端口
//argv[3] 表示建立多少个client
//argv[4] 表示每建立多少个client就延时等待1s
int main( int argc, char* argv[] )
{
    //signal(SIGHUP, SIG_IGN); //开启的话，就捕获不到终端窗口关闭的信号了。即窗口关闭，进程仍然进行。
    signal(SIGPIPE, SIG_IGN);
    signal_exit_handler();
    int background = 0;
    if (background)
    {
        daemonize();
    }
    assert( argc == 5 );
    int maxclients = atoi( argv[ 3 ] ) + CONFIG_FDSET_INCR;
    adjustOpenFilesLimit(maxclients);
    int epoll_fd = epoll_create( 1024 );/* 1024 is just a hint for the kernel */
    start_conn( epoll_fd, atoi( argv[ 3 ] ), argv[1], atoi( argv[2] ), atoi( argv[4] ) );
    //epoll_event events[ 10000 ];
    epoll_event *events = (epoll_event*)malloc(sizeof(struct epoll_event) * (maxclients));
    char buffer[ 2048 ];
    while ( !stop )
    {
        int fds = epoll_wait( epoll_fd, events, maxclients, 2000 );
        for ( int i = 0; i < fds; i++ )
        {
            int sockfd = events[i].data.fd;
            if ( events[i].events & EPOLLIN )
            {
                if ( ! read_once( sockfd, buffer, 2048 ) )
                {
                    close_conn( epoll_fd, sockfd );
                }
                struct epoll_event event;
                //event.events = EPOLLOUT | EPOLLET | EPOLLERR;
                event.events = EPOLLOUT | EPOLLERR;
                event.data.fd = sockfd;
                epoll_ctl( epoll_fd, EPOLL_CTL_MOD, sockfd, &event );
            }
            else if( events[i].events & EPOLLOUT )
            {
                if ( ! write_nbytes( sockfd, request, strlen( request ) ) )
                {
                    close_conn( epoll_fd, sockfd );
                }
                struct epoll_event event;
                //event.events = EPOLLIN | EPOLLET | EPOLLERR;
                event.events = EPOLLIN | EPOLLERR;
                event.data.fd = sockfd;
                epoll_ctl( epoll_fd, EPOLL_CTL_MOD, sockfd, &event );
            }
            else if( events[i].events & EPOLLERR )
            {
                close_conn( epoll_fd, sockfd );
            }
        }
    }
    close(epoll_fd);
    if (events)
    {
        free(events);
    }
    printf("exit!\n");
}
```
注意：addfd函数，初始化是event.events = EPOLLOUT | EPOLLERR;输出



输入终端命令，开始压力测试

./mystressclient 172.16.6.161 8011 20000 100

./mystressclient 192.168.83.128 1883 50000 2000

//172.16.6.161 表示服务器IP地址
//8011  表示服务器端口
//20000  表示建立20000个client
//100 表示每建立100个client就延时等待1s