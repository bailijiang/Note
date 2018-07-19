<!-- MarkdownTOC -->

- [1. Makefile相关](#1-makefile相关)
- [2. 大端/小端存储方式验证](#2-大端小端存储方式验证)
- [3. server/client Socket最简实现](#3-serverclient-socket最简实现)
- [4. TCP建立和断开连接](#4-tcp建立和断开连接)
- [5. goto语句相关](#5-goto语句相关)
- [6. 多进程并发服务器实现](#6-多进程并发服务器实现)
- [7. CLOSE_WAIT问题](#7-close_wait问题)
- [8. 多线程并发服务器实现](#8-多线程并发服务器实现)

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

<a id="5-goto语句相关"></a>
#### 5. goto语句相关
* goto语句使用范围: a:当前函数 b:当前文件 c:当前项目  
    -  选a

<a id="6-多进程并发服务器实现"></a>
#### 6. 多进程并发服务器实现
* warp.c warp.h: 错误处理
    - 封装为首字母大写的函数,仍然可以在vim中用2+K查询man page 

<a id="7-close_wait问题"></a>
#### 7. CLOSE_WAIT问题
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

<a id="8-多线程并发服务器实现"></a>
#### 8. 多线程并发服务器实现
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


