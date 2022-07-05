# MySQL+Centos+Docker环境下载安装卸载
---
## 一、传统方式下载安装mysql
**[mysql官网：https://www.mysql.com/](https://www.mysql.com/)**

### 1.下载安装包


### 2. 查看解压后的安装包
```
[root@blog00 ~]# ls
anaconda-ks.cfg
initial-setup-ks.cfg
mysql-8.0.29-1.el7.x86_64.rpm-bundle.tar
mysql-community-client-8.0.29-1.el7.x86_64.rpm
mysql-community-client-plugins-8.0.29-1.el7.x86_64.rpm
mysql-community-common-8.0.29-1.el7.x86_64.rpm
mysql-community-debuginfo-8.0.29-1.el7.x86_64.rpm
mysql-community-devel-8.0.29-1.el7.x86_64.rpm
mysql-community-embedded-compat-8.0.29-1.el7.x86_64.rpm
mysql-community-icu-data-files-8.0.29-1.el7.x86_64.rpm
mysql-community-libs-8.0.29-1.el7.x86_64.rpm
mysql-community-libs-compat-8.0.29-1.el7.x86_64.rpm
mysql-community-server-8.0.29-1.el7.x86_64.rpm
mysql-community-server-debug-8.0.29-1.el7.x86_64.rpm
mysql-community-test-8.0.29-1.el7.x86_64.rpm
```
### 3. 查找并卸载系统自带的数据库管理系统，与之后安装的mysql冲突
```
[root@blog00 ~]# rpm -qa|grep mariadb
mariadb-libs-5.5.68-1.el7.x86_64
[root@blog00 ~]# rpm -e --nodeps mariadb-libs 
[root@blog00 ~]# rpm -qa|grep mariadb
```

### 4. 安装包与包存在依赖，要按照顺序安装mysql
```

[root@blog00 ~]# [root@blog00 ~]# rpm -ivh mysql-community-common-8.0.29-1.el7.x86_64.rpm 
警告：mysql-community-common-8.0.29-1.el7.x86_64.rpm: 头V4 RSA/SHA256 Signature, 密钥 ID 3a79bd29: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:mysql-community-common-8.0.29-1.e################################# [100%]


[root@blog00 ~]# rpm -ivh mysql-community-client-plugins-8.0.29-1.el7.x86_64.rpm 
警告：mysql-community-client-plugins-8.0.29-1.el7.x86_64.rpm: 头V4 RSA/SHA256 Signature, 密钥 ID 3a79bd29: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:mysql-community-client-plugins-8.################################# [100%]


[root@blog00 ~]# rpm -ivh mysql-community-libs-8.0.29-1.el7.x86_64.rpm 
警告：mysql-community-libs-8.0.29-1.el7.x86_64.rpm: 头V4 RSA/SHA256 Signature, 密钥 ID 3a79bd29: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:mysql-community-libs-8.0.29-1.el7################################# [100%]


[root@blog00 ~]# rpm -ivh mysql-community-client-8.0.29-1.el7.x86_64.rpm 
警告：mysql-community-client-8.0.29-1.el7.x86_64.rpm: 头V4 RSA/SHA256 Signature, 密钥 ID 3a79bd29: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:mysql-community-client-8.0.29-1.e################################# [100%]


[root@blog00 ~]# rpm -ivh mysql-community-icu-data-files-8.0.29-1.el7.x86_64.rpm 
警告：mysql-community-icu-data-files-8.0.29-1.el7.x86_64.rpm: 头V4 RSA/SHA256 Signature, 密钥 ID 3a79bd29: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:mysql-community-icu-data-files-8.################################# [100%]


[root@blog00 ~]# rpm -ivh mysql-community-server-8.0.29-1.el7.x86_64.rpm 
警告：mysql-community-server-8.0.29-1.el7.x86_64.rpm: 头V4 RSA/SHA256 Signature, 密钥 ID 3a79bd29: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:mysql-community-server-8.0.29-1.e################################# [100%]
```
### 5. 初始化
```

[root@blog00 ~]# mysqld --initialize --console
```

### 6. 修改安装目录的所有者和所属者，让mysql用户有权限可以直接使用mysql
```

[root@blog00 ~]# chown -R mysql:mysql /var/lib/mysql/
[root@blog00 ~]# 
```

### 7. 启动mysql
```
[root@blog00 ~]# systemctl start mysqld
[root@blog00 ~]#
``` 


### 8. 初始化时，会给一个临时的用户密码，查看这个密码
```
[root@blog00 ~]# cat /var/log/mysqld.log|grep localhost
2022-07-05T04:02:41.546077Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: 3XBVQIisJy&y
```

### 9. 登入mysql
```
[root@blog00 ~]# mysql -uroot -p
Enter password:                                          #输入密码
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.29

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```

### 10. 修改密码
```
mysql> alter user 'root'@'localhost' identified by '123456';
Query OK, 0 rows affected (0.00 sec)
```
### 11. 重新登入，确认已经修改密码成功并安装好
```
[root@blog00 ~]# mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 8.0.29 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases
    -> exit
    -> ^C
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)

[root@blog00 ~]# mysql --version
mysql  Ver 8.0.29 for Linux on x86_64 (MySQL Community Server - GPL)
```
---

## 二、传统方式卸载mysql

### 1. 关闭mysql服务
```
[root@blog00 ~]# systemctl stop mysqld
```
### 2. 查看mysql服务
```
mysql-community-client-8.0.29-1.el7.x86_64
mysql-community-common-8.0.29-1.el7.x86_64
mysql-community-client-plugins-8.0.29-1.el7.x86_64
mysql-community-server-8.0.29-1.el7.x86_64
mysql-community-libs-8.0.29-1.el7.x86_64
mysql-community-icu-data-files-8.0.29-1.el7.x86_64
```

### 3. 卸载这些服务
```
[root@blog00 ~]# yum remove mysql-community-server.x86_64 
已加载插件：fastestmirror, langpacks
正在解决依赖关系
--> 正在检查事务
---> 软件包 mysql-community-server.x86_64.0.8.0.29-1.el7 将被 删除
--> 解决依赖关系完成
base/7/x86_64                                                | 3.6 kB  00:00:00     
extras/7/x86_64                                              | 2.9 kB  00:00:00     
updates/7/x86_64                                             | 2.9 kB  00:00:00     
updates/7/x86_64/primary_db                                  |  16 MB  00:00:04     

依赖关系解决

==================================================================================== Package                      架构         版本               源               大小
====================================================================================正在删除:
 mysql-community-server       x86_64       8.0.29-1.el7       installed       240 M

事务概要
====================================================================================移除  1 软件包

安装大小：240 M
是否继续？[y/N]：y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
警告：RPM 数据库已被非 yum 程序修改。
** 发现 2 个已存在的 RPM 数据库问题， 'yum check' 输出如下：
2:postfix-2.10.1-9.el7.x86_64 有缺少的需求 libmysqlclient.so.18()(64bit)
2:postfix-2.10.1-9.el7.x86_64 有缺少的需求 libmysqlclient.so.18(libmysqlclient_18)(64bit)
  正在删除    : mysql-community-server-8.0.29-1.el7.x86_64                      1/1 
  验证中      : mysql-community-server-8.0.29-1.el7.x86_64                      1/1 

删除:
  mysql-community-server.x86_64 0:8.0.29-1.el7                                      

完毕！


[root@blog00 ~]# yum remove mysql-community-icu-data-files.x86_64 
已加载插件：fastestmirror, langpacks
正在解决依赖关系
--> 正在检查事务
---> 软件包 mysql-community-icu-data-files.x86_64.0.8.0.29-1.el7 将被 删除
--> 解决依赖关系完成

依赖关系解决

==================================================================================== Package                            架构       版本             源             大小
====================================================================================正在删除:
 mysql-community-icu-data-files     x86_64     8.0.29-1.el7     installed     3.5 M

事务概要
====================================================================================移除  1 软件包

安装大小：3.5 M
是否继续？[y/N]：y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在删除    : mysql-community-icu-data-files-8.0.29-1.el7.x86_64              1/1 
  验证中      : mysql-community-icu-data-files-8.0.29-1.el7.x86_64              1/1 

删除:
  mysql-community-icu-data-files.x86_64 0:8.0.29-1.el7                              

完毕！
[root@blog00 ~]# 


[root@blog00 ~]# yum remove mysql-community-client.x86_64 
已加载插件：fastestmirror, langpacks
正在解决依赖关系
--> 正在检查事务
---> 软件包 mysql-community-client.x86_64.0.8.0.29-1.el7 将被 删除
--> 解决依赖关系完成

依赖关系解决

==================================================================================== Package                      架构         版本               源               大小
====================================================================================正在删除:
 mysql-community-client       x86_64       8.0.29-1.el7       installed        71 M

事务概要
====================================================================================移除  1 软件包

安装大小：71 M
是否继续？[y/N]：y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在删除    : mysql-community-client-8.0.29-1.el7.x86_64                      1/1 
  验证中      : mysql-community-client-8.0.29-1.el7.x86_64                      1/1 

删除:
  mysql-community-client.x86_64 0:8.0.29-1.el7                                      

完毕！


[root@blog00 ~]# yum remove mysql-community-libs.x86_64 
已加载插件：fastestmirror, langpacks
正在解决依赖关系
--> 正在检查事务
---> 软件包 mysql-community-libs.x86_64.0.8.0.29-1.el7 将被 删除
--> 解决依赖关系完成

依赖关系解决

==================================================================================== Package                    架构         版本                 源               大小
====================================================================================正在删除:
 mysql-community-libs       x86_64       8.0.29-1.el7         installed       7.5 M

事务概要
====================================================================================移除  1 软件包

安装大小：7.5 M
是否继续？[y/N]：y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在删除    : mysql-community-libs-8.0.29-1.el7.x86_64                        1/1 
  验证中      : mysql-community-libs-8.0.29-1.el7.x86_64                        1/1 

删除:
  mysql-community-libs.x86_64 0:8.0.29-1.el7                                        

完毕！

[root@blog00 ~]# rpm -qa|grep -i mysql
mysql-community-common-8.0.29-1.el7.x86_64
mysql-community-client-plugins-8.0.29-1.el7.x86_64
[root@blog00 ~]# yum remove mysql-community-client-plugins.x86_64 
已加载插件：fastestmirror, langpacks
正在解决依赖关系
--> 正在检查事务
---> 软件包 mysql-community-client-plugins.x86_64.0.8.0.29-1.el7 将被 删除
--> 解决依赖关系完成

依赖关系解决

==================================================================================== Package                            架构       版本             源             大小
====================================================================================正在删除:
 mysql-community-client-plugins     x86_64     8.0.29-1.el7     installed      14 M

事务概要
====================================================================================移除  1 软件包

安装大小：14 M
是否继续？[y/N]：y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在删除    : mysql-community-client-plugins-8.0.29-1.el7.x86_64              1/1 
  验证中      : mysql-community-client-plugins-8.0.29-1.el7.x86_64              1/1 

删除:
  mysql-community-client-plugins.x86_64 0:8.0.29-1.el7                              

完毕！


[root@blog00 ~]# rpm -qa|grep -i mysql
mysql-community-common-8.0.29-1.el7.x86_64
[root@blog00 ~]# yum remove mysql-community-common.x86_64 
已加载插件：fastestmirror, langpacks
正在解决依赖关系
--> 正在检查事务
---> 软件包 mysql-community-common.x86_64.0.8.0.29-1.el7 将被 删除
--> 解决依赖关系完成

依赖关系解决

==================================================================================== Package                      架构         版本               源               大小
====================================================================================正在删除:
 mysql-community-common       x86_64       8.0.29-1.el7       installed       9.6 M

事务概要
====================================================================================移除  1 软件包

安装大小：9.6 M
是否继续？[y/N]：y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在删除    : mysql-community-common-8.0.29-1.el7.x86_64                      1/1 
  验证中      : mysql-community-common-8.0.29-1.el7.x86_64                      1/1 

删除:
  mysql-community-common.x86_64 0:8.0.29-1.el7                                      

完毕！
[root@blog00 ~]# 
```


### 4. 查询并彻底删除剩余安装包及其相关文件
```
[root@blog00 ~]# rpm -qa|grep mysql

[root@blog00 ~]# ls
anaconda-ks.cfg
initial-setup-ks.cfg
mysql-8.0.29-1.el7.x86_64.rpm-bundle.tar
mysql-community-client-8.0.29-1.el7.x86_64.rpm
mysql-community-client-plugins-8.0.29-1.el7.x86_64.rpm
mysql-community-common-8.0.29-1.el7.x86_64.rpm
mysql-community-debuginfo-8.0.29-1.el7.x86_64.rpm
mysql-community-devel-8.0.29-1.el7.x86_64.rpm
mysql-community-embedded-compat-8.0.29-1.el7.x86_64.rpm
mysql-community-icu-data-files-8.0.29-1.el7.x86_64.rpm
mysql-community-libs-8.0.29-1.el7.x86_64.rpm
mysql-community-libs-compat-8.0.29-1.el7.x86_64.rpm
mysql-community-server-8.0.29-1.el7.x86_64.rpm
mysql-community-server-debug-8.0.29-1.el7.x86_64.rpm
mysql-community-test-8.0.29-1.el7.x86_64.rpm

[root@blog00 ~]# rm -rf mysql*
[root@blog00 ~]# ls
anaconda-ks.cfg       
initial-setup-ks.cfg 

[root@blog00 ~]# find / -name mysql
/etc/selinux/targeted/active/modules/100/mysql
/var/lib/mysql
/var/lib/mysql/mysql
/usr/lib64/mysql

[root@blog00 ~]# rm -rf /var/lib/mysql/
[root@blog00 ~]# rm -rf /usr/lib64/mysql/

[root@blog00 ~]# rpm -qa|grep mysql
[root@blog00 ~]# find / -name mysql
/etc/selinux/targeted/active/modules/100/mysql

[root@blog00 ~]# rm -rf /etc/selinux/targeted/active/modules/100/mysql/
[root@blog00 ~]# rpm -qa|grep mysql
[root@blog00 ~]# find / -name mysql

```


---
## 三、下载安装docker
**[docker官网：https://www.docker.com/](https://www.docker.com/)**
**[install on centos官方文档：https://docs.docker.com/engine/install/centos/](https://docs.docker.com/engine/install/centos/)**

### 1. 若电脑上原有docker，卸载旧版本的docker
```
yum remove docker \
           docker-client \
           docker-client-latest \
           docker-common \
           docker-latest \
           docker-latest-logrotate \
           docker-logrotate \
           docker-engine

```

### 2. 下载安装docker
### 3. 更新yum并配置阿里云的yum源
```
[root@blog00 ~]# yum install -y yum-utils
已加载插件：fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * base: mirrors.huaweicloud.com
 * extras: mirrors.huaweicloud.com
 * updates: mirrors.huaweicloud.com
软件包 yum-utils-1.1.31-54.el7_8.noarch 已安装并且是最新版本
无须任何处理

[root@blog00 ~]# yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
已加载插件：fastestmirror, langpacks
adding repo from: https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
grabbing file https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo to /etc/yum.repos.d/docker-ce.repo
repo saved to /etc/yum.repos.d/docker-ce.repo
```
### 4. 下载最新版本的docker
```
[root@blog00 ~]# yum install -y docker-ce
已加载插件：fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * base: mirrors.huaweicloud.com
 * extras: mirrors.huaweicloud.com
 * updates: mirrors.huaweicloud.com
正在解决依赖关系
--> 正在检查事务
---> 软件包 docker-ce.x86_64.3.20.10.17-3.el7 将被 安装
--> 正在处理依赖关系 container-selinux >= 2:2.74，它被软件包 3:docker-ce-20.10.17-3.el7.x86_64 需要
--> 正在处理依赖关系 containerd.io >= 1.4.1，它被软件包 3:docker-ce-20.10.17-3.el7.x86_64 需要
--> 正在处理依赖关系 docker-ce-cli，它被软件包 3:docker-ce-20.10.17-3.el7.x86_64 需要
--> 正在处理依赖关系 docker-ce-rootless-extras，它被软件包 3:docker-ce-20.10.17-3.el7.x86_64 需要
--> 正在检查事务
---> 软件包 container-selinux.noarch.2.2.119.2-1.911c772.el7_8 将被 安装
---> 软件包 containerd.io.x86_64.0.1.6.6-3.1.el7 将被 安装
---> 软件包 docker-ce-cli.x86_64.1.20.10.17-3.el7 将被 安装
--> 正在处理依赖关系 docker-scan-plugin(x86-64)，它被软件包 1:docker-ce-cli-20.10.17-3.el7.x86_64 需要
---> 软件包 docker-ce-rootless-extras.x86_64.0.20.10.17-3.el7 将被 安装
--> 正在处理依赖关系 fuse-overlayfs >= 0.7，它被软件包 docker-ce-rootless-extras-20.10.17-3.el7.x86_64 需要
--> 正在处理依赖关系 slirp4netns >= 0.4，它被软件包 docker-ce-rootless-extras-20.10.17-3.el7.x86_64 需要
--> 正在检查事务
---> 软件包 docker-scan-plugin.x86_64.0.0.17.0-3.el7 将被 安装
---> 软件包 fuse-overlayfs.x86_64.0.0.7.2-6.el7_8 将被 安装
--> 正在处理依赖关系 libfuse3.so.3(FUSE_3.2)(64bit)，它被软件包 fuse-overlayfs-0.7.2-6.el7_8.x86_64 需要
--> 正在处理依赖关系 libfuse3.so.3(FUSE_3.0)(64bit)，它被软件包 fuse-overlayfs-0.7.2-6.el7_8.x86_64 需要
--> 正在处理依赖关系 libfuse3.so.3()(64bit)，它被软件包 fuse-overlayfs-0.7.2-6.el7_8.x86_64 需要
---> 软件包 slirp4netns.x86_64.0.0.4.3-4.el7_8 将被 安装
--> 正在检查事务
---> 软件包 fuse3-libs.x86_64.0.3.6.1-4.el7 将被 安装
--> 解决依赖关系完成

依赖关系解决

==================================================================================== Package                   架构   版本                       源                大小
====================================================================================正在安装:
 docker-ce                 x86_64 3:20.10.17-3.el7           docker-ce-stable  22 M
为依赖而安装:
 container-selinux         noarch 2:2.119.2-1.911c772.el7_8  extras            40 k
 containerd.io             x86_64 1.6.6-3.1.el7              docker-ce-stable  33 M
 docker-ce-cli             x86_64 1:20.10.17-3.el7           docker-ce-stable  29 M
 docker-ce-rootless-extras x86_64 20.10.17-3.el7             docker-ce-stable 8.2 M
 docker-scan-plugin        x86_64 0.17.0-3.el7               docker-ce-stable 3.7 M
 fuse-overlayfs            x86_64 0.7.2-6.el7_8              extras            54 k
 fuse3-libs                x86_64 3.6.1-4.el7                extras            82 k
 slirp4netns               x86_64 0.4.3-4.el7_8              extras            81 k

事务概要
====================================================================================安装  1 软件包 (+8 依赖软件包)

总下载量：97 M
安装大小：394 M
Downloading packages:
警告：/var/cache/yum/x86_64/7/extras/packages/container-selinux-2.119.2-1.911c772.el7_8.noarch.rpm: 头V3 RSA/SHA256 Signature, 密钥 ID f4a80eb5: NOKEY
container-selinux-2.119.2-1.911c772.el7_8.noarch.rpm 的公钥尚未安装
(1/9): container-selinux-2.119.2-1.911c772.el7_8.noarch.rpm  |  40 kB  00:00:00     
警告：/var/cache/yum/x86_64/7/docker-ce-stable/packages/docker-ce-20.10.17-3.el7.x86_64.rpm: 头V4 RSA/SHA512 Signature, 密钥 ID 621e9f35: NOKEY
docker-ce-20.10.17-3.el7.x86_64.rpm 的公钥尚未安装
(2/9): docker-ce-20.10.17-3.el7.x86_64.rpm                   |  22 MB  00:00:29     
(3/9): containerd.io-1.6.6-3.1.el7.x86_64.rpm                |  33 MB  00:00:44     
(4/9): docker-ce-rootless-extras-20.10.17-3.el7.x86_64.rpm   | 8.2 MB  00:00:11     
(5/9): fuse-overlayfs-0.7.2-6.el7_8.x86_64.rpm               |  54 kB  00:00:00     
(6/9): slirp4netns-0.4.3-4.el7_8.x86_64.rpm                  |  81 kB  00:00:00     
(7/9): fuse3-libs-3.6.1-4.el7.x86_64.rpm                     |  82 kB  00:00:00     
(8/9): docker-scan-plugin-0.17.0-3.el7.x86_64.rpm            | 3.7 MB  00:00:05     
(9/9): docker-ce-cli-20.10.17-3.el7.x86_64.rpm               |  29 MB  00:00:42     
------------------------------------------------------------------------------------总计                                                   1.4 MB/s |  97 MB  01:11     
从 file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7 检索密钥
导入 GPG key 0xF4A80EB5:
 用户ID     : "CentOS-7 Key (CentOS 7 Official Signing Key) <security@centos.org>"
 指纹       : 6341 ab27 53d7 8a78 a7c2 7bb1 24c6 a8a7 f4a8 0eb5
 软件包     : centos-release-7-9.2009.0.el7.centos.x86_64 (@anaconda)
 来自       : /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
从 https://mirrors.aliyun.com/docker-ce/linux/centos/gpg 检索密钥
导入 GPG key 0x621E9F35:
 用户ID     : "Docker Release (CE rpm) <docker@docker.com>"
 指纹       : 060a 61c5 1b55 8a7f 742b 77aa c52f eb6b 621e 9f35
 来自       : https://mirrors.aliyun.com/docker-ce/linux/centos/gpg
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在安装    : docker-scan-plugin-0.17.0-3.el7.x86_64                          1/9 
  正在安装    : 1:docker-ce-cli-20.10.17-3.el7.x86_64                           2/9 
  正在安装    : 2:container-selinux-2.119.2-1.911c772.el7_8.noarch              3/9 
  正在安装    : containerd.io-1.6.6-3.1.el7.x86_64                              4/9 
  正在安装    : slirp4netns-0.4.3-4.el7_8.x86_64                                5/9 
  正在安装    : fuse3-libs-3.6.1-4.el7.x86_64                                   6/9 
  正在安装    : fuse-overlayfs-0.7.2-6.el7_8.x86_64                             7/9 
  正在安装    : 3:docker-ce-20.10.17-3.el7.x86_64                               8/9 
  正在安装    : docker-ce-rootless-extras-20.10.17-3.el7.x86_64                 9/9 
  验证中      : fuse3-libs-3.6.1-4.el7.x86_64                                   1/9 
  验证中      : containerd.io-1.6.6-3.1.el7.x86_64                              2/9 
  验证中      : docker-ce-rootless-extras-20.10.17-3.el7.x86_64                 3/9 
  验证中      : 1:docker-ce-cli-20.10.17-3.el7.x86_64                           4/9 
  验证中      : slirp4netns-0.4.3-4.el7_8.x86_64                                5/9 
  验证中      : 2:container-selinux-2.119.2-1.911c772.el7_8.noarch              6/9 
  验证中      : 3:docker-ce-20.10.17-3.el7.x86_64                               7/9 
  验证中      : docker-scan-plugin-0.17.0-3.el7.x86_64                          8/9 
  验证中      : fuse-overlayfs-0.7.2-6.el7_8.x86_64                             9/9 

已安装:
  docker-ce.x86_64 3:20.10.17-3.el7                                                 

作为依赖被安装:
  container-selinux.noarch 2:2.119.2-1.911c772.el7_8                                
  containerd.io.x86_64 0:1.6.6-3.1.el7                                              
  docker-ce-cli.x86_64 1:20.10.17-3.el7                                             
  docker-ce-rootless-extras.x86_64 0:20.10.17-3.el7                                 
  docker-scan-plugin.x86_64 0:0.17.0-3.el7                                          
  fuse-overlayfs.x86_64 0:0.7.2-6.el7_8                                             
  fuse3-libs.x86_64 0:3.6.1-4.el7                                                   
  slirp4netns.x86_64 0:0.4.3-4.el7_8                                                

完毕！
```
### 5. 启动docker并设置为开机自启动
```
[root@blog00 ~]# systemctl start docker
[root@blog00 ~]# systemctl enable docker
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
```
### 6. 查看docker版本和状态
```
[root@blog00 ~]# docker version
Client: Docker Engine - Community
 Version:           20.10.17
 API version:       1.41
 Go version:        go1.17.11
 Git commit:        100c701
 Built:             Mon Jun  6 23:05:12 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.17
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.17.11
  Git commit:       a89b842
  Built:            Mon Jun  6 23:03:33 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.6
  GitCommit:        10c12954828e7c7c9b6e0ea9b0c02b01407d3ae1
 runc:
  Version:          1.1.2
  GitCommit:        v1.1.2-0-ga916309
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

[root@blog00 ~]# systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since 二 2022-07-05 13:54:56 CST; 21min ago
     Docs: https://docs.docker.com
 Main PID: 10451 (dockerd)
   CGroup: /system.slice/docker.service
           └─10451 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/contain...
7月 05 13:54:55 blog00 dockerd[10451]: time="2022-07-05T13:54:55.766895745+08:...pc
7月 05 13:54:55 blog00 dockerd[10451]: time="2022-07-05T13:54:55.766900005+08:...pc
7月 05 13:54:55 blog00 dockerd[10451]: time="2022-07-05T13:54:55.780607945+08:...."
7月 05 13:54:56 blog00 dockerd[10451]: time="2022-07-05T13:54:56.068928870+08:...s"
7月 05 13:54:56 blog00 dockerd[10451]: time="2022-07-05T13:54:56.120704619+08:...g"
7月 05 13:54:56 blog00 dockerd[10451]: time="2022-07-05T13:54:56.166920644+08:...."
7月 05 13:54:56 blog00 dockerd[10451]: time="2022-07-05T13:54:56.185253005+08:...17
7月 05 13:54:56 blog00 dockerd[10451]: time="2022-07-05T13:54:56.185377856+08:...n"
7月 05 13:54:56 blog00 systemd[1]: Started Docker Application Container Engine.
7月 05 13:54:56 blog00 dockerd[10451]: time="2022-07-05T13:54:56.197419780+08:...k"
Hint: Some lines were ellipsized, use -l to show in full.

```

### 7. 更换为阿里云镜像源
**[阿里云镜像加速器：https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)需要个人注册**
```
[root@blog00 ~]# vim /etc/docker/daemon.json

{
  "registry-mirrors": ["https://moagjanz.mirror.aliyuncs.com"]
}

:wq
```
### 8. 重启docker
```
[root@blog00 ~]# systemctl restart docker

```
---

## 四、通过docker安装mysql
### 1. 拉取最新mysql
**[查看最新mysql源：https://hub.docker.com/_/mysql?tab=tags&page=1&ordering=last_updated](https://hub.docker.com/_/mysql?tab=tags&page=1&ordering=last_updated)**

```
[root@blog00 ~]# docker pull mysql:8.0.29-oracle
8.0.29-oracle: Pulling from library/mysql
337897a5aaf7: Pull complete 
aab90e595994: Pull complete 
ca3fb1d80e26: Pull complete 
1c39c2294433: Pull complete 
05d4e890a735: Pull complete 
1aa3c9d5a27e: Pull complete 
33be0cdbeece: Pull complete 
c24746683571: Pull complete 
bb7fdf5b1139: Pull complete 
81ca0a906ad0: Pull complete 
Digest: sha256:e9a9bb6104727e8936450b3c03a4b2b5256a302ad23f6ab3e5507b83f5f4b8b5
Status: Downloaded newer image for mysql:8.0.29-oracle
docker.io/library/mysql:8.0.29-oracle

[root@blog00 ~]# docker images
REPOSITORY   TAG             IMAGE ID       CREATED      SIZE
mysql        8.0.29-oracle   dc55abd4f242   4 days ago   444MB
```

### 2. 创建并运行容器
```
[root@blog00 ~]# docker run --name mysql -d -p 3307:3306 --restart=always -e MYSQL_ROOT_PASSWORD=123456 mysql:8.0.29-oracle 
8273efb723520a18c05af295540c6ecfd4113748cfacef2441911c8df5a7bf42
```
**语法:**`docker run [--name containerName] -d -p 3307:3306[--restart=always]imageName[:tag]`
其中`containerName`表示自定义容器名；    
`-d`表示后台运行；    
`-p`表示端口映射到容器内部端口，3307是外部访问docker的端口，3306是docker内部访问mysql的端口；  
`MYSQL_ROOT_PASSWORD=123456`表示设置数据库初始密码  
`--restart=always`表示当前容器随docker重启而自动启动，加上前文设置，即代表随系统重启而启动  
`imageName`表示镜像名称；  
`tag`表示镜像版本；

### 3. 开放3307端口，用以远程访问mysql
```
#添加端口
[root@blog00 ~]# firewall-cmd --zone=public --add-port=3307/tcp --permanent 
success
#重新加载
[root@blog00 ~]# firewall-cmd --reload
success
```
### 4. 查看容器日志
```
[root@blog00 ~]# docker logs mysql
2022-07-05 06:50:52+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.29-1.el8 started.
...省略...
```
### 5. 查看容器状态,可以确定mysql已启动
```
[root@blog00 ~]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                                                  NAMES
8273efb72352   mysql:8.0.29-oracle   "docker-entrypoint.s…"   17 minutes ago   Up 17 minutes   33060/tcp, 0.0.0.0:3307->3306/tcp, :::3307->3306/tcp   mysql
[root@blog00 ~]# 
```

### 6. 进入容器内部，进入并查看mysql数据库
```
[root@blog00 ~]# docker exec -it mysql bash
bash-4.4# mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.29 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)

mysql> exit
Bye
bash-4.4# exit
exit
[root@blog00 ~]# 
```
---
## 五、通过docker卸载mysql
### 1. 停止容器
```
#查看容器
[root@blog00 ~]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                                                  NAMES
8273efb72352   mysql:8.0.29-oracle   "docker-entrypoint.s…"   51 minutes ago   Up 51 minutes   33060/tcp, 0.0.0.0:3307->3306/tcp, :::3307->3306/tcp   mysql
#停止容器
[root@blog00 ~]# docker stop mysql
mysql
[root@blog00 ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

### 2. 删除容器
```
#查看容器
[root@blog00 ~]# docker ps -a
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS                      PORTS     NAMES
8273efb72352   mysql:8.0.29-oracle   "docker-entrypoint.s…"   53 minutes ago   Exited (0) 37 seconds ago             mysql
#删除容器mysql
[root@blog00 ~]# docker rm  mysql
mysql
[root@blog00 ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
### 3. 删除mysql镜像
```
#查看镜像
[root@blog00 ~]# docker images
REPOSITORY   TAG             IMAGE ID       CREATED      SIZE
mysql        8.0.29-oracle   dc55abd4f242   4 days ago   444MB
#删除对应的镜像
[root@blog00 ~]# docker rmi dc55abd4f242
Untagged: mysql:8.0.29-oracle
Untagged: mysql@sha256:e9a9bb6104727e8936450b3c03a4b2b5256a302ad23f6ab3e5507b83f5f4b8b5
Deleted: sha256:dc55abd4f24284f81521e19ffeea65f3def1b08b3f327b18717e6417288d52b5
Deleted: sha256:c7a55bb75b5b4485c0c729d85e7f60208baf466c65d7a889d1670fc1f7fb86c9
Deleted: sha256:a8a1e9fd7b7639168b164d5382949896aaf40a49f065973fba38500c580eca42
Deleted: sha256:1b5acfe05432b77a6eac31c2d4e23fd061fbed07dd7ee4c813ec7ca90fbfc8b9
Deleted: sha256:bf34b85d9b9d1456486926eb440de5a7b9247fad34f0698780ec5c5deaaac6c0
Deleted: sha256:801cb29ab0cc59d0d9860c7bd94666df9f30bd9fc6a67ba65bae9bd38c3c9404
Deleted: sha256:39d08834f7c7f62325c0a2a21a8efe367693606cd4e7f0dc74858b460c345aa8
Deleted: sha256:5ff20c22c12833121c55ca587248f65ba968df189b84fa9b1fd3db50f54dad04
Deleted: sha256:76057d27d5743c051f5723168ad1342c049c3fc464e2babd8b19f09a5a8a11f0
Deleted: sha256:9e6f324473c07a1461527d607b43350d790188364a27f8a89f163339828b81a7
Deleted: sha256:a13278944e6e2e2a3c673f61f3d751743554192e8655d1438f889c8df5d4f6fd

[root@blog00 ~]# docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```
---

## 六、总结
**通过docker安装下载部署mysql更方便快捷，是现在以及未来的趋势，但也不能忘记传统方式下载安装部署mysql的方式**

---

#### END
