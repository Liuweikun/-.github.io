# Centos 网络配置
---
### VMware提供三种网络连接方式
* **桥接模式**
    虚拟机直接连接外部网络的模式，主机起到网桥作用。这种模式下，虚拟机可以直接访问外部网络，并且对外部网络是可见的。
* **NAT**
    虚拟机与主机构建一个专用网络，并通过虚拟网络地址转换(NAT)设备对IP进行转换，虚拟机通过共享主机IP可以访问外部网络，但外部网络无法访问虚拟机
* **仅主机模式**
    虚拟机只与主机共享一个专用网络，与外部网络无法通信   

---
###修改网卡IP地址
**1. 进入root权限**
**2. 输入`ifconfig`**,找到确定你要更改的**网络接口名称****。
例如，这里我要更改的网口名称为`ens33`
```
[root@blog00 /]# ifconfig
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.118.129  netmask 255.255.255.0  broadcast 192.168.118.255
        inet6 fe80::c1ec:715:4335:104d  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:59:7e:46  txqueuelen 1000  (Ethernet)
        RX packets 9791  bytes 2431586 (2.3 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1935  bytes 392175 (382.9 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0 
```
**3. 进入并编辑网卡设置文件**
`[root@blog00 /]# vim /etc/sysconfig/network-scripts/ifcfg-ens33`
**查看原始网卡配置**：
```
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="dhcp"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="b89cfcaa-d65b-4ec1-93ae-6a0393e5c7b4"
DEVICE="ens33"
ONBOOT="yes"
~               
```
**4. 修改过程**
* 将动态IP改为目标静态IP  
  `BOOTPROTO="dhcp"`改为`BOOTPROTO="static"`
* 将开机不激活网卡改为开机启动激活网卡  
  `ONBOOT=no`改为`ONBOOT=yes`
并新增**IP地址、网关、DNS域名解析服务器IP地址**，最后修改为：
```
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="static"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="b89cfcaa-d65b-4ec1-93ae-6a0393e5c7b4"
DEVICE="ens33"
ONBOOT="yes"
#IP地址
IPADDR=192.168.118.128
#子网掩码
NETMASK=255.255.255.0
#网关
GATEWAY=192.168.118.2
#域名解析服务器
DNS1=192.168.118.2
DNS2=233.5.5.5
```
保存退出

**5. 重启网卡**
```
[root@blog00 ~]# service network restart 
Restarting network (via systemctl):                        [  确定  ]
```

**6. 查看修改后的网口信息**
```
[root@blog00 ~]# ifconfig
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.118.128  netmask 255.255.255.0  broadcast 192.168.118.255
        inet6 fe80::c1ec:715:4335:104d  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:59:7e:46  txqueuelen 1000  (Ethernet)
        RX packets 19418  bytes 4406437 (4.2 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 5528  bytes 948411 (926.1 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
**并检测是否连通**
```
PING baidu.com (220.181.38.148) 56(84) bytes of data.
64 bytes from 220.181.38.148 (220.181.38.148): icmp_seq=1 ttl=128 time=8.48 ms
64 bytes from 220.181.38.148 (220.181.38.148): icmp_seq=2 ttl=128 time=8.55 ms
64 bytes from 220.181.38.148 (220.181.38.148): icmp_seq=3 ttl=128 time=7.31 ms 

--- baidu.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 7.310/8.119/8.559/0.577 ms


[root@blog00 ~]# ping 192.168.118.2
PING 192.168.118.2 (192.168.118.2) 56(84) bytes of data.
64 bytes from 192.168.118.2: icmp_seq=1 ttl=128 time=0.078 ms
64 bytes from 192.168.118.2: icmp_seq=2 ttl=128 time=0.096 ms
64 bytes from 192.168.118.2: icmp_seq=3 ttl=128 time=0.177 ms

--- 192.168.118.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 1999ms
rtt min/avg/max/mdev = 0.078/0.117/0.177/0.043 ms
```
---
#### END





