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
- [11. mutiio-epoll1最简](#11-mutiio-epoll1最简)
- [12. UDP服务器实现](#12-udp服务器实现)

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
* ![](image\TCP状态转换图.PNG)

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
    - 多进程/多线程: I/O密集型(耗时长)
    - select/epoll: CPU密集型(计算,耗时短)
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

        //每次调用select会轮询阻塞rset, 当有event发生时, 会将rset相应的位置1(修改rset), 并返回连接就绪个数
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

<a id="11-mutiio-epoll1最简"></a>
#### 11. mutiio-epoll1最简
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

* udp组播: