<!-- MarkdownTOC -->

- [1. 系统调用](#1-%E7%B3%BB%E7%BB%9F%E8%B0%83%E7%94%A8)
- [2. open/close : man 2 open](#2-openclose--man-2-open)
- [3. Linux内存布局](#3-linux%E5%86%85%E5%AD%98%E5%B8%83%E5%B1%80)
- [4. read / write](#4-read--write)
- [5. cp命令实现\(read/write\)](#5-cp%E5%91%BD%E4%BB%A4%E5%AE%9E%E7%8E%B0readwrite)
- [6. fgetc, fputc / read, write 区别](#6-fgetc-fputc--read-write-%E5%8C%BA%E5%88%AB)
- [7. perror \(man 3 perror\)](#7-perror-man-3-perror)
- [8. 阻塞和非阻塞](#8-%E9%98%BB%E5%A1%9E%E5%92%8C%E9%9D%9E%E9%98%BB%E5%A1%9E)
- [9. lseek](#9-lseek)
- [10. 位操作\(bitmap位图\)](#10-%E4%BD%8D%E6%93%8D%E4%BD%9Cbitmap%E4%BD%8D%E5%9B%BE)
- [11. fcntl函数](#11-fcntl%E5%87%BD%E6%95%B0)
- [12. 查找宏定义](#12-%E6%9F%A5%E6%89%BE%E5%AE%8F%E5%AE%9A%E4%B9%89)
- [13. ioctl](#13-ioctl)
- [14. strtok 拆分字符串与可重入和不可重入函数](#14-strtok-%E6%8B%86%E5%88%86%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%8E%E5%8F%AF%E9%87%8D%E5%85%A5%E5%92%8C%E4%B8%8D%E5%8F%AF%E9%87%8D%E5%85%A5%E5%87%BD%E6%95%B0)
- [15. ext2](#15-ext2)
- [16. stat](#16-stat)
- [17. 管道文件](#17-%E7%AE%A1%E9%81%93%E6%96%87%E4%BB%B6)
- [18. 软链接](#18-%E8%BD%AF%E9%93%BE%E6%8E%A5)
- [19. inode/dentry](#19-inodedentry)
- [20. 数据块寻址](#20-%E6%95%B0%E6%8D%AE%E5%9D%97%E5%AF%BB%E5%9D%80)
- [21. sticky黏住位](#21-sticky%E9%BB%8F%E4%BD%8F%E4%BD%8D)
- [22. 拓展文件 lseek / truncate](#22-%E6%8B%93%E5%B1%95%E6%96%87%E4%BB%B6-lseek--truncate)
- [23. link](#23-link)
- [24. 程序运行时创建临时文件](#24-%E7%A8%8B%E5%BA%8F%E8%BF%90%E8%A1%8C%E6%97%B6%E5%88%9B%E5%BB%BA%E4%B8%B4%E6%97%B6%E6%96%87%E4%BB%B6)
- [25. 获取改变当前目录getcwd/chdir](#25-%E8%8E%B7%E5%8F%96%E6%94%B9%E5%8F%98%E5%BD%93%E5%89%8D%E7%9B%AE%E5%BD%95getcwdchdir)
- [26. 目录](#26-%E7%9B%AE%E5%BD%95)
- [27. 递归遍历目录中的文件](#27-%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86%E7%9B%AE%E5%BD%95%E4%B8%AD%E7%9A%84%E6%96%87%E4%BB%B6)
- [28. 回调函数](#28-%E5%9B%9E%E8%B0%83%E5%87%BD%E6%95%B0)
- [29. dup / dup2 重定向](#29-dup--dup2-%E9%87%8D%E5%AE%9A%E5%90%91)
- [30. while无{}时的分号;](#30-while%E6%97%A0%E6%97%B6%E7%9A%84%E5%88%86%E5%8F%B7)
- [31. 短路运算](#31-%E7%9F%AD%E8%B7%AF%E8%BF%90%E7%AE%97)
- [32. sizeof运算符](#32-sizeof%E8%BF%90%E7%AE%97%E7%AC%A6)
- [33. 内存中数据分布](#33-%E5%86%85%E5%AD%98%E4%B8%AD%E6%95%B0%E6%8D%AE%E5%88%86%E5%B8%83)
- [34. 段错误](#34-%E6%AE%B5%E9%94%99%E8%AF%AF)
- [35. 标准I/O提供的3种类型的缓冲](#35-%E6%A0%87%E5%87%86io%E6%8F%90%E4%BE%9B%E7%9A%843%E7%A7%8D%E7%B1%BB%E5%9E%8B%E7%9A%84%E7%BC%93%E5%86%B2)
- [36. find命令](#36-find%E5%91%BD%E4%BB%A4)
- [37. 求1~n之间的所有素数](#37-%E6%B1%821%7En%E4%B9%8B%E9%97%B4%E7%9A%84%E6%89%80%E6%9C%89%E7%B4%A0%E6%95%B0)
- [38. 获取进程环境变量](#38-%E8%8E%B7%E5%8F%96%E8%BF%9B%E7%A8%8B%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
- [39. CPU/MMU](#39-cpummu)
- [40. CentOS7 kernel源代码路径](#40-centos7-kernel%E6%BA%90%E4%BB%A3%E7%A0%81%E8%B7%AF%E5%BE%84)
- [41. 根据pid查找运行程序](#41-%E6%A0%B9%E6%8D%AEpid%E6%9F%A5%E6%89%BE%E8%BF%90%E8%A1%8C%E7%A8%8B%E5%BA%8F)
- [42. fork创建子进程并用循环因子i区分](#42-fork%E5%88%9B%E5%BB%BA%E5%AD%90%E8%BF%9B%E7%A8%8B%E5%B9%B6%E7%94%A8%E5%BE%AA%E7%8E%AF%E5%9B%A0%E5%AD%90i%E5%8C%BA%E5%88%86)
- [43. 多进程gdb调试](#43-%E5%A4%9A%E8%BF%9B%E7%A8%8Bgdb%E8%B0%83%E8%AF%95)
- [44. exec函数族](#44-exec%E5%87%BD%E6%95%B0%E6%97%8F)
- [45. 孤儿进程 / 僵尸进程](#45-%E5%AD%A4%E5%84%BF%E8%BF%9B%E7%A8%8B--%E5%83%B5%E5%B0%B8%E8%BF%9B%E7%A8%8B)
- [46. wait\(\)阻塞回收僵尸进程](#46-wait%E9%98%BB%E5%A1%9E%E5%9B%9E%E6%94%B6%E5%83%B5%E5%B0%B8%E8%BF%9B%E7%A8%8B)
- [47. 回收指定进程 waitpid\(\)](#47-%E5%9B%9E%E6%94%B6%E6%8C%87%E5%AE%9A%E8%BF%9B%E7%A8%8B-waitpid)
- [48. 父进程fork 3 个子进程，三个子进程一个调用ps命令，一个调用自定义程序1\(正常\), 一个调用自定义程序2\(会出段错误\)。父进程使用waitpid对其子进程全部进行回收, 并打印状态](#48-%E7%88%B6%E8%BF%9B%E7%A8%8Bfork-3-%E4%B8%AA%E5%AD%90%E8%BF%9B%E7%A8%8B%EF%BC%8C%E4%B8%89%E4%B8%AA%E5%AD%90%E8%BF%9B%E7%A8%8B%E4%B8%80%E4%B8%AA%E8%B0%83%E7%94%A8ps%E5%91%BD%E4%BB%A4%EF%BC%8C%E4%B8%80%E4%B8%AA%E8%B0%83%E7%94%A8%E8%87%AA%E5%AE%9A%E4%B9%89%E7%A8%8B%E5%BA%8F1%E6%AD%A3%E5%B8%B8-%E4%B8%80%E4%B8%AA%E8%B0%83%E7%94%A8%E8%87%AA%E5%AE%9A%E4%B9%89%E7%A8%8B%E5%BA%8F2%E4%BC%9A%E5%87%BA%E6%AE%B5%E9%94%99%E8%AF%AF%E3%80%82%E7%88%B6%E8%BF%9B%E7%A8%8B%E4%BD%BF%E7%94%A8waitpid%E5%AF%B9%E5%85%B6%E5%AD%90%E8%BF%9B%E7%A8%8B%E5%85%A8%E9%83%A8%E8%BF%9B%E8%A1%8C%E5%9B%9E%E6%94%B6-%E5%B9%B6%E6%89%93%E5%8D%B0%E7%8A%B6%E6%80%81)
- [49. 全双工/半双工/单工](#49-%E5%85%A8%E5%8F%8C%E5%B7%A5%E5%8D%8A%E5%8F%8C%E5%B7%A5%E5%8D%95%E5%B7%A5)
- [50. 通过pipe管道进行进程间通信](#50-%E9%80%9A%E8%BF%87pipe%E7%AE%A1%E9%81%93%E8%BF%9B%E8%A1%8C%E8%BF%9B%E7%A8%8B%E9%97%B4%E9%80%9A%E4%BF%A1)
- [51. pipe / fork / dup2 / execlp 练习1父子](#51-pipe--fork--dup2--execlp-%E7%BB%83%E4%B9%A01%E7%88%B6%E5%AD%90)
- [52. pipe / fork / dup2 / execlp 练习2 兄弟](#52-pipe--fork--dup2--execlp-%E7%BB%83%E4%B9%A02-%E5%85%84%E5%BC%9F)
- [53. pipe一读多写/一写多读](#53-pipe%E4%B8%80%E8%AF%BB%E5%A4%9A%E5%86%99%E4%B8%80%E5%86%99%E5%A4%9A%E8%AF%BB)
- [54. 统计当前系统中进程ID大于1000的进程个数](#54-%E7%BB%9F%E8%AE%A1%E5%BD%93%E5%89%8D%E7%B3%BB%E7%BB%9F%E4%B8%AD%E8%BF%9B%E7%A8%8Bid%E5%A4%A7%E4%BA%8E1000%E7%9A%84%E8%BF%9B%E7%A8%8B%E4%B8%AA%E6%95%B0)
- [55. pipe管道大小 4096 bytes](#55-pipe%E7%AE%A1%E9%81%93%E5%A4%A7%E5%B0%8F-4096-bytes)
- [56. 获取管道缓冲区大小fpathconf](#56-%E8%8E%B7%E5%8F%96%E7%AE%A1%E9%81%93%E7%BC%93%E5%86%B2%E5%8C%BA%E5%A4%A7%E5%B0%8Ffpathconf)
- [57. FIFO命名管道](#57-fifo%E5%91%BD%E5%90%8D%E7%AE%A1%E9%81%93)
- [58. fifo一写多读](#58-fifo%E4%B8%80%E5%86%99%E5%A4%9A%E8%AF%BB)
- [59. mmap](#59-mmap)
- [60. mmap非血缘IPC通信](#60-mmap%E9%9D%9E%E8%A1%80%E7%BC%98ipc%E9%80%9A%E4%BF%A1)
- [61. 软件项目设计实现流程](#61-%E8%BD%AF%E4%BB%B6%E9%A1%B9%E7%9B%AE%E8%AE%BE%E8%AE%A1%E5%AE%9E%E7%8E%B0%E6%B5%81%E7%A8%8B)
- [62. 多进程拷贝大文件](#62-%E5%A4%9A%E8%BF%9B%E7%A8%8B%E6%8B%B7%E8%B4%9D%E5%A4%A7%E6%96%87%E4%BB%B6)
- [63. 交互shell, myshell.c](#63-%E4%BA%A4%E4%BA%92shell-myshellc)
- [64. QQ_IPC](#64-qq_ipc)
- [65. 信号sig的概念](#65-%E4%BF%A1%E5%8F%B7sig%E7%9A%84%E6%A6%82%E5%BF%B5)
- [66. typedef 类型定义](#66-typedef-%E7%B1%BB%E5%9E%8B%E5%AE%9A%E4%B9%89)
- [67. sigaction函数](#67-sigaction%E5%87%BD%E6%95%B0)
- [68. pause函数](#68-pause%E5%87%BD%E6%95%B0)
- [69. 父进程利用sigaction信号捕捉处理多个僵尸进程](#69-%E7%88%B6%E8%BF%9B%E7%A8%8B%E5%88%A9%E7%94%A8sigaction%E4%BF%A1%E5%8F%B7%E6%8D%95%E6%8D%89%E5%A4%84%E7%90%86%E5%A4%9A%E4%B8%AA%E5%83%B5%E5%B0%B8%E8%BF%9B%E7%A8%8B)
- [70. sigsuspend解决时序竞态问题](#70-sigsuspend%E8%A7%A3%E5%86%B3%E6%97%B6%E5%BA%8F%E7%AB%9E%E6%80%81%E9%97%AE%E9%A2%98)
- [71. 父子进程交替数数](#71-%E7%88%B6%E5%AD%90%E8%BF%9B%E7%A8%8B%E4%BA%A4%E6%9B%BF%E6%95%B0%E6%95%B0)
- [72. 创建session会话/作业](#72-%E5%88%9B%E5%BB%BAsession%E4%BC%9A%E8%AF%9D%E4%BD%9C%E4%B8%9A)
- [73. 创建daemon守护进程](#73-%E5%88%9B%E5%BB%BAdaemon%E5%AE%88%E6%8A%A4%E8%BF%9B%E7%A8%8B)
- [74. 线程的概念](#74-%E7%BA%BF%E7%A8%8B%E7%9A%84%E6%A6%82%E5%BF%B5)
- [75. 创建线程](#75-%E5%88%9B%E5%BB%BA%E7%BA%BF%E7%A8%8B)
- [76. 退出单个线程pthread_exit\(NULL\)](#76-%E9%80%80%E5%87%BA%E5%8D%95%E4%B8%AA%E7%BA%BF%E7%A8%8Bpthread_exitnull)
- [77. pthread_join阻塞等待线程退出, 获取线程退出状态](#77-pthread_join%E9%98%BB%E5%A1%9E%E7%AD%89%E5%BE%85%E7%BA%BF%E7%A8%8B%E9%80%80%E5%87%BA-%E8%8E%B7%E5%8F%96%E7%BA%BF%E7%A8%8B%E9%80%80%E5%87%BA%E7%8A%B6%E6%80%81)
- [78. pthread_detach 分离单个线程](#78-pthread_detach-%E5%88%86%E7%A6%BB%E5%8D%95%E4%B8%AA%E7%BA%BF%E7%A8%8B)
- [79. pthread_cancel杀死一个线程](#79-pthread_cancel%E6%9D%80%E6%AD%BB%E4%B8%80%E4%B8%AA%E7%BA%BF%E7%A8%8B)
- [80. 设置线程属性pthread_attr](#80-%E8%AE%BE%E7%BD%AE%E7%BA%BF%E7%A8%8B%E5%B1%9E%E6%80%A7pthread_attr)
- [81. 线程同步的概念](#81-%E7%BA%BF%E7%A8%8B%E5%90%8C%E6%AD%A5%E7%9A%84%E6%A6%82%E5%BF%B5)
- [82. 多线程拷贝大文件, 并显示拷贝进度](#82-%E5%A4%9A%E7%BA%BF%E7%A8%8B%E6%8B%B7%E8%B4%9D%E5%A4%A7%E6%96%87%E4%BB%B6-%E5%B9%B6%E6%98%BE%E7%A4%BA%E6%8B%B7%E8%B4%9D%E8%BF%9B%E5%BA%A6)
- [83. 线程死锁](#83-%E7%BA%BF%E7%A8%8B%E6%AD%BB%E9%94%81)
- [84. 读写锁](#84-%E8%AF%BB%E5%86%99%E9%94%81)
- [85. 生产者消费者-pthread_cond](#85-%E7%94%9F%E4%BA%A7%E8%80%85%E6%B6%88%E8%B4%B9%E8%80%85-pthread_cond)
- [86. semaphore旗语\(信号量\)](#86-semaphore%E6%97%97%E8%AF%AD%E4%BF%A1%E5%8F%B7%E9%87%8F)
- [87. 进程间同步pthread_mutexattr_t](#87-%E8%BF%9B%E7%A8%8B%E9%97%B4%E5%90%8C%E6%AD%A5pthreadmutexattrt)
- [88. 进程间文件锁fcntl/flock](#88-%E8%BF%9B%E7%A8%8B%E9%97%B4%E6%96%87%E4%BB%B6%E9%94%81fcntlflock)
- [89. 哲学家吃饭问题](#89-%E5%93%B2%E5%AD%A6%E5%AE%B6%E5%90%83%E9%A5%AD%E9%97%AE%E9%A2%98)
- [90. sem_timedwait](#90-sem_timedwait)

<!-- /MarkdownTOC -->

<a id="1-%E7%B3%BB%E7%BB%9F%E8%B0%83%E7%94%A8"></a>
#### 1. 系统调用
* 本质: 内核提供的函数
* 完成系统和应用程序之间的传递
* write 函数就是一个系统调用: libc -> printf -> write -> sys_write -> 内核驱动 -> 终端显示
    * ![](image\系统调用.PNG)

<a id="2-openclose--man-2-open"></a>
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
* open O_CREAT 创建的文件权限与当前系统下的 umask 有关: 0777 -> 0775  (mode & ~umask)
* O_TRUNC : 文件清空
* open 常见错误:
    - 打开文件不存在 errno: 2
    - 以写方式打开只读文件(打开文件没有对应权限)  errno: 13
    - 以只写方式打开目录  errno: 21
    - 不加权限位: 
        + `open("tmp.file", O_CREAT | O_RDWR);`
        + `\\open("tmp.file", O_CREAT | O_RDWR, 0644);`
        + 编译时不报错, 不使用不报错
        + 使用了, 运行时才报错

* 每打开一个文件都有一个I/O缓冲区

<a id="3-linux%E5%86%85%E5%AD%98%E5%B8%83%E5%B1%80"></a>
#### 3. Linux内存布局
![](image\内存布局图.PNG)

<a id="4-read--write"></a>
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

<a id="5-cp%E5%91%BD%E4%BB%A4%E5%AE%9E%E7%8E%B0readwrite"></a>
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

<a id="6-fgetc-fputc--read-write-%E5%8C%BA%E5%88%AB"></a>
#### 6. fgetc, fputc / read, write 区别
* C标准库fgetc, fputc 比 read, write 性能高
    - 原因: 预读入, 缓输出机制
    - read,write 从用户区到内核区切换非常耗时
    - fgetc, fputc 有自己的缓冲区 4096
    - ![](image\预读入缓输出机制.PNG)
* strace 跟踪程序系统调用: strace ./app
* 所有的系统调用都应该判断返回值 errno

<a id="7-perror-man-3-perror"></a>
#### 7. perror (man 3 perror)
* vim中查看man: 3+K
* !man 3 perror

<a id="8-%E9%98%BB%E5%A1%9E%E5%92%8C%E9%9D%9E%E9%98%BB%E5%A1%9E"></a>
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

<a id="9-lseek"></a>
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

<a id="10-%E4%BD%8D%E6%93%8D%E4%BD%9Cbitmap%E4%BD%8D%E5%9B%BE"></a>
#### 10. 位操作(bitmap位图)
* 本质是通过二进制位直接操作整数
* and与操作用于取出特定位(然后根据需要进行判定)----->位读 &
* or或操作用于指定位的无条件赋值              ----->位写 |
* `flags |= O_NONBLOCK; // 在flags中加入O_NONBLOCK属性`

<a id="11-fcntl%E5%87%BD%E6%95%B0"></a>
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

<a id="12-%E6%9F%A5%E6%89%BE%E5%AE%8F%E5%AE%9A%E4%B9%89"></a>
#### 12. 查找宏定义
* man
* 头文件 .h
* `grep -r "TIOCGWINSZ" /usr/lib/x86_64-redhat-linux6E/include/sys/ioctl.h`
* `https://elixir.bootlin.com/linux/latest/source`

<a id="13-ioctl"></a>
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

<a id="14-strtok-%E6%8B%86%E5%88%86%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%8E%E5%8F%AF%E9%87%8D%E5%85%A5%E5%92%8C%E4%B8%8D%E5%8F%AF%E9%87%8D%E5%85%A5%E5%87%BD%E6%95%B0"></a>
#### 14. strtok 拆分字符串与可重入和不可重入函数
* 不可重入函数(执行期间不可中断, 无法正常返回): 使用了全局资源
* 多个线程或进程同时调用一个函数, 此函数会修改全局共享资源, 会导致数据不安全(不可重入/线程不安全), 反之则是可重入(线程安全)函数; 
* strtok_r 是可重入函数: 使用二级指针
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
    return 0;
}
```

<a id="15-ext2"></a>
#### 15. ext2
* ![](image\ext2文件系统.PNG)
* 如何确定一个分区有多少个 BlockGroup: 
    - BlockBitmap 1KB x 8 = 8k bit 个Block
    - 整个分区s个Block, 那么就有 s/8k 个BlockGroup

<a id="16-stat"></a>
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

<a id="17-%E7%AE%A1%E9%81%93%E6%96%87%E4%BB%B6"></a>
#### 17. 管道文件
* 特点: 半双工, 阻塞
* 应用: 进程间通信, 一端写一端读(类似于golang: channel)
* 创建管道文件: mkfifo file_name.pipe
* 管道大小: 64K
* 模拟进程间通信程序: a.c, b.c(你一句我一句)
* 双管道, a写b读, b写a读
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

<a id="18-%E8%BD%AF%E9%93%BE%E6%8E%A5"></a>
#### 18. 软链接
* 穿透(符号链接): vi, vim, stat, ...
* 不穿透: lstat, ls, readlink, ...

<a id="19-inodedentry"></a>
#### 19. inode/dentry
* inode: 
    - 保存文件属性: 权限, size, owner, xxxtime
* dentry: 
    - 保存文件名和inode号

<a id="20-%E6%95%B0%E6%8D%AE%E5%9D%97%E5%AF%BB%E5%9D%80"></a>
#### 20. 数据块寻址
* inode表, 每个inode在ext2中是128字节 
* 一个inode对应一个文件, 一个文件对应一个或多个Block
* 一个BlockGoup有多少个8K, 就有多少个inode
* 数据块寻址
* ![](image\数据块寻址.PNG)
* 索引项代表block的号

* inode表中, 每个inode中的Block[]下标索引项占4字节: 每4个字节标识一个Block, 后3个是间接寻址, 里面存放的是Block的编号(不是地址)
* 当一个Block块大小为1K时(b=1K=1024Bytes), 最多可表示$(b/4)^3+(b/4)^2+(b/4)+12$=16843020 个数据块, 即16843020K字节, 即一个inode对应的一个文件最大为 16.06GB

<a id="21-sticky%E9%BB%8F%E4%BD%8F%E4%BD%8D"></a>
#### 21. sticky黏住位
* chmod 01777 ttt
* 设置sticky位的文件或目录会始终存在内存里
* 对任意用户可读写的目录需要设置sticky位(chmod o+t dir), 否则其他用户能删除不是自己创建的文件

<a id="22-%E6%8B%93%E5%B1%95%E6%96%87%E4%BB%B6-lseek--truncate"></a>
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

<a id="23-link"></a>
#### 23. link
* 创建硬连接, 相当于 ln
* 硬连接创建的文件与原文件共享一个inode
* 硬连接不能创建目录

<a id="24-%E7%A8%8B%E5%BA%8F%E8%BF%90%E8%A1%8C%E6%97%B6%E5%88%9B%E5%BB%BA%E4%B8%B4%E6%97%B6%E6%96%87%E4%BB%B6"></a>
#### 24. 程序运行时创建临时文件
```
/*
 *unlink函数是删除一个dentry
 */
#include <unistd.h>
#include <fcntl.h>
#include <stdlib.h>
#include <string.h>
#include <stdio.h>


int main(void)
{
    int fd;
    char *p = "test of unlink\n";
    char *p2 = "after write something.\n";

    fd = open("temp.txt", O_RDWR|O_CREAT|O_TRUNC, 0644);
    if(fd < 0){
        perror("open temp error");
        exit(1);
    }
    int ret = unlink("temp.txt");        //具备了被释放的条件
    if(ret < 0){
        perror("unlink error");
        exit(1);
    }

    ret = write(fd, p, strlen(p));
    if (ret == -1) {
        perror("-----write error");
    }

    p[0] = 'H';

    printf("hi! I'm printf\n");
    ret = write(fd, p2, strlen(p2));
    if (ret == -1) {
        perror("-----write error");
    }


    printf("Enter anykey continue\n");
    getchar();

    close(fd);


    return 0;
}
```

<a id="25-%E8%8E%B7%E5%8F%96%E6%94%B9%E5%8F%98%E5%BD%93%E5%89%8D%E7%9B%AE%E5%BD%95getcwdchdir"></a>
#### 25. 获取改变当前目录getcwd/chdir
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(int argc, char** argv)
{   
    char buf[64];
    printf("pwd: %s\n", getcwd(buf, 64));

    chdir(argv[1]);

    printf("pwd: %s\n", getcwd(buf, 64));

    return 0;
}
```

<a id="26-%E7%9B%AE%E5%BD%95"></a>
#### 26. 目录
* 目录也是文件
* 目录文件内包括目录项
* 打开/关闭/读取目录项
```
#include <unistd.h>
#include <dirent.h>
#include <stdlib.h>
#include <stdio.h>

int main(int argc, char** argv)
{
    DIR *dp;
    struct dirent *sdp;

    dp = opendir(argv[1]);
    if(dp == NULL)
    {
        perror("opendir error");
        exit(1);
    }
    while((sdp = readdir(dp)) != NULL)
    {
        if(sdp->d_name[0] != '.')
        {
            printf("%s\n", sdp->d_name);
        }
    }

    
    closedir(dp);

    return 0;
}
```

<a id="27-%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86%E7%9B%AE%E5%BD%95%E4%B8%AD%E7%9A%84%E6%96%87%E4%BB%B6"></a>
#### 27. 递归遍历目录中的文件
* listAllFiles.c:
```
#include <stdio.h>
#include <unistd.h>
#include <dirent.h>
#include <stdlib.h>
#include <sys/stat.h>
#include <string.h>
#include <assert.h>

#define MAX_PATH_LEN 512

int count =0;
char dirPath[MAX_PATH_LEN];

void listAllFiles(char *dirname)
{
    assert(dirname != NULL);
    
    char path[512];
    struct dirent *filename;
    DIR *dir;
    dir = opendir(dirname);
    if(dir == NULL)
    {
        printf("open dir %s error!\n",dirname);
        exit(1);
    }
    
    while((filename = readdir(dir)) != NULL)
    {
        
        if(!strcmp(filename->d_name,".")||!strcmp(filename->d_name,".."))
            continue;
            
        sprintf(path,"%s/%s",dirname,filename->d_name);
        
        struct stat s;
        lstat(path,&s);
        
        if(S_ISDIR(s.st_mode))
        {
            listAllFiles(path);
        }
        else
        {
            printf("%d. %s\n",++count,filename->d_name);
        }
    }
    closedir(dir);
}


int main(int argc, char **argv)
{

    if(argc != 2)
    {
        printf("one dir required!(for eample: ./a.out /home/myFolder)\n");
        exit(1);
    }
    strcpy(dirPath,argv[1]);
    listAllFiles(dirPath);
    printf("total files:%d\n",count);
    return 0;
}
```

* ls_R.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/stat.h>
#include <dirent.h>

#define MAX_LENGTH 256


void fetchdir(const char *dirname, void (*fcn)(char *name))
{
    DIR *dir = opendir(dirname);
    struct dirent *filename;
    char path[MAX_LENGTH];

    if(dir == NULL)
    {
        perror("opendir error");
        return;
    }
    while((filename = readdir(dir)) != NULL)
    {
        if(!strcmp(filename->d_name, ".") || !strcmp(filename->d_name, ".."))
        {
            continue;
        }

        if(strlen(dirname)+strlen(filename->d_name)+2 > sizeof(path))
        {
            perror("name too long");
        }
        else
        {
            sprintf(path, "%s/%s", dirname, filename->d_name);
            (*fcn)(path);
        }
    }
    closedir(dir);
}


void isfile(char *name)
{
    struct stat sbuf;
    if(stat(name, &sbuf) == -1)
    {
        perror("file name error");
        exit(1);
    }
    if((sbuf.st_mode & S_IFMT) == S_IFDIR)
    {
        fetchdir(name, isfile);
    }
    printf("%s  %8ld\n", name, sbuf.st_size);

}

int main(int argc, char** argv)
{
    int i = 0;

    if(argc == 1)
    {
        isfile(".");
    }
        
    for(i = 1;i < argc; ++i)
    {
        isfile(argv[i]);
    }


    return 0;
}
```

<a id="28-%E5%9B%9E%E8%B0%83%E5%87%BD%E6%95%B0"></a>
#### 28. 回调函数
* 要确定谁是调用函数, 谁是回调函数
* 调用函数在参数中指定回调函数的类型
* 回调函数不是必须的, 但可增强灵活性
* 回调函数是面向对象中多态的理论基础
* 回调函数的本质是函数指针

<a id="29-dup--dup2-%E9%87%8D%E5%AE%9A%E5%90%91"></a>
#### 29. dup / dup2 重定向
* dup2(old_fd, new_fd) 
    - 是将old_fd 下标(指针), 拷贝到new_fd
    - new_fd 与 old_fd 指向同一个文件, 即old_fd所指向的文件
    - 相当于将new_fd重定向到了old_fd
* 实现 cat read.c > re_read.c
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>

int main(int argc, char** argv)
{
    char *s_name = argv[1];
    char *d_name = argv[2];
    int s_fd;
    int d_fd;
    char buf[1024];
    int ret;

    s_fd = open(s_name, O_RDONLY);
    d_fd = open(d_name, O_RDWR | O_CREAT, 0644);
    if(s_fd == -1 || d_fd == -1)
    {
        perror("open error");
        exit(1);
    }

    dup2(d_fd, STDOUT_FILENO);  
    while((ret = read(s_fd, buf, 1024)) != 0)
    {
        //write(d_fd, buf, ret);
        printf("%s", buf);

    }
    
    
    close(s_fd);
    close(d_fd);

    return 0;
}
```

<a id="30-while%E6%97%A0%E6%97%B6%E7%9A%84%E5%88%86%E5%8F%B7"></a>
#### 30. while无{}时的分号;
```
#include <stdio.h>
#include <stdlib.h>


int main(void)
{
    int n = 0;
    while (n++ <= 2);
    
    printf("n = %d\n", n); // n = 4

    return 0;
}
```

<a id="31-%E7%9F%AD%E8%B7%AF%E8%BF%90%E7%AE%97"></a>
#### 31. 短路运算
* &&前面是0时,&&符号后面的不计算
* ||前面不是0时,||号后面的不计算

<a id="32-sizeof%E8%BF%90%E7%AE%97%E7%AC%A6"></a>
#### 32. sizeof运算符
* 当数组作为函数参数时, 会自动退化成指针, 所以对函数参数做sizeof时, 大小为4
```
    void test(char a[10])
    {
        printf("a = %d\n", sizeof(a));  // a = 4
    }
```
* 当数组作为表达式时, sizeof 为该数组的空间大小
```
    char p[10] = "hello";
    printf("p = %d\n", sizeof(p));
```

<a id="33-%E5%86%85%E5%AD%98%E4%B8%AD%E6%95%B0%E6%8D%AE%E5%88%86%E5%B8%83"></a>
#### 33. 内存中数据分布
* .bss: 未初始化的全局变量   初始化为0的全局变量 static修饰（静态变量）的未初始化的或初始化为0的变量
    - 通常是指用来存放程序中未初始化的全局变量的一块内存区域, BSS是英文Block Started by Symbol的简称, BSS段属于静态内存分配
    - readelf -S/-s xx.o  可以查看.bss段记录
* .data：初始化为非0的全局变量    static修饰（静态变量）的初始化为非0的变量
* rodata：常量、只读全局变量
* stack：局部变量
* (变量/函数...)只声明不定义, 编译器不会分配地址空间, 如果使用, 会报错

<a id="34-%E6%AE%B5%E9%94%99%E8%AF%AF"></a>
#### 34. 段错误
* 对只读区域进程写操作  char　*p = "hello"; p[0] = 'H';
* 对非访问区域进行读写操 地址1000
* stack空间耗尽      函数内部 int arr[N]= {1};  #define N 100000000

<a id="35-%E6%A0%87%E5%87%86io%E6%8F%90%E4%BE%9B%E7%9A%843%E7%A7%8D%E7%B1%BB%E5%9E%8B%E7%9A%84%E7%BC%93%E5%86%B2"></a>
#### 35. 标准I/O提供的3种类型的缓冲
* 全缓冲
* 行缓冲: printf("test \n"); 没有\n就不输出, fflush(stdout) 可以刷新缓冲区
* 无缓冲: stderr

<a id="36-find%E5%91%BD%E4%BB%A4"></a>
#### 36. find命令
`find /usr/ -size +900k -size -2M | xargs ls -lh > result.txt
`

<a id="37-%E6%B1%821%7En%E4%B9%8B%E9%97%B4%E7%9A%84%E6%89%80%E6%9C%89%E7%B4%A0%E6%95%B0"></a>
#### 37. 求1~n之间的所有素数
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(int argc, char *argv[])
{
    int i, m, n;
    int count = 1;          //用来设置显示格式

    if (argc != 2) {
        printf("Enter: ./a.out number\n");
        exit(1);
    }

    printf("1～%d之间的素数有：\n", n = atoi(argv[1]));

    for (i = 2; i <= n; i++) {
        for(m = 2; m < i; m++) {
            if(i % m == 0) {
                break;
            }
        }
        if (m >= i) //内层for循环正常退出,不是break的(已经判定为素数的)
            printf("%2d %c", i, (count++) % 10 ? ' ' : '\n');
    } 
    printf("\n");

    return 0;
}
```

<a id="38-%E8%8E%B7%E5%8F%96%E8%BF%9B%E7%A8%8B%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F"></a>
#### 38. 获取进程环境变量
* environ:
```
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    extern char** environ;
    int i;
    for(i = 0 ;environ[i] != NULL; ++i)
    {
        printf("%s\n", environ[i]);
    }

    printf("-----------------\n");
    printf("PATH: %s\n", getenv("PATH"));

    return 0;
}
```
* getenv:
```
#include <stdlib.h>
#include <stdio.h>

int main(void)
{
        char *user = (char*)malloc(100);

        user = getenv("USER");
        printf("user: %s\n", user);

        
        free(user);

        return 0;
}
```
* setenv / unsetenv

<a id="39-cpummu"></a>
#### 39. CPU/MMU
* CPU: 4级流水机制 (预取器, 译码器, ALU, 寄存器)
* MMU: 虚拟内存映射 (Memery Management Unit)

<a id="40-centos7-kernel%E6%BA%90%E4%BB%A3%E7%A0%81%E8%B7%AF%E5%BE%84"></a>
#### 40. CentOS7 kernel源代码路径
* PCB进程控制块的 struct task_struct 在源代码中的定义位置: 
    - `vim /usr/src/kernels/3.10.0-693.el7.x86_64/include/linux/sched.h`
* 查询源码定义路径: `grep -r "struct task_struct {" /usr/src/kernels/3.10.0-693.el7.x86_64/include/linux/`

<a id="41-%E6%A0%B9%E6%8D%AEpid%E6%9F%A5%E6%89%BE%E8%BF%90%E8%A1%8C%E7%A8%8B%E5%BA%8F"></a>
#### 41. 根据pid查找运行程序
* `ps aux | grep 2177`
* `ps aux | grep init`

<a id="42-fork%E5%88%9B%E5%BB%BA%E5%AD%90%E8%BF%9B%E7%A8%8B%E5%B9%B6%E7%94%A8%E5%BE%AA%E7%8E%AF%E5%9B%A0%E5%AD%90i%E5%8C%BA%E5%88%86"></a>
#### 42. fork创建子进程并用循环因子i区分
* 在父进程中，fork返回新创建子进程的进程ID；
* 在子进程中，fork返回0；
* 如果出现错误，fork返回一个负值；
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int cnt = 0;

int main(int argc, char** argv)
{
    int n = 5;
    int i;
    pid_t pid;

    for(i = 0; i < n; ++i)
    {
        pid = fork();
        if(pid == 0)
        {
            break;
        }
    }
    if(pid == -1)
    {
        perror("fork error");
        exit(1);
    }else if(i == n)
    {
        printf("Parent pid=%d  ppid=%d\n", getpid(), getppid());
    }else
    {
        sleep(i);
        printf("num: %d child process\n", i+1);
        printf("Child pid=%d, ppid=%d\n", getpid(), getppid());
    }


    return 0;
}
```

<a id="43-%E5%A4%9A%E8%BF%9B%E7%A8%8Bgdb%E8%B0%83%E8%AF%95"></a>
#### 43. 多进程gdb调试
* set follow-fork-mode child
* set follow-fork-mode parent
* 一定要在fork()之前设定才有意义

<a id="44-exec%E5%87%BD%E6%95%B0%E6%97%8F"></a>
#### 44. exec函数族
* p代表PATH环境变量, 没有p的要加路径
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(void)
{
        pid_t pid;
        pid = fork();
        if(pid == -1)
        {
                perror("fork error");
                exit(1);
        }else if(pid > 0)
        {
                sleep(1);
                printf("Parent Process: %d  Parent ID: %d\n", getpid(), getppid());
        }else if(pid == 0)
        {
                char *argv1[] = {"ls", "-l", "-a", NULL};
                //execlp("ls", "ls", "-l", "-a", NULL);
                //execl("./test", "test", NULL);
                execvp("ls", argv1);
                perror("execlp error");
                exit(1);
        }
        return 0;
}

```

<a id="45-%E5%AD%A4%E5%84%BF%E8%BF%9B%E7%A8%8B--%E5%83%B5%E5%B0%B8%E8%BF%9B%E7%A8%8B"></a>
#### 45. 孤儿进程 / 僵尸进程
* 父进程先于子进程结束, 子进程变为孤儿进程
* 查看进程ID: `ps ajx`
* PPID PID PGID SID(Session ID)
* 僵尸进程不能用kill清除掉
* 可以手动 kill -9 父进程来回收僵尸进程

<a id="46-wait%E9%98%BB%E5%A1%9E%E5%9B%9E%E6%94%B6%E5%83%B5%E5%B0%B8%E8%BF%9B%E7%A8%8B"></a>
#### 46. wait()阻塞回收僵尸进程
* 一个wait()只能回收1个子进程
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main(void)
{
        pid_t pid, wpid;
        int status;

        pid = fork();
        if(pid == 0)
        {
                sleep(3);
                printf("Child process id: %d\n", getpid());
                return 19;
        }
        else if(pid > 0)
        {
                printf("Parent process id: %d\n", getpid());
                wpid = wait(&status);
                printf("wpid: %d\n", wpid);

                if(WIFEXITED(status))
                {
                        printf("exit with %d\n", WEXITSTATUS(status));
                }else if(WIFSIGNALED(status))
                {
                        printf("kill by %d\n", WTERMSIG(status));
                }

        }


        return 0;
}
```

<a id="47-%E5%9B%9E%E6%94%B6%E6%8C%87%E5%AE%9A%E8%BF%9B%E7%A8%8B-waitpid"></a>
#### 47. 回收指定进程 waitpid()
* 非阻塞回收: `wpid = waitpid(tmppid, NULL, WNOHANG);`
* 阻塞指定回收: `wpid = waitpid(tmppid, NULL, 0);`
* 阻塞全部回收: `wpid = waitpid(-1, NULL, 0);`
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int cnt = 0;

int main(int argc, char** argv)
{
        int n = 5;
        int i;
        pid_t pid, wpid, tmppid;

        for(i = 0; i < n; ++i)
        {
                pid = fork();
                if(pid == 0)
                {
                        break;
                }
                if(i == 1)
                {
                        tmppid = pid;
                }
        }
        if(pid == -1)
        {
                perror("fork error");
                exit(1);
        }else if(i == n)
        {
                printf("Parent pid=%d  ppid=%d\n", getpid(), getppid());
                //wpid = wait(NULL);
                //wpid = waitpid(tmppid, NULL, WNOHANG);
                //wpid = waitpid(tmppid, NULL, 0);
                while(--i)
                {
                        wpid = waitpid(-1, NULL, 0);
                        printf("wpid: %d\n", wpid);
                }
                printf("tmppid: %d\n", tmppid);
        }else
        {
                //sleep(i);
                printf("num: %d child process\n", i+1);
                printf("Child pid=%d, ppid=%d\n", getpid(), getppid());
        }


        return 0;
}
```

<a id="48-%E7%88%B6%E8%BF%9B%E7%A8%8Bfork-3-%E4%B8%AA%E5%AD%90%E8%BF%9B%E7%A8%8B%EF%BC%8C%E4%B8%89%E4%B8%AA%E5%AD%90%E8%BF%9B%E7%A8%8B%E4%B8%80%E4%B8%AA%E8%B0%83%E7%94%A8ps%E5%91%BD%E4%BB%A4%EF%BC%8C%E4%B8%80%E4%B8%AA%E8%B0%83%E7%94%A8%E8%87%AA%E5%AE%9A%E4%B9%89%E7%A8%8B%E5%BA%8F1%E6%AD%A3%E5%B8%B8-%E4%B8%80%E4%B8%AA%E8%B0%83%E7%94%A8%E8%87%AA%E5%AE%9A%E4%B9%89%E7%A8%8B%E5%BA%8F2%E4%BC%9A%E5%87%BA%E6%AE%B5%E9%94%99%E8%AF%AF%E3%80%82%E7%88%B6%E8%BF%9B%E7%A8%8B%E4%BD%BF%E7%94%A8waitpid%E5%AF%B9%E5%85%B6%E5%AD%90%E8%BF%9B%E7%A8%8B%E5%85%A8%E9%83%A8%E8%BF%9B%E8%A1%8C%E5%9B%9E%E6%94%B6-%E5%B9%B6%E6%89%93%E5%8D%B0%E7%8A%B6%E6%80%81"></a>
#### 48. 父进程fork 3 个子进程，三个子进程一个调用ps命令，一个调用自定义程序1(正常), 一个调用自定义程序2(会出段错误)。父进程使用waitpid对其子进程全部进行回收, 并打印状态
* exercise_fork.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main(int argc, char** argv)
{
    int n = 4;
    int i;
    pid_t pid, wpid;
    int status;

    for(i = 1; i < n; ++i)
    {
        pid = fork();
        if(pid == 0)
        {
            break;
        }
    }
    if(pid == -1)
    {
        perror("fork error");
        exit(1);
    }else if(i == n)
    {
        printf("Parent pid=%d  ppid=%d\n", getpid(), getppid());
        while(--i)
        {
            wpid = waitpid(-1, &status, 0);
            printf("wpid: %d\n", wpid);
            
            if(WIFEXITED(status))
            {
                printf("exit with %d\n", WEXITSTATUS(status));
            }else if(WIFSIGNALED(status))
            {
                printf("kill by %d\n", WTERMSIG(status));
            }

        }
    }else if(i == 1)
    {
        sleep(i);
        printf("num: %d child process\n", i+1);
        printf("Child pid=%d, ppid=%d\n", getpid(), getppid());
        execlp("/bin/ps", "ps", "-a", "-u", "-x", NULL);
        
    }else if(i == 2)
    {
        sleep(i);
        printf("num: %d child process\n", i+1);
        printf("Child pid=%d, ppid=%d\n", getpid(), getppid());
        execl("./test", "test", NULL);
        
    }else if(i == 3)
    {
        sleep(i);
        printf("num: %d child process\n", i+1);
        printf("Child pid=%d, ppid=%d\n", getpid(), getppid());
        execl("./segerr", "segerr", NULL);
        
    }

    return 0;
}
```
* segerr.c:
```
#include <stdio.h>
#include <signal.h>

int main(void)
{
    printf("SEG ERR\n");
    raise(SIGSEGV); 

    return 0;
}
```

<a id="49-%E5%85%A8%E5%8F%8C%E5%B7%A5%E5%8D%8A%E5%8F%8C%E5%B7%A5%E5%8D%95%E5%B7%A5"></a>
#### 49. 全双工/半双工/单工
* 全双工: 双向读写(电话)
* 半双工: 双向半双工, 一端写一端读(对讲机), 管道pipe
* 单工: 单向写/读(遥控器)

<a id="50-%E9%80%9A%E8%BF%87pipe%E7%AE%A1%E9%81%93%E8%BF%9B%E8%A1%8C%E8%BF%9B%E7%A8%8B%E9%97%B4%E9%80%9A%E4%BF%A1"></a>
#### 50. 通过pipe管道进行进程间通信
* 先创建管道pipe, 再fork()子进程
* 管道两端可分别用描述字fd[0]以及fd[1]来描述，需要注意的是，管道的两端是固定了任务的。即一端只能用于读，由描述字fd[0]表示，称其为管道读端；另一端则只能用于写，由描述字fd[1]来表示，称其为管道写端。
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>
#include <string.h>

void sys_err(const char* str)
{
        perror(str);
        exit(1);
}

int main(void)
{
        int fd[2];
        char *str = "hello pipe\n";
        char buf[1024];
        int ret;
        pid_t pid;

        ret = pipe(fd);
        if(ret == -1)
                sys_err("pipe err");            
        
        pid = fork();
        if(pid == -1)
        {
                sys_err("fork err");
        }else if(pid > 0)
        {
                close(fd[0]);
                write(fd[1], str, strlen(str));
                wait(NULL);
                close(fd[1]);
        }else if(pid == 0)
        {
                close(fd[1]);
                ret = read(fd[0], buf, sizeof(buf));
                write(STDOUT_FILENO, buf, ret);
                close(fd[0]);
        }               

        return 0;
}
```

<a id="51-pipe--fork--dup2--execlp-%E7%BB%83%E4%B9%A01%E7%88%B6%E5%AD%90"></a>
#### 51. pipe / fork / dup2 / execlp 练习1父子
* 让父进程去读fd[0]管道pipe, 可以避免父进程先结束, 出现孤儿进程
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>
#include <string.h>

void sys_err(const char* str)
{
        perror(str);
        exit(1);
}

int main(void)
{
        int fd[2];
        int ret;
        pid_t pid;

        ret = pipe(fd);
        if(ret == -1)
                sys_err("pipe err");

        pid = fork();
        if(pid == -1)
        {
                sys_err("fork err");
        }else if(pid > 0)
        {
                close(fd[1]);
                dup2(fd[0], STDIN_FILENO);
                execlp("wc", "wc", "-l", NULL);
        }else if(pid == 0)
        {
                close(fd[0]);
                dup2(fd[1], STDOUT_FILENO);
                execlp("ls", "ls", NULL);
        }

        return 0;
}
```

<a id="52-pipe--fork--dup2--execlp-%E7%BB%83%E4%B9%A02-%E5%85%84%E5%BC%9F"></a>
#### 52. pipe / fork / dup2 / execlp 练习2 兄弟
* 父进程自己读写pipe, 无法保证单向半双工, 需要在父进程中关闭rw, 即fd[0], fd[1], 才能保证子进程(兄弟)顺利读写
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>
#include <string.h>

void sys_err(const char* str)
{
        perror(str);
        exit(1);
}

int main(void)
{
        int fd[2];
        int ret;
        pid_t pid;
        int i = 0;

        ret = pipe(fd);
        if(ret == -1)
                sys_err("pipe err");

        for(i = 0; i < 2; ++i)
        {
                pid = fork();
                if(pid == 0)
                        break;
        }

        if(pid == -1)
        {
                sys_err("fork err");
        }else if(i == 2)
        {
                close(fd[0]);
                close(fd[1]);
                while(i--)
                {
                        wait(NULL);
                }
        }else if(i == 0)
        {
                close(fd[0]);
                dup2(fd[1], STDOUT_FILENO);
                execlp("ls", "ls", NULL);
        }else if(i == 1)
        {
                close(fd[1]);
                dup2(fd[0], STDIN_FILENO);
                execlp("wc", "wc", "-l", NULL);
        }

        return 0;
}
```

<a id="53-pipe%E4%B8%80%E8%AF%BB%E5%A4%9A%E5%86%99%E4%B8%80%E5%86%99%E5%A4%9A%E8%AF%BB"></a>
#### 53. pipe一读多写/一写多读


<a id="54-%E7%BB%9F%E8%AE%A1%E5%BD%93%E5%89%8D%E7%B3%BB%E7%BB%9F%E4%B8%AD%E8%BF%9B%E7%A8%8Bid%E5%A4%A7%E4%BA%8E1000%E7%9A%84%E8%BF%9B%E7%A8%8B%E4%B8%AA%E6%95%B0"></a>
#### 54. 统计当前系统中进程ID大于1000的进程个数
* 思路: `ps aux`-> strtok -> atoi -> >1000
* count_pid.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_LEN 1024

int main(void)
{
        char *buf = (char*)malloc(MAX_LEN);
        int cnt = 0;
        int pid_num = 1000;

        while(fgets(buf, MAX_LEN, stdin) != NULL)
        {
                //printf("%s", buf);
                char *temp = strtok(buf, " ");
                if(temp)
                {
                        temp = strtok(NULL, " ");
                        //printf("%s\n", temp);
                        if(atoi(temp) > pid_num)
                        {
                                cnt++;
                        }
                }

        }

        printf("pid > 1000:  %d\n", cnt);

        free(buf);

        return 0;
}
```
* pipe_count_pid.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>
#include <string.h>

void sys_err(const char* str)
{
        perror(str);
        exit(1);
}

int main(void)
{
        int fd[2];
        int ret;
        pid_t pid;
        int i = 0;

        ret = pipe(fd);
        if(ret == -1)
                sys_err("pipe err");

        for(i = 0; i < 2; ++i)
        {
                pid = fork();
                if(pid == 0)
                        break;
        }

        if(pid == -1)
        {
                sys_err("fork err");
        }else if(i == 2)
        {
                close(fd[0]);
                close(fd[1]);
                while(i--)
                {
                        wait(NULL);
                }
        }else if(i == 0)
        {
                close(fd[0]);
                dup2(fd[1], STDOUT_FILENO);
                execlp("ps", "ps", "-a", "-u", "-x", NULL);     
        }else if(i == 1)
        {
                close(fd[1]);
                dup2(fd[0], STDIN_FILENO);
                execlp("./count_pid", "count_pid", NULL);
        }               
        
        return 0;
}
```

<a id="55-pipe%E7%AE%A1%E9%81%93%E5%A4%A7%E5%B0%8F-4096-bytes"></a>
#### 55. pipe管道大小 4096 bytes
* `ulimit -a`
* `pipe size            (512 bytes, -p) 8`
* 512 * 8 = 4096

<a id="56-%E8%8E%B7%E5%8F%96%E7%AE%A1%E9%81%93%E7%BC%93%E5%86%B2%E5%8C%BA%E5%A4%A7%E5%B0%8Ffpathconf"></a>
#### 56. 获取管道缓冲区大小fpathconf
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>
#include <string.h>

void sys_err(const char* str)
{
        perror(str);
        exit(1);
}

int main(void)
{
        int fd[2];
        int ret;

        ret = pipe(fd);
        if(ret == -1)
                sys_err("pipe err");
        long len = fpathconf(fd[0], _PC_PIPE_BUF);
        if(len == -1)
        {
                sys_err("fpathconf err");
        }       
        printf("pipe buf: %ld\n", len);
        
        return 0;
}
```

<a id="57-fifo%E5%91%BD%E5%90%8D%E7%AE%A1%E9%81%93"></a>
#### 57. FIFO命名管道
* 不相关的进程可以同时使用的管道
* `mkfifo myfifo`


<a id="58-fifo%E4%B8%80%E5%86%99%E5%A4%9A%E8%AF%BB"></a>
#### 58. fifo一写多读
* fifo_w.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/types.h>

void sys_err(char *str)
{
    perror(str);
    exit(1);
}

int main(int argc, char** argv)
{
    char buf[4096];
    int fd;
    int i = 0;
    
    if(argc < 2)
    {
        printf("./fifo_w myfifo\n");
        return -1;
    }
    fd = open(argv[1], O_WRONLY);
    if(fd < 0)
    {
        sys_err("open fifo err");
    }
    while(1)
    {
        sprintf(buf, "hello no: %d\n", i++);
        write(fd, buf, strlen(buf));
        sleep(2);
    }

    close(fd);  

    return 0;
}
```
* fifo.r.o:
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/types.h>

void sys_err(char *str)
{
    perror(str);
    exit(1);
}

int main(int argc, char** argv)
{
    char buf[4096];
    int fd, len;
    
    if(argc < 2)
    {
        printf("./fifo_r myfifo\n");
        return -1;
    }
    fd = open(argv[1], O_RDONLY);
    if(fd < 0)
    {
        sys_err("open fifo err");
    }
    while(1)
    {
        len = read(fd, buf, sizeof(buf));
        write(STDOUT_FILENO, buf, len);
        sleep(3);
    }

    close(fd);  

    return 0;
}
```

<a id="59-mmap"></a>
#### 59. mmap
* 将磁盘中的文件映射到内存, 直接进行读写操作
* 好处: 文件映射到内存后, 所有操作指针的函数就都可以使用了(string, mem...)
* mmap_test.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <sys/mman.h>
#include <string.h>
#include <unistd.h>

int main(void)
{
        char *mp;
        int fd, len, ret;

        fd = open("testfile", O_RDWR | O_TRUNC | O_CREAT, 0644);
        if(fd < 0)
        {
                perror("open err");
                exit(1);
        }

        ftruncate(fd, 4);
        len = lseek(fd, 0, SEEK_END);

        mp = mmap(NULL, len, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
        if(mp == MAP_FAILED)
        {
                perror("mmap err");
                exit(1);
        }

        strcpy(mp, "abc\n");
        ret = munmap(mp, len);
        if(ret == -1)
        {
                perror("munmap err");
                exit(1);
        } 
        

        close(fd);

        return 0;
}
```

<a id="60-mmap%E9%9D%9E%E8%A1%80%E7%BC%98ipc%E9%80%9A%E4%BF%A1"></a>
#### 60. mmap非血缘IPC通信
* 一写多读 / 一读多写 均可
* mmap_w.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <sys/mman.h>
#include <string.h>
#include <unistd.h>

struct STU {
    int id;
    char name[20];
    char sex;
};

void sys_err(char *str)
{
    perror(str);
    exit(1);
}


int main(int argc, char** argv)
{
        char *mp;
        int fd, ret;
        struct STU student = {10, "Liangxi", 'f'};

        if(argc < 2)
        {
                printf("a.out file_shared\n");
                exit(-1);
        }

        fd = open(argv[1], O_RDWR | O_CREAT, 0644);
        if(fd < 0)
        {
                perror("open err");
                exit(1);
        }

        ftruncate(fd, sizeof(student));

        mp = mmap(NULL, sizeof(student), PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
        if(mp == MAP_FAILED)
        {
                perror("mmap err");
                exit(1);
        }
        close(fd);

        while(1)
        {
                memcpy(mp, &student, sizeof(student));
                student.id++;
                sleep(1);
        }


        ret = munmap(mp, sizeof(student));
        if(ret == -1)
        {
                perror("munmap err");
                exit(1);
        }

        return 0;
}
```
* mmap_r.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <sys/mman.h>
#include <string.h>
#include <unistd.h>

struct STU {
    int id;
    char name[20];
    char sex;
};

void sys_err(char *str)
{
    perror(str);
    exit(1);
}


int main(int argc, char** argv)
{
    int fd, ret;
    struct STU student; 
    struct STU *sp;

    if(argc < 2)
    {
        printf("a.out file_shared\n");
        exit(-1);
    }
    
    fd = open(argv[1], O_RDONLY);
    if(fd < 0)
    {
        perror("open err");
        exit(1);
    }
    
    sp = mmap(NULL, sizeof(student), PROT_READ, MAP_SHARED, fd, 0);
    if(sp == MAP_FAILED)
    {
        perror("mmap err");
        exit(1);
    }
    close(fd);

    while(1)
    {
        printf("student id: %d - name: %s - sex: %c\n", sp->id, sp->name, sp->sex);
        usleep(10000);
    }
    
    
    ret = munmap(sp, sizeof(student));
    if(ret == -1)
    {
        perror("munmap err");
        exit(1);
    } 
    
    return 0;
}
```

<a id="61-%E8%BD%AF%E4%BB%B6%E9%A1%B9%E7%9B%AE%E8%AE%BE%E8%AE%A1%E5%AE%9E%E7%8E%B0%E6%B5%81%E7%A8%8B"></a>
#### 61. 软件项目设计实现流程
* __功能模块图__: 总目标 -> 拆分成需求点 -> 根据需求点画出系统架构图
* 关键技术难点实现方法
* __设计数据结构__: 根据功能模块图, (struct/class 数组, 链表), 算法, 设计模式
* UML建模: 确定对象属性, 功能, 关联关系(1:1, 1:n, n:n)
* __数据业务流程图__: 顺序, 分支, 循环
* 时序图
* __列出实现步骤__: 分模块, 逐步由易到难代码实现
* __功能拆分__: 对功能进行2~3层拆分, 按步骤实现, 边写边测(printf), 从最简单的开始, 每次最好不要超过20行代码
* 测试(gdb)

<a id="62-%E5%A4%9A%E8%BF%9B%E7%A8%8B%E6%8B%B7%E8%B4%9D%E5%A4%A7%E6%96%87%E4%BB%B6"></a>
#### 62. 多进程拷贝大文件 
* multiprocess_cp.c
* 只要有open/close, mmap/munmap等操作, 就需要定义临时指针, 以保证关闭的有效性(避免指针操作过程中的写操作修改原值)
```
#include <stdio.h>
#include <stdlib.h>
#include <sys/mman.h>
#include <unistd.h>
#include <string.h>
#include <sys/stat.h>
#include <sys/wait.h>
#include <fcntl.h>

void err_int(int ret, const char *err)
{
    if(ret == -1)
    {
        perror(err);
        exit(1);
    }
    return;
}

void err_char(char *ret, const char *err)
{
    if(ret == MAP_FAILED)
    {
        perror(err);
        exit(1);    
    }
    return;
}


int main(int argc, char** argv)
{
    int fd_src, fd_dst, len, ret, i, pn;
    char *mp_src, *mp_dst, *tmp_srcp, *tmp_dstp;
    struct stat sbuf;
    pid_t pid;

    if(argc < 3 || argc > 4)
    {
        printf("arg num invalid, please input: ./a.out src_file dst_file [process number]\n");
        exit(1);
    }else if(argc == 3)
    {
        pn = 5;
    }else if(argc == 4)
    {
        pn = atoi(argv[3]);
    }

    fd_src = open(argv[1], O_RDONLY);
    err_int(fd_src, "open fd_src err");

    fd_dst = open(argv[2], O_RDWR | O_TRUNC | O_CREAT, 0644);
    err_int(fd_dst, "open fd_dst err");

    ret = fstat(fd_src, &sbuf);
    err_int(ret, "fstat err");
    
    len = sbuf.st_size;
    if(len < pn)
    {
        pn = len;
    }

    ret = ftruncate(fd_dst, len);
    err_int(ret, "ftruncate err");

    mp_src = mmap(NULL, len, PROT_READ, MAP_SHARED, fd_src, 0);     
    err_char(mp_src, "mmap mp_src err");

    mp_dst = mmap(NULL, len, PROT_READ | PROT_WRITE, MAP_SHARED, fd_dst, 0);
    err_char(mp_dst, "mmap mp_dst err");

    tmp_srcp = mp_src;
    tmp_dstp = mp_dst;
    
    int bs = len / pn;
    int mod = len % bs;
    

    for(i = 0; i < pn; ++i)
    {
        pid = fork();
        if(pid == 0)
        {
            break;
        }
    }

    if(i == pn) // parent process
    {
        for(i = 0; i < pn; ++i)
        {
            wait(NULL);
        }
    }else if(i == (pn - 1)) // last process
    {
        memcpy(tmp_dstp + i * bs, tmp_srcp + i * bs, bs + mod);
    }else if(i == 0)    // first process
    {
        memcpy(tmp_dstp, tmp_srcp, bs );
    }else   // middle process
    {
        memcpy(tmp_dstp + i * bs, tmp_srcp + i * bs, bs);
    }

    munmap(mp_src, len);
    munmap(mp_dst, len);


    close(fd_src);
    close(fd_dst);

    return 0;
}
```

<a id="63-%E4%BA%A4%E4%BA%92shell-myshellc"></a>
#### 63. 交互shell, myshell.c 
* 总体步骤:
    + 接收用户输入命令字符串，拆分命令及参数存储。（自行设计数据存储结构）
    + 实现普通命令加载功能
    + 实现输入、输出重定向的功能
    + 实现管道
    + 支持多重管道

* 接收用户输入命令字符串，拆分命令及参数存储（struct数组） 实现普通命令加载功能
    + 单行stdin回显, exit退出shell
    + stdin字符串拆分, 填充数组回显
    + fork, execl 执行单行命令加参数
```
int main(void)
{
        //int len;
        int i = 0;
        char buf[MAX_LINE];
        pid_t pid;
        char *cur_cmd, *next_cmd;

        struct cmd c1;
        c1.in = NULL;
        c1.out = NULL;

        while(1)
        {
                printf("myshell%% ");

                if(!fgets(buf, MAX_LINE, stdin) || strcmp(buf, "exit\n") == 0)
                {
                        exit(0);
                }
                if(buf[strlen(buf)-1] == '\n')
                {
                        buf[strlen(buf) - 1] = '\0';
                }
                //printf("%s\n", buf);


                cur_cmd = buf;
                while((next_cmd = strsep(&cur_cmd, " ")))
                {
                        c1.argv[i] = next_cmd;
                        //printf("%s ", c1.argv[i]);
                        i++;
                }
                //printf("\n");


                pid = fork();
                if(pid > 0)
                {
                        wait(NULL);
                }else if(pid == 0)
                {
                        execvp(c1.argv[0], c1.argv);
                        fprintf(stderr, "executing %s error\n", c1.argv[0]);
                        exit(127);
                }

        }

        return 0;
}
```

* 实现输入、输出重定向的功能:
    - 在子进程中 dup2 , `>` dup2(file, STDOUT_FILENO), `<` dup2(file, STDIN_FILENO)

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/wait.h>

#define MAX_LINE 4096
#define MAX_ARG 8
#define MAX_PIPE 16

struct
{
    char *argv[MAX_ARG];
    char *in, *out;
}cmd;

int parse(char *buf)
{
    char *p = buf;
    int n = 0;
    cmd.in = cmd.out = NULL;
    
    while(*p != '\0')
    {
        if(*p == ' ')
        {
            *p++ = '\0';
            continue;
        }
        if(*p == '>')
        {
            *p++ = '\0';
            while(*(++p) == ' ');
            cmd.out = p;
            continue;
        }
        if(*p == '<')
        {
            *p++ = '\0';
            while(*(++p) == ' ');
            cmd.in = p;
            continue;
        }
        if(*p != ' ' && ((p == buf) || *(p - 1) == '\0'))
        {
            cmd.argv[n++] = p++;
            continue;
        }
        

        p++;
    }

    cmd.argv[n] = NULL;

    return 0;
}


int main(void)
{
    int i = 0;
    char buf[MAX_LINE];
    pid_t pid;
    char *cur_cmd, *next_cmd;
    int fd;

    while(1)
    {
        printf("myshell%% ");
    
        if(!fgets(buf, MAX_LINE, stdin) || strcmp(buf, "exit\n") == 0)
        {
            exit(0);
        }
        if(buf[strlen(buf)-1] == '\n')
        {
            buf[strlen(buf) - 1] = '\0';
        }
        //printf("%s\n", buf);
        
            
        next_cmd = buf;

        parse(next_cmd);
        
       
        pid = fork();
        if(pid > 0)
        {
            wait(NULL);
        }else if(pid == 0)
        {   
            if(cmd.in)
            {
                fd = open(cmd.in, O_RDONLY);
                if(fd != -1)
                {
                    dup2(fd, STDIN_FILENO);
                }
            }
            if(cmd.out)
            {
                fd = open(cmd.out, O_WRONLY | O_TRUNC | O_CREAT, 0644);
                if(fd != -1)
                {
                    dup2(fd, STDOUT_FILENO);
                }
            }
            execvp(cmd.argv[0], cmd.argv);
            fprintf(stderr, "executing %s error\n", cmd.argv[0]);
            exit(127);
        }

    }

    return 0;
}

```
* 实现多管道, 多进程
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/wait.h>

#define MAX_LINE 4096
#define MAX_ARG 8
#define MAX_PIPE 16

struct
{
    char *argv[MAX_ARG];
    char *in, *out;
}cmd[MAX_PIPE+1];

int parse(char *buf, int num)
{
    char *p = buf;
    int n = 0;
    cmd[num].in = cmd[num].out = NULL;
    
    while(*p != '\0')
    {
        if(*p == ' ')
        {
            *p++ = '\0';
            continue;
        }
        if(*p == '>')
        {
            *p++ = '\0';
            while(*(++p) == ' ');
            cmd[num].out = p;
            continue;
        }
        if(*p == '<')
        {
            *p++ = '\0';
            while(*(++p) == ' ');
            cmd[num].in = p;
            continue;
        }
        if(*p != ' ' && ((p == buf) || *(p - 1) == '\0'))
        {
            cmd[num].argv[n++] = p++;
            continue;
        }
        

        p++;
    }

    cmd[num].argv[n] = NULL;

    return 0;
}


int main(void)
{
    int cmd_num, pipe_num;
    int i = 0;
    int j = 0;
    char buf[MAX_LINE];
    pid_t pid;
    char *cur_cmd, *next_cmd;
    int pfd[MAX_PIPE][2];
    int fd;

    while(1)
    {
        printf("myshell%% ");
    
        if(!fgets(buf, MAX_LINE, stdin) || strcmp(buf, "exit\n") == 0)
        {
            exit(0);
        }
        if(buf[strlen(buf)-1] == '\n')
        {
            buf[strlen(buf) - 1] = '\0';
        }
            
        next_cmd = buf;
        cmd_num = 0;
        pipe_num = 0;

        while((cur_cmd = strsep(&next_cmd, "|")))
        {
            parse(cur_cmd, cmd_num);
            cmd_num++;
        }   

        pipe_num = cmd_num - 1;
        
        for(i = 0; i < pipe_num; ++i)
        {
            if(pipe(pfd[i]))
            {
                perror("pipe err");
                exit(1);
            }
        }

        for(i = 0; i < cmd_num; ++i)
        {
            pid = fork();
            if(pid == 0)
                break;
        }


        if(pid > 0)
        {   
            for(i = 0; i < pipe_num; ++i)
            {
                close(pfd[i][0]);
                close(pfd[i][1]);
            }
            for(i = 0; i < cmd_num; ++i)
            {
                wait(NULL);
            }
        }else if(pid == 0)
        {   
            if(pipe_num)
            {
                if(i == 0)
                {
                    dup2(pfd[i][1], STDOUT_FILENO);
                    close(pfd[i][0]);
                    for(j = 1; j < pipe_num; ++j)
                    {
                        close(pfd[j][0]);
                        close(pfd[j][1]);
                    }
                }else if(i == (cmd_num - 1))
                {
                    dup2(pfd[i-1][0], STDIN_FILENO);
                    close(pfd[i-1][1]);
                    for(j = 0; j < pipe_num - 1; ++j)
                    {
                        close(pfd[j][0]);
                        close(pfd[j][1]);
                    }

                }else
                {
                    dup2(pfd[i-1][0], STDIN_FILENO);
                    close(pfd[i-1][1]);
                    dup2(pfd[i][1], STDOUT_FILENO);
                    close(pfd[i][0]);
                    for(j = 0; j < pipe_num; ++j)
                    {
                        if(j != i - 1 || j != i)
                        {
                            close(pfd[j][0]);
                            close(pfd[j][1]);
                        }
                    }
                }

            }
            if(cmd[i].in)
            {
                fd = open(cmd[i].in, O_RDONLY);
                if(fd != -1)
                {
                    dup2(fd, STDIN_FILENO);
                }
            }
            if(cmd[i].out)
            {
                fd = open(cmd[i].out, O_WRONLY | O_TRUNC | O_CREAT, 0644);
                if(fd != -1)
                {
                    dup2(fd, STDOUT_FILENO);
                }
            }
            execvp(cmd[i].argv[0], cmd[i].argv);
            fprintf(stderr, "executing %s error\n", cmd[i].argv[0]);
            exit(127);
        }

    }

    return 0;
}
```

<a id="64-qq_ipc"></a>
#### 64. QQ_IPC
* qq_ipc_client.c 本地fifo读写struct测试
```
#ifndef _QQ_IPC_H
#define _QQ_IPC_H

struct QQ_DATA_INFO
{
    int protocol;
    char srcname[20];
    char dstname[20];
    char data[100];
};

#endif
```

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/stat.h>
#include <error.h>
#include <fcntl.h>
#include <sys/types.h>
#include "qq_ipc.h"

#define SEV_FIFO "SEV_FIFO"

void sys_err(const char* err)
{
    perror(err);
    exit(1);
}

int main(int argc, char** argv)
{
    int sev_fd, cli_fd;
    int len;
    struct QQ_DATA_INFO dbuf, tmpbuf;
    char cmd_buf[100];
    char *username;

    if(argc < 2)
    {
        printf("./qq_ipc_client dstname\n");
        exit(1);
    }

    username = argv[1];
    mkfifo(username, 0777);

    cli_fd = open(username, O_RDWR | O_NONBLOCK);

    dbuf.protocol = 1;
    strcpy(dbuf.srcname, username);

    write(cli_fd, &dbuf, sizeof(dbuf));

    len = read(cli_fd, &tmpbuf, sizeof(tmpbuf));
    if(len > 0)
    {
        printf("srcname: %s, protocol: %d\n", tmpbuf.srcname, tmpbuf.protocol);
    }

    unlink(username);
    close(cli_fd);

    return 0;
}
```

* qq_ipc_client.c 本地NOBLOCK读写屏幕显示 
    - segment falut段错误, 可以用gdb跟踪
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/stat.h>
#include <errno.h>
#include <fcntl.h>
#include <sys/types.h>
#include "qq_ipc.h"

#define SEV_FIFO "SEV_FIFO"

void sys_err(const char* err)
{
    perror(err);
    exit(1);
}


int main(int argc, char** argv)
{
    int sev_fd, cli_fd;
    int len, flag;
    struct QQ_DATA_INFO dbuf, tmpbuf, talkbuf;
    char cmd_buf[100];
    char *username;

    // login
    if(argc < 2)
    {
        printf("./qq_ipc_client dstname\n");
        exit(1);
    }

    username = argv[1];
    mkfifo(username, 0777);

    cli_fd = open(username, O_RDWR | O_NONBLOCK);

    dbuf.protocol = 1;
    strcpy(dbuf.srcname, username);

    flag = fcntl(STDIN_FILENO, F_GETFL);
    flag |= O_NONBLOCK;
    fcntl(STDIN_FILENO, F_SETFL, flag);

    write(cli_fd, &dbuf, sizeof(dbuf));
    //printf("qq:");
    //fflush(STDIN_FILENO);

    while(1)
    {
        // display
        len = read(cli_fd, &tmpbuf, sizeof(tmpbuf));
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
        //printf("qq:");
        //fflush(STDIN_FILENO);
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
                write(cli_fd, &talkbuf, sizeof(talkbuf));
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
            write(cli_fd, &talkbuf, sizeof(talkbuf));
        }
    }

    unlink(username);
    close(cli_fd);

    return 0;
}
```
    - qq_ipc_client.c (final):
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/stat.h>
#include <errno.h>
#include <fcntl.h>
#include <sys/types.h>
#include "qq_ipc.h"

#define SEV_FIFO "SEV_FIFO"

void sys_err(const char* err)
{
    perror(err);
    exit(1);
}


int main(int argc, char** argv)
{
    int sev_fd, cli_fd;
    int len, flag;
    struct QQ_DATA_INFO dbuf, tmpbuf, talkbuf;
    char cmd_buf[100];
    char *username;

    // login
    if(argc < 2)
    {
        printf("./qq_ipc_client dstname\n");
        exit(1);
    }
    
    if((sev_fd = open(SEV_FIFO, O_WRONLY)) < 0)
    {
        sys_err("open serv_fd err");
    }

    username = argv[1];
    mkfifo(username, 0777);

    cli_fd = open(username, O_RDONLY|O_NONBLOCK);

    dbuf.protocol = 1; //client init default: login
    strcpy(dbuf.srcname, username);

    flag = fcntl(STDIN_FILENO, F_GETFL);
    flag |= O_NONBLOCK;
    fcntl(STDIN_FILENO, F_SETFL, flag);

    write(sev_fd, &dbuf, sizeof(dbuf));
    //printf("qq:");
    //fflush(STDIN_FILENO);

    while(1)
    {
        // display
        len = read(cli_fd, &tmpbuf, sizeof(tmpbuf));
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
        //printf("qq:");
        //fflush(STDIN_FILENO);
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
                write(sev_fd, &talkbuf, sizeof(talkbuf));
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
            write(sev_fd, &talkbuf, sizeof(talkbuf));
        }
    }

    unlink(username);
    close(cli_fd);
    close(sev_fd);

    return 0;
}
```


* qq_ipc_server.c:
    - fifo接收client发送的struct, 并回显
    - 链表的CRUD, 先做创建node, insert, c/s联合测试server端 printf head->username
    - `gcc -g qq_ipc_server.c server_record.c -I ./ -Wall -o qq_ipc_server`

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <fcntl.h>
#include <errno.h>
#include <sys/stat.h>
#include <sys/types.h>
#include "qq_ipc.h"
#include "server_record.h"

#define SEV_FIFO "SEV_FIFO"

record head = NULL;

void sys_err(const char* err)
{
    perror(err);
    exit(1);
}

void login_qq(struct QQ_DATA_INFO *dbuf, record *head)
{
    //create record
    int user_fd;
    user_fd = open(dbuf->srcname, O_WRONLY);
    record r = create_record(dbuf->srcname, user_fd);

    //insert
    insert_record(head, r);
}

int main(int argc, char** argv)
{
    int serv_fd;
    struct QQ_DATA_INFO dbuf;

    if(access(SEV_FIFO, F_OK) != 0)
    {
        mkfifo(SEV_FIFO, 0644);
    }
    serv_fd = open(SEV_FIFO, O_RDONLY);
    if(serv_fd == -1)
    {
        sys_err("open serv_fd err");
    }

    server_record_init(&head);

    while(1)
    {
        read(serv_fd, &dbuf, sizeof(dbuf));
        switch(dbuf.protocol)
        {
            case 1: 
                login_qq(&dbuf, &head); 
                printf("record: %s  %d\n", head->username, head->user_fifo_fd);
                break;
            default: continue;
        }
    }

        


    unlink(SEV_FIFO);
    close(serv_fd);

    return 0;
}
```

    - 链表摘掉delete, freenode
    - 链表的search, destroy
    - qq_ipc_server.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <fcntl.h>
#include <errno.h>
#include <sys/stat.h>
#include <sys/types.h>
#include "qq_ipc.h"
#include "server_record.h"

#define SEV_FIFO "SEV_FIFO"

record head = NULL;

void sys_err(const char* err)
{
    perror(err);
    exit(1);
}

void login_qq(struct QQ_DATA_INFO *dbuf, record *head)
{
    //create record
    int user_fd;
    user_fd = open(dbuf->srcname, O_WRONLY);
    record r = create_record(dbuf->srcname, user_fd);

    //insert
    insert_record(head, r);
}

void talk_qq(struct QQ_DATA_INFO *dbuf, record *head)
{
    record p = search_record(head, dbuf->dstname);
    if(p == NULL)
    {
        struct QQ_DATA_INFO offline;
        offline.protocol = 3;
        strcpy(offline.dstname, dbuf->dstname);
        record sp = search_record(head, dbuf->srcname);
        write(sp->user_fifo_fd, &offline, sizeof(offline));
    }else   // client online, then talk
    {
        write(p->user_fifo_fd, dbuf, sizeof(*dbuf));
    }

}

void logout_qq(struct QQ_DATA_INFO *dbuf, record *head)
{
    record p = search_record(head, dbuf->srcname);
    if(p == NULL)
        return;
    close(p->user_fifo_fd); // 777
    delete_record(head, p);
    free_record(p);
}

int main(int argc, char** argv)
{
    int serv_fd;
    struct QQ_DATA_INFO dbuf;

    if(access(SEV_FIFO, F_OK) != 0)
    {
        mkfifo(SEV_FIFO, 0644);
    }
    serv_fd = open(SEV_FIFO, O_RDONLY);
    if(serv_fd == -1)
    {
        sys_err("open serv_fd err");
    }

    server_record_init(&head);

    while(1)
    {
        read(serv_fd, &dbuf, sizeof(dbuf));
        switch(dbuf.protocol)
        {
            case 1: 
                login_qq(&dbuf, &head); 
                printf("record: %s  %d\n", head->username, head->user_fifo_fd);
                break;
            case 2:
                talk_qq(&dbuf, &head);
                break;
            case 4:
                logout_qq(&dbuf, &head);
                //printf("record: %s  %d\n", head->username, head->user_fifo_fd);
                break;
            default: continue;
        }
    }

    unlink(SEV_FIFO);
    close(serv_fd);

    return 0;
}
```
    - server_record.h:
```
#ifndef _SERVER_RECORD_H_
#define _SERVER_RECORD_H_


#define SEV_FIFO "SEV_FIFO"

typedef struct server_record* record;
struct server_record
{
    char username[20];
    int user_fifo_fd;
    record next;
};

// init, create, insert, delete, free, find
void server_record_init(record *head);
record create_record(char* username, int user_fifo_fd);
void insert_record(record *head, record r);
record search_record(record *head, char* keyname);
void delete_record(record *head, record r);
void free_record(record r);
void destroy_link(record *head);
void travel_record(record *head, void (*visit)(record)); // 群发扩展

#endif
```
    - server_record.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <fcntl.h>
#include <errno.h>
#include <sys/stat.h>
#include <sys/types.h>
#include "qq_ipc.h"
#include "server_record.h"

void server_record_init(record *head)
{
    *head = NULL;
}

record create_record(char* username, int user_fifo_fd)
{
    record p = (record)malloc(sizeof(struct server_record));
    strcpy(p->username, username);
    p->user_fifo_fd = user_fifo_fd;
    p->next = NULL;

    return p;
}

void insert_record(record *head, record r)
{
    r->next = *head;
    *head = r;
}

record search_record(record *head, char* keyname)
{
    record p;
    for(p = *head; p != NULL; p = p->next)
    {
        if(strcmp(p->username, keyname) == 0)
        {
            return p;
        }
    }
    return NULL;
}

void delete_record(record *head, record r)
{
    record p;
    if(r == *head)
    {
        *head = r->next;
        return;
    }
    for(p = *head; p != NULL; p = p->next)
    {
        if(p->next == r)
        {
            p->next = r->next;
            return;
        }
    }

}

void free_record(record r)
{
    free(r);
}

void destroy_link(record *head)
{
    record p = *head, q;
    while(p != NULL)
    {
        q = p->next;
        free(p);
        p = q;
    }
    *head = NULL;
}
void travel_record(record *head, void (*visit)(record))
{
    record p;
    for(p = *head; p != NULL; p=p->next)
    {
        visit(p);
    }
}
```

<a id="65-%E4%BF%A1%E5%8F%B7sig%E7%9A%84%E6%A6%82%E5%BF%B5"></a>
#### 65. 信号sig的概念
* signal是软件产生,有一定延时, 而中断是有CPU硬件产生,可靠无延时
* 信号是由kernel内核发送的
* 阻塞信号集mask, 未决信号集pending
* kill函数发送信号要使用宏macro名, 不要使用数字(不同系统中信号编号不一致, 但宏名不会出错)
* kill一组进程: kill -9 -PGID
```
cat | cat | cat | cat | wc -l
ps ajx
kill -9 -3144
```
* alarm: 每个进程都有且只有一个定时器, 定时发信号, 返回剩余时间, 与进程状态无关, 精度为秒second
    - 测试系统1秒钟计数次数 alarm.c
```
int main(int argc, char** argv)
{
    int i;
    alarm(1);
    for(i = 0;;++i)
    {
        printf("%d\n", i);
    }

    return 0;
}
```
```
// 去除屏幕等待时间
// 从I/O入手优化程序
bryan@ubuntu:/Code/signal_test$ time ./alarm > out
Alarm clock

real    0m1.002s
user    0m0.224s
sys     0m0.740s
```
// 10147007 次
* setitimer: 与alarm类似, 精度为微秒us
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>
#include <errno.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <sys/mman.h>
#include <signal.h>
#include <sys/time.h>

/*
int setitimer(int which, const struct itimerval *new_value,
                     struct itimerval *old_value);

struct itimerval {

    struct timeval it_interval; 
    struct timeval it_value;    
};

struct timeval {

    time_t      tv_sec;         
    suseconds_t tv_usec;       
};
*/

//unsigned int alarm(unsigned int seconds);

unsigned int my_alarm(unsigned int seconds)
{
    struct itimerval new_value, old_value;
    int ret;

    new_value.it_interval.tv_sec = 0;
    new_value.it_interval.tv_usec = 0;
    new_value.it_value.tv_sec = seconds;
    new_value.it_value.tv_usec = 0;

    ret = setitimer(ITIMER_REAL, &new_value, &old_value);
    if(ret == -1)
    {
        perror("setitimer err");
        exit(1);
    }
    printf("remain: %d", (int)old_value.it_value.tv_sec);

    return old_value.it_value.tv_sec;

}


int main(int argc, char** argv)
{
    int i;
    time_t remain;

    remain = my_alarm(2);
    printf("remain: %d\n", (int)remain);

    while(1)
    {
        printf("%d\n", i++);
    }

    return 0;
}
```
* setitimer_interval.c:
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>
#include <errno.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <sys/mman.h>
#include <signal.h>
#include <sys/time.h>

/*
int setitimer(int which, const struct itimerval *new_value,
                     struct itimerval *old_value);

struct itimerval {

    struct timeval it_interval; 
    struct timeval it_value;    
};

struct timeval {

    time_t      tv_sec;         
    suseconds_t tv_usec;       
};
*/

void do_sig(int a)
{
    printf("I love you\n");
}

int main(int argc, char** argv)
{
    
    struct itimerval it, old_it;

    it.it_interval.tv_sec = 2;
    it.it_interval.tv_usec = 0;
    it.it_value.tv_sec = 1;
    it.it_value.tv_usec = 0;

    signal(SIGALRM, do_sig);

    if(setitimer(ITIMER_REAL, &it, &old_it) == -1)
    {
        perror("signal err");
        exit(1);
    }

    while(1);

    return 0;
}
```
* sigemptyset, sigaddset, sigprocmask, sigpending, sigismember 测试
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>
#include <errno.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <sys/mman.h>
#include <signal.h>

void print_pending(sigset_t *ped)
{
    int i;
    int ret;

    for(i = 1; i < 32; ++i)
    {
        ret = sigismember(ped, i);
        if(ret == 1)
        {
            putchar('1');
        }else
        {
            putchar('0');
        }
    }
    printf("\n");
}


int main(int argc, char** argv)
{
    sigset_t set, ped;

    sigemptyset(&set);

    sigaddset(&set, SIGINT);
    sigaddset(&set, SIGKILL);
    sigaddset(&set, SIGSTOP);
    sigaddset(&set, SIGTSTP);

    sigprocmask(SIG_BLOCK, &set, NULL);

    while(1)
    {
        sigpending(&ped);
        print_pending(&ped);
        sleep(1);
    }

    return 0;
}
```

<a id="66-typedef-%E7%B1%BB%E5%9E%8B%E5%AE%9A%E4%B9%89"></a>
#### 66. typedef 类型定义
* `void (*sighandler)(int);` sighandler是函数指针(变量, 可赋值)
* `typedef void (*sighandler_t)(int);` sighandler_t是类型

<a id="67-sigaction%E5%87%BD%E6%95%B0"></a>
#### 67. sigaction函数
```
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <errno.h>
#include <signal.h>

/*
   struct sigaction {

   void     (*sa_handler)(int);
   void     (*sa_sigaction)(int, siginfo_t *, void *);
   sigset_t   sa_mask;
   int        sa_flags;
   void     (*sa_restorer)(void);
   };
*/

void do_sig(int a)
{
    printf("Hi, Signal: %d, How do you do?\n", a);
    sleep(10);
    printf("slept 10sec\n");
}

int main(int argc, char** argv)
{
    struct sigaction act;
    
    act.sa_handler = do_sig;
    act.sa_flags = 0;
    sigemptyset(&act.sa_mask);
    sigaddset(&act.sa_mask, SIGQUIT); // 在信号处理函数do_sig执行期间不响应SIGQUIT(ctrl+\)

    sigaction(SIGINT, &act, NULL);
    
    while(1);

    return 0;
}
```

<a id="68-pause%E5%87%BD%E6%95%B0"></a>
#### 68. pause函数
* 使用 pause, alarm, sigaction函数实现 sleep()函数
```
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <errno.h>
#include <signal.h>

/*
   struct sigaction {

   void     (*sa_handler)(int);
   void     (*sa_sigaction)(int, siginfo_t *, void *);
   sigset_t   sa_mask;
   int        sa_flags;
   void     (*sa_restorer)(void);
   };
*/

//unsigned int sleep(unsigned int seconds);

void do_sig(int a)
{

}

unsigned int my_sleep(unsigned int sec)
{

    struct sigaction act, old_act;
    int unslept;

    act.sa_handler = do_sig;
    act.sa_flags = 0;
    sigemptyset(&act.sa_mask);
    //sigaddset(&act.sa_mask, SIGALRM);

    sigaction(SIGALRM, &act, &old_act);

    alarm(sec);
    pause();

    unslept = alarm(0);
    sigaction(SIGALRM, &old_act, NULL);

    return unslept;
}


int main(int argc, char** argv)
{
    while(1)
    {
        my_sleep(2);
        printf("slept 2 seconds\n");
    }

    return 0;
}
```

<a id="69-%E7%88%B6%E8%BF%9B%E7%A8%8B%E5%88%A9%E7%94%A8sigaction%E4%BF%A1%E5%8F%B7%E6%8D%95%E6%8D%89%E5%A4%84%E7%90%86%E5%A4%9A%E4%B8%AA%E5%83%B5%E5%B0%B8%E8%BF%9B%E7%A8%8B"></a>
#### 69. 父进程利用sigaction信号捕捉处理多个僵尸进程
* 可以在父进程中的每1次响应SIG_CHLD的do_sig中通过while循环waitpid的方式回收多个同时死亡的子进程, 从而避免僵尸进程的产生
```
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <errno.h>
#include <signal.h>
#include <sys/wait.h>

void do_sig(int a)
{
    pid_t pid;
    int status;

    while((pid = waitpid(0, &status, WNOHANG)) > 0)
    {
        if(WIFEXITED(status))
        {
            printf("Child pid: %d exited status: %d\n", pid, WEXITSTATUS(status));
        }else if(WIFSIGNALED(status))
        {
            printf("Child pid: %d term signal: %d\n", pid, WTERMSIG(status));
        }
    }
}

int main(int argc, char** argv)
{
    int i;
    pid_t pid;
// 阻塞SIG_CHLD
    for(i = 0; i < 10; ++i)
    {
        pid = fork();
        if(pid == 0)
        {
            break;
        }else if(pid < 0)
        {
            perror("fork err");
            exit(1);
        }
    }


    if(pid == 0)
    {
        printf("child%d pid: %d\n", i, getpid());
        sleep(1);
        if(i == 3)
        {
            alarm(1);
            pause();
        }
        return i+1;
    }else if(pid > 0)
    {
        struct sigaction act;
        
        act.sa_handler = do_sig;
        sigemptyset(&act.sa_mask);
        act.sa_flags = 0;
        sigaction(SIGCHLD, &act, NULL);
//解除阻塞SIG_CHLD
        while(1)
        {
            printf("Parent: %d slept 1 seconds\n", getpid());
            sleep(1);
        }
    }


    return 0;
}
```

<a id="70-sigsuspend%E8%A7%A3%E5%86%B3%E6%97%B6%E5%BA%8F%E7%AB%9E%E6%80%81%E9%97%AE%E9%A2%98"></a>
#### 70. sigsuspend解决时序竞态问题
* sigsuspend的使用场景: 多进程高并发服务器中, 进程在失去cpu时间片的时候仍然可以响应信号并处理
* 使用pause()函数, 会导致在恢复信号集之前程序挂起
```
#include <unistd.h>
#include <signal.h>
#include <stdio.h> 

void handler(int sig)    //信号处理函数的实现
{
    printf("SIGINT sig\n");
}

int main(void)
{
    sigset_t new,old;
    struct sigaction act;

    act.sa_handler = handler;  //信号处理函数handler
    sigemptyset(&act.sa_mask);
    act.sa_flags = 0;
    sigaction(SIGINT, &act, 0);  //准备捕捉SIGINT信号(注册)

    sigemptyset(&new);
    sigaddset(&new, SIGINT);
    sigprocmask(SIG_BLOCK, &new, &old);  //将SIGINT信号阻塞，同时保存当前信号集
    printf("Blocked\n");    //模拟阻塞了SIGINT
    
    pause(); // 因为之前阻塞了SIGINT, 所以SIGINT在挂起期间永远不会递达, 程序将永久挂起

    sigprocmask(SIG_SETMASK, &old, NULL);  //恢复原阻塞信号集
    printf("sigprocmask recovered\n");

    return 0;

}
```

* 使用sigsuspend()函数在程序挂起期间临时撤销对wait信号集中的SIGUSR1以外的信号阻塞(只临时阻塞SIGUSR1), 在挂起期间, 程序可以对除SIGUSR1以外的信号(如SIGINT)进行处理, 执行sigaction
* sigsuspend实际是将临时阻塞SIGUSR1和pause结合起来原子操作
* 临时替换信号集wait和挂起是一个原子操作
* 程序在sigsuspend(&wait);之前和之后对SIGINT(Ctrl+c)信号都不响应, 只有在sigsuspend挂起期间对SIGINT响应并处理
```
#include <unistd.h>
#include <signal.h>
#include <stdio.h> 

void handler(int sig)    //信号处理函数的实现
{
    printf("SIGINT sig\n");
}

int main(void)
{
    sigset_t new,old, wait;
    struct sigaction act;

    act.sa_handler = handler;  //信号处理函数handler
    sigemptyset(&act.sa_mask);
    act.sa_flags = 0;
    sigaction(SIGINT, &act, 0);  //准备捕捉SIGINT信号

    sigemptyset(&new);
    sigaddset(&new, SIGINT);

    //sigprocmask作用: 改变当前进程的阻塞信号集
    sigprocmask(SIG_BLOCK, &new, &old);  //将SIGINT信号阻塞(模拟Ctrl+c失效)，同时保存当前信号集

    printf("Blocked\n");    // 模拟阻塞SIGINT(Ctrl+c), 模拟失去cpu

    sigemptyset(&wait);
    sigaddset(&wait, SIGUSR1);

    //pause(); 是挂起期间对所有信号都响应

    // 在程序挂起期间对wait(SIGUSR1)临时不响应, 而响应其他所有信号,如SIGINT, 挂起结束后恢复. 
    // 程序挂起期间可以不想被SIGUSR1打断
    // 挂起期间可以用SIGINT(Ctrl+c)唤醒进程
    sigsuspend(&wait);  

    sigprocmask(SIG_SETMASK, &old, NULL);  //恢复原阻塞信号集
    printf("sigprocmask recovered\n");

    return 0;

}
```

<a id="71-%E7%88%B6%E5%AD%90%E8%BF%9B%E7%A8%8B%E4%BA%A4%E6%9B%BF%E6%95%B0%E6%95%B0"></a>
#### 71. 父子进程交替数数
* 使用全局变量flag, 出现时序竞态问题
* 出现问题的原因(流程): 
    - 原本正常情况: 父子进程都是 响应sigaction->处理sa_handler->发信号kill
    - 但当父进程在发完信号kill后失去了cpu(挂起了/卡住了), 导致子进程响应处理发信号后, 父进程才恢复cpu, 此时父进程又响应了一次子进程发来的信号, 并进行处理, 然后flag=0, 之后由于子进程已经发完信号, 导致父进程再没有可响应的信号了, 父进程的flag永远变成了0, 而无法进入if条件判断, 也无法再发kill信号给子进程了, 最终程序无法继续运行 
```
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
#include <stdlib.h>

int n = 0;
int flag = 0; // 读时共享, 写时复制, 父子进程的flag不同

void sys_err(char *str)
{
    perror(str);
    exit(1);
}

void do_sig_parent(int sig)
{
    printf("Parent count: %d\n", n);
    n += 2;
    flag = 1;
    //sleep(1);
}
void do_sig_child(int sig)
{
    printf("Child count: %d\n", n);
    n += 2;
    flag = 1;
    //sleep(1);
}

int main(void)
{
    pid_t pid;
    struct sigaction act;

    pid = fork();
    if(pid < 0)
    {
        sys_err("fork err");
    }

    if(pid > 0)
    {
        sleep(1);
        n = 1;
        act.sa_handler = do_sig_parent;
        sigemptyset(&act.sa_mask);
        act.sa_flags = 0;
        sigaction(SIGUSR1, &act, NULL);

        do_sig_parent(0);

        while(1)
        {
            if(flag == 1)
            {
                kill(pid, SIGUSR2);
                // ... 失去cpu
                flag = 0;
            }

        }

    }else if(pid == 0)
    {
        n = 2;
        act.sa_handler = do_sig_child;
        sigemptyset(&act.sa_mask);
        act.sa_flags = 0;
        sigaction(SIGUSR2, &act, NULL);

        while(1)
        {
            if(flag == 1)
            {
                kill(getppid(), SIGUSR1);
                flag = 0;
            }
        }
    }


    return 0;
}
```
* 使用信号阻塞sigprocmask解决全局变量时序竞态问题 alternate_count2.c:
```
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
#include <stdlib.h>

int num1 = 0;
int num2 = 1;
pid_t pid;

void sys_err(char *str)
{
    perror(str);
    exit(1);
}

void do_sig_parent(int sig)
{
    printf("Parent\t count: %d\n", num2);
    num2 += 2;
    kill(pid, SIGUSR2);
}
void do_sig_child(int sig)
{
    printf("Child\t count: %d\n", num1);
    num1 += 2;
    kill(getppid(), SIGUSR1);
}

int main(void)
{
    struct sigaction act1, act2;
    sigset_t set;
    sigemptyset(&set);
    sigaddset(&set, SIGUSR2);
    sigprocmask(SIG_BLOCK, &set, NULL);

    pid = fork();

    if(pid < 0)
    {
        sys_err("fork err");
    }

    if(pid > 0)
    {
        sleep(1);
        
        act1.sa_handler = do_sig_parent;
        sigemptyset(&act1.sa_mask);
        act1.sa_flags = 0;
        sigaction(SIGUSR1, &act1, NULL);

        kill(pid, SIGUSR2); // begin
        
        while(1);

    }else if(pid == 0)
    {
        act2.sa_handler = do_sig_child;
        sigemptyset(&act2.sa_mask);
        act2.sa_flags = 0;
        sigaction(SIGUSR2, &act2, NULL);
        
        sigprocmask(SIG_UNBLOCK, &set, NULL);

        while(1);
    }

    return 0;
}
```

<a id="72-%E5%88%9B%E5%BB%BAsession%E4%BC%9A%E8%AF%9D%E4%BD%9C%E4%B8%9A"></a>
#### 72. 创建session会话/作业
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>
#include <errno.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <sys/mman.h>
#include <sys/wait.h>
#include <signal.h>

int main(int argc, char** argv)
{
    pid_t pid = fork();

    if(pid == 0)
    {
        printf("Child getpid: %d\n", getpid());
        printf("Child getpgid: %d\n", getpgid(0));
        printf("Child getsid: %d\n", getsid(0));
        sleep(2);
        
        setsid();

        printf("Child getpid: %d\n", getpid());
        printf("Child getpgid: %d\n", getpgid(0));
        printf("Child getsid: %d\n", getsid(0));

        exit(0);
    }

    return 0;
}
```

<a id="73-%E5%88%9B%E5%BB%BAdaemon%E5%AE%88%E6%8A%A4%E8%BF%9B%E7%A8%8B"></a>
#### 73. 创建daemon守护进程
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>
#include <errno.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <sys/mman.h>
#include <sys/wait.h>
#include <signal.h>
#include <time.h>

void mydaemon(void)
{
    pid_t pid = fork();
    if(pid < 0)
    {
        perror("fork err");
        exit(1);
    }
    if(pid > 0)
    {
        exit(0);
    }

    pid = setsid();
    if(pid < 0)
    {
        perror("setsid err");
        exit(1);
    }

    if(chdir("/") < 0)
    {
        perror("chdir err");
        exit(1);
    }

    umask(0022);
    
    close(0);
    int fd = open("/dev/null", O_RDWR);
    if(fd == -1)
    {
        perror("open err");
        exit(1);
    }
    dup2(fd, STDOUT_FILENO);
    dup2(fd, STDERR_FILENO);

    return;

}


int main(int argc, char** argv)
{
    mydaemon();
    
    int fd = open("/tmp/mydaemon_time", O_RDWR | O_TRUNC | O_CREAT, 0644);
    if(fd == -1)
    {
        perror("open tmp err");
        exit(1);
    }
    while(1)
    {
        time_t ct = time(NULL);
        dprintf(fd, "%s\n", ctime(&ct));
        sleep(5);
    }
    close(fd);

    return 0;
}
```

<a id="74-%E7%BA%BF%E7%A8%8B%E7%9A%84%E6%A6%82%E5%BF%B5"></a>
#### 74. 线程的概念
* 线程本质还是进程
* 不要在多线程的程序中使用信号signal
* `ps -Lf pid` : 查看进程下的线程
* 线程是最小的执行单位, 每个线程独立一个stack栈空间
* 线程共享数据方便
* 进程间不共享全局变量, 只能通过pipe/fifo/file/mmap/signal通信, 不如线程方便
* 线程id和线程号不同, 线程id是进程内部使用的, 线程号是不同进程之间使用
* 查看线程库版本NPTL(Native POSIX Thread Library): `getconf GNU_LIBPTHREAD_VERSION`

<a id="75-%E5%88%9B%E5%BB%BA%E7%BA%BF%E7%A8%8B"></a>
#### 75. 创建线程
* `int pthread_create(pthread_t *thread, const pthread_attr_t *attr,
                          void *(*start_routine) (void *), void *arg);`
    - thread: 线程id(传出参数, 即创建的线程id)
    - attr: 属性, 一般传NULL
    - start_routine: 函数指针, 线程需要执行的主控函数名(操作)
    - arg: 函数指针start_routine 的参数
* makefile: -lpthread
```
src = $(wildcard *.c)
target = $(patsubst %.c, %, $(src))

ALL: $(target)
CFLAGES = -Wall -g -lpthread

$(target):%:%.c
    gcc $^ -o $@ $(CFLAGES)
clean:
    -rm -rf $(target)
.PHONY:clean ALL
```
* 标准输出行缓冲, 标准错误无缓冲
* pthread_create.c:
```
#include <stdio.h>
#include <stdlib.h>

#include <pthread.h>


void* tfn(void* arg)
{
    printf("tfn ---- pid: %d  tid: %lu\n", getpid(), pthread_self());
    
    return (void*)0;
}

int main(int argc, char** argv)
{
    pthread_t tid;

    printf("main ---- pid: %d  tid: %lu\n", getpid(), pthread_self());

    int ret = pthread_create(&tid, NULL, tfn, NULL);
    if(ret != 0)
    {
        fprintf(stderr, "pthread create err: %s\n", strerror(ret));
        exit(1);
    }
    sleep(1);

    return 0;
}
```
* more_pthread.c:
    - `(void *)i` : 将i的值赋值给void * 指针, 有warning, 但此处__不能__用地址传参`(void *)&i`
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

#include <pthread.h>


void* tfn(void* arg)
{
    //int i = *((int*)arg);
    int i;
    i = (int)arg;
    sleep(i);
    printf("Num: %d tfn ---- pid: %d  tid: %lu\n", i + 1, getpid(), pthread_self());

    return NULL;
}

int main(int argc, char** argv)
{
    pthread_t tid;
    int i, ret;



    for(i = 0; i < 5; ++i)
    {
        ret = pthread_create(&tid, NULL, tfn, (void *)i);   // 将i的值赋值给void * 指针, 有warning
        if(ret != 0)
        {
            fprintf(stderr, "pthread create err: %s\n", strerror(ret));
            exit(1);
        }
    }


    sleep(i);
    printf("main ---- pid: %d  tid: %lu\n", getpid(), pthread_self());

    return 0;
}
```

<a id="76-%E9%80%80%E5%87%BA%E5%8D%95%E4%B8%AA%E7%BA%BF%E7%A8%8Bpthread_exitnull"></a>
#### 76. 退出单个线程pthread_exit(NULL)
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>
#include <errno.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <sys/mman.h>
#include <sys/wait.h>
#include <signal.h>
#include <pthread.h>


void func(void)
{
    pthread_exit(NULL);
}


void* tfn(void* arg)
{
    //int i = *((int*)arg);
    int i;
    i = (int)arg;
    if(i == 2)
    {
        func();
    }
    sleep(i);
    printf("Num: %d tfn ---- pid: %d  tid: %lu\n", i + 1, getpid(), pthread_self());

    return NULL;
}

int main(int argc, char** argv)
{
    pthread_t tid;
    int i, ret;

    for(i = 0; i < 5; ++i)
    {
        ret = pthread_create(&tid, NULL, tfn, (void*)i);
        if(ret != 0)
        {
            fprintf(stderr, "pthread create err: %s\n", strerror(ret));
            exit(1);
        }
    }

    printf("main ---- pid: %d  tid: %lu\n", getpid(), pthread_self());

    // return 0; // exit();
    pthread_exit(NULL);
}
```

<a id="77-pthread_join%E9%98%BB%E5%A1%9E%E7%AD%89%E5%BE%85%E7%BA%BF%E7%A8%8B%E9%80%80%E5%87%BA-%E8%8E%B7%E5%8F%96%E7%BA%BF%E7%A8%8B%E9%80%80%E5%87%BA%E7%8A%B6%E6%80%81"></a>
#### 77. pthread_join阻塞等待线程退出, 获取线程退出状态
* 回收单线程
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

#include <pthread.h>

//存放:线程执行结果集
typedef struct
{
    int var;
    char str[64];
}exit_t;


void* tfn(void* arg)
{
    exit_t *ret = (exit_t *)arg;
    ret->var = 77;
    strcpy(ret->str, "hello pthread_join");

    //return (void *)ret;
    pthread_exit((void *)ret);
}

int main(int argc, char** argv)
{
    pthread_t tid;
    exit_t *ret = malloc(sizeof(exit_t));
    
    pthread_create(&tid, NULL, tfn, (void *)ret);
    pthread_join(tid, (void **)&ret);

    printf("ret var=%d  str=%s\n", ret->var, ret->str);

    free(ret);

    return 0;
}
```
* 循环回收多线程
    - 同一进程下的所有线程共享全局变量
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

#include <pthread.h>

int g_var = 100;

typedef struct
{
    int var;
    char str[64];
}exit_t;


void *tfn(void *arg)
{
    exit_t *ret = (exit_t *)arg;
    sleep(ret->var);

    if(ret->var == 1)
    {
        strcpy(ret->str, "hello pthread 2");
        g_var = 222;
        printf("g_var: %d\n", g_var);
        pthread_exit((void *)ret);
    }
    else if(ret->var == 3)
    {
        strcpy(ret->str, "hello pthread 4");
        g_var = 444;
        printf("g_var: %d\n", g_var);
        pthread_exit((void *)ret);
    }
    printf("g_var: %d\n", g_var);
    strcpy(ret->str, "hello pthread");

    pthread_exit((void *)ret);
}

int main(int argc, char** argv)
{
    int i;
    pthread_t tid[5];
    exit_t *ret[5];

    for(i=0;i<5;++i)
    {
        ret[i] = malloc(sizeof(exit_t));
    }


    for(i = 0;i<5;++i)
    {
        ret[i]->var = i;
        pthread_create(&tid[i], NULL, tfn, (void *)ret[i]);
    }

    for(i = 0;i<5;++i)
    {
        pthread_join(tid[i], (void **)&ret[i]);
        printf("ret var=%d  str=%s\n", ret[i]->var, ret[i]->str);
    }

    for(i=0;i<5;++i)
    {
        free(ret[i]);
    }

    //return 0;
    pthread_exit(NULL);
}
```

<a id="78-pthread_detach-%E5%88%86%E7%A6%BB%E5%8D%95%E4%B8%AA%E7%BA%BF%E7%A8%8B"></a>
#### 78. pthread_detach 分离单个线程
* 查看进程下的线程 `ps -T -p <pid>`
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

#include <pthread.h>


void *tfn(void *arg)
{
    printf("tfn ---- pid: %d  tid: %lu\n", getpid(), pthread_self());

    pthread_exit(NULL);
}

int main(int argc, char** argv)
{
    pthread_t tid;
    void *val;
    int ret;

    pthread_create(&tid, NULL, tfn, NULL);
    pthread_detach(tid);

    ret = pthread_join(tid,(void **)&val);

    if(ret != 0)
    {
        fprintf(stderr, "pthread join err: %s\n", strerror(ret));
        exit(1);
    }


    pthread_exit(NULL);
}
```

<a id="79-pthread_cancel%E6%9D%80%E6%AD%BB%E4%B8%80%E4%B8%AA%E7%BA%BF%E7%A8%8B"></a>
#### 79. pthread_cancel杀死一个线程
* pthread_cancel: 以一个系统调用(alarm/pause/...printf/sleep)为取消点, 到达取消点才能把线程杀死, 如果没有系统调用(如空的while循环), 则无法杀死线程
* pthread_testcancel: 杀死无系统调用的线程
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>


void *tfn(void *arg)
{
    //while(1); // 无法杀死线程的原因
    while(1)
    {

        pthread_testcancel(); // 杀死无系统调用的线程
    }

    printf("tfn ---- pid: %d  tid: %lu\n", getpid(), pthread_self());
    sleep(2);
    return (void *)888;
}

int main(int argc, char** argv)
{
    pthread_t tid;
    void *val;
    int ret;

    pthread_create(&tid, NULL, tfn, NULL);
    pthread_cancel(tid);
    ret = pthread_join(tid,(void **)&val);
    if(ret != 0)
    {
        fprintf(stderr, "pthread join err: %s\n", strerror(ret));
        exit(1);
    }
    printf("pthread exit with: %d\n", (int)val);

    pthread_exit(NULL);
}
```

<a id="80-%E8%AE%BE%E7%BD%AE%E7%BA%BF%E7%A8%8B%E5%B1%9E%E6%80%A7pthread_attr"></a>
#### 80. 设置线程属性pthread_attr
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>


void *tfn(void *arg)
{
    int n = 3;
    while(n--)
    {
        printf("pthread n: %d\n", n);
        sleep(1);
    }
    return (void *)0;
}

int main(int argc, char** argv)
{
    pthread_t tid;
    void *val;
    int ret;
#if 1
    pthread_attr_t attr;
    pthread_attr_init(&attr);
    pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_DETACHED);
    pthread_create(&tid, &attr, tfn, NULL);
    pthread_attr_destroy(&attr);

#else
    pthread_create(&tid, NULL, tfn, NULL);
    pthread_detach(tid);
#endif
    
    ret = pthread_join(tid,(void **)&val);

    if(ret != 0)
    {
        fprintf(stderr, "pthread join err: %s\n", strerror(ret));
        exit(1);
    }else
    {
        printf("pthread exit with: %d\n", (int)val);
    }


    pthread_exit(NULL);
}
```

<a id="81-%E7%BA%BF%E7%A8%8B%E5%90%8C%E6%AD%A5%E7%9A%84%E6%A6%82%E5%BF%B5"></a>
#### 81. 线程同步的概念
* 线程同步是指线程间协同工作(线程安全)
* 多个线程/进程,共同操作一个共享资源的情况,都需要同步(协调工作), 避免混乱
* 建议锁对全局变量无效

<a id="82-%E5%A4%9A%E7%BA%BF%E7%A8%8B%E6%8B%B7%E8%B4%9D%E5%A4%A7%E6%96%87%E4%BB%B6-%E5%B9%B6%E6%98%BE%E7%A4%BA%E6%8B%B7%E8%B4%9D%E8%BF%9B%E5%BA%A6"></a>
#### 82. 多线程拷贝大文件, 并显示拷贝进度
* multi_pthread_cp.c: (5个步骤实现)
    - 进度条使用pthread_mutex
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>
#include <pthread.h>
#include <sys/stat.h>
#include <sys/mman.h>

#define EQ_NUM 50

unsigned long long complish = 0;
char *mp_src, *mp_dst, *tmp_srcp, *tmp_dstp;
pthread_mutex_t lock;

typedef struct
{
    int off_set;
    int size;
    int t_no;
}arg_t;


void err_int(int ret, const char *str)
{
    if(ret == -1)
    {
        perror(str);
        exit(1);
    }
    return;
}
void err_str(char *ret, const char *str)
{
    if(ret == MAP_FAILED)
    {
        perror(str);
        exit(1);
    }
    return;
}

void *tfn(void *arg)
{
    arg_t *arg_p = (arg_t *)arg;
    memcpy(tmp_dstp + arg_p->off_set, tmp_srcp + arg_p->off_set, arg_p->size);
    
    pthread_mutex_lock(&lock);
    complish += (unsigned long long)arg_p->size;
    pthread_mutex_unlock(&lock);

    return (void *)0;
}

void *draw_progress(void *file_size)
{
    int bytes_per_eq = *((int *)file_size) / EQ_NUM;
    int draw = 0;
    int should_draw_num = 0;
    while(draw < EQ_NUM)
    {
        should_draw_num = complish / bytes_per_eq;
        for(;draw < should_draw_num;++draw)
        {
            putchar('=');
            fflush(stdout);
        }
    }
    putchar('\n');
    return NULL;
}

int main(int argc, char** argv)
{
    int i;
    int src_fd, dst_fd;
    int t_num;
    int ret;
    struct stat sbuf;
    int file_size;
    pthread_t *tid;
    int thrd_cp_size;

    if(argc != 4)
    {
        printf("usage: %s src dst pthreadNum\n", argv[0]);
        exit(1);
    }
    
    src_fd = open(argv[1], O_RDONLY);
    err_int(src_fd, "open src err");

    dst_fd = open(argv[2], O_RDWR | O_TRUNC | O_CREAT, 0644);
    err_int(dst_fd, "open dst err");
    
    t_num = atoi(argv[3]);

    ret = fstat(src_fd, &sbuf);
    err_int(ret, "fstat err");

    file_size = sbuf.st_size;

    ret = ftruncate(dst_fd, file_size);
    err_int(ret, "ftruncate err");

    mp_src = mmap(NULL, file_size, PROT_READ, MAP_SHARED, src_fd, 0);
    err_str(mp_src, "mmap src err");

    mp_dst = mmap(NULL, file_size, PROT_READ | PROT_WRITE, MAP_SHARED, dst_fd, 0);
    err_str(mp_dst, "mmap dst err");

    tmp_srcp = mp_src;
    tmp_dstp = mp_dst;

    close(src_fd);
    close(dst_fd);
    
    thrd_cp_size = file_size / t_num;
    
    arg_t *arg_arr;
    arg_arr = (arg_t *)malloc(sizeof(arg_t) * t_num);
    for(i = 0; i < t_num; ++i)
    {
        arg_arr[i].off_set = i * thrd_cp_size;
        arg_arr[i].size = thrd_cp_size;
        arg_arr[i].t_no = i;
    }
    arg_arr[t_num - 1].size += file_size % t_num;

    pthread_t draw_tid;
    pthread_create(&draw_tid, NULL, draw_progress, (void *)&file_size);

    tid = (pthread_t *)malloc(sizeof(pthread_t) * t_num);
    for(i = 0; i < t_num; ++i)
    {
        //usleep(10000); // slow copy speed
        pthread_create(&tid[i], NULL, tfn, (void *)&arg_arr[i]);
    }


    for(i = 0; i < t_num; ++i)
    {
        pthread_join(tid[i], NULL);
    }
    pthread_join(draw_tid, NULL);

    munmap(mp_src, file_size);
    munmap(mp_dst, file_size);

    free(arg_arr);
    free(tid);

    pthread_mutex_destroy(&lock);

    pthread_exit(NULL);
}
```

<a id="83-%E7%BA%BF%E7%A8%8B%E6%AD%BB%E9%94%81"></a>
#### 83. 线程死锁
* 线程死锁不是一下就能锁住的, 而是trylock一段时间或次数后, 锁如果仍然存在, 才会真正死锁住, 所以在pthread_mutex_destroy前 sleep(3)秒
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>

int m, n;
pthread_mutex_t m_lock, n_lock;

void *tfn1(void *arg)
{
    pthread_mutex_lock(&m_lock);
    m++;
    sleep(1);
    pthread_mutex_lock(&n_lock);
    n++;

    pthread_mutex_unlock(&m_lock);
    pthread_mutex_unlock(&n_lock);

    pthread_exit(NULL);
}
void *tfn2(void *arg)
{
    pthread_mutex_lock(&n_lock);
    n++;
    sleep(1);
    pthread_mutex_lock(&m_lock);
    m++;

    pthread_mutex_unlock(&n_lock);
    pthread_mutex_unlock(&m_lock);

    pthread_exit(NULL);
}


int main(int argc, char** argv)
{
    pthread_t tid1, tid2;

    pthread_mutex_init(&m_lock, NULL);
    pthread_mutex_init(&n_lock, NULL);

    
    pthread_create(&tid1, NULL, tfn1, NULL);
    pthread_create(&tid2, NULL, tfn2, NULL);
    
    sleep(3); // keypoint 给线程加锁创建时间
    printf("m: %d  n: %d\n", m, n);
    
    pthread_mutex_destroy(&m_lock);
    pthread_mutex_destroy(&n_lock);

    pthread_join(tid1, NULL);
    pthread_join(tid2, NULL);

    printf("m: %d  n: %d\n", m, n);

    pthread_exit(NULL);
}
```

<a id="84-%E8%AF%BB%E5%86%99%E9%94%81"></a>
#### 84. 读写锁
* 读共享, 写独占
* 读写锁并行时, 写锁优先级高
* 既有读又有写时, 读写锁性能比互斥要高, 只有写时, 读写锁与互斥性能一样
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <pthread.h>

int counter;
pthread_rwlock_t rwlock;


void *tfn_write(void *arg)
{
    int i = (int)arg;
    int t;

    while(1)
    {
        pthread_rwlock_wrlock(&rwlock);
        usleep(1000);
        t = counter;
        printf("write:\t%d\t tid: %lu\t counter: %d\t ++counter: %d\n", i, pthread_self(), t, ++counter);
        pthread_rwlock_unlock(&rwlock);
        usleep(10000);
    }
    return NULL;
}
void *tfn_read(void *arg)
{
    int i = (int)arg;

    while(1)
    {
        pthread_rwlock_rdlock(&rwlock);
        printf("read:\t%d\t tid: %lu\t counter: %d\t\n", i, pthread_self(), counter);
        pthread_rwlock_unlock(&rwlock);
        usleep(2000);
    }

    return NULL;
}

int main(int argc, char** argv)
{   
    int i;
    pthread_t tid[8];

    pthread_rwlock_init(&rwlock, NULL);

    for(i = 0; i < 3; ++i)
    {
        pthread_create(&tid[i], NULL, tfn_write, (void *)i);
    }
    for(i = 0; i < 5; ++i)
    {
        pthread_create(&tid[i+3], NULL, tfn_read, (void *)i);
    }
    for(i = 0; i < 8; ++i)
    {
        pthread_join(tid[i], NULL);
    }

    pthread_rwlock_destroy(&rwlock);

    return 0;
}
```

<a id="85-%E7%94%9F%E4%BA%A7%E8%80%85%E6%B6%88%E8%B4%B9%E8%80%85-pthread_cond"></a>
#### 85. 生产者消费者-pthread_cond
* 作用: 有了条件变量机制以后，只有生产者完成生产，才会引起消费者之间的竞争。提高了程序效率;
* pthread_cond_wait的任务: 阻塞, 解锁, 唤醒其他抢锁线程, 条件满足pthread_cond_signal唤醒后加锁继续执行
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <pthread.h>

pthread_cond_t has_product = PTHREAD_COND_INITIALIZER;
pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;

struct product 
{
    int num;
    struct product *next;
};
struct product *head;   // shared link data

void *consumer(void *arg)
{
    struct product *del;
    for(;;)
    {
        pthread_mutex_lock(&lock);  //先抢锁
        while(head == NULL)
        {
            pthread_cond_wait(&has_product, &lock);
        }
        del = head;
        head = del->next;
        pthread_mutex_unlock(&lock);

        printf("Consumer: %lu\t del: %d\n", pthread_self(), del->num);
        free(del);
        sleep(rand() % 4);
    }
}

void *producer(void *arg)
{
    struct product *add;
    for(;;)
    {
        add = (struct product*)malloc(sizeof(struct product));
        add->num = rand() % 1000 + 1;
        printf("Producer: %lu\t add: %d\n", pthread_self(), add->num);

        pthread_mutex_lock(&lock);
        add->next = head;
        head = add;
        pthread_mutex_unlock(&lock);

        pthread_cond_signal(&has_product);

        sleep(rand() % 4);
    }
}

int main(int argc, char** argv)
{
    pthread_t pid, cid; // pid: product tid ; cid: consumer tid
    srand(time(NULL));

    pthread_create(&pid, NULL, producer, NULL);
    
    pthread_create(&cid, NULL, consumer, NULL);
    pthread_create(&cid, NULL, consumer, NULL);
    pthread_create(&cid, NULL, consumer, NULL);
    pthread_create(&cid, NULL, consumer, NULL);


    pthread_join(cid, NULL);
    pthread_join(pid, NULL);

    return 0;
}
```

<a id="86-semaphore%E6%97%97%E8%AF%AD%E4%BF%A1%E5%8F%B7%E9%87%8F"></a>
#### 86. semaphore旗语(信号量)
* 作用: 控制对共享资源进行竞争的进程/线程数量
* 进程/线程 都可以使用
* 生产者消费者-semaphore
* wait:--   post:++
* sem_wait到blank_num--为0时才会阻塞
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <pthread.h>
#include <semaphore.h>

#define NUM 5

sem_t blank_num, product_num;
int queue[NUM];
int idx;
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;

void *consumer(void *arg)
{
    //int i = 0;
    while(1)
    {
        sem_wait(&product_num);
        printf("Consume: %lu\t product: %d\n", pthread_self(), queue[idx]);
        queue[idx] = 0;
        sem_post(&blank_num);

        pthread_mutex_lock(&mutex);
        idx = (idx + 1) % NUM;
        pthread_mutex_unlock(&mutex);

        sleep(rand()%1);
    }
    return NULL;
}

void *producer(void *arg)
{
    int i = 0;
    while(1)
    {   
        sem_wait(&blank_num);
        queue[i] = rand() % 1000 + 1;
        printf("Produce: \t%d\n", queue[i]);
        sem_post(&product_num);
        i = (i + 1) % NUM;

        sleep(rand()%1);
    }
    return NULL;
}

int main(int argc, char** argv)
{
    pthread_t pid, cid; // pid: product tid ; cid: consumer tid
    srand(time(NULL));

    sem_init(&blank_num, 0, NUM);
    sem_init(&product_num, 0, 0);

    pthread_create(&pid, NULL, producer, NULL);
    
    pthread_create(&cid, NULL, consumer, NULL);
    pthread_create(&cid, NULL, consumer, NULL);
    pthread_create(&cid, NULL, consumer, NULL);
    pthread_create(&cid, NULL, consumer, NULL);


    pthread_join(cid, NULL);
    pthread_join(pid, NULL);

    sem_destroy(&blank_num);
    sem_destroy(&product_num);

    return 0;
}

```

<a id="87-%E8%BF%9B%E7%A8%8B%E9%97%B4%E5%90%8C%E6%AD%A5pthreadmutexattrt"></a>
#### 87. 进程间同步pthread_mutexattr_t
* 进程间同步操作共享区主要用mmap(pipe/fifo是伪文件)
* 一个共享资源(变量/数组/链表/struct)就有一把对应的锁mutex, 所以可以把锁mutex和锁属性与一个共享数据资源封装到一个struct中
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/mman.h>
#include <sys/wait.h>
#include <pthread.h>

struct mt
{
    int num;
    pthread_mutexattr_t mutexattr;
    pthread_mutex_t mutex;
};

int main(int argc, char** argv)
{
    pid_t pid;
    struct mt *mm;
    int i;

    mm = mmap(NULL, sizeof(*mm), PROT_READ | PROT_WRITE, MAP_SHARED | MAP_ANON, -1, 0);
    memset(mm, 0, sizeof(*mm));

    pthread_mutexattr_init(&mm->mutexattr);
    pthread_mutexattr_setpshared(&mm->mutexattr, PTHREAD_PROCESS_SHARED);
    pthread_mutex_init(&mm->mutex, &mm->mutexattr);

    pid = fork();
    if(pid == 0)
    {
        // child process
        for(i = 0; i < 10; ++i)
        {
            pthread_mutex_lock(&mm->mutex);
            mm->num += 1;
            printf("Child: \tnum: %d\n", mm->num);
            pthread_mutex_unlock(&mm->mutex);
            sleep(1);
        }
    }else if(pid > 0)
    {
        //parent process
        for(i = 0; i < 10; ++i)
        {
            sleep(1);
            pthread_mutex_lock(&mm->mutex);
            mm->num += 2;
            printf("Parent: \tnum: %d\n", mm->num);
            pthread_mutex_unlock(&mm->mutex);
        }
        wait(NULL);
    }


    pthread_mutexattr_destroy(&mm->mutexattr);
    pthread_mutex_destroy(&mm->mutex);

    munmap(mm, sizeof(*mm));
    
    return 0;
}
```

<a id="88-%E8%BF%9B%E7%A8%8B%E9%97%B4%E6%96%87%E4%BB%B6%E9%94%81fcntlflock"></a>
#### 88. 进程间文件锁fcntl/flock
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>

void sys_err(const char* err)
{
    perror(err);
    exit(1);
}

int main(int argc, char** argv)
{
    struct flock f_lock;
    int fd;

    if(argc < 2)
    {
        printf("usage: %s filename\n", argv[0]);
        exit(1);
    }

    fd = open(argv[1], O_RDWR);
    if(fd == -1)
    {
        sys_err("open err");
    }
    
    //f_lock.l_type = F_RDLCK;
    f_lock.l_type = F_WRLCK;

    f_lock.l_whence = SEEK_SET;
    f_lock.l_start = 0;
    f_lock.l_len = 0;

    fcntl(fd, F_SETLKW, &f_lock);
    printf("get lock\n");
    sleep(10);

    f_lock.l_type = F_UNLCK;
    fcntl(fd, F_SETLKW, &f_lock);
    printf("un flock\n");

    close(fd);

    return 0;
}
```

<a id="89-%E5%93%B2%E5%AD%A6%E5%AE%B6%E5%90%83%E9%A5%AD%E9%97%AE%E9%A2%98"></a>
#### 89. 哲学家吃饭问题
* 线程实现
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>

pthread_mutex_t m[5]; // 5 chopstick

void *tfn(void *arg)
{
    int i, l, r;
    srand(time(NULL));
    i = (int)arg;

    if(i == 4)
    {
        l = 0;  // 避免极端震荡现象(5个人同时拿, 没拿到, 同时放), 4号哲学家E拿筷子方向与别人相反即可
        r = i;
    }else
    {
        l = i;
        r = i + 1;
    }

    while(1)
    {
        pthread_mutex_lock(&m[l]);
        if(pthread_mutex_trylock(&m[r]) == 0)
        {
            printf("philosopher: %c eating\n", i+'A');
            pthread_mutex_unlock(&m[r]);
        }
        pthread_mutex_unlock(&m[l]);
        sleep(rand()%5);
    }
    return NULL;
}


int main(int argc, char** argv)
{
    pthread_t tid[5]; // 5 philosopher
    int i;

    for(i=0;i<5;++i)
    {
        pthread_mutex_init(&m[i], NULL);
    }

    for(i=0;i<5;++i)
    {
        pthread_create(&tid[i], NULL, tfn, (void *)i);
    }

    for(i=0;i<5;++i)
    {
        pthread_join(tid[i], NULL);
    }
    
    for(i=0;i<5;++i)
    {
        pthread_mutex_destroy(&m[i]);
    }
    
    return 0;
}
```
* 进程实现
    - semaphore变成互斥mutex: sem_init(&s[i], 1, 1);  //信号量初值制定为1，信号量，变成了互斥锁
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <time.h>
#include <sys/mman.h>
#include <sys/wait.h>
#include <semaphore.h>

int main(int argc, char** argv)
{   
    int i;
    sem_t *s;
    pid_t pid;

    s = mmap(NULL, sizeof(sem_t)*5, PROT_READ | PROT_WRITE, MAP_SHARED | MAP_ANON, -1, 0);
    if(s == MAP_FAILED)
    {
        perror("mmap err");
        exit(1);
    }

    for(i=0;i<5;++i)
    {
        sem_init(&s[i], 1, 1);
    }

    for(i=0;i<5;++i)
    {
        pid = fork();
        if(pid == 0)
            break;
    }

    if(i<5)
    {
        int l, r;
        srand(time(NULL));

        if(i == 4)
        {
            l = 0, r = i;
        }else
        {
            l = i, r = i + 1;
        }
        
        while(1)
        {
            sem_wait(&s[l]);
            if(sem_trywait(&s[r]) == 0)
            {
                printf("Philosopher: %c eating\n", i + 'A');
                sem_post(&s[r]);
            }
            sem_post(&s[l]);
            sleep(rand() % 5);
        }
        exit(0);
    }


    for(i=0;i<5;++i)
    {
        wait(NULL);
    }
    for(i=0;i<5;++i)
    {
        sem_destroy(&s[i]);
    }
    munmap(s, sizeof(sem_t)*5);

    return 0;
}
```

<a id="90-sem_timedwait"></a>
#### 90. sem_timedwait
* 共享资源: STDIN_FILENO
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <semaphore.h>
#include <pthread.h>
#include <time.h>

#define N 1024

sem_t s;
char buf[N];

void *tfn(void *arg)
{

    while(1)
    {
        read(STDIN_FILENO, buf, N);
        sem_post(&s);
    }
    return NULL;
}


int main(void)
{
    pthread_t tid;
    struct timespec t = {0, 0};

    sem_init(&s, 0, 0);
    pthread_create(&tid, NULL, tfn, NULL);

    while(1)
    {
        sem_timedwait(&s, &t);
        printf("hello world: %s\n", buf);
        t.tv_sec = time(NULL) + 5;
        t.tv_nsec = 0;
    }

    pthread_join(tid, NULL);
    sem_destroy(&s);

    return 0;
}
```