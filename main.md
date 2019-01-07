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

Cache Server Mode : 在这里我们选Local。
   1. Local	缓存服务器模式选择本地模式，可以单机使用。
   2. Remote 输入IP地址连接服务器。
   3. disabled 禁用Cache Server。
   
Maximum Cache Size : 最大缓存大小，默认是10GB。

Custom cache location : 勾选上可以选择自定义缓存文件夹，不勾选则默认在C盘。

Check Cache Size : 可以获得当前缓存文件的内存大小。

Clean Cache : 清除缓存。

###### 服务器模式
1. 先前往 [Node.JS website](https://nodejs.org/en/download/) 下载安装LTS。
2. 打开安装好的Node.js command prompt，选择下方命令中的一个执行。
    - Npm安装：
    ```npm install unity-cache-server -g```
	- Github安装：
    ```npm install github:Unity-Technologies/unity-cache-server -g```
3. 找到自动安装完成的文件，找到Windows命令脚本 `unity-cache-server.cmd` 双击运行即可开启默认配置的Cache Server。
4. 客户端使用：
    1. 打开Unity Preferences -> CacheServer界面，选择Cache Server Mode = Remote。
	2. 填入IP地址 【服务器ip】，点击Check Connection检测连接，出现Connection successful即为连接成功。注：如果服务器在本地计算机上，需要使用127.0.0.1:端口号
    3. 资源导入时产生的数据文件会自动上传到服务器上，切换平台会自动从服务器上下载。

### 服务器注意事项：
###### 手动窗口操作（已经实现Windows Server自动）
1. 每次手动打开unity-cache-server.cmd即打开缓存服务，关闭窗口即关闭缓存服务，所以使用Unity Cache Server需要保持窗口没有被关闭。
2. 配置服务器设置：关闭服务器，在服务器文件夹中可以找到名为default.yml的配置文件，通过修改其中参数进行设置，再重新打开unity-cache-server.cmd即可。

###### 开机自动启动与崩溃重启服务
[参考文章](https://blog.csdn.net/wuming22222/article/details/51714111)
修改配置文件后建议重启计算机
1. 首先需要两个小程序instsrv.exe和srvany.exe[下载传送门](http://www.techeez.com/windows-tips/techeez-com-31)
2. 解压后将instsrv.exe和srvany.exe放到C:\Windows\目录下（因为创建了的Windows服务运行时会使用到srvany.exe）
3. 创建Windows服务：打开DOS命令窗口到达该目录下输入命令，并回车，如图：

![创建Windows服务cmd指令](https://raw.githubusercontent.com/XieShou/Unity-Cache-Server/master/3.png)

4. 计算机->右键->管理，可看到： 

![Windows服务窗口](https://raw.githubusercontent.com/XieShou/Unity-Cache-Server/master/4.jpg)

5. 修改注册表:
“运行”中写命令“regedit”回车，打开注册表编辑器
	1. 定位到HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\UnityCacheServer
	2. 在UnityCacheServer右键->新建->项， 命名Parameters
	3. 右键Parameters->新建->字符串值，命名Application
	4. 右键Application->修改，写入应用程序或命令脚本等的绝对路径（包括盘符和文件的尾缀名） 点击确定 
	
6. 重启电脑可以看到我们的Windows服务自动运行了。即自动执行了unity-cache-server.cmd。

7. 删除服务的方法：
今后若想移除上面新建的Windows服务，首先停止该服务然后到目录C:\Windows\下执行命令：
```instsrv UnityCacheServer remove ```
回车，提示：```The service was successfully deleted!``` 即表示删除成功。 

8. 在计算机-管理-服务窗口中，找到UnityCacheServer，右键-属性，打开属性设置界面，如图设置即可自动恢复，重启服务。

![自动恢复](https://raw.githubusercontent.com/XieShou/Unity-Cache-Server/master/5.png)

