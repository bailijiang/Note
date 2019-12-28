Ubuntu16.04安装boost库

到boost官网安装一个boost库的压缩包，我下的是1.67.0版本

执行下面的命令解压  
tar -xzvf boost_1_67_0.tar.gz

解压出来以后，进入解压出来的文件夹，执行下面的命令  
sudo ./bootstrap.sh

在执行下面的命令，这样头文件就被默认安装在/usr/local/include头文件下，库文件就被默认安装在  
/usr/local/lib下  
sudo ./b2 install

这个时候就已经安装好了，不过在编译的时候可能还会有一点小问题，比如有下面的代码

'''

    #include <iostream>
    #include <boost/regex.hpp>
    #include <boost/algorithm/string.hpp>
    using namespace std;
     
    int main(){
        string str = "data-num=\"1056\"";
        boost::regex reg("\\d{1,6}");//{1,6}表示\d重复1-6次，\d表示匹配整数
        boost::smatch what;
        string::const_iterator begin = str.begin();
        string::const_iterator end = str.end();
     
        boost::regex_search(begin,end,what,reg);
        string result(what[0].first,what[0].second);
        cout << result << endl;
        return 0;
    }

'''

这里用到了一个regex.hpp的库，那么我们在编译的时候会还需要加上相应库的链接，如下（-I选项是添加头文件的路径，-L选项是添加库文件的路径，-l是具体哪个库文件）

g++ -o test test.cc -std=c++11  -I /usr/local/include -L /usr/local/lib  -lboost_regex
如果执行起来碰到下面的问题

./test: error while loading shared libraries: libboost_regex.so.1.67.0: cannot open shared object file: No such file or directory  
这是因为系统不知道***.so文件在哪个位置，找不到该文件。
这个时候就要在/etc/ld.so.conf中加入xxx.so所在的目录，
因为我们的.so库文件是放在/usr/local/lib目录下，所以要在该文件中加入这一行这个路径就可以了。添加完以后执行下面的命令

ldconfig  
这样执行起来就会通过了。


* 测试代码： boost_1_71_0
```

    #include <iostream>
    #include <boost/asio.hpp>
    #include <boost/date_time/posix_time/posix_time.hpp>

    int main()
    {
        boost::asio::io_service io;

        boost::asio::deadline_timer t(io, boost::posix_time::seconds(5));
        t.wait();

        std::cout << "Hello, world Boost Asio!" << std::endl;

        return 0;

    }

```


```
bryan@bryan-ThinkPad-E570:~/Code/Cpp$ cat /etc/ld.so.conf.d/boost.conf 
/usr/local/lib

sudo ldconfig

bryan@bryan-ThinkPad-E570:~/Code/Cpp$ g++ -std=c++11 -o test test.cpp -lboost_thread -lpthread
bryan@bryan-ThinkPad-E570:~/Code/Cpp$ ./test 
Hello, world Boost Asio!
```