#### 1. vim
* 分屏创建新文件   vsp: head.h 
* 切换分屏: ctrl+w+w 
* 多行缩进: gg + = + G
* 撤销: u
* 恢复(上一次撤销): Ctrl+r 

#### 2. gcc
* 预处理, 编译, 汇编, 链接
* ![](image\gcc编译过程.PNG)
* gcc -g -o test01.c test01  // 编译带gdb调试信息的
* -I 指定头文件目录: gcc -g test01.c -I ../ -Wall -o test01
* -Wall  输出所有warning (warning all)

#### 3. toolchain
* nm 命令查看二进制文件符号: 全局变量, 全局函数  nm test01.o
    - T 实现了的
    - U 未实现的, 可能在.h里实现
    - D 全局变量
* objdump 反汇编: objdump -dS test01.o
* ELF: 二进制文件存储格式
* LSB: 小端法 (内存地址, 低存低, 高存高)
```
[root@centos180 test01_pro]# file test01.o
test01.o: ELF 64-bit LSB relocatable, x86-64, version 1 (SYSV), not stripped
```
* readelf -a test01.o 读取二进制文件

#### 4. 程序在Linux内存中的存储位置
* Linux 中 stack区 为 8M
* stack 和 heap 的存储方向相反

#### 5. 函数库
* 代码复用: 函数->模块->函数库
* 静态库: .a 复制静态库到代码中, 速度快, 占用内存大, 应用于核心程序
    - ar 制作静态库工具: ar rs libmymath.a add.o sub.o mul.o
    - 使用静态库: gcc main.c -L ./ -l mymath -I ../ -o app
* 动态库: libxxx.so
    - 动态库加载到内存中
    - 共享在内存中
    - 只共享代码, 不共享数据
    - 节省内存, 动态更新加载, 相对速度慢
    - 制作: 
```
    gcc -fPIC -c add.c
    gcc -shared -o libmymath.so add.o sub.o
    gcc main.c -o app -L ./ -l mymath -I ../
    ./app: error while loading shared libraries: libmymath.so: cannot open shared object file: No such file or directory
```
* 动态链接器: 工作在程序运行阶段 /lib64/ld-linux-x86-64.so.2
* 链接器: 工作在编译阶段
* ldd app: 查看需要加载哪些动态链接库
* 配置动态链接库路径: vi ~/.bashrc -> export LD_LIBRARY_PATH=./  

#### 5. gdb
* 调试逻辑错误
* 命令: gdb file_name
    - list 1
    - b 20
    - run
    - step  : 进入函数内部
    - next  : 跳过该函数
    - finish : 终止当前函数
    - p var
    - ptype i
    - step
    - b 22 if j = 20
    - info b
    - delete 2
    - disable 3
    - display i
    - continue  : 跳到下一个断点
    - quit
    - start
    - bt    : 查看栈帧
    - info locals   : 查看栈帧局部变量的值
    - frame 1   : 切换栈帧

* python exception 报错: `cp -rf gdb/python/lib/gdb/* /usr/local/share/gdb/python/gdb/`
* 栈帧: 由栈帧组成栈, 栈帧保存局部变量和临时值(临时变量地址)
* 段错误: 非法访问内存
    - 排错方法: gdb -> run -> 停止位置即为出现段错误的位置

#### 6. makefile
* 组成: 目标, 依赖, 命令 (变量, 函数)
    - 目标:依赖<br>
      (tab)命令
* makefile / Makefile
```
app:add.o sub.o main.o
    gcc add.o sub.o main.o -o app
add.o:add.c
    gcc -c add.c -o add.o
sub.o:sub.c
    gcc -c sub.c -o sub.o
main.o:main.c
    gcc -c main.c -o main.o

```
* 变量
```
src = add.c sub.c main.c
obj = add.o sub.o main.o
CC = gcc
app:$(obj)
    $(CC) $(obj) -o app
add.o:add.c
    $(CC) -c add.c -o add.o
sub.o:sub.c
    $(CC) -c sub.c -o sub.o
main.o:main.c
    $(CC) -c main.c -o main.o
clean:
    rm -rf $(obj) app
```
* make clean -n : 测试不执行
* 自动变量: 
    - $@ : 命令中的目标
    - $< : 命令中的第一个条件
    - $^ : 命令中的条件
```
src = add.c sub.c main.c
obj = add.o sub.o main.o
CC = gcc
app:$(obj)
    $(CC) $^ -o $@
add.o:add.c
    $(CC) -c $< -o $@
sub.o:sub.c
    $(CC) -c $< -o $@
main.o:main.c
    $(CC) -c $< -o $@
clean:
    rm -rf $(obj) app
```
* 函数:
    - 找到当前目录下所有.c的文件, 赋值给src: 
        + `src = $(wildcard *.c)`
    - 把src变量里所有后缀为.c的文件替换成.o:
        + `obj = $(patsubst %.c, %.o, $(src))`
```
src = $(wildcard *.c)
obj = $(patsubst %.c, %.o, $(src))
CC = gcc
app:$(obj)
    $(CC) $^ -o $@
add.o:add.c
    $(CC) -c $< -o $@
sub.o:sub.c
    $(CC) -c $< -o $@
main.o:main.c
    $(CC) -c $< -o $@
clean:
    rm -rf $(obj) app
```
* 模式替换:
```
src = $(wildcard *.c)
obj = $(patsubst %.c, %.o, $(src))
CC = gcc

app:$(obj)
    $(CC) $^ -o $@

%.o:%.c
    $(CC) -c $< -o $@

clean:
    rm -rf $(obj) app
```
* 伪目标声明: 
    - `.PHONY:clean`
* 执行文件名不为makefile的文件: make -f m1
* 指定终极目标: ALL:app
* 静态模式规则: `$(obj):%.o:%.c` 针对$(obj) 这个集合使用 %.o:%.c模式(目标:依赖)去执行后面的命令
* 完整形态:
```
src = $(wildcard *.c)
obj = $(patsubst %.c, %.o, $(src))
CC = gcc

target = app
ALL:$(target)

$(target):$(obj)
    $(CC) $^ -o $@

$(obj):%.o:%.c
    $(CC) -c $< -o $@

clean:
    -rm -rf $(obj) app

.PHONY:clean ALL
```

#### 7. C项目目录结构
* ![](image\c项目目录结构.PNG)
* 项目中的makefile:
```
src = $(wildcard ./src/*.c)
obj = $(patsubst ./src/%.c, ./obj/%.o, $(src))
CC = gcc
inc_path = ./inc/

target = app
ALL:$(target)
CFLAGES = -I

$(target):$(obj)
    $(CC) $^ -o $@

$(obj):./obj/%.o:./src/%.c
    $(CC) -c $< -o $@ $(CFLAGES) $(inc_path)

clean:
    -rm -rf $(obj) app

.PHONY:clean ALL
```

#### 8. 将目录下的所有.c文件单独生成可执行文件
```
src = $(wildcard *.c)
target = $(patsubst %.c, %, $(src))

ALL: $(target)
CFLAGES = -Wall -g

$(target):%:%.c
    gcc $^ -o $@ $(CFLAGES)
clean:
    -rm -rf $(target)
.PHONY:clean ALL
```
