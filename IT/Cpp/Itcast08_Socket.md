<!-- MarkdownTOC -->

- [1. Makefile相关](#1-makefile相关)
- [2. 大端/小端存储方式验证](#2-大端小端存储方式验证)
- [3. server/client Socket最简实现](#3-serverclient-socket最简实现)
- [4. TCP建立和断开连接](#4-tcp建立和断开连接)
- [5. goto语句相关](#5-goto语句相关)
- [6. 多进程socket并发服务器实现及CLOSE_WAIT问题解决](#6-多进程socket并发服务器实现及close_wait问题解决)
- [7. 多线程并发服务器实现](#7-多线程并发服务器实现)
- [8. 多线程QQ聊天室socket](#8-多线程qq聊天室socket)
- [9. 多路I/O复用概念](#9-多路io复用概念)
- [10. multiI/O-select](#10-multiio-select)
- [11. multiio-epoll1最简](#11-multiio-epoll1最简)
- [12. UDP服务器实现](#12-udp服务器实现)
- [13. 得到结构体成员偏移位置offsetof函数](#13-得到结构体成员偏移位置offsetof函数)
- [14. epoll调用epoll_wait的两种触发方式LT/ET-pipe](#14-epoll调用epoll_wait的两种触发方式ltet-pipe)
- [15. epoll调用epoll_wait的两种触发方式LT/ET-Socket-Block](#15-epoll调用epoll_wait的两种触发方式ltet-socket-block)
- [16. epoll调用epoll_wait的两种触发方式LT/ET-Socket-NonBlock](#16-epoll调用epoll_wait的两种触发方式ltet-socket-nonblock)
- [17. epoll非阻塞io事件驱动服务器Reactor模型-重要](#17-epoll非阻塞io事件驱动服务器reactor模型-重要)
- [18. 线程池模型](#18-线程池模型)
- [19. C语言项目设计开发流程](#19-c语言项目设计开发流程)
- [20. Shell相关](#20-shell相关)
- [21. C语言中使用正则表达式](#21-c语言中使用正则表达式)
- [22. C程序中打日志](#22-c程序中打日志)

<!-- /MarkdownTOC -->


<a id="1-makefile相关"></a>
#### 1. Makefile相关
* 共享库.so文件当接口(函数名/参数/返回值)发生改变时需要升级主版本号
* soname: gcc -shared 生成库so文件时指定的 libxxx.so
    - /etc/ld.so.conf 加入一行 /Code/project/src
    - 执行ldconfig自动生成soname
* linkname: 编译时链接器使用, 是realname的软链接
* ![](image\Makefile.PNG)
* gcc -shared -fPIC -o 1.so 1.c:
    - -fPIC 作用于编译阶段，告诉编译器产生与位置无关代码(Position-Independent Code)，则产生的代码中，没有绝对地址，全部使用相对地址，故而代码可以被加载器加载到内存的任意位置，都可以正确的执行。这正是共享库所要求的，共享库被加载时，在内存的位置不是固定的。

<a id="2-大端小端存储方式验证"></a>
#### 2. 大端/小端存储方式验证
* 数字 0x12345678在内存中的表示形式
    - 小端: 低地址存低字节(78:低字节)
        + 低地址 --------------------> 高地址<br>
            78  |  56  |  34  |  12
    - 大端: 低地址存高字节
        + 低地址 --------------------> 高地址<br>
            12  |  34  |  56  |  78
* 32bit宽的数0x12345678在Little-endian模式以及Big-endian模式）CPU内存中的存放方式(假设从地址0x4000开始存放)为:
![](image\Big_Little_endian.PNG)
* 网络字节序是大端存储Big_endian(Unix是大端)
* x86 CPU是小端存储Little_endian
```
int checkEndian()
{
    int a = 1;
    char *p = (char *)&a;
 
    return (*p == 1);
}
// 如果是大端，*p的结果是0
```
```
#include <stdio.h>

int checkCPU()
{
    // 联合体union的存放顺序是所有成员都从低地址开始存放
    union
    {
        int a;
        char b;
    }c;

    c.a = 1;
    return (c.b == 1);
}
// c.b == 1 低地址放低字节 -> 小端

int main(int argc, char** argv)
{
    if(checkCPU())
    {
        printf("System is Little endian\n");
    }else
    {
        printf("System is Big endian\n");
    }

    return 0;
}
```

<a id="3-serverclient-socket最简实现"></a>
#### 3. server/client Socket最简实现
* server.c
    - 服务器需要bind端口
    - `cat /proc/sys/net/ipv4/tcp_max_syn_backlog`  // 128
```
#include <stdio.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <string.h>
#include <sys/socket.h>
#include <arpa/inet.h>

#define SERV_PORT 8000

int main(void)
{
    int sfd, cfd;
    struct sockaddr_in serv_addr, cli_addr;
    socklen_t addr_len;
    char buf[4096], client_ip[128];
    int i, len;

    //socket
    sfd = socket(AF_INET, SOCK_STREAM, 0);
    
    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    serv_addr.sin_addr.s_addr = htonl(INADDR_ANY);
    
    //bind
    bind(sfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));

    //listen
    listen(sfd, 128);   //128是服务端响应客户端3次握手阶段的数量
    
    //accept
    printf("wait for connect...\n");
    addr_len = sizeof(cli_addr);
    cfd = accept(sfd, (struct sockaddr*)&cli_addr, &addr_len);
    printf("client IP: %s\t%d\n",
            inet_ntop(AF_INET, &cli_addr.sin_addr.s_addr, client_ip, sizeof(client_ip)),
            ntohs(cli_addr.sin_port));

    //read write
    while(1)
    {
        len = read(cfd, buf, sizeof(buf));
        write(STDOUT_FILENO, buf, len);

        for(i=0;i<len;++i)
        {
            buf[i] = toupper(buf[i]);
        }
        write(cfd, buf, len);
    }

    close(sfd);
    close(cfd);

    return 0;
}
```
* client.c:
    - 客户端不需要bind端口, 随机分配
```
#include <stdio.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <string.h>
#include <sys/socket.h>
#include <arpa/inet.h>

#define SERV_PORT 8000

int main(int argc, char** argv)
{
    if(argc < 2)
    {
        printf("usage: %s serverip\n", argv[0]);
        return 1;
    }
    
    struct sockaddr_in serv_addr;
    int sfd, len;
    char buf[4096];

    //sockaddr_in
    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    inet_pton(AF_INET, argv[1], &serv_addr.sin_addr.s_addr);

    //socket
    sfd = socket(AF_INET, SOCK_STREAM, 0);

    //connect
    connect(sfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));
    
    //write read
    while(fgets(buf, sizeof(buf), stdin))
    {
        write(sfd, buf, strlen(buf));
        len = read(sfd, buf, sizeof(buf));
        write(STDOUT_FILENO, buf, len);
    }

    close(sfd);

    return 0;
}
```
* 查看socket端口: netstat -anp | grep 8000
* TCP/UDP网络端口测试: `nc -p 31337 -w 5 host.example.com 42`

<a id="4-tcp建立和断开连接"></a>
#### 4. TCP建立和断开连接
* mss 1460 = MTU 1500 - IP 20 - TCP 20
* ![](image\TCP连接建立断开.PNG)
* 11种TCP状态: 实线表示客户, 虚线表示服务器 (使用 netstat 可以查看TCP状态)
* ![](image\TCP状态转换图.PNG)
* ![](image\TCP状态转换解释图.PNG)
* 简单解释：
l  CLOSED：初始状态，表示TCP连接是“关闭着的”或“未打开的”。

l  LISTEN ：表示服务器端的某个SOCKET处于监听状态，可以接受客户端的连接。

l  SYN_RCVD ：表示服务器接收到了来自客户端请求连接的SYN报文。在正常情况下，这个状态是服务器端的SOCKET在建立TCP连接时的三次握手会话过程中的一个中间状态，很短暂，基本上用netstat很难看到这种状态，除非故意写一个监测程序，将三次TCP握手过程中最后一个ACK报文不予发送。当TCP连接处于此状态时，再收到客户端的ACK报文，它就会进入到ESTABLISHED 状态。

l  SYN_SENT ：这个状态与SYN_RCVD 状态相呼应，当客户端SOCKET执行connect()进行连接时，它首先发送SYN报文，然后随即进入到SYN_SENT 状态，并等待服务端的发送三次握手中的第2个报文。SYN_SENT 状态表示客户端已发送SYN报文。

l  ESTABLISHED ：表示TCP连接已经成功建立。

l  FIN_WAIT_1 ：这个状态得好好解释一下，其实FIN_WAIT_1 和FIN_WAIT_2 两种状态的真正含义都是表示等待对方的FIN报文。而这两种状态的区别是：FIN_WAIT_1状态实际上是当SOCKET在ESTABLISHED状态时，它想主动关闭连接，向对方发送了FIN报文，此时该SOCKET进入到FIN_WAIT_1 状态。而当对方回应ACK报文后，则进入到FIN_WAIT_2 状态。当然在实际的正常情况下，无论对方处于任何种情况下，都应该马上回应ACK报文，所以FIN_WAIT_1 状态一般是比较难见到的，而FIN_WAIT_2 状态有时仍可以用netstat看到。

l  FIN_WAIT_2 ：上面已经解释了这种状态的由来，实际上FIN_WAIT_2状态下的SOCKET表示半连接，即有一方调用close()主动要求关闭连接。注意：FIN_WAIT_2 是没有超时的（不像TIME_WAIT 状态），这种状态下如果对方不关闭（不配合完成4次挥手过程），那这个 FIN_WAIT_2 状态将一直保持到系统重启，越来越多的FIN_WAIT_2 状态会导致内核crash。

l  TIME_WAIT ：表示收到了对方的FIN报文，并发送出了ACK报文。 TIME_WAIT状态下的TCP连接会等待2*MSL（Max Segment Lifetime，最大分段生存期，指一个TCP报文在Internet上的最长生存时间。每个具体的TCP协议实现都必须选择一个确定的MSL值，RFC 1122建议是2分钟，但BSD传统实现采用了30秒，Linux可以cat /proc/sys/net/ipv4/tcp_fin_timeout看到本机的这个值），然后即可回到CLOSED 可用状态了。如果FIN_WAIT_1状态下，收到了对方同时带FIN标志和ACK标志的报文时，可以直接进入到TIME_WAIT状态，而无须经过FIN_WAIT_2状态。（这种情况应该就是四次挥手变成三次挥手的那种情况）

l  CLOSING ：这种状态在实际情况中应该很少见，属于一种比较罕见的例外状态。正常情况下，当一方发送FIN报文后，按理来说是应该先收到（或同时收到）对方的ACK报文，再收到对方的FIN报文。但是CLOSING 状态表示一方发送FIN报文后，并没有收到对方的ACK报文，反而却也收到了对方的FIN报文。什么情况下会出现此种情况呢？那就是当双方几乎在同时close()一个SOCKET的话，就出现了双方同时发送FIN报文的情况，这是就会出现CLOSING 状态，表示双方都正在关闭SOCKET连接。

l  CLOSE_WAIT ：表示正在等待关闭。怎么理解呢？当对方close()一个SOCKET后发送FIN报文给自己，你的系统毫无疑问地将会回应一个ACK报文给对方，此时TCP连接则进入到CLOSE_WAIT状态。接下来呢，你需要检查自己是否还有数据要发送给对方，如果没有的话，那你也就可以close()这个SOCKET并发送FIN报文给对方，即关闭自己到对方这个方向的连接。有数据的话则看程序的策略，继续发送或丢弃。简单地说，当你处于CLOSE_WAIT 状态下，需要完成的事情是等待你去关闭连接。

l  LAST_ACK ：当被动关闭的一方在发送FIN报文后，等待对方的ACK报文的时候，就处于LAST_ACK 状态。当收到对方的ACK报文后，也就可以进入到CLOSED 可用状态了。

<a id="5-goto语句相关"></a>
#### 5. goto语句相关
* goto语句使用范围: a:当前函数 b:当前文件 c:当前项目  
    -  选a

<a id="6-多进程socket并发服务器实现及close_wait问题解决"></a>
#### 6. 多进程socket并发服务器实现及CLOSE_WAIT问题解决
* warp.c warp.h: 错误处理
    - 封装为首字母大写的函数,仍然可以在vim中用2+K查询man page 
* server主动/被动 断开client端socket连接, 避免CLOSE_WAIT 占用端口和进程的问题
* server.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <fcntl.h>
#include <string.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include "wrap.h"
#include <signal.h>
#include <sys/wait.h>
#include <sys/types.h>
#include <stdbool.h>
#include <netinet/tcp.h>

#define SERV_PORT 8000

void do_sig(int n)
{
    while(waitpid(0, NULL, WNOHANG) > 0)
    {

    };
}

int main(void)
{
    int cfd, lfd; //lfd: listen fd
    struct sockaddr_in serv_addr, cli_addr;
    socklen_t addr_len;
    char buf[4096], client_ip[128];
    int i, len;
    pid_t pid;
    struct sigaction act;

    //sigaction
    act.sa_handler = do_sig;
    act.sa_flags = 0;
    sigemptyset(&act.sa_mask);

    sigaction(SIGCHLD, &act, NULL);
    //sigaction(SIGPIPE, &act, NULL);

    //socket
    lfd = socket(AF_INET, SOCK_STREAM, 0);

    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    //serv_addr.sin_addr.s_addr = htonl(INADDR_ANY);
    serv_addr.sin_addr.s_addr = inet_addr("192.168.30.190");

    //bind
    Bind(lfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));

    //listen
    Listen(lfd, 128);

    printf("wait for connect...\n");

    //read write
    while(1)
    {
        //accept
        addr_len = sizeof(cli_addr);
        cfd = Accept(lfd, (struct sockaddr*)&cli_addr, &addr_len);
        
        int keepAlive = 1; // 开启keepalive属性
        int keepIdle = 60; // 如该连接在60秒内没有任何数据往来,则进行探测 
        int keepInterval = 5; // 探测时发包的时间间隔为5 秒
        int keepCount = 3; // 探测尝试的次数.如果第1次探测包就收到响应了,则后2次的不再发.

        setsockopt(cfd, SOL_SOCKET, SO_KEEPALIVE, (void *)&keepAlive, sizeof(keepAlive));
        setsockopt(cfd, IPPROTO_TCP, TCP_KEEPIDLE, (void*)&keepIdle, sizeof(keepIdle));
        setsockopt(cfd, IPPROTO_TCP, TCP_KEEPINTVL, (void *)&keepInterval, sizeof(keepInterval));
        setsockopt(cfd, IPPROTO_TCP, TCP_KEEPCNT, (void *)&keepCount, sizeof(keepCount));
        
        printf("client IP: %s\t%d\n",
                inet_ntop(AF_INET, &cli_addr.sin_addr.s_addr, client_ip, sizeof(client_ip)),
                ntohs(cli_addr.sin_port));

        pid = fork();
        if(pid == 0)
        {
            Close(lfd);
            while(1)
            {
                len = Read(cfd, buf, sizeof(buf));
                if(strcmp(buf, "exit\n") == 0 || len <= 0)
                {
                    printf("client exit\n");
                    Close(cfd);
                    return 0;
                }
                Write(STDOUT_FILENO, buf, len);

                for(i=0;i<len;++i)
                {
                    buf[i] = toupper(buf[i]);
                }
                Write(cfd, buf, len);
            }
        }else if(pid > 0)
        {
            Close(cfd);
        }else
        {
            perr_exit("fork err");
        }

    }

    Close(lfd);

    return 0;
}
```
* client.c:
```
#include <stdio.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <string.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include "wrap.h"

#define SERV_PORT 8000

int main(int argc, char** argv)
{
    if(argc < 2)
    {
        printf("usage: %s serverip\n", argv[0]);
        return 1;
    }
    
    struct sockaddr_in serv_addr;
    int sfd, len;
    char buf[4096];

    //sockaddr_in
    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    inet_pton(AF_INET, argv[1], &serv_addr.sin_addr.s_addr);

    //socket
    sfd = Socket(AF_INET, SOCK_STREAM, 0);

    //connect
    Connect(sfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));
    
    //write read
    while(fgets(buf, sizeof(buf), stdin))
    {
        Write(sfd, buf, strlen(buf));
        if(strcmp(buf, "exit\n") == 0)
        {
            shutdown(sfd, SHUT_RDWR);
            break;
        }
        len = Read(sfd, buf, sizeof(buf));
        Write(STDOUT_FILENO, buf, len);
        
    }

    Close(sfd);

    return 0;
}
```

<a id="7-多线程并发服务器实现"></a>
#### 7. 多线程并发服务器实现
* 将线程栈的大小从8M减为1M可以增加线程并发数量
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <string.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include "wrap.h"
#include <sys/types.h>
#include <pthread.h>

#define SERV_PORT 8000

void *do_work(void *arg)
{
    pthread_detach(pthread_self());

    int len, i;
    int cfd = (int)arg;
    char buf[1024];

    while(1)
    {
        len = Read(cfd, buf, sizeof(buf));
        if(strcmp(buf, "exit\n") == 0 || len <= 0)
        {
            printf("client exit\n");
            Close(cfd);
            break;
        }
        Write(STDOUT_FILENO, buf, len);

        for(i=0;i<len;++i)
        {
            buf[i] = toupper(buf[i]);
        }
        Write(cfd, buf, len);

    }
    return NULL;
}

int main(void)
{
    int cfd, lfd; //lfd: listen fd
    struct sockaddr_in serv_addr, cli_addr;
    socklen_t addr_len;
    char client_ip[128];
    pthread_t tid;

    //socket
    lfd = Socket(AF_INET, SOCK_STREAM, 0);

    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    serv_addr.sin_addr.s_addr = inet_addr("192.168.30.190");

    //bind
    Bind(lfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));

    //listen
    Listen(lfd, 128);

    printf("wait for connect...\n");

    //read write
    while(1)
    {
        //accept
        addr_len = sizeof(cli_addr);
        cfd = Accept(lfd, (struct sockaddr*)&cli_addr, &addr_len);


        printf("client IP: %s\t%d\n",
                inet_ntop(AF_INET, &cli_addr.sin_addr.s_addr, client_ip, sizeof(client_ip)),
                ntohs(cli_addr.sin_port));

        pthread_create(&tid, NULL, do_work, (void *)cfd);
    }

    Close(lfd);

    return 0;
}
```

<a id="8-多线程qq聊天室socket"></a>
#### 8. 多线程QQ聊天室socket
* pthread, mutex, linked_list, 
* qq_socket_server.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <fcntl.h>
#include <errno.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <netinet/tcp.h>
#include <arpa/inet.h>
#include "server_record.h"
#include "qq_socket.h"
#include "wrap.h"

#define SERV_PORT 8000

//shared
record head = NULL;
pthread_mutex_t mutex;


void sys_err(const char* err)
{
    perror(err);
    exit(1);
}

void login_qq(int cfd, struct QQ_DATA_INFO *dbuf, record *head)
{
    //create record
    pthread_mutex_lock(&mutex);
    record r = create_record(dbuf->srcname, cfd);

    //insert
    insert_record(head, r);
    pthread_mutex_unlock(&mutex);
}

void talk_qq(struct QQ_DATA_INFO *dbuf, record *head)
{
    record p = search_record(head, dbuf->dstname);
    if(p == NULL)
    {
        struct QQ_DATA_INFO offline;
        offline.protocol = 3;
        strcpy(offline.dstname, dbuf->dstname);
        record srcp = search_record(head, dbuf->srcname);
        write(srcp->cfd, &offline, sizeof(offline));
    }else
    {
        write(p->cfd, dbuf, sizeof(*dbuf));
    }

}

void logout_qq(struct QQ_DATA_INFO *dbuf, record *head)
{
    record p = search_record(head, dbuf->srcname);
    if(p == NULL)
        return;
    close(p->cfd);
    pthread_mutex_lock(&mutex);
    delete_record(head, p);
    pthread_mutex_unlock(&mutex);
    free_record(p);
}

void *tfn(void *arg)
{
    pthread_detach(pthread_self());
    int cfd = (int)arg;
    struct QQ_DATA_INFO dbuf;
    
    while(1)
    {
        read(cfd, &dbuf, sizeof(dbuf));
        switch(dbuf.protocol)
        {
            case 1: 
                login_qq(cfd, &dbuf, &head); 
                printf("record: %s  %d\n", head->username, head->cfd);
                break;
            case 2:
                talk_qq(&dbuf, &head);
                break;
            case 4:
                logout_qq(&dbuf, &head);
                break;
            default: continue;
        }
    }
}



int main(int argc, char** argv)
{

    int lfd, cfd;
    struct sockaddr_in serv_addr, cli_addr;
    socklen_t addr_len;
    char client_ip[128];
    pthread_t tid;
    
    pthread_mutex_init(&mutex, NULL);

    //socket
    lfd = Socket(AF_INET, SOCK_STREAM, 0);

    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    serv_addr.sin_addr.s_addr = inet_addr("192.168.30.190");

    Bind(lfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));

    Listen(lfd, 128);
    printf("wait for connect...\n");
    
    server_record_init(&head);

    while(1)
    {
        addr_len = sizeof(cli_addr);
        cfd = Accept(lfd, (struct sockaddr*)&cli_addr, &addr_len);
        
        int keepAlive = 1; 
        int keepIdle = 60; 
        int keepInterval = 5;
        int keepCount = 3;

        setsockopt(cfd, SOL_SOCKET, SO_KEEPALIVE, (void *)&keepAlive, sizeof(keepAlive));
        setsockopt(cfd, IPPROTO_TCP, TCP_KEEPIDLE, (void*)&keepIdle, sizeof(keepIdle));
        setsockopt(cfd, IPPROTO_TCP, TCP_KEEPINTVL, (void *)&keepInterval, sizeof(keepInterval));
        setsockopt(cfd, IPPROTO_TCP, TCP_KEEPCNT, (void *)&keepCount, sizeof(keepCount));

        printf("client IP: %s\t%d\n",
                inet_ntop(AF_INET, &cli_addr.sin_addr.s_addr, client_ip, sizeof(client_ip)),
                ntohs(cli_addr.sin_port));

        pthread_create(&tid, NULL, tfn, (void *)cfd);
    }

    destroy_link(&head);

    Close(cfd);
    Close(lfd);

    pthread_mutex_destroy(&mutex);

    return 0;
}
```
* qq_socket_client.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <errno.h>
#include <fcntl.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include "qq_socket.h"
#include "wrap.h"

#define SERV_IP "192.168.30.190"
#define SERV_PORT 8000

void sys_err(const char* err)
{
    perror(err);
    exit(1);
}


int main(int argc, char** argv)
{
    int sfd;
    int len, flag;
    struct QQ_DATA_INFO dbuf, tmpbuf, talkbuf;
    char cmd_buf[100];
    char *username;
    struct sockaddr_in serv_addr;

    if(argc < 2)
    {
        printf("usage: %s username\n", argv[0]);
        exit(1);
    }
    
    username = argv[1];
    
    //sockaddr_in
    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    inet_pton(AF_INET, SERV_IP, &serv_addr.sin_addr.s_addr);

    //socket
    sfd = Socket(AF_INET, SOCK_STREAM, 0);

    //connect
    Connect(sfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));
    
    //login
    dbuf.protocol = 1;
    strcpy(dbuf.srcname, username);
    write(sfd, &dbuf, sizeof(dbuf));

    //nonblock
    flag = fcntl(STDIN_FILENO, F_GETFL);
    flag |= O_NONBLOCK;
    fcntl(STDIN_FILENO, F_SETFL, flag);

    flag = fcntl(sfd, F_GETFL);
    flag |= O_NONBLOCK;
    fcntl(sfd, F_SETFL, flag);

    while(1)
    {
        // display
        len = read(sfd, &tmpbuf, sizeof(tmpbuf));
        if(len > 0)
        {
            if(tmpbuf.protocol == 3)
            {
                // dstname offline
                printf("%s offline\n", tmpbuf.dstname);
            }else if(tmpbuf.protocol == 2)
            {
                printf("%s said: %s\n", tmpbuf.srcname, tmpbuf.data);
            }else
            {
                continue;
            }
        }else if(len < 0)
        {
            if(errno != EAGAIN)
            {
                sys_err("client read err");
            }
        }

        // send
        len = read(STDIN_FILENO, &cmd_buf, sizeof(cmd_buf));
        
        if(len > 0)
        {
            char *dname, *databuf;
            memset(&talkbuf, 0, sizeof(talkbuf));
            cmd_buf[len] = '\0';

            dname = strtok(cmd_buf, "#\n");
            if(dname == NULL)
            {
                continue;
            }
            if(strcmp(dname, "exit") == 0)
            {
                // logoff
                talkbuf.protocol = 4;
                strcpy(talkbuf.srcname, username);
                write(sfd, &talkbuf, sizeof(talkbuf));
                break;
            }else
            {
                // talk
                databuf = strtok(NULL, "\0");
                if(databuf == NULL)
                    continue;
                talkbuf.protocol = 2;
                strcpy(talkbuf.srcname, username);
                strcpy(talkbuf.dstname, dname);
                strcpy(talkbuf.data, databuf);
            }
            write(sfd, &talkbuf, sizeof(talkbuf));
        }
    }

    

    return 0;
}
```

<a id="9-多路io复用概念"></a>
#### 9. 多路I/O复用概念
* 业务应用场景:
    - 多进程/多线程: I/O密集型(I/O耗时长)
    - select/epoll: CPU密集型(计算多,I/O耗时短)
* select/epoll 原理: 内核阻塞监听所有文件描述符, 当有数据时再返回给业务处理程序(一个进程,顺序处理) 

<a id="10-multiio-select"></a>
#### 10. multiI/O-select
* server.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <sys/time.h>
#include <arpa/inet.h>
#include <netinet/in.h>
#include "wrap.h"

#define SERV_PORT 8000
#define MAXLINE 4096


int main(void)
{
    int sfd, cfd, maxfd, maxi, nready;
    int client[FD_SETSIZE];
    struct sockaddr_in serv_addr, cli_addr;
    socklen_t addr_len;
    char buf[MAXLINE], client_ip[128];
    int i;
    ssize_t len;
    fd_set rset, allset; // readset

    //socket
    sfd = socket(AF_INET, SOCK_STREAM, 0);
    
    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    serv_addr.sin_addr.s_addr = htonl(INADDR_ANY);
    
    //bind
    bind(sfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));

    //listen
    listen(sfd, 128);

    //accept
    printf("wait for connect...\n");

    maxi = -1;
    for(i=0;i<FD_SETSIZE;++i)
    {
        client[i] = -1;
    }

    maxfd = sfd;

    FD_ZERO(&allset);
    FD_SET(sfd, &allset);

    while(1)
    {
        //每次循环在select前, 必须先copy需要监视的fd_set
        rset = allset;

        //每次调用select会轮询阻塞rset, 当有event发生时, 会将rset相应的位 置1(修改rset), 并返回连接就绪个数
        nready = select(maxfd+1, &rset, NULL, NULL, NULL);  //nready: 已经连接就绪的fd个数(需要处理数据的socket连接数)
        if(nready < 0)
        {
            perror("select err");
            exit(1);
        }

        //如果有新连接, add到allset
        if(FD_ISSET(sfd, &rset))
        {
            addr_len = sizeof(cli_addr);
            cfd = Accept(sfd, (struct sockaddr*)&cli_addr, &addr_len);
            printf("client IP: %s\t%d\n",
                    inet_ntop(AF_INET, &cli_addr.sin_addr.s_addr, client_ip, sizeof(client_ip)),
                    ntohs(cli_addr.sin_port));
            
            for(i=0;i<FD_SETSIZE;++i)
            {
                if(client[i] < 0)
                {
                    client[i] = cfd;
                    break;
                }
            }

            if(i > FD_SETSIZE)
            {
                fputs("too many client connect...\n", stderr);
                exit(1);
            }

            if(cfd > maxfd)
                maxfd = cfd;
            if(i > maxi)
                maxi = i;

            FD_SET(cfd, &allset);
        
            if(--nready == 0)
                continue;
        }

        //处理listenfd以外的连接event(业务)
        for(i=0;i<=maxi;++i)
        {
            if(client[i] < 0)
                continue;
            if(FD_ISSET(client[i], &rset))
            {
                if((len = Read(client[i], buf, MAXLINE)) == 0)
                {
                    Close(client[i]);
                    FD_CLR(client[i], &allset);
                    client[i] = -1;
                }else
                {
                    Write(STDOUT_FILENO, buf, len);
                    int j;
                    for(j=0;j<len;++j)
                    {
                        buf[j] = toupper(buf[j]);
                    }
                    Write(client[i], buf, len);
                }
                if(--nready == 0)
                    break;
            }
            

        }
    }
    Close(sfd);

    return 0;
}
```

<a id="11-multiio-epoll1最简"></a>
#### 11. multiio-epoll1最简
* Api: epoll_create, epoll_ctl, epoll_wait, struct epoll_event 
* server.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <sys/time.h>
#include <sys/epoll.h>
#include <arpa/inet.h>
#include <netinet/in.h>
#include "wrap.h"

#define SERV_PORT 8000
#define MAXLINE 4096
#define OPEN_MAX 10000


int main(int argc, char *argv[])
{
    int sfd, connfd, sockfd, nready;
    struct sockaddr_in serv_addr, cli_addr;
    socklen_t addr_len;
    char buf[MAXLINE], client_ip[128];
    int i, j, num;
    ssize_t len, efd, res;
    struct epoll_event tep, ep[OPEN_MAX];   //tep: temp ep event
    

    //socket
    sfd = Socket(AF_INET, SOCK_STREAM, 0);
    
    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    serv_addr.sin_addr.s_addr = htonl(INADDR_ANY);
    
    //bind
    Bind(sfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));

    //listen
    Listen(sfd, 128);

    //epoll create rb_tree
    efd = epoll_create(OPEN_MAX);
    if(efd == -1)
    {
        perror("epoll create err");
        exit(1);
    }

    // tep(server event)->add->ep[]
    tep.events = EPOLLIN;
    tep.data.fd = sfd;
    res = epoll_ctl(efd, EPOLL_CTL_ADD, sfd, &tep);
    if(res == -1)
        perror("epoll ctl add tep err");

    //accept
    printf("wait for connect...\n");

    while(1)
    {
        nready = epoll_wait(efd, ep, OPEN_MAX, -1);
        if(nready == -1)
        {
            perror("epoll wait err");
            exit(1);
        }

        for(i=0;i<nready;++i)
        {
            // find EPOLLIN
            if(!ep[i].events & EPOLLIN)
                continue;

            // new connect add to ep[]
            if(ep[i].data.fd == sfd)
            {
                addr_len = sizeof(cli_addr);
                connfd = Accept(sfd, (struct sockaddr*)&cli_addr, &addr_len);
                printf("client IP: %s\t%d\n",
                    inet_ntop(AF_INET, &cli_addr.sin_addr.s_addr, client_ip, sizeof(client_ip)),
                    ntohs(cli_addr.sin_port));
                printf("connect client num: %d\tconnfd: %d\n", ++num, connfd);

                tep.events = EPOLLIN;
                tep.data.fd = connfd;
                res = epoll_ctl(efd, EPOLL_CTL_ADD, connfd, &tep);
                if(res == -1)
                    perror("epoll ctrl add new err");
            }
            else
            {
                // handle business
                sockfd = ep[i].data.fd;
                len = Read(sockfd, buf, MAXLINE);

                if(len == 0)
                {   // client close
                    res = epoll_ctl(efd, EPOLL_CTL_DEL, sockfd, NULL);
                    if(res == -1)
                        perror("epoll del err");
                    Close(sockfd);
                    printf("client[%d] closed connection\n", sockfd);
                }else if(len < 0)
                {
                    perror("read err");
                    res = epoll_ctl(efd, EPOLL_CTL_DEL, sockfd, NULL);
                    Close(sockfd);
                }else
                {
                    Write(STDOUT_FILENO, buf, len);
                    for(j=0;j<len;++j)
                    {
                        buf[j] = toupper(buf[j]);
                    }
                    Write(sockfd, buf, len);
                }

            }
        }
    }
    
    Close(sfd);
    Close(efd);

    return 0;
}
```

<a id="12-udp服务器实现"></a>
#### 12. UDP服务器实现
* 应用场景: 局域网, 不经过路由器
* udp服务器c/s:
    - 支持并发请求
    - server.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <fcntl.h>
#include <string.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include "wrap.h"
#include <sys/types.h>
#include <netinet/tcp.h>

#define SERV_PORT 8000
#define SERV_IP "192.168.30.190"

int main(int argc, char *argv[])
{
    int sfd;
    struct sockaddr_in serv_addr, cli_addr;
    socklen_t addr_len;
    char buf[4096], client_ip[128];
    int i, len;
    

    //socket UDP
    sfd = Socket(AF_INET, SOCK_DGRAM, 0);

    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    serv_addr.sin_addr.s_addr = inet_addr(SERV_IP);

    //bind
    Bind(sfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));

    //listen
    //listen(sfd, 128);

    printf("wait for connect...\n");

    //read write
    while(1)
    {
        //recv
        addr_len = sizeof(cli_addr);
        len = recvfrom(sfd, buf, sizeof(buf), 0, (struct sockaddr*)&cli_addr, &addr_len);
        
        printf("client IP: %s\t%d\n",
                inet_ntop(AF_INET, &cli_addr.sin_addr.s_addr, client_ip, sizeof(client_ip)),
                ntohs(cli_addr.sin_port));

        //handle business
        for(i=0;i<len;++i)
        {
            buf[i] = toupper(buf[i]);
        }
        //sendto
        sendto(sfd, buf, len, 0, (struct sockaddr*)&cli_addr, addr_len);
    }

    Close(sfd);

    return 0;
}
```
    - client.c:
```
#include <stdio.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <string.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include "wrap.h"

#define SERV_PORT 8000
#define SERV_IP "192.168.30.190"

int main(int argc, char** argv)
{
    
    struct sockaddr_in serv_addr;
    socklen_t addr_len;
    int sfd, len;
    char buf[4096];

    //sockaddr_in
    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    inet_pton(AF_INET, SERV_IP, &serv_addr.sin_addr.s_addr);

    //socket UDP
    sfd = Socket(AF_INET, SOCK_DGRAM, 0);

    while(fgets(buf, sizeof(buf), stdin))
    {
        //sendto
        addr_len = sizeof(serv_addr);
        sendto(sfd, buf, strlen(buf), 0, (struct sockaddr*)&serv_addr, addr_len);

        //recv
        len = recvfrom(sfd, buf, sizeof(buf), 0, NULL, 0);
        buf[len] = '\0';
        printf("recv: %s\n", buf);
    }

    Close(sfd);

    return 0;
}
```
* udp广播:
    - server.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <fcntl.h>
#include <string.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include "wrap.h"
#include <sys/types.h>
#include <netinet/tcp.h>

#define SERV_PORT 8000
#define CLIENT_PORT 9000
#define BROADCAST_IP "192.168.30.255"

int main(int argc, char *argv[])
{
    int sfd;
    struct sockaddr_in serv_addr, cli_addr;
    socklen_t addr_len;
    char buf[4096] = "Bryan.bai udp broadcast\n";
    

    //socket UDP
    sfd = Socket(AF_INET, SOCK_DGRAM, 0);

    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    serv_addr.sin_addr.s_addr = inet_addr("192.168.30.190");

    //bind
    Bind(sfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));
    
    //open broadcast
    int flag = 1;
    setsockopt(sfd, SOL_SOCKET, SO_BROADCAST, &flag, sizeof(flag));

    //generate client broadcast addr
    bzero(&cli_addr, sizeof(cli_addr));
    cli_addr.sin_family = AF_INET;
    cli_addr.sin_port = htons(CLIENT_PORT);
    inet_pton(AF_INET, BROADCAST_IP, &cli_addr.sin_addr.s_addr);

    //read write
    while(1)
    {
        addr_len = sizeof(cli_addr);

        //sendto
        sendto(sfd, buf, strlen(buf), 0, (struct sockaddr*)&cli_addr, addr_len);
        printf("sendto broadcast...\n");
        sleep(1);
    }

    Close(sfd);

    return 0;
}
```
    - client.c:
```
#include <stdio.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <string.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include "wrap.h"

#define SERV_PORT 8000
#define CLIENT_PORT 9000

int main(int argc, char** argv)
{
    
    struct sockaddr_in cli_addr, serv_addr;
    int cfd, len;
    char buf[4096];

    //init client socket
    bzero(&cli_addr, sizeof(cli_addr));

    cli_addr.sin_family = AF_INET;
    cli_addr.sin_port = htons(CLIENT_PORT);
    inet_pton(AF_INET, "0.0.0.0", &cli_addr.sin_addr.s_addr);

    //socket UDP
    cfd = Socket(AF_INET, SOCK_DGRAM, 0);

    //bind
    Bind(cfd, (struct sockaddr*)&cli_addr, sizeof(cli_addr));   

    while(1)
    {
        //recv
        socklen_t servlen;
        servlen = sizeof(servlen);
        len = recvfrom(cfd, buf, sizeof(buf), 0, (struct sockaddr*)&serv_addr, &servlen);
        buf[len] = '\0';
        printf("recv: %s\n", buf);
    }

    Close(cfd);

    return 0;
}
```
* udp组播:
    - server.c:
        + `struct ip_mreqn group;`
        + 查找struct定义文件的具体行数: 
            * `grep -R -n "struct ip_mreqn {" /usr/include/*`
        + steps: sfd(bind), group(multiaddr/address/if), setsockopt, cli_addr(GROUP), sendto
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <fcntl.h>
#include <string.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include "wrap.h"
#include <sys/types.h>
#include <netinet/tcp.h>
#include <net/if.h>

#define SERV_PORT 8000
#define CLIENT_PORT 9000
#define SERV_IP "192.168.30.190"
#define GROUP "239.0.0.8"

int main(int argc, char *argv[])
{
    int sfd, cfd;
    struct sockaddr_in serv_addr, cli_addr;
    socklen_t addr_len;
    char buf[4096] = "Bryan UDP Group\n";
    struct ip_mreqn group;
    

    //socket UDP
    sfd = Socket(AF_INET, SOCK_DGRAM, 0);

    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    serv_addr.sin_addr.s_addr = inet_addr(SERV_IP);

    //bind
    Bind(sfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));

    //group
    inet_pton(AF_INET, GROUP, &group.imr_multiaddr);
    inet_pton(AF_INET, SERV_IP, &group.imr_address);
    group.imr_ifindex = if_nametoindex("ens33");

    //setsockopt
    setsockopt(sfd,IPPROTO_IP, IP_MULTICAST_IF, &group, sizeof(group));
    
    //client(GROUP)
    bzero(&cli_addr, sizeof(cli_addr));
    cli_addr.sin_family = AF_INET;
    cli_addr.sin_port = htons(CLIENT_PORT);
    inet_pton(AF_INET, GROUP, &cli_addr.sin_addr.s_addr);
    
    printf("wait for connect...\n");

    while(1)
    {
        //sendto
        addr_len = sizeof(cli_addr);
        sendto(sfd, buf, strlen(buf), 0, (struct sockaddr*)&cli_addr, addr_len);
        sleep(1);
    }

    Close(sfd);
    Close(cfd);

    return 0;
}
```
    - client.c:
        + steps: cfd(bind), group(multiaddr/address/if), setsockopt, recvfrom
```
#include <stdio.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <string.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <net/if.h>
#include "wrap.h"

#define CLI_PORT 9000
#define GROUP "239.0.0.8"

int main(int argc, char** argv)
{
    
    struct sockaddr_in cli_addr;
    int cfd, len;
    char buf[4096];
    struct ip_mreqn group;

    //bind cfd
    bzero(&cli_addr, sizeof(cli_addr));
    cli_addr.sin_family = AF_INET;
    cli_addr.sin_port = htons(CLI_PORT);
    inet_pton(AF_INET, "0.0.0.0", &cli_addr.sin_addr.s_addr);
    cfd = Socket(AF_INET, SOCK_DGRAM, 0);
    Bind(cfd, (struct sockaddr*)&cli_addr, sizeof(cli_addr));

    //group
    inet_pton(AF_INET, GROUP, &group.imr_multiaddr);
    inet_pton(AF_INET, "0.0.0.0", &group.imr_address);
    group.imr_ifindex = if_nametoindex("ens33");
    
    //setsockopt(add cli to group)
    setsockopt(cfd, IPPROTO_IP, IP_ADD_MEMBERSHIP, &group, sizeof(group));

    while(1)
    {
        //recv
        len = recvfrom(cfd, buf, sizeof(buf), 0, NULL, 0);
        buf[len] = '\0';
        printf("recv: %s\n", buf);
    }

    Close(cfd);

    return 0;
}
```

<a id="13-得到结构体成员偏移位置offsetof函数"></a>
#### 13. 得到结构体成员偏移位置offsetof函数
* `size = offsetof(struct sockaddr_un, sun_path) + strlen(un.sun_path);`
* `#define offsetof(TYPE, MEMBER) ((int)&((TYPE *)0)->MEMBER)`
* `(int)&((struct sockaddr_un *)0->sun_path)`: 将0强转为struct指针 -> 偏移sun_path -> &取地址 -> (int)强转 得到偏移量

<a id="14-epoll调用epoll_wait的两种触发方式ltet-pipe"></a>
#### 14. epoll调用epoll_wait的两种触发方式LT/ET-pipe
* LT(Level Triggered) 水平触发: 有就绪就返回(默认触发方式)
* ET(Edge Triggered) 边缘触发: 有状态改变再返回(效率高, 代码复杂度高)
* epoll_pipe_ET.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <sys/epoll.h>
#include <unistd.h>
#include <errno.h>

#define MAXLINE 10

int main(void)
{
    int efd, i;
    int pfd[2];
    pipe(pfd);  //pipe两端: pfd[0]读 / pfd[1]写
    pid_t pid;
    char buf[MAXLINE];
    char ch = 'a';

    pid = fork();
    if(pid == 0)
    {
        //write pfd[1]
        close(pfd[0]);
        while(1)
        {
            for(i=0;i<MAXLINE/2;++i)
            {
                buf[i] = ch;
            }
            buf[i-1] = '\n';
            ch++;

            for(;i<MAXLINE;++i)
            {
                buf[i] = ch;
            }
            buf[i-1] = '\n';
            ch++;
            
            write(pfd[1], buf, sizeof(buf));

            sleep(5);
        }
        close(pfd[1]);

    }else if(pid > 0)
    {
        //read pfd[0]
        close(pfd[1]);
        struct epoll_event event;
        struct epoll_event resevent[10];
        int res, len;

        efd = epoll_create(10);

        //event.events = EPOLLIN;
        event.events = EPOLLIN | EPOLLET;
        event.data.fd = pfd[0];

        epoll_ctl(efd, EPOLL_CTL_ADD, pfd[0], &event);

        while(1)
        {
            res = epoll_wait(efd, resevent, 10, -1);    // -1 永久阻塞
            printf("res: %d\n", res);
            if(resevent[0].data.fd == pfd[0])
            {
                len = read(pfd[0], buf, MAXLINE / 2);
                write(STDOUT_FILENO, buf, len);
            }

        }

        close(efd);
        close(pfd[0]);
    }else
    {
        perror("fork err");
        exit(1);
    }

    return 0;
}
```

<a id="15-epoll调用epoll_wait的两种触发方式ltet-socket-block"></a>
#### 15. epoll调用epoll_wait的两种触发方式LT/ET-Socket-Block
* server端epoll_wait读多行的情况下, 即使是ET触发方式, server也会阻塞在read上, 因为cfd是阻塞模式, 此时server无法epoll_wait响应其他客户端请求
* server.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <sys/time.h>
#include <sys/epoll.h>
#include <arpa/inet.h>
#include <netinet/in.h>
#include "wrap.h"

#define SERV_PORT 8000
#define MAXLINE 10


int main(int argc, char *argv[])
{
    int sfd, cfd;
    struct sockaddr_in serv_addr, cli_addr;
    socklen_t addr_len;
    char buf[MAXLINE], cli_ip[128];
    ssize_t len, efd, res;
    struct epoll_event event, resevent[10];
    

    //socket
    sfd = Socket(AF_INET, SOCK_STREAM, 0);
    
    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    serv_addr.sin_addr.s_addr = htonl(INADDR_ANY);
    
    //bind
    Bind(sfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));

    //listen
    Listen(sfd, 128);

    //accept
    addr_len = sizeof(cli_addr);
    cfd = Accept(sfd, (struct sockaddr*)&cli_addr, &addr_len);
    printf("recieved from %s : %d\n",
            inet_ntop(AF_INET, &cli_addr.sin_addr.s_addr, cli_ip, addr_len),
            ntohs(cli_addr.sin_port));

    //epoll create rb_tree
    efd = epoll_create(10);
    if(efd == -1)
    {
        perror("epoll create err");
        exit(1);
    }

    // cfd add to efd
    event.events = EPOLLIN | EPOLLET;
    //event.events = EPOLLIN;
    event.data.fd = cfd;
    epoll_ctl(efd, EPOLL_CTL_ADD, cfd, &event);


    while(1)
    {
        res = epoll_wait(efd, resevent, 10, -1);
        printf("epoll wait res: %d\n", (int)res);

        if(resevent[0].data.fd == cfd)
        {
            len = Read(cfd, buf, MAXLINE/2);
            Write(STDOUT_FILENO, buf, len);
            len = Read(cfd, buf, MAXLINE/2);
            Write(STDOUT_FILENO, buf, len);
            len = Read(cfd, buf, MAXLINE/2);
            Write(STDOUT_FILENO, buf, len);
            len = Read(cfd, buf, MAXLINE/2);
            Write(STDOUT_FILENO, buf, len);
            //len = Read(cfd, buf, sizeof(buf));
            //Write(STDOUT_FILENO, buf, len);
            //len = Read(cfd, buf, sizeof(buf));
            //Write(STDOUT_FILENO, buf, len);
            //len = Read(cfd, buf, sizeof(buf));
            //Write(STDOUT_FILENO, buf, len);
        }
    }
    
    Close(sfd);
    Close(cfd);

    return 0;
}
```
* client.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <string.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <netinet/in.h>
#include "wrap.h"

#define SERV_PORT 8000
#define SERV_IP "192.168.30.190"
#define MAXLINE 10

int main(int argc, char** argv)
{
    
    struct sockaddr_in serv_addr;
    socklen_t addr_len;
    int sfd, i;
    char buf[MAXLINE], ch = 'a';

    //sockaddr_in
    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    inet_pton(AF_INET, SERV_IP, &serv_addr.sin_addr.s_addr);

    //socket
    sfd = socket(AF_INET, SOCK_STREAM, 0);

    //connect
    addr_len = sizeof(serv_addr);
    connect(sfd, (struct sockaddr*)&serv_addr, addr_len);
    
    //write read
    while(1)
    {
        for(i=0;i<MAXLINE / 2;++i)
        {
            buf[i] = ch;
        }
        buf[i-1] = '\n';
        ch++;
        for(;i<MAXLINE;++i)
        {
            buf[i] = ch;
        }
        buf[i-1] = '\n';
        ch++;
        Write(sfd, buf, sizeof(buf));
        sleep(10);
    }

    close(sfd);

    return 0;
}
```

<a id="16-epoll调用epoll_wait的两种触发方式ltet-socket-nonblock"></a>
#### 16. epoll调用epoll_wait的两种触发方式LT/ET-Socket-NonBlock
* 此处是教学代码, 无法响应多client, 不重要
* server端将cfd改为非阻塞模式NonBlock配合ET触发方式提高server并发处理能力
* 此模式不适合socket长连接持续读写数据
* server.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <sys/time.h>
#include <sys/epoll.h>
#include <arpa/inet.h>
#include <netinet/in.h>
#include "wrap.h"
#include <fcntl.h>

#define SERV_PORT 8000
#define MAXLINE 10


int main(int argc, char *argv[])
{
    int sfd, cfd, flag;
    struct sockaddr_in serv_addr, cli_addr;
    socklen_t addr_len;
    char buf[MAXLINE], cli_ip[128];
    ssize_t len, efd, res;
    struct epoll_event event, resevent[10];
    

    //socket
    sfd = Socket(AF_INET, SOCK_STREAM, 0);
    
    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    serv_addr.sin_addr.s_addr = htonl(INADDR_ANY);
    
    //bind
    Bind(sfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));

    //listen
    Listen(sfd, 128);

    //accept
    addr_len = sizeof(cli_addr);
    cfd = Accept(sfd, (struct sockaddr*)&cli_addr, &addr_len);
    printf("recieved from %s : %d\n",
            inet_ntop(AF_INET, &cli_addr.sin_addr.s_addr, cli_ip, addr_len),
            ntohs(cli_addr.sin_port));

    //set cfd nonblock
    flag = fcntl(cfd, F_GETFL);
    flag |= O_NONBLOCK;
    fcntl(cfd, F_SETFL, flag);

    //epoll create rb_tree
    efd = epoll_create(10);
    if(efd == -1)
    {
        perror("epoll create err");
        exit(1);
    }

    // cfd add to efd
    event.events = EPOLLIN | EPOLLET;
    //event.events = EPOLLIN;
    event.data.fd = cfd;
    epoll_ctl(efd, EPOLL_CTL_ADD, cfd, &event);


    while(1)
    {
        res = epoll_wait(efd, resevent, 10, -1);
        printf("epoll wait res: %d\n", (int)res);

        if(resevent[0].data.fd == cfd)
        {
            while((len = Read(cfd, buf, MAXLINE / 2)) > 0)
            {
                Write(STDOUT_FILENO, buf, len);
            }
        }
    }

    Close(sfd);
    Close(cfd);

    return 0;
}
```

<a id="17-epoll非阻塞io事件驱动服务器reactor模型-重要"></a>
#### 17. epoll非阻塞io事件驱动服务器Reactor模型-重要
* 功能: server并发接收client数据, 根据事件(EPOLLIN/EPOLLOUT)处理业务, 经过业务处理后发送回client
* 实现思想: 
    - 文件描述符变成非阻塞
    - 为每个被监控的文件描述符(socket)注册回调callback函数, 利用回调函数callback, 实现多态(acceptconn/recvdata/senddata)
* 数据结构: struct epoll_event events[] 数组, events[i].data.ptr 指针指向自定义 struct myevent_s, 其中注册了callback回调函数, 为了在调用时实现多态
* 事件驱动特点: 可以在读/写之前先针对滑动窗口进行就绪判断
    - 缓存未满: 写就绪 EPOLLIN
    - 缓存非空: 读就绪 EPOLLOUT
* dev_steps: 
    - 设计数据结构: epoll_event, struct myevent_s{callback ...}
    - 程序运行流程
    - API(功能,函数名,参数,返回值): initlistensocket, event_set, event_add, acceptconn, recvdata, event_del, senddata
    - 实现main函数
    - 逐个实现API, 逐个测试
    - 整体测试
* 如果需要将处理完的数据转发到不同server(808->WAS->SaveCenter), 那么处理数据的server(WAS)同时是接收数据server(SaveCenter)的client端
* epoll_loop/server.c:
```
// epoll基于非阻塞I/O事件驱动(反应堆)
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>
#include <sys/socket.h>
#include <sys/epoll.h>
#include <sys/types.h>
#include <arpa/inet.h>
#include <errno.h>
#include <time.h>
#include <ctype.h>
#include "wrap.h"

#define MAX_EVENTS 1024
#define SERV_PORT 8000
#define BUF_LEN 128


struct myevent_s {
    int fd;
    int events;
    void *arg;  //指向这个结构体自己的地址
    void (*callback)(int fd, int events, void *arg);
    int status; //fd是否在rb_tree中被监控 0:不在  1:在
    char buf[BUF_LEN];
    int len;    //实际读到的长度
    long last_active;   //计时使用
};

int g_efd;
struct myevent_s g_events[MAX_EVENTS+1];

//封装自定义struct myevent_s, 注册call_back函数
void event_set(struct myevent_s *ev, int fd, void (*callback)(int, int, void*), void *arg)
{
    ev->fd = fd;
    ev->events = 0;
    ev->arg = arg;
    ev->callback = callback;
    ev->status = 0;
    ev->last_active = time(NULL);

    return;
}

//将eventset好的myevent_s赋值给epoll_event.data.ptr, 然后epoll_ctrl到g_efd红黑树中
void event_add(int efd, int events, struct myevent_s *ev)
{
    //将myevent_s转换成epoll_event后再add到epoll红黑树中
    struct epoll_event epv = {0, {0}};
    epv.data.ptr = ev;
    epv.events = ev->events = events;
    int op;

    if(ev->status == 1)
    {
        op = EPOLL_CTL_MOD;
    }else
    {
        op = EPOLL_CTL_ADD;
        ev->status = 1;
    }

    if (epoll_ctl(efd, op, ev->fd, &epv) < 0)
        printf("event add failed [fd=%d], events[%d]\n", ev->fd, events);
    else
        printf("event add OK [fd=%d], op=%d, events[%0X]\n", ev->fd, op, events);

    return;
}

void recvdata(int fd, int events, void *arg);
void senddata(int fd, int events, void *arg);

//listenfd调用callback
void acceptconn(int lfd, int events, void *arg)
{
    struct sockaddr_in cin;
    socklen_t len = sizeof(cin);
    int cfd, i;

    if((cfd = accept(lfd, (struct sockaddr*)&cin, &len)) == -1)
    {
        if(errno != EAGAIN && errno != EINTR)
        {

        }
        printf("%s accept %s\n", __func__, strerror(errno));
        return;
    }
    do{
        //找到g_events中未使用的位置i
        for(i=0;i<MAX_EVENTS;++i)
        {
            if(g_events[i].status == 0)
                break;
        }
        if(i == MAX_EVENTS)
        {
            printf("%s: max connet limit[%d]\n", __func__, MAX_EVENTS);
            break;
        }
        int flag = 0;
        if((flag = fcntl(cfd, F_SETFL, O_NONBLOCK)) < 0)
        {
            printf("%s: fcntl nonblocking failed, %s\n", __func__, strerror(errno));
            break;
        }
        event_set(&g_events[i], cfd, recvdata, &g_events[i]);
        event_add(g_efd, EPOLLIN, &g_events[i]);
    }while(0);
    printf("new connect [%s:%d][time:%ld], pos[%d]\n", inet_ntoa(cin.sin_addr), ntohs(cin.sin_port), g_events[i].last_active, i);
    return;
}

void eventdel(int efd, struct myevent_s *ev)
{
    struct epoll_event epv = {0, {0}};
    if(ev->status != 1)
        return;
    epv.data.ptr = ev;
    epoll_ctl(efd, EPOLL_CTL_DEL, ev->fd, &epv);
    return;
}

// cfd callback
void recvdata(int fd, int events, void *arg)
{
    struct myevent_s *ev = (struct myevent_s*)arg;  // 临时指针接收参数
    int len;

    len = recv(fd, ev->buf, sizeof(ev->buf), 0);
    eventdel(g_efd, ev);

    if(len > 0)
    {
        ev->len = len;
        ev->buf[len] = '\0';    //buf: 可以填充未来业务相关的struct(如: struct QQ_INFO ...)
        printf("C[%d]: %s\n", fd, ev->buf); // 模拟处理业务
        int j;
        for(j=0;j<len;++j)
        {
            ev->buf[j] = toupper(ev->buf[j]);
        }
        ev->buf[len] = '\0';

        event_set(ev, fd, senddata, ev);    // fd可以根据需要换成其他socket(SaveCenter)
        event_add(g_efd, EPOLLOUT, ev);
    }else if(len == 0)
    {
        close(ev->fd);
        printf("[fd=%d] pos[%ld], closed\n", fd, ev - g_events);
    }else
    {
        close(ev->fd);
        printf("recv[fd=%d] error[%d]:%s\n", fd, errno, strerror(errno));
    }
    return;
}

// cfd callback
void senddata(int fd, int events, void *arg)
{
    struct myevent_s *ev = (struct myevent_s*)arg;
    int len;
    
    len = send(fd, ev->buf, ev->len, 0);    //send: 往另一个socket写message
    if(len > 0)
    {
        printf("send[fd=%d], [%d]%s\n", fd, len, ev->buf);
        eventdel(g_efd, ev);
        event_set(ev, fd, recvdata, ev);
        event_add(g_efd, EPOLLIN, ev);
    }else
    {
        close(ev->fd);
        eventdel(g_efd, ev);
        printf("send[fd=%d] error %s\n", fd, strerror(errno));
    }
    return;
}

void initlistensocket(int efd, short port)
{
    struct sockaddr_in serv_addr;
    int lfd = Socket(AF_INET, SOCK_STREAM, 0);
    fcntl(lfd, F_SETFL, O_NONBLOCK);

    bzero(&serv_addr, sizeof(serv_addr));
    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(port);
    serv_addr.sin_addr.s_addr = INADDR_ANY;

    Bind(lfd, (struct sockaddr*)&serv_addr, sizeof(serv_addr));

    Listen(lfd, 128);

    event_set(&g_events[MAX_EVENTS], lfd, acceptconn, &g_events[MAX_EVENTS]);   // 将listenfd放在rb_tree的最后一个位置, 并设置回调acceptconn
    event_add(efd, EPOLLIN, &g_events[MAX_EVENTS]);
}

int main(int argc, char* argv[])
{
    unsigned short port = SERV_PORT;
    if(argc == 2)
        port = atoi(argv[1]);

    g_efd = epoll_create(MAX_EVENTS+1);

    initlistensocket(g_efd, port);

    struct epoll_event events[MAX_EVENTS+1];

    int i, checkpos = 0;

    while(1)
    {
        /* 超时验证，每次测试100个链接，不测试listenfd 当客户端60秒内没有和服务器通信，则关闭此客户端链接 */
        long now = time(NULL);
        //每次epoll_wait返回后检测100个event, 第一次0~100, 第二次101~200 ...
        for(i=0;i<100;++i, ++checkpos)
        {
            if(checkpos == MAX_EVENTS)
                checkpos = 0;
            if(g_events[checkpos].status != 1)
                continue;
            long duration = now - g_events[checkpos].last_active;
            if(duration >= 60)
            {
                close(g_events[checkpos].fd);
                printf("[fd=%d] timeout\n", g_events[checkpos].fd);
                eventdel(g_efd, &g_events[checkpos]);
            }
        }

        int nfd = epoll_wait(g_efd, events, MAX_EVENTS+1, 1000);    //timeout: 1000 ms (NonBlock)
        if(nfd < 0)
        {
            printf("epoll wait err\n");
            break;
        }

        for(i=0;i<nfd;++i)
        {
            //ev指向ptr, 准备调用callback函数
            struct myevent_s *ev = (struct myevent_s *)events[i].data.ptr;
            if((events[i].events & EPOLLIN) && (ev->events & EPOLLIN) )
            {
                ev->callback(ev->fd, events[i].events, ev->arg);    //多态
                //...其他处理
            }
            if((events[i].events & EPOLLOUT) && (ev->events & EPOLLOUT))
            {
                ev->callback(ev->fd, events[i].events, ev->arg);    //多态
                //...其他处理
            }
        }
    }

    /* 退出前释放所有资源 */
    return 0;
}
```
* client.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <ctype.h>
#include <sys/types.h>
#include <string.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <netinet/in.h>
#include "wrap.h"

#define SERV_PORT 8000
#define SERV_IP "192.168.30.190"
#define MAXLINE 10

int main(int argc, char** argv)
{
    
    struct sockaddr_in serv_addr;
    socklen_t addr_len;
    int sfd, i;
    char buf[MAXLINE], ch = 'a';
    char buf_r[MAXLINE];
    ssize_t len;

    //sockaddr_in
    bzero(&serv_addr, sizeof(serv_addr));

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    inet_pton(AF_INET, SERV_IP, &serv_addr.sin_addr.s_addr);

    //socket
    sfd = socket(AF_INET, SOCK_STREAM, 0);

    //connect
    addr_len = sizeof(serv_addr);
    connect(sfd, (struct sockaddr*)&serv_addr, addr_len);
    
    //write 
    while(1)
    {
        for(i=0;i<MAXLINE / 2;++i)
        {
            buf[i] = ch;
        }
        buf[i-1] = '\n';
        ch++;
        for(;i<MAXLINE;++i)
        {
            buf[i] = ch;
        }
        buf[i-1] = '\n';
        ch++;
        Write(sfd, buf, sizeof(buf));
        
        len = Read(sfd, buf_r, sizeof(buf_r));
        buf_r[len] = '\0';
        printf("read: %s\n", buf_r);

        sleep(10);
    }

    close(sfd);

    return 0;
}
```

<a id="18-线程池模型"></a>
#### 18. 线程池模型
* 基于生产者消费者模型
* 共享资源: 任务队列(互斥锁)
    - 环形队列
* 线程池中的工作线程阻塞在条件变量(任务队列不为空)上, 处理完业务后继续阻塞在条件变量上

<a id="19-c语言项目设计开发流程"></a>
#### 19. C语言项目设计开发流程
* ![](image\C语言项目设计流程.png)

<a id="20-shell相关"></a>
#### 20. Shell相关
* 查看内建命令: `man bash-builtins`
    - `bash  defines  the  following  built-in commands: :, ., [, alias, bg, bind, break, builtin, case, cd, command, compgen, complete, continue, declare, dirs, disown, echo, enable, eval, exec, exit, export, fc, fg, getopts, hash, help, history, if, jobs, kill,
       let, local, logout, popd, printf, pushd, pwd, read, readonly, return, set, shift, shopt, source, suspend, test, times, trap, type, typeset, ulimit, umask, unalias, unset, until, wait, while.`
    - ls不是内建命令
* shell变量赋值=号前后不能加空格
* 创建dir脚本:
```
#!/bin/bash

is_dir()
{
    DIR_NAME=$1
    
    if [ ! -d $DIR_NAME ]
    then 
        return 1
    else
        return 0
    fi
}

for DIR in "$@"
do
    if is_dir "$DIR"
    then :
    else
        echo "$DIR is not exist! Creating now..."
        mkdir $DIR > /dev/null 2>&1
        if [ $? -ne 0 ]; then
            echo "Cannot create directory $DIR"
            exit 1
        fi
    fi
done
```
* sed以行为处理单位
* awk能以行为处理单位, 也能以列为处理单位

<a id="21-c语言中使用正则表达式"></a>
#### 21. C语言中使用正则表达式
* regcomp()
* regexec()
* regfree()

<a id="22-c程序中打日志"></a>
#### 22. C程序中打日志
* 开启 syslog / rsyslog 守护进程
* `#include <syslog.h>`
    - openlog()
    - syslog()
    - closelog()