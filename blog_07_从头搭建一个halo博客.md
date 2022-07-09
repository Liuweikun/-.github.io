# halo 博客搭建过程

---

### 一. JDK安装
```
[root@VM-20-7-centos ~]# yum install java-11-openjdk -y
……
[root@VM-20-7-centos ~]# java -version
openjdk version "11.0.15" 2022-04-19 LTS
OpenJDK Runtime Environment 18.9 (build 11.0.15+9-LTS)
OpenJDK 64-Bit Server VM 18.9 (build 11.0.15+9-LTS, mixed mode, sharing)
```
### 二. 将halo jar包与nginx安装包通过SFTP等工具上传至服务器
```
[root@VM-20-7-centos ~]# ls
halo-1.4.17.jar  nginx-1.17.10.tar.gz
[root@VM-20-7-centos ~]# pwd
/root
```

### 三. 在后台运行jar包
```
[root@VM-20-7-centos ~]# nohup java -jar halo-1.4.17.jar &
[1] 11656
[root@VM-20-7-centos ~]# nohup: ignoring input and appending output to ‘nohup.out’

[root@VM-20-7-centos ~]#
```

### 四. 安装nginx服务器

#### 1. 创建nginx文件夹并进入该文件夹
```
[root@VM-20-7-centos ~]# cd /usr/local
[root@VM-20-7-centos local]# mkdir nginx
[root@VM-20-7-centos local]# ls
bin  etc  games  include  lib  lib64  libexec  nginx  qcloud  sa  sbin  share  src
[root@VM-20-7-centos local]# cd nginx/
```

#### 2. 解压nginx安装包
```
[root@VM-20-7-centos nginx]# tar zxvf /root/nginx-1.17.10.tar.gz -C ./
……
[root@VM-20-7-centos nginx]# ls
nginx-1.17.10
```

#### 3. 安装依赖
```
[root@VM-20-7-centos nginx]# yum -y install pcre-devel
[root@VM-20-7-centos nginx]# yum -y install openssl openssl-devel
```

#### 4. 编译安装nginx
```
[root@VM-20-7-centos nginx]# cd nginx-1.17.10/
[root@VM-20-7-centos nginx-1.17.10]# ./configure
……
[root@VM-20-7-centos nginx-1.17.10]# make && make install
```

#### 5. 启动nginx服务
```
[root@VM-20-7-centos nginx-1.17.10]# cd /usr/local/nginx
[root@VM-20-7-centos nginx]# ls
conf  html  logs  nginx-1.17.10  sbin
[root@VM-20-7-centos nginx]# cd sbin
[root@VM-20-7-centos sbin]# ./nginx
```

#### 6. 使用nginx代理网站,修改配置文件
```
[root@VM-20-7-centos sbin]# cd /usr/local/nginx/conf/
[root@VM-20-7-centos conf]# ls
fastcgi.conf            koi-utf             nginx.conf           uwsgi_params
fastcgi.conf.default    koi-win             nginx.conf.default   uwsgi_params.default
fastcgi_params          mime.types          scgi_params          win-utf
fastcgi_params.default  mime.types.default  scgi_params.default

[root@VM-20-7-centos conf]# vim nginx.conf

server {
        listen       80;
        listen       [::]:80;
        server_name  baixiaochun.xyz;             #写你网站的域名
        client_max_body_size  1024m;            #上传速度解除默认的1m
        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            proxy_pass http://blog;
            proxy_set_header HOST $host;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            
        }
```

#### 7. 保存退出后重启nginx服务
```
[root@VM-20-7-centos conf]# cd /usr/local/nginx/sbin/
[root@VM-20-7-centos sbin]# ls
nginx
[root@VM-20-7-centos sbin]# ./nginx -s reload
```

### 五. 在云服务器商申请域名注册，通过后再申请备案，备案通过后，将网站域名与公网IP绑定即可通过域名访问网站，这里不再赘述，详情咨询云服务器商

---

#### END
