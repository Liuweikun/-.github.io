# centos 7目录结构和各文件系统作用

---

## 查看centos根目录结构
```
[user1@blog00 /]$ ll
总用量 28
lrwxrwxrwx.   1 root root    7 6月  30 07:48 bin -> usr/bin
dr-xr-xr-x.   5 root root 4096 6月  30 07:53 boot
drwxr-xr-x.  19 root root 3260 7月   1 05:20 dev
drwxr-xr-x. 145 root root 8192 7月   1 05:20 etc
drwxr-xr-x.   3 root root   19 6月  30 07:52 home
lrwxrwxrwx.   1 root root    7 6月  30 07:48 lib -> usr/lib
lrwxrwxrwx.   1 root root    9 6月  30 07:48 lib64 -> usr/lib64
drwxr-xr-x.   2 root root    6 4月  11 2018 media
drwxr-xr-x.   2 root root    6 4月  11 2018 mnt
drwxr-xr-x.   3 root root   16 6月  30 07:50 opt
dr-xr-xr-x. 262 root root    0 7月   1 05:20 proc
dr-xr-x---.  15 root root 4096 7月   1 05:21 root
drwxr-xr-x.  44 root root 1300 7月   1 05:21 run
lrwxrwxrwx.   1 root root    8 6月  30 07:48 sbin -> usr/sbin
drwxr-xr-x.   2 root root    6 4月  11 2018 srv
dr-xr-xr-x.  13 root root    0 7月   1 05:20 sys
drwxrwxrwt.  19 root root 4096 7月   1 05:22 tmp
drwxr-xr-x.  13 root root  155 6月  30 07:48 usr
drwxr-xr-x.  21 root root 4096 6月  30 07:53 var
```

![Linux目录结构](https://www.runoob.com/wp-content/uploads/2014/06/d0c50-linux2bfile2bsystem2bhierarchy.jpg)

---


### 1. 根目录 /
* 电脑有且只有一个根目录，所有的文件和目录从根目录开始，例如输入`/home`,其实是在告诉电脑，先从/(根目录)开始，再进入到home目录，类似windows里的我的电脑作用。

### 2. /bin
* /bin 存放着标准的默认(缺省)的Linux工具，即最经常用到的命令。比如`ls`,`vi`还有`more`,这个目录包含在类似windows中path系统变量里,当你输入ls,系统会去/bin目录下查找是不是有ls这个程序。
  
### 3. /boot
* linux内核以及启动引导加载程序所需要的文件
  
### 4. /dev
* 这里存放与设备，包括外设有关的文件，例如连接打印机，系统就是从这个目录开始工作，另外包括一些磁盘驱动，USB驱动都放在这个目录。

### 5. /etc 
* 存放了系统所有程序配置方面的文件，也包含用于启动或者停止单个程序的shell脚本，例如安装了某个程序的套件，想要修改时，配置文件就在这个目录里

### 6. /home
* 存放普通用户个人的数据，具体到每个用户的设置文件，用户的桌面文件夹，用户的数据，每个用户有自己的用户目录，位置在`/home/用户名`

### 7. /lib 
* 是Library库的缩写，是32位的库目录，存放着系统最基本的动态连接共享库，类似windows里的DLL文件，几乎所有程序都需要用到这些共享库,是/usr/lib的快捷方式

### 8. /lib64
* 与lib作用相似，是64位的库目录，是/usr/lib64的快捷方式

### 9. /meida
* 用于挂载系统识别后的：外接USB接口的移动硬盘，U盘，CD，DVD驱动器等临时目录

### 10. /mnt
* 一般用于存放挂载储存设备的挂载目录，与meida功能类似，区别在于让用户临时挂载别的文件系统

### 11. /opt
* 是optional可选择的缩写，是给主机额外安装软件所摆放的目录，比如安装一个数据库，就可以放在这个目录下，默认是空的

### 12. /proc
* 是Processes进程的缩写，系统运行时，进程信息以及内核信息(比如CPU，硬盘分区，内存信息)均存放在这里，proc并不是真正的文件系统,不在硬盘，而在内存上，系统内存的映射，是一种伪文件，虚拟文件系统

### 13. /root
* 是系统管理员的主目录，系统管理员是上帝，可以对系统做任何事，因此要小心使用root权限
  
### 14. /run
* 是一个临时文件系统，存储系统启动以来的信息，当系统重启后，这个目录下的文件应该被删掉或者清除，是/var/run的快捷方式

### 15. /sbin
* 是root用户可以执行命令的存放地，普通用户无权限访问，大多涉及系统管理维护的命令的存放地，是/usr/sbin的快捷方式

### 16. /srv
* 存放一些服务启动后需要提取的特定服务相关的数据

### 17. /sys
* sysfs文件系统集成三种文件系统的信息，针对进程信息的proc文件系统，针对设备的devfs文件系统，针对伪终端的devpts文件系统
* 该文件系统是内核设备树的直观反映
* 当一个内核对象被创建时，对应的文件和目录也在内核对象子系统中被创建

### 18. /tmp
* temporary临时的缩写，用于存放一些临时文件的，有些Linux系统会对这个目录定期自动清理，重要数据别放这

### 19. /usr
* unix shared resources共享资源的缩写，是一个非常重要的目录，用户的很多程序和文件都存放在这里，类似Windows里的program files目录
```
[user1@blog00 usr]$ ls
bin  games    lib    libexec  sbin   src
etc  include  lib64  local    share  tmp
```
* /usr/bin用于存放程序
* /usr/games用于存放游戏
* /usr/lib存放不能直接运行，但却是很多程序运行必需的函数库文件
* /usr/sbin 存放root用户使用的高级管理程序和系统守护程序
* /usr/src 内核源代码默认的放置目录
* /usr/local存放那些手动安装的软件，不是通过apt-get安装的文件
* /usr/share用于存放一些共享的数据，比如音乐文件，图片

### 20. /var
* variable变量的缩写，存放着不断扩充的东西，将经常被修改的目录放在这个目录下，包括各种日志文件数据库电子邮件临时文件等

---
### 一般情况下别动的目录：
  **/ , /boot , /dev , /proc , /srv , /sys , /run**

### 谨慎添加修改，别删除的目录：
**/bin , /sbin , /lib , /lib64 , /etc /usr**

### 可以根据需要自行修改添加删除的目录：
**/tmp , /opt , /home , /root , /var , /meida , /mnt**

---
##### END

  
 






