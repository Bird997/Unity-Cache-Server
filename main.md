# Unity Cache Server

- 目的：减少团队项目中由于单个成员修改资源文件导致团队所有成员重新导入资源的时间。
- 方法：第一个人上传资源时产生的导入数据自动上传到Server，让其他成员下载。
- 由于手游平台有很多类型，所以在开发中很经常会涉及Unity平台的切换，如果没有搭建 CacheServer 的话，每次切换都要在本地跑一边资源的导入，这是非常耗时的。所以 CacheServer 的搭建是非常有必要的。
- 哪些改变会导致重新生成导入数据？
    1. 资源文件本身的改变。
    2. 导入操作的设置的改变。
    3. 资源导入版本号的改变。
    4. 平台的改变。

---
### Hero西安的各位：
###### Unity客户端使用
1. 打开Unity Preferences -> CacheServer界面，选择Cache Server Mode = Remote。
2. 填入IP地址 【10.1.3.19】，点击Check Connection检测连接，出现Connection successful即为连接成功。
3. 资源导入时产生的数据文件会自动上传到服务器上，切换平台会自动从服务器上下载。
###### 服务器主机的使用
> 服务器主机已经开启了windows服务，不需要下面的手动操作，每次开机都会自动开启缓存服务（参考“服务器注意事项”），服务如果遇到崩溃，会自动重启。
1. 每次开机打开电脑桌面的unity-cache-server.cmd，即根据配置文件的设置开启Cache Server。
2. 配置文件是快捷方式源目录下，config文件夹中的defult.yml文件。
3. 缓存文件都保存在 .cache_fs文件夹中。

---
### 对承载Cache Server的计算机的需求
1. 为了获得最佳性能，必须有足够的RAM来容纳整个导入的项目文件夹。此外，最好有一台硬盘速度快、以太网连接速度快的电脑。硬盘也应该有足够的空闲空间。另一方面，缓存服务器的CPU使用率非常低。

2. 缓存服务器和版本控制之间的主要区别之一是，缓存的数据总是可以在本地重建。它只是一个提高性能的工具。因此，在Internet上使用缓存服务器是没有意义的。如果您有一个分布式团队，您应该在每个位置放置一个单独的缓存服务器。

3. 缓存服务器在Linux或Mac OS X计算机上最佳运行。Windows文件系统对于缓存服务器如何存储数据的优化不是特别好，Windows上的文件锁定问题可能会导致Linux或Mac OS X上不会出现的问题。
    > the Windows file system is not particularly well optimized for how the Asset Cache Server stores data and problems with file locking on Windows can cause issues that don’t occur on Linux or Mac OSX.

### 使用方法
###### 本地单机模式

如图设置即可使用本地版本的Cache Server，不需要安装其他东西
![window](https://raw.githubusercontent.com/XieShou/Unity-Cache-Server/master/1.jpg)

