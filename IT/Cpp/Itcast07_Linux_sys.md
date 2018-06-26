#### 1. 系统调用
* 本质: 内核提供的函数
* 完成系统和应用程序之间的传递
* write 函数就是一个系统调用: libc -> printf -> write -> sys_write -> 内核驱动 -> 终端显示
    * ![](image\系统调用.PNG)

#### 2. open/close : man 2 open
* open 返回一个文件描述符
* vim 中看man: 2+K
```
#include <unistd.h>
#include <fcntl.h>
#include <stdio.h>
#include <errno.h>

int main(void)  
{
    //int fd = open("./dict.txt", O_RDONLY);    
    //int fd = open("./dict.cp", O_RDONLY | O_CREAT, 0644); 
    int fd = open("./dict.cp1", O_RDWR | O_TRUNC);  
    printf("fd: %d\n", fd);
    printf("errno: %d\n", errno);

    close(fd);  
    printf("close ok\n");

    return 0;
}
```
* 文件权限用8进制表示: 0644    0777
* open O_CREAT 创建的文件权限于当前系统下的 umask 有关: 0777 -> 0775  (mode & ~umask)
* O_TRUNC : 文件清空
* open 常见错误:
    - 打开文件不存在 errno: 2
    - 以写方式打开只读文件(打开文件没有对应权限)  errno: 13
    - 以只写方式打开目录  errno: 21

#### 3. Linux内存布局
![](image\内存布局图.PNG)

#### 4. read / write
* 返回的是实际读到/写到的大小
```
[root@centos180 file_IO_test]# cat read.c
#include <unistd.h>
#include <fcntl.h>
#include <stdio.h>
#include <errno.h>

int main(void)  
{   
    char buf[1024];
    int ret = 0;
    int fd = open("./dict.txt", O_RDWR);    
    printf("fd: %d\n", fd);
    printf("errno: %d\n", errno);

    while((ret = read(fd, buf, sizeof(buf))) != 0)
    {
        write(STDOUT_FILENO, buf, ret);
    }


    close(fd);  
    printf("close ok\n");

    return 0;
}
```

#### 5. cp命令实现(read/write)
```
[root@centos180 file_IO_test]# cat mycopy.c 
#include <unistd.h>
#include <fcntl.h>
#include <stdio.h>
#include <errno.h>

int main(int argc, char* argv[])    
{   
    char buf[1024];
    int ret = 0;
    
    char *src = argv[1];
    char *des = argv[2];

    
    int src_fd = open(src, O_RDONLY);
    int des_fd = open(des, O_WRONLY | O_CREAT, 0644);

    //printf("fd: %d\n", fd);
    printf("errno: %d\n", errno);

    while((ret = read(src_fd, buf, sizeof(buf))) != 0)
    {
        write(des_fd, buf, ret);
    }


    close(src_fd);
    close(des_fd);  
    printf("close ok\n");

    return 0;
}
```

#### 6. fgetc, fputc / read, write 区别
* C标准库fgetc, fputc 比 read, write 性能高
    - 原因: 预读入, 缓输出机制
    - read,write 从用户区到内核区切换非常耗时
    - fgetc, fputc 有自己的缓冲区 4096
    - ![](image\预读入缓输出机制.PNG)
* strace 跟踪程序系统调用: strace ./app
* 所有的系统调用都应该判断返回值 errno

#### 7. perror (man 3 perror)
* vim中查看man: 3+K
* !man 3 perror

#### 8. 阻塞和非阻塞
* 阻塞
```
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main(void)
{
    char buf[1024];
    int n = 0;

    n = read(STDIN_FILENO, buf, sizeof(buf));
    if(n<0)
    {
        perror("read err");
        exit(1);
    }
    write(STDOUT_FILENO, buf, n);

    return 0;
}
```
* 非阻塞
```
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <errno.h>
#include <string.h>
#include <unistd.h>

#define MSG_TRY "try again\n"

int main(void)
{
    char buf[1024];
    int fd, n;
    
    fd = open("/dev/tty", O_RDONLY | O_NONBLOCK);
    if(fd < 0)
    {
        perror("open tty error");
        exit(1);
    }
tryagain:
    n = read(fd, buf, 1024);
    if(n < 0)
    {
        if(errno != EAGAIN)
        {
            perror("read error");
            exit(1);
        }
        sleep(3);
        write(STDOUT_FILENO, MSG_TRY, sizeof(MSG_TRY));
        goto tryagain;
    }
    write(STDOUT_FILENO, buf, n);
    close(fd);

    return 0;
}
```
* timeout:
```
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <errno.h>
#include <string.h>
#include <unistd.h>

#define MSG_TRY "try again\n"
#define MSG_TIMEOUT "time out\n"

int main(void)
{
    char buf[1024];
    int fd, n, i;
    
    fd = open("/dev/tty", O_RDONLY | O_NONBLOCK);
    if(fd < 0)
    {
        perror("open tty error");
        exit(1);
    }
    printf("fd: %d\n", fd);

    for(i = 0; i < 5; ++i)
    {
        n = read(fd, buf, 1024);
        if(n < 0)
        {
            if(errno != EAGAIN)
            {
                perror("read error");
                exit(1);
            }
            sleep(2);
            write(STDOUT_FILENO, MSG_TRY, strlen(MSG_TRY));
        }
    }
    if(i == 5)
    {
        write(STDOUT_FILENO, MSG_TIMEOUT, strlen(MSG_TIMEOUT));
    }
    else
    {
        write(STDOUT_FILENO, buf, n);
    }
    close(fd);

    return 0;
}
```

#### 9. lseek
* 文件的读写指针只有一个
* write 完, 指针指向文件末尾
```
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <errno.h>
#include <string.h>
#include <unistd.h>


int main(void)
{
    char msg[] = "It's a test for lseek.\n";
    int fd, n;
    char ch;    

    fd = open("lseek.txt", O_RDWR | O_CREAT, 0644); 
    if(fd < 0)
    {
        perror("open error");
        exit(1);
    }

    write(fd, msg, strlen(msg));
    lseek(fd, 10, SEEK_SET);

    while((n = read(fd, &ch, 1)))
    {
        if(n < 0)
        {
            perror("read error");
            exit(1);
        }
        write(STDOUT_FILENO, &ch, 1);
    }
    close(fd);

    return 0;
}
```
* 文件拓展:
    - 必须发生文件IO后才有效
```
lseek(fd, 99, SEEK_SET);
write(fd, msg, strlen(msg));
```
* od : 按照编码格式显示无法显示的字符
```
[root@centos180 file_IO_test]# od -tcx lseek.txt 
0000000  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
               00000000        00000000        00000000        00000000
*
0000140  \0  \0  \0   I   t   '   s       a       t   e   s   t       f
               49000000        20732774        65742061        66207473
0000160   o   r       l   s   e   e   k   .  \n
               6c20726f        6b656573        00000a2e
0000172
```
* 获取文件大小
`int len = lseek(fd, 0, SEEK_END);`

#### 10. 位操作(bitmap位图)
* 本质是通过二进制位直接操作整数
* and与操作用于取出特定位(然后根据需要进行判定)----->位读 &
* or或操作用于指定位的无条件赋值              ----->位写 |
* `flags |= O_NONBLOCK; // 在flags中加入O_NONBLOCK属性`

#### 11. fcntl函数
* 动态添加非阻塞操作
```
    #include <unistd.h>
    #include <fcntl.h>
    #include <errno.h>
    #include <stdio.h>
    #include <stdlib.h>
    #include <string.h>

    #define MSG_TRY "try again\n"

    int main(void)
    {
        char buf[10];
        int flags, n;

        flags = fcntl(STDIN_FILENO, F_GETFL); //获取stdin属性信息
        if(flags == -1){
            perror("fcntl error");
            exit(1);
        }
        flags |= O_NONBLOCK; // 在flags中加入O_NONBLOCK属性
        int ret = fcntl(STDIN_FILENO, F_SETFL, flags);
        if(ret == -1){
            perror("fcntl error");
            exit(1);
        }

    tryagain:
        n = read(STDIN_FILENO, buf, 10);
        if(n < 0){
            if(errno != EAGAIN){        
                perror("read /dev/tty");
                exit(1);
            }
            sleep(3);
            write(STDOUT_FILENO, MSG_TRY, strlen(MSG_TRY));
            goto tryagain;
        }
        write(STDOUT_FILENO, buf, n);

        return 0;
    }
```

#### 12. 查找宏定义
* man
* 头文件 .h
* `grep -r "TIOCGWINSZ" /usr/lib/x86_64-redhat-linux6E/include/sys/ioctl.h`
* `https://elixir.bootlin.com/linux/latest/source`

#### 13. ioctl
* &size 传出参数
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/ioctl.h>

int main(void)
{
    struct winsize size;

    if (isatty(STDOUT_FILENO) == 0)     //
        exit(1);

    if(ioctl(STDOUT_FILENO, TIOCGWINSZ, &size)<0) {  // &size 传出参数
        perror("ioctl TIOCGWINSZ error");
        exit(1);
    }
    printf("%d rows, %d columns\n", size.ws_row, size.ws_col);

    return 0;
}
```

#### 14. strtok 拆分字符串
```
#include<stdio.h>
#include<string.h>
int main(void)
{
    char buf[]="hello@boy@this@is@heima";
    char*temp = strtok(buf,"@");
    while(temp)
    {
        printf("%s ",temp);
        temp = strtok(NULL,"@");
    }
    return0;
}
```

#### 15. ext2
* ![](image\ext2文件系统.PNG)
* 如何确定一个分区有多少个 BlockGroup: 
    - BlockBitmap 1k x 8 = 8k bit 个Block
    - 整个分区s个Block, 那么就有 s/8k 个BlockGoup

#### 16. stat
* 获取文件大小
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/stat.h>


int main(void)
{
    int ret = 0;
    struct stat sbuf;

    ret = stat("makefile", &sbuf);
    if(ret == -1)
    {
        perror("stat error");
        exit(1);
    }
    printf("len: %ld\n", sbuf.st_size);

    return 0;
}
```
* 获取文件类型
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/stat.h>


int main(int argc, char* argv[])
{
    int ret = 0;
    struct stat sbuf;

    ret = stat(argv[1], &sbuf);
    if(ret == -1)
    {
        perror("stat error");
        exit(1);
    }
    printf("len: %ld\n", sbuf.st_size);
    printf("mode: %d\n", (int)sbuf.st_mode);

    if(S_ISREG(sbuf.st_mode))
    {
        printf("It's a reguler file.\n");
    }
    else if(S_ISDIR(sbuf.st_mode))
    {
        printf("It's a dir.\n");
    }
    else if(S_ISLNK(sbuf.st_mode))
    {
        printf("It's a link file.\n");  
    }
    else if(S_ISFIFO(sbuf.st_mode))
    {
        printf("It's a pipe file.\n");
    }


    return 0;
}
```

#### 17. 管道文件
* 特点: 半双工, 阻塞
* 应用: 进程间通信, 一端写一端读(类似于golang: channel)
* 创建管道文件: mkfifo file_name.pipe
* 模拟进程间通信程序: a.c, b.c(你一句我一句)
* a.c:
```
#include<sys/types.h>
#include<sys/stat.h>
#include<fcntl.h>
#include<unistd.h>
#include<strings.h>
#include<stdio.h>
#include<string.h>
int main(int argc,char** argv)
{
        int fdw=open(argv[1],O_WRONLY);
        int fdr=open(argv[2],O_RDONLY);
        printf("fdr=%d fdw=%d\n",fdr,fdw);
        char buf[128]="";
        while(1) //如果管道写端关闭，读端如果read,会返回0
        {
                bzero(buf,sizeof(buf));
                read(0,buf,sizeof(buf));
                write(fdw,buf,strlen(buf)-1);  //去掉标准输入最后读到的回车
                bzero(buf,sizeof(buf));
                read(fdr,buf,sizeof(buf));
                printf("b:%s\n",buf);                      
        }
        return 0;
}
```
* b.c:
```
#include<sys/types.h>
#include<sys/stat.h>
#include<fcntl.h>
#include<unistd.h>
#include<strings.h>
#include<stdio.h>
#include<string.h>
int main(int argc,char** argv)
{
        int fdr=open(argv[1],O_RDONLY);
        int fdw=open(argv[2],O_WRONLY);
        printf("fdr=%d fdw=%d\n",fdr,fdw);
        char buf[128]="";
        while(1)
        {
                bzero(buf,sizeof(buf));
                read(fdr,buf,sizeof(buf));
                printf("a:%s\n",buf);
                bzero(buf,sizeof(buf));
                read(0,buf,sizeof(buf));
                write(fdw,buf,strlen(buf)-1);  //去掉标准输入最后读到的回车
        }
        return 0;
}
```

#### 18. 软链接
* 穿透(符号链接): vi, vim, stat, ...
* 不穿透: lstat, ls, readlink, ...

#### 19. inode/dentry
* inode: 
    - 保存文件属性: 权限, size, owner, xxxtime
* dentry: 
    - 保存文件名和inode号

#### 20. 数据块寻址
* inode表, 每个inode在ext2中是128字节 
* 一个inode对应一个文件, 一个文件对应多个Block
* 一个BlockGoup有多少个8K, 就有多少个inode
* 数据块寻址
* ![](image\数据块寻址.PNG)

* inode表中, 每个inode[]下标索引项占4字节: 每4个字节标识一个Block, 后3个是间接寻址
* 当一个Block块大小为1K时(b=1K=1024Bytes), 最多可表示$(b/4)^3+(b/4)^2+(b/4)+12$=16843020 个数据块, 即16843020K字节, 即一个inode对应的一个文件最大为 16.06GB

#### 21. sticky黏住位
* chmod 01777 ttt
* 设置sticky位的文件或目录会始终存在内存里

#### 22. 拓展文件 lseek / truncate
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>

int main(int argc, char** argv)
{
    int fd = open("./file.t", O_RDWR | O_CREAT, 0644);
    if(fd == -1)
    {
        perror("open error");
        exit(1);
    }
#if 0       
    lseek(fd, 99, SEEK_SET);
    write(fd, "s", 1);
#endif
    int ret = truncate("./file.t", 200);
    if(ret < 0)
    {
        perror("truncate error");
        exit(1);
    }
    
    close(fd);


    return 0;
}
```

#### 23. link
* 创建硬连接, 相当于 ln
* 硬连接创建的文件与原文件共享一个inode

#### 24. 
