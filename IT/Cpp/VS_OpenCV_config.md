VS2013怎么配置opencv才能不用每次都重新配置一遍？

**目录**  

1.  解压OpenCV
2.  配置系统环境变量
3.  创建VS新项目
4.  添加项目属性表
5.  配置项目属性表
6.  引用项目属性表



**声明：**  

1.  以下展示内容，配置方案为OpenCV2.4.9+VS2013+win10+x64，但方法适用于OpenCV2.4.8、OpenCV2.4.9、OpenCV2.4.10，适用于win7、win8.1、win10，适用于x64和x84（下面会讲解如何对应修改）。如果问为什么适用？因为我都试过啊~  

2.  会一次性配置的知友请直接看第四部分和第六部分，因为其余部分跟一次性配置是一样的。


**一、解压OpenCV**  
比如本人解压在G盘根目录下  

![](https://pic3.zhimg.com/50/d6135179281f46ce18ff9a6209d380d0_hd.jpg)





**二、配置系统环境变量**  

打开“系统属性”，点击“环境变量”  

![](https://pic2.zhimg.com/50/d470fcfbc0bac0f7169574a144951dbc_hd.jpg)




双击Path的值部分  

![](https://pic1.zhimg.com/50/f7d668ca7ff2646e6189b2d43f899c9a_hd.jpg)




填上OpenCV的bin路径  

![](https://pic3.zhimg.com/50/c02b13011b23eead03bc7e944bb7d7e9_hd.jpg)




注意：  
1、此时电脑最好重启下。  
2、如果你的应用程序是win32控制台应用程序，则选择x32的路径；如果你的应用程序是win64控制台应用程序，则选择x64的路径。\
3、vc10, vc11, vc12 分别表示VS2010, VS2012, VS2013的Visual Studio使用的编译器版本，根据自己的VS版本来填写正确的编译器版本号。  


**三、创建VS新项目**  

![](https://pic2.zhimg.com/50/7f15f6fc91c9b851f0260c8cd93f20ef_hd.jpg)



![](https://pic1.zhimg.com/50/d8f52c315f4a1f2f42624026d95fbd66_hd.jpg)



![](https://pic4.zhimg.com/50/6f8bce65bad6ea387ca404858b7fc55e_hd.jpg)





**四、添加项目属性表**  

“视图”打开“属性窗口”  

![](https://pic2.zhimg.com/50/99070c1452bd971d6652d9b1c7f7c939_hd.jpg)



![](https://pic2.zhimg.com/50/23b8967b62c6e5407d776d43e27b0ad5_hd.jpg)




右击“Debug|Win32”点击“添加新项目属性表”（如果想在Release模式下使用OpenCV，右击Release|Win32，同理可得）

![](https://pic4.zhimg.com/50/e83136838687ba83e81db9bbf4ede131_hd.jpg)




给属性表命名  

![](https://pic1.zhimg.com/50/7aed98411d80eb2c9eb51c42fd21f4e1_hd.jpg)




**五、配置项目属性表**  

双击创建的属性表  

![](https://pic1.zhimg.com/50/27db5dbe81b73a35f8579b2b70004b68_hd.jpg)




5.1添加包含目录  

![](https://pic4.zhimg.com/50/7bc1ca5bb25dc0e38ef967d8ddb33c97_hd.jpg)



![](https://pic3.zhimg.com/50/93d0664f7307cf57a640398c5454b7f7_hd.jpg)




5.2添加附加库目录  

![](https://pic4.zhimg.com/50/be61e855039c30f20822fc95ee0919f9_hd.jpg)



![](https://pic3.zhimg.com/50/55abca94d7eae1b35ac9cc5124bce092_hd.jpg)




注意：  
1、如果你的应用程序是win32控制台应用程序，则选择x32的路径；如果你的应用程序是win64控制台应用程序，则选择x64的路径。  
2、vc10, vc11, vc12 分别表示VS2010, VS2012, VS2013的Visual Studio使用的编译器版本，根据自己的VS版本来填写正确的编译器版本号。  

5.3添加附加依赖项  

附加依赖项如下：  
opencv_imgproc249d.lib  
opencv_calib3d249d.lib  
opencv_contrib249d.lib  
opencv_core249d.lib  
opencv_features2d249d.lib  
opencv_flann249d.lib  
opencv_gpu249d.lib  
opencv_highgui249d.lib  
opencv_legacy249d.lib  
opencv_ml249d.lib  
opencv_nonfree249d.lib  
opencv_objdetect249d.lib  
opencv_ocl249d.lib  
opencv_photo249d.lib  
opencv_stitching249d.lib  
opencv_superres249d.lib  
opencv_ts249d.lib  
opencv_video249d.lib  
opencv_videostab249d.lib  

注意：  
1、249对应OpenCV2.4.9，其他版本做对应修改。  
2、后缀d代表Debug模式，如果是Release模式去掉d。依个人情况选择Debug模式或者Release模式。  


![](https://pic4.zhimg.com/50/38daa2039c439378a5289b447b98725e_hd.jpg)



![](https://pic3.zhimg.com/50/dbfdceec72dfc7038037de6aa7bb5e66_hd.jpg)





**六、引用项目属性表**  

打开项目目录，你会看到如下文件  

![](https://pic3.zhimg.com/50/fd97db6c52b4ce078b34489c08b79aa5_hd.jpg)





以后新建项目时，打开属性管理器，右击Debug|Win32选择添加现有属性表上述文件即可。  

![](https://pic2.zhimg.com/50/2c92811433387ccd5fa4a702f655171e_hd.jpg)



![](https://pic3.zhimg.com/50/3fb1930f79f0172255ab0cdbb30b1edd_hd.jpg)





========================  

以上，应该算**VS一次配置永久使用OpenCV**了^ ^
