## zlog guide note

* zlog 特性：

  * 日志格式定制;
  * 多种输出，包括动态文件、静态文件、stdout、stderr、syslog、用户自定义输出函数;
  * 运行时手动、自动刷新配置文件（同时保证安全);
  * 高性能，在我的笔记本上达到25万条日志每秒, 大概是syslog(3)配合rsyslogd的1000倍速
    度;
  * 用户自定义等级;
  * 多线程和多进程环境下保证安全转档;
  * 精确到微秒;
  * 简单调用包装dzlog（一个程序默认只用一个分类）;
  * MDC，线程键-值对的表，可以扩展用户自定义的字段;
  * 自诊断，可以在运行时输出zlog自己的日志和配置状态;
  * 不依赖其他库，只要是个POSIX系统就成(当然还要一个C99兼容的vsnprintf);
<<<<<<< HEAD

=======
<<<<<<< HEAD
=======
* zlog 编译安装：
  * 编译和安装zlog
    下载 zlog-latest-stable.tar.gz
    $ tar -zxvf zlog-latest-stable.tar.gz
    $ cd zlog-1.2.*/
    $ make
    $ sudo make install
    or
    $ sudo make PREFIX=/usr/local/ install
    PREFIX指明了安装的路径，安转完之后为了让你的程序能找到zlog动态库
    $ sudo vi /etc/ld.so.conf
    /usr/local/lib
    $ sudo ldconfig

>>>>>>> f63244efe62324fe98aa3d3258ac65a6c7030eee
>>>>>>> 9b4253deb3f4d1b97f0d59e3e0f3159e333127f6
* glibc 版本(2.1以上)：

  * ll /usr/lib64/libc.so*
    -rw-r--r--. 1 root root 253 Nov  5  2016 /usr/lib64/libc.so
    lrwxrwxrwx. 1 root root  12 Dec 17 11:21 /usr/lib64/libc.so.6 -> libc-2.17.so

* zlog版本：zlog-1.2.7

* zlog 1.2版本新增功能：

  * (a) 对管道的支持，从此zlog可以外接cronolog这样的日志过滤程序来输出；
  * (b) 全面的日志转档支持；
  * (c) 其他兼容性的代码改动；

* zlog有3个重要的概念：分类(Category)、规则(Rule)、格式(Format);

* 一个日志分类 category 可以同时对应多个规则 rule, 每个规则 rule 都有自己的级别，输出，格式；

* 通配符：

  * 通配符“ * ” 可以匹配所有分类；
  * 通配符"_" 表示上级分类；

* 配置文件：

  * 单位大小写不敏感：1GB == 1gb == 1gB

  * []代表一个节的开始，四个小节的顺序不能变，依次为global-levels-formats-rules

  * [globle]

    * strict init: 严格检查所有的格式和规则

    * reload conf period : 在一段时间间隔后自动重载配置文件

    * buffer min: 在堆上为每个线程申请缓存

      buffer max

    * rotate lock file: 这个选项指定了一个锁文件，用来保证多进程情况下日志安全转档;

      * rotate lock file = self  在这种情况下，zlog不会创建任何锁文件，用配置
        文件作为锁文件

    * default format: "%d %V [%p:%F:%L] %m%n"

    * file perms: 这个指定了创建日志文件的缺省访问权限  600

    * fsync period: 

      * 在每条规则写了一定次数的日志到文件后，zlog会调用fsync(3)来让操作系统马上把数
        据写到硬盘;
      * 刷缓存到文件;
      * 可以用同步IO文件；

  * [levels]

    * (level string) = (level int), (syslog level, optional)
    * (level int)必须在[1,253]这个范围内，越大越重要

  * [formats]

<<<<<<< HEAD
    * 转换字符 参考 zlog-UsersGuide 5.4.1;
=======
<<<<<<< HEAD
    * 转换字符 参考 zlog-UsersGuide 5.4.1;
=======
    * 转换字符 参考 5.4.1;
>>>>>>> f63244efe62324fe98aa3d3258ac65a6c7030eee
>>>>>>> 9b4253deb3f4d1b97f0d59e3e0f3159e333127f6

    * %c : 分类名；

    * %p : pid 来源于getpid();

    * %M : MDC (mapped diagnostic context)

    * %V: 日志级别，大写

      %v: 日志级别，小写

    * %t : 线程ID；

      %T：线程ID， 长整型；

    * 宽度修饰符： 左对齐标识，减号(-)

<<<<<<< HEAD
    * 时间字符: 参考  zlog-UsersGuide 5.4.3;
=======
<<<<<<< HEAD
    * 时间字符: 参考  zlog-UsersGuide 5.4.3;
=======
    * 时间字符: 参考 5.4.3;
>>>>>>> f63244efe62324fe98aa3d3258ac65a6c7030eee
>>>>>>> 9b4253deb3f4d1b97f0d59e3e0f3159e333127f6

  * [rules] : (category).(level) (output), (options, optional); (format name, optional)

  * zlog有6个默认的级别："DEBUG",  "INFO",  "NOTICE",  "WARN",  "ERROR" 和 "FATAL"

    * aa.DEBUG意味着任何大于等于DEBUG级别的日志会被输出

  * 千万不要在守护进程的规则里面加上 >stdout 或 >stderr

  * zlog本身的直接文件输出能保证即使是多进程，同时调用zlog写一个日志文件也不会产生
    交错;

  * 输出到有进程号区分的日志，每天，在$HOME/log目录，每1GB转档一次，保持5个
    日志文件。
    *.* "%E(HOME)/log/aa.%p.%d(%F).log",1GB * 5

  * 同步 IO 文件：

    * 在文件路径前加上一个"-"就打开了同步IO选项

  * 文件转档：

    * 按日期拆分： ```*.*  "aa.%d(%F).log"```
    * 按照日志大小切分:  ``` *.* "aa.log", 10MB ```

  * 检查 zlog 配置文件语法： $ ./zlog-chk-conf zlog.conf

    * [localhost@src] ./zlog-chk-conf /etc/zlog.conf /code/test_zlog/test_hello.conf
      --[/etc/zlog.conf] syntax right
      --[/code/test_zlog/test_hello.conf] syntax right

* zlog 接口 API：

  * int zlog_init(const char *confpath);
    int zlog_reload(const char *confpath);
    void zlog_fini(void);

  * zlog_get_category()从zlog的全局分类表里面找到分类

  * 所有写日志的函数：

    /* zlog macros */
    zlog_fatal(cat, format, ...)
    zlog_error(cat, format, ...)
    zlog_warn(cat, format, ...)
    zlog_notice(cat, format, ...)
    zlog_info(cat, format, ...)
    zlog_debug(cat, format, ...)
    /* vzlog macros */
    vzlog_fatal(cat, format, args)
    vzlog_error(cat, format, args)
    vzlog_warn(cat, format, args)
    vzlog_notice(cat, format, args)
    vzlog_info(cat, format, args)
    vzlog_debug(cat, format, args)
    /* hzlog macros */
    hzlog_fatal(cat, buf, buf_len)
    hzlog_error(cat, buf, buf_len)
    hzlog_warn(cat, buf, buf_len)
    hzlog_notice(cat, buf, buf_len)
    hzlog_info(cat, buf, buf_len)
    hzlog_debug(cat, buf, buf_len)

<<<<<<< HEAD
=======
<<<<<<< HEAD
=======
    

>>>>>>> f63244efe62324fe98aa3d3258ac65a6c7030eee
>>>>>>> 9b4253deb3f4d1b97f0d59e3e0f3159e333127f6
  * dzlog是忽略分类(zlog_category_t)的一组简单zlog接口

* zlog 高阶使用

  * MDC: Mapped Diagnostic Context
    *  MDC就是一个键-值对表;
    *  使用场景： 需要在同样的日志行为区分不同的业务数据的时候 (多态)
  * 诊断 zlog 本身:
    * zlog有自己的日志——诊断日志。这个日志通常是关闭的，可以通过环境变量来打开:
      $ export ZLOG_PROFILE_DEBUG=/tmp/zlog.debug.log
      $ export ZLOG_PROFILE_ERROR=/tmp/zlog.error.log
  * 用户自定义等级
  * 用户自定义输出 

<<<<<<< HEAD
=======
<<<<<<< HEAD
>>>>>>> 9b4253deb3f4d1b97f0d59e3e0f3159e333127f6
* zlog 编译安装：

  * 编译和安装zlog
    下载 zlog-latest-stable.tar.gz
    $ tar -zxvf zlog-latest-stable.tar.gz
    $ cd zlog-1.2.*/
    $ make
    $ sudo make install
    or
    $ sudo make PREFIX=/usr/local/ install
    PREFIX指明了安装的路径，安转完之后为了让你的程序能找到zlog动态库
    $ sudo vi /etc/ld.so.conf
    /usr/local/lib
    $ sudo ldconfig

* zlog Hello World 实验代码
  test_hello.c, test_hello.conf

``` 
$ vi test_hello.c
#include <stdio.h>
#include "zlog.h"
int main(int argc, char** argv)
{
int rc;
zlog_category_t *c;
rc = zlog_init("test_hello.conf");
if (rc) {
printf("init failed\n");
return -1;
}
c = zlog_get_category("my_cat");
if (!c) {
printf("get cat fail\n");
zlog_fini();
return -2;
}
zlog_info(c, "hello, zlog");
zlog_fini();
return 0;
```

* 写一个配置文件，放在和test_hello.c同样的目录下:
  $ vi test_hello.conf
  [formats]
  simple = "%m%n"
  [rules]
  my_cat.DEBUG >stdout; simple

* 编译、然后运行
  $ cc -c -o test_hello.o test_hello.c -I/usr/local/include
  $ cc -o test_hello test_hello.o -L/usr/local/lib -lzlog
  $ ./test_hello
<<<<<<< HEAD
  hello, zlog
=======
  hello, zlog
=======
>>>>>>> f63244efe62324fe98aa3d3258ac65a6c7030eee
>>>>>>> 9b4253deb3f4d1b97f0d59e3e0f3159e333127f6
