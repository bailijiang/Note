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


#### 3. 