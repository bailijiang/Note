<!-- MarkdownTOC -->

- [1. Makefile相关](#1-makefile相关)
- [2. 大端/小端存储方式验证](#2-大端小端存储方式验证)
- [3. server/client Socket最简实现](#3-serverclient-socket最简实现)

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
* 网络字节序是大端存储Big_endian
* Linux(Ubuntu16)系统是小端存储Little_endian
```
#include <stdio.h>

int checkCPU()
{
    union
    {
        int a;
        char b;
    }c;

    c.a = 1;
    return (c.b == 1);
}


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
    listen(sfd, 128);

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
