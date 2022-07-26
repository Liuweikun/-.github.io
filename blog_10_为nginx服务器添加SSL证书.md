# 为nginx服务器部署SSL证书
---
## 一、准备工作
1. SSL证书与证书私钥
2. 网站有域名，且备案已经通过
3. nginx服务器，可以正常运行80端口

## 二、详细过程
### 1.查看防火墙情况，开启443端口(若为云服务器，则开启云服务器防火墙443端口)
```
[root@blog00 ~]# firewall-cmd --zone=public --add-port=443/tcp --permanent
success

[root@blog00 ~]# firewall-cmd --reload
success

[root@blog00 ~]# firewall-cmd --zone=public --query-port=443/tcp
yes

[root@blog00 ~]# firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: ens33
  sources: 
  services: dhcpv6-client ssh
  ports: 3307/tcp 8090/tcp 80/tcp 443/tcp
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
```
### 2.创建存放SSL证书文件夹，并通过SFTP工具将证书导入
```
[root@VM-20-7-centos ssl]# pwd
/usr/local/nginx/ssl

#导入后，一个是证书，一个是私钥
[root@VM-20-7-centos ssl]# ls
baixiaochun.xyz_bundle.crt  baixiaochun.xyz.key

#记录下两个文件的路径，后续有用
/usr/local/nginx/ssl/baixiaochun.xyz_bundle.crt
/usr/local/nginx/ssl/baixiaochun.xyz.key

```

### 3.查看nginx是否有SSL模块，并备份部分文件
```
查看发现nginx没有SSL模块
[root@VM-20-7-centos sbin]# ./nginx -V
nginx version: nginx/1.17.10
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC) 
configure arguments:

#停止运行nginx
[root@VM-20-7-centos sbin]# ./nginx -s stop

#备份nginx.conf文件
[root@VM-20-7-centos sbin]# cd ..
[root@VM-20-7-centos nginx]# cd conf/
[root@VM-20-7-centos conf]# cp nginx.conf nginx.text
```

### 4.切换到nginx安装目录，添加ssl模块
```
[root@VM-20-7-centos nginx-1.17.10]# ./configure \
> --prefix=/usr/local/nginx/ \
> --with-http_stub_status_module \
> --with-http_ssl_module

```

### 5.编译
```
#切记不要用make install
[root@VM-20-7-centos nginx-1.17.10]# make
```

### 6.备份源服务文件,并将刚编译好的nginx覆盖原有nginx
```
[root@VM-20-7-centos nginx-1.17.10]# mv /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.old

[root@VM-20-7-centos nginx-1.17.10]# ls
auto     CHANGES.ru  configure  html     Makefile  objs    src
CHANGES  conf        contrib    LICENSE  man       README

[root@VM-20-7-centos nginx-1.17.10]# cp ./objs/nginx /usr/local/nginx/sbin/nginx
```

### 7.查看nginx情况，并启动nginx查看
```
[root@VM-20-7-centos nginx-1.17.10]# cd /usr/local/nginx/sbin/
[root@VM-20-7-centos sbin]# ls
nginx  nginx.old

[root@VM-20-7-centos sbin]# ./nginx -V
nginx version: nginx/1.17.10
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC) 
built with OpenSSL 1.0.2k-fips  26 Jan 2017
TLS SNI support enabled
configure arguments: --prefix=/usr/local/nginx/ --with-http_stub_status_module --with-http_ssl_module

```

### 8.修改nginx.conf文件中ssl模块配置
```
[root@VM-20-7-centos conf]# pwd
/usr/local/nginx/conf
[root@VM-20-7-centos conf]# vim nginx.conf
# HTTPS server
    #
    server {
        listen       443 ssl;
        server_name  baixiaochun.xyz www.baixiaochun.xyz;

        ssl_certificate      /usr/local/nginx/ssl/baixiaochun.xyz_bundle.crt;
        ssl_certificate_key  /usr/local/nginx/ssl/baixiaochun.xyz.key;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;
        
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
        ssl_ciphers  ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
        ssl_prefer_server_ciphers  on;

        location / {
            proxy_pass http://blog;
            proxy_set_header HOST $host;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            
        }
    }

}
```

### 9.验证nginx配置文件是否有问题，一切顺利则重启nginx
```
[root@VM-20-7-centos conf]# cd /usr/local/nginx/sbin/
[root@VM-20-7-centos sbin]# ./nginx -t
nginx: the configuration file /usr/local/nginx//conf/nginx.conf syntax is ok
nginx: configuration file /usr/local/nginx//conf/nginx.conf test is successful

[root@VM-20-7-centos sbin]# ./nginx -s reload
```

### 10.访问网站，或通过浏览器测试，查看部署成功
```
[root@VM-20-7-centos sbin]# curl https://baixiaochun.xyz
[root@VM-20-7-centos sbin]# curl https://www.baixiaochun.xyz
```

### 11.设置HTTP跳转至HTTPS
```
#再次备份nginx.conf文件
[root@VM-20-7-centos conf]# cp nginx.conf nginx.text
cp: overwrite ‘nginx.text’? y

# 设置跳转:在http部分添加一个永久强制跳转命令
[root@VM-20-7-centos conf]# vim nginx.conf

 upstream blog{
        server 127.0.0.1:8090;
    }

    server {
        listen       80;
        listen       [::]:80;
        server_name  baixiaochun.xyz www.baixiaochun.xyz;
        client_max_body_size  500m;
        return 301 https://$host$request_uri;
```

### 12.检查无误后，重启nginx服务器
```
[root@VM-20-7-centos conf]# cd ../sbin/
[root@VM-20-7-centos sbin]# ./nginx -t
nginx: the configuration file /usr/local/nginx//conf/nginx.conf syntax is ok
nginx: configuration file /usr/local/nginx//conf/nginx.conf test is successful

[root@VM-20-7-centos sbin]# ./nginx -s reload

```
### 13.测试：用户输入http网站会强制301跳转至https网站
```
[root@VM-20-7-centos /]# curl I http://baixiaochun.xyz
curl: (6) Could not resolve host: I; Unknown error
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx/1.17.10</center>
</body>
</html>

[root@VM-20-7-centos /]# curl I http://www.baixiaochun.xyz
curl: (6) Could not resolve host: I; Unknown error
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx/1.17.10</center>
</body>
</html>
```
---
#### END



