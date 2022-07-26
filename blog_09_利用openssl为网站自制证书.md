# 在centos系统利用openssl自制加密证书

---

## 一、准备CA机构证书

### 1. 利用openssl生成CA的私钥
```
[root@blog00 ~]# cd https
[root@blog00 https]# mkdir ca
[root@blog00 https]# cd ca

#生成CA机构的私钥
[root@blog00 ca]# openssl genrsa -des3 -out ca.key 2048
Generating RSA private key, 2048 bit long modulus
.........+++
......................................................................+++
e is 65537 (0x10001)
Enter pass phrase for ca.key:                                  #用des3算法为私钥加密，这里设置的是私钥的密码
Verifying - Enter pass phrase for ca.key:    admin      
[root@blog00 ca]# ls
ca.key
[root@blog00 ca]# cat ca.key              #查看私钥，是被des3算法加密过的，即使有人进入到了服务器后台，在不知道私钥的密码情况下，无法使用私钥
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,A65A4761E0B0B9DB

/AxDGVItQxlQNOp+jGnoNHF6/vrpjGEKjiXqhUoDIr6YSSFTH1iNUwlkx01ECmw0
yd1onotDeMV6px+48y99cMxBbIdNTmJ5/KxTBo86YVf3+cpvznPOdP/UTIKpHh1H
kK5ql/6K9JgvTfCijbT+talNS7Xa8f5GLkVbqpwn1U4X3GdlxXhc4mpi7HV7a5TI
BBcmP0kdUjAPCOw016aqYPGvaPcxQXm/9kNufBmy2JtZbB7gc2fTwQ51mXBE0IcZ
ihBD36+tR19+rwHw0BYUQM5/owJQ7ypvqDSmX0qTL84r7/TmIwQouwq9zaESSUWs
To+3bEsT0EW6N4dhXhJ/DIY4tCK8H6JOWMFV691TZPb+5vqajWs7PHzaUKPXHHmY
JwXzm97NqPFI68SgBMHzNBMBYVxI854/5HQphPFTynqFLgkL/Tr34s0/2DBupoGc
0JrSCZvCo6jo3rOq+betjMiSte4CPY86Soija9SJlXeTd7ftU4eDBG0FTFjLgI9t
SAJEf2TsRkoor61Q46t5SZCRZ03DciFCUsjjlzC3JMMfxTWRf98WWRlAwP1VTazX
zENwjnaTPVAQuADO3sd7VtS/Zp3tcYk9w0kwSHnm23bQlVB2F4dtdlLaLCu7mxKT
csSq/+me/pyxmh6ZPSkU37s/P7zc3L38IM84YaBKFwu5XkGMqY94SYMlT6DFZS0W
J19xwnEgbqrYJMiXajJLFe43UA37sNXA83YPXUgbnnxynjSReNVVvFVlk74dBVq1
HDcqX6rJgai6PMwMtlz+NnkAIUsCiZZJnfg3UBVrC5TEaQUezU9XpngNCUQbCTjR
E9qucI4Q2xu2Fr2zeVzflQ1pC685+4oxMzvu3UFyccsFBe68CTWo1VWhhaT8+UvN
aatq6ex19IBSKQ0bm8aAAv5gdzXUN26+fcAUMY5MdsopXeFhraPfv4Yo5qZ6+gYH
2fqQcLE7mJ7sB6ljcNrMUge6HIfH6bZgDuGGX/X0A4lzv6bWbM+48binBToNMMnx
QYToNf5zZZzVF/NXEMcEsNWfxzPF8Gc7rnhHneJKo1MagYJTD1bIZY2lZIC2WTVW
HdN/ukqe56lVynGSdogLIcgtz6QQDkOQVHtq3LWSrU9heNc5mQEvFe5PlrYkjYdO
GsXiifxZbP8nBm94egvKK7mPWJsrO/NNIPZkiN0HO8zaI/irJ/yK9CfBsWeZvDrP
l5vR8/u2fcs1bWlS/ePFSy7uqFnTumq9JIEEXbnX2ey2QiStPMEi4ftEpcuJw6iI
S9uUURqymu7EgDZyzxe5cdLw5O89Tha4x439LZ8CxrgzaRvui1L50Fcj5dW8LLNa
O2YHp2aSZ3uGiMVb7JHW7GKdFach6Fev1wcg6nPvmYmbxi7mIy2r46t9WSF6zC9t
cC2TGVejD7KNV9Y72ufh4BCke5lhkauICZJXUxzCpAOrbXUMuygMTweFjNqmu7RD
PfB5QniXDCT3l9l1+DAyqavgr6Iakxlkl0/ns5wOk/iGKDwaRlg2RS3QiDXjV6Kw
l9eQiH4GEA/JdkmetqvgkhsC6QTvR/Ug1bPrjUJ/SUa4neh5VlyPo+6Qs3Xkt9M6
-----END RSA PRIVATE KEY-----
```
---

### 2. 利用openssl生成CA的根证书
```
#利用openssl生成一个私钥为ca.key文件为基础的，采用x509格式的，有效期限为365天的，包含CA公钥的CA根证书
[root@blog00 ca]# openssl req -x509 -key ca.key -out ca.crt -days 365

#输入私钥的密码         
Enter pass phrase for ca.key:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
#输入CA机构所在国家
Country Name (2 letter code) [XX]:CN
#输入CA机构所在省份
State or Province Name (full name) []:BJ
#输入CA机构所在城市
Locality Name (eg, city) [Default City]:BJ
#输入CA机构所在公司名称
Organization Name (eg, company) [Default Company Ltd]:CATest
#输入CA机构所在公司具体单元部门
Organizational Unit Name (eg, section) []:test
#输入CA机构域名
Common Name (eg, your name or your server's hostname) []:www.test.com
#输入CA机构邮箱
Email Address []:test@test.com
[root@blog00 ca]# ls
ca.crt  ca.key
[root@blog00 ca]# cat ca.crt 
-----BEGIN CERTIFICATE-----
MIIDyzCCArOgAwIBAgIJAIHKX8Fyc/xYMA0GCSqGSIb3DQEBCwUAMHwxCzAJBgNV
BAYTAkNOMQswCQYDVQQIDAJCSjELMAkGA1UEBwwCQkoxDzANBgNVBAoMBkNBVGVz
dDENMAsGA1UECwwEdGVzdDEVMBMGA1UEAwwMd3d3LnRlc3QuY29tMRwwGgYJKoZI
hvcNAQkBFg10ZXN0QHRlc3QuY29tMB4XDTIyMDcxMDA5MzIzMloXDTIzMDcxMDA5
MzIzMlowfDELMAkGA1UEBhMCQ04xCzAJBgNVBAgMAkJKMQswCQYDVQQHDAJCSjEP
MA0GA1UECgwGQ0FUZXN0MQ0wCwYDVQQLDAR0ZXN0MRUwEwYDVQQDDAx3d3cudGVz
dC5jb20xHDAaBgkqhkiG9w0BCQEWDXRlc3RAdGVzdC5jb20wggEiMA0GCSqGSIb3
DQEBAQUAA4IBDwAwggEKAoIBAQDk24QLKwLCzg2jX5yThWsz3YoF07kntE6jbZnc
SHX9RPJFEOogBda/7fTfZhJrGoDJK9Q4q4z7VQUxNptl+9GdJ5Q4/cdlE63+PWtr
3TJq+KY7DpRHjmBntoGnxqI7VQTqiEGGVOxaIcjWlZXANaw87neVa3dTxzhkD8QX
FssOUx7qNWRIFoxt5UuVzINwlTFApunfnFhBhj6gMIW6mb+K805PUcxckxqV39F0
mtnNLFkR3weNvOAxYGtfwhGDxaGuSYjF9q20WrE3AzVIy9met2LoLEwjoYop1SSo
jT+jmgNYx7Y5NKGZpbOVURGqYEuL+h3KPVHa1R8DsFUWlOIHAgMBAAGjUDBOMB0G
A1UdDgQWBBT8w6RPrWjC8/mWly+icAUFeRIjUTAfBgNVHSMEGDAWgBT8w6RPrWjC
8/mWly+icAUFeRIjUTAMBgNVHRMEBTADAQH/MA0GCSqGSIb3DQEBCwUAA4IBAQAj
yXTGJ2TGm09BXOd9rvOSOp7R2g0ZUg/MK0n4++hcouvmDPe4yrksY6UpCwn+JiyV
0rP63snz4VBi+IwAD6B5kZlaDCbILBDpiucX8RsACN6Vdny3JJL17EuhRm8Pj1jX
6A3SQ4/qWcSheqWFKQU1j2PQycv6h45t4Cs0KwsLOAVBQ10Spwx29HKknTmgo7di
mfix+MN9G4ECpFrc09z8V5ZEbwk4ki2NpLzNM2mjuUl5aNjK7xbEJldRpKt1/Eyz
Hf1zxQfyxgreOQe8HXuEt+sHslNO/l5zNsaENLS8eahd76g0IzMsfKiPIYRxQ7lv
SaWhghi61h9eF58D5PRt
-----END CERTIFICATE-----

#通过命令查看根证书内容
[root@blog00 ca]# openssl x509 -in ca.crt -text -noout
Certificate:
    Data:
        Version: 3 (0x2)                   #版本号
        Serial Number:                     #这个证书的序列号
            81:ca:5f:c1:72:73:fc:58        
    Signature Algorithm: sha256WithRSAEncryption      #签名算法
        Issuer: C=CN, ST=BJ, L=BJ, O=CATest, OU=test, CN=www.test.com/emailAddress=test@test.com           #CA机构的标识信息
        Validity
            Not Before: Jul 10 09:32:32 2022 GMT       #有效期
            Not After : Jul 10 09:32:32 2023 GMT
        Subject: C=CN, ST=BJ, L=BJ, O=CATest, OU=test, CN=www.test.com/emailAddress=test@test.com            #证书拥有者的标识信息
        Subject Public Key Info:                         #公钥
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:e4:db:84:0b:2b:02:c2:ce:0d:a3:5f:9c:93:85:
                    6b:33:dd:8a:05:d3:b9:27:b4:4e:a3:6d:99:dc:48:
                    75:fd:44:f2:45:10:ea:20:05:d6:bf:ed:f4:df:66:
                    12:6b:1a:80:c9:2b:d4:38:ab:8c:fb:55:05:31:36:
                    9b:65:fb:d1:9d:27:94:38:fd:c7:65:13:ad:fe:3d:
                    6b:6b:dd:32:6a:f8:a6:3b:0e:94:47:8e:60:67:b6:
                    81:a7:c6:a2:3b:55:04:ea:88:41:86:54:ec:5a:21:
                    c8:d6:95:95:c0:35:ac:3c:ee:77:95:6b:77:53:c7:
                    38:64:0f:c4:17:16:cb:0e:53:1e:ea:35:64:48:16:
                    8c:6d:e5:4b:95:cc:83:70:95:31:40:a6:e9:df:9c:
                    58:41:86:3e:a0:30:85:ba:99:bf:8a:f3:4e:4f:51:
                    cc:5c:93:1a:95:df:d1:74:9a:d9:cd:2c:59:11:df:
                    07:8d:bc:e0:31:60:6b:5f:c2:11:83:c5:a1:ae:49:
                    88:c5:f6:ad:b4:5a:b1:37:03:35:48:cb:d9:9e:b7:
                    62:e8:2c:4c:23:a1:8a:29:d5:24:a8:8d:3f:a3:9a:
                    03:58:c7:b6:39:34:a1:99:a5:b3:95:51:11:aa:60:
                    4b:8b:fa:1d:ca:3d:51:da:d5:1f:03:b0:55:16:94:
                    e2:07
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Subject Key Identifier: 
                FC:C3:A4:4F:AD:68:C2:F3:F9:96:97:2F:A2:70:05:05:79:12:23:51
            X509v3 Authority Key Identifier: 
                keyid:FC:C3:A4:4F:AD:68:C2:F3:F9:96:97:2F:A2:70:05:05:79:12:23:51

            X509v3 Basic Constraints: 
                CA:TRUE
    Signature Algorithm: sha256WithRSAEncryption           #签名算法
         23:c9:74:c6:27:64:c6:9b:4f:41:5c:e7:7d:ae:f3:92:3a:9e:
         d1:da:0d:19:52:0f:cc:2b:49:f8:fb:e8:5c:a2:eb:e6:0c:f7:
         b8:ca:b9:2c:63:a5:29:0b:09:fe:26:2c:95:d2:b3:fa:de:c9:
         f3:e1:50:62:f8:8c:00:0f:a0:79:91:99:5a:0c:26:c8:2c:10:
         e9:8a:e7:17:f1:1b:00:08:de:95:76:7c:b7:24:92:f5:ec:4b:
         a1:46:6f:0f:8f:58:d7:e8:0d:d2:43:8f:ea:59:c4:a1:7a:a5:
         85:29:05:35:8f:63:d0:c9:cb:fa:87:8e:6d:e0:2b:34:2b:0b:
         0b:38:05:41:43:5d:12:a7:0c:76:f4:72:a4:9d:39:a0:a3:b7:
         62:99:f8:b1:f8:c3:7d:1b:81:02:a4:5a:dc:d3:dc:fc:57:96:
         44:6f:09:38:92:2d:8d:a4:bc:cd:33:69:a3:b9:49:79:68:d8:
         ca:ef:16:c4:26:57:51:a4:ab:75:fc:4c:b3:1d:fd:73:c5:07:
         f2:c6:0a:de:39:07:bc:1d:7b:84:b7:eb:07:b2:53:4e:fe:5e:
         73:36:c6:84:34:b4:bc:79:a8:5d:ef:a8:34:23:33:2c:7c:a8:
         8f:21:84:71:43:b9:6f:49:a5:a1:82:18:ba:d6:1f:5e:17:9f:
         03:e4:f4:6d
```

---


## 二、生成网站的签名请求证书

### 1. 利用openssl生成网站私钥
```
[root@blog00 https]# mkdir myblog
[root@blog00 https]# ls
ca  myblog
[root@blog00 https]# cd myblog/
[root@blog00 myblog]# openssl genrsa -des3 -out myblog.com.key 2048
Generating RSA private key, 2048 bit long modulus
....................................................................................+++
.............................+++
e is 65537 (0x10001)
Enter pass phrase for myblog.com.key:                     #网站私钥密码
Verifying - Enter pass phrase for myblog.com.key:   
```

### 2.利用openssl生成签名请求文件
```
#通过私钥生成网站签名请求文件
[root@blog00 myblog]# openssl req -new -key myblog.com.key -out myblog.com.csr
Enter pass phrase for myblog.com.key:           #网站私钥密码
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:CN
State or Province Name (full name) []:BJ
Locality Name (eg, city) [Default City]:CP
Organization Name (eg, company) [Default Company Ltd]:myblog
Organizational Unit Name (eg, section) []:test
Common Name (eg, your name or your server's hostname) []:myblog.com              #域名
Email Address []:test@myblog.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:                 
An optional company name []:             
```

### 3.查看生成的签名请求证书文件
```
#直接打开无法查看
[root@blog00 myblog]# cat myblog.com.csr 
-----BEGIN CERTIFICATE REQUEST-----
MIICwTCCAakCAQAwfDELMAkGA1UEBhMCQ04xCzAJBgNVBAgMAkJKMQswCQYDVQQH
DAJDUDEPMA0GA1UECgwGbXlibG9nMQ0wCwYDVQQLDAR0ZXN0MRMwEQYDVQQDDApt
eWJsb2cuY29tMR4wHAYJKoZIhvcNAQkBFg90ZXN0QG15YmxvZy5jb20wggEiMA0G
CSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC273PPbJxHwFEa0RspSTnh2YmIbtpD
PWUmJAWzVGs6v8e1Nn6uKxnZZQQrzBmdne6c+PmQXL/e/bdpd09yGRtSUoKGVhcR
HMvUV8NqC1Lb/T6UxQGcD8vqYtNoeG4x8MaiTxQEgkzzxudkwP4gm22pN44ETFTP
6A9XF8kbuL/KthkZP/UIASJPaq247W2k5D9N0Ej8ZiNfCWSFrixaPQRNAbciuKDI
QQLkfrAeqGjgd1CZe32tb/+Y59ypWH+SfcGlJFFFRZGxy8/xzuZTNw44WKVQKLsK
8aW9m9s4ZQaSy71A8E9Fz7KR8DONw7zHZt4vDUGZIzsn98AAOWaknoOfAgMBAAGg
ADANBgkqhkiG9w0BAQsFAAOCAQEAJZz7sVIqeHAwg5vP+kOV1cpX5ewCVElORf0M
mzZMukJcPTR/zU4ntir1xJ6SGMmjUQQNCKXe4PatQHG4YkA/xRW58fNoFTGIkwsH
aAAE9Qx76Vdnv5wdOarK0pTH8kAxvQMTIHTC/yNmXR2VGVku2aTn4SjQ3fPE6Xad
+NrEJXa12g3FmRQd7VFHL0wkOw/YisEmChaX5dNok5Nfp0jC9YaRQhmbMFD0nfw6
k68XK2yFMYeSMOTbV9ztbjDNDiJhe9uM3UU1hdY6tbVdLWfG6nxe98G2doV+7/hk
TXREecunhK1zprBn5dhwo1x7qVtqIN6MNn9btEv4ZBR3lQxCyw==
-----END CERTIFICATE REQUEST-----

[root@blog00 myblog]# openssl req -text -noout -verify -in myblog.com.csr
verify OK
Certificate Request:                              #证书请求说明
    Data:
        Version: 0 (0x0)
        Subject: C=CN, ST=BJ, L=CP, O=myblog, OU=test,  CN=myblog.com/emailAddress=test@myblog.com        网站基本信息
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption     #网站公钥
                Public-Key: (2048 bit)
                Modulus:
                    00:b6:ef:73:cf:6c:9c:47:c0:51:1a:d1:1b:29:49:
                    39:e1:d9:89:88:6e:da:43:3d:65:26:24:05:b3:54:
                    6b:3a:bf:c7:b5:36:7e:ae:2b:19:d9:65:04:2b:cc:
                    19:9d:9d:ee:9c:f8:f9:90:5c:bf:de:fd:b7:69:77:
                    4f:72:19:1b:52:52:82:86:56:17:11:1c:cb:d4:57:
                    c3:6a:0b:52:db:fd:3e:94:c5:01:9c:0f:cb:ea:62:
                    d3:68:78:6e:31:f0:c6:a2:4f:14:04:82:4c:f3:c6:
                    e7:64:c0:fe:20:9b:6d:a9:37:8e:04:4c:54:cf:e8:
                    0f:57:17:c9:1b:b8:bf:ca:b6:19:19:3f:f5:08:01:
                    22:4f:6a:ad:b8:ed:6d:a4:e4:3f:4d:d0:48:fc:66:
                    23:5f:09:64:85:ae:2c:5a:3d:04:4d:01:b7:22:b8:
                    a0:c8:41:02:e4:7e:b0:1e:a8:68:e0:77:50:99:7b:
                    7d:ad:6f:ff:98:e7:dc:a9:58:7f:92:7d:c1:a5:24:
                    51:45:45:91:b1:cb:cf:f1:ce:e6:53:37:0e:38:58:
                    a5:50:28:bb:0a:f1:a5:bd:9b:db:38:65:06:92:cb:
                    bd:40:f0:4f:45:cf:b2:91:f0:33:8d:c3:bc:c7:66:
                    de:2f:0d:41:99:23:3b:27:f7:c0:00:39:66:a4:9e:
                    83:9f
                Exponent: 65537 (0x10001)
        Attributes:
            a0:00
    Signature Algorithm: sha256WithRSAEncryption                 #签名算法
         25:9c:fb:b1:52:2a:78:70:30:83:9b:cf:fa:43:95:d5:ca:57:
         e5:ec:02:54:49:4e:45:fd:0c:9b:36:4c:ba:42:5c:3d:34:7f:
         cd:4e:27:b6:2a:f5:c4:9e:92:18:c9:a3:51:04:0d:08:a5:de:
         e0:f6:ad:40:71:b8:62:40:3f:c5:15:b9:f1:f3:68:15:31:88:
         93:0b:07:68:00:04:f5:0c:7b:e9:57:67:bf:9c:1d:39:aa:ca:
         d2:94:c7:f2:40:31:bd:03:13:20:74:c2:ff:23:66:5d:1d:95:
         19:59:2e:d9:a4:e7:e1:28:d0:dd:f3:c4:e9:76:9d:f8:da:c4:
         25:76:b5:da:0d:c5:99:14:1d:ed:51:47:2f:4c:24:3b:0f:d8:
         8a:c1:26:0a:16:97:e5:d3:68:93:93:5f:a7:48:c2:f5:86:91:
         42:19:9b:30:50:f4:9d:fc:3a:93:af:17:2b:6c:85:31:87:92:
         30:e4:db:57:dc:ed:6e:30:cd:0e:22:61:7b:db:8c:dd:45:35:
         85:d6:3a:b5:b5:5d:2d:67:c6:ea:7c:5e:f7:c1:b6:76:85:7e:
         ef:f8:64:4d:74:44:79:cb:a7:84:ad:73:a6:b0:67:e5:d8:70:
         a3:5c:7b:a9:5b:6a:20:de:8c:36:7f:5b:b4:4b:f8:64:14:77:
         95:0c:42:cb
```
---

## 三、通过CA为网站签名请求文件进行签名，颁发网站证书

### 1.通过CA生成网站证书
```
[root@blog00 https]# cd ca
[root@blog00 ca]# pwd
/root/https/ca
[root@blog00 ca]# openssl x509 -req -in ../myblog/myblog.com.csr -CA ca.crt -CAkey ca.key  -set_serial 01 -out myblog.com.crt -days 360
Signature ok
subject=/C=CN/ST=BJ/L=CP/O=myblog/OU=test/CN=myblog.com/emailAddress=test@myblog.com
Getting CA Private Key
Enter pass phrase for ca.key:             #输入CA私钥密码
[root@blog00 ca]# 

```
### 2. 查看颁发的网站证书
```
[root@blog00 ca]# openssl x509 -in myblog.com.crt -text -noout
Certificate:
    Data:
        Version: 1 (0x0)
        Serial Number: 1 (0x1)                 #证书序列号
    Signature Algorithm: sha256WithRSAEncryption       #证书签名算法
        Issuer: C=CN, ST=BJ, L=BJ, O=CATest, OU=test, CN=www.test.com/emailAddress=test@test.com             #CA的基本信息
        Validity                                            #证书有效期限
            Not Before: Jul 13 01:50:29 2022 GMT
            Not After : Jul  8 01:50:29 2023 GMT
        Subject: C=CN, ST=BJ, L=CP, O=myblog, OU=test,        CN=myblog.com/emailAddress=test@myblog.com           #被签名网站基本信息
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption          #被签网站公钥
                Public-Key: (2048 bit)
                Modulus:
                    00:b6:ef:73:cf:6c:9c:47:c0:51:1a:d1:1b:29:49:
                    39:e1:d9:89:88:6e:da:43:3d:65:26:24:05:b3:54:
                    6b:3a:bf:c7:b5:36:7e:ae:2b:19:d9:65:04:2b:cc:
                    19:9d:9d:ee:9c:f8:f9:90:5c:bf:de:fd:b7:69:77:
                    4f:72:19:1b:52:52:82:86:56:17:11:1c:cb:d4:57:
                    c3:6a:0b:52:db:fd:3e:94:c5:01:9c:0f:cb:ea:62:
                    d3:68:78:6e:31:f0:c6:a2:4f:14:04:82:4c:f3:c6:
                    e7:64:c0:fe:20:9b:6d:a9:37:8e:04:4c:54:cf:e8:
                    0f:57:17:c9:1b:b8:bf:ca:b6:19:19:3f:f5:08:01:
                    22:4f:6a:ad:b8:ed:6d:a4:e4:3f:4d:d0:48:fc:66:
                    23:5f:09:64:85:ae:2c:5a:3d:04:4d:01:b7:22:b8:
                    a0:c8:41:02:e4:7e:b0:1e:a8:68:e0:77:50:99:7b:
                    7d:ad:6f:ff:98:e7:dc:a9:58:7f:92:7d:c1:a5:24:
                    51:45:45:91:b1:cb:cf:f1:ce:e6:53:37:0e:38:58:
                    a5:50:28:bb:0a:f1:a5:bd:9b:db:38:65:06:92:cb:
                    bd:40:f0:4f:45:cf:b2:91:f0:33:8d:c3:bc:c7:66:
                    de:2f:0d:41:99:23:3b:27:f7:c0:00:39:66:a4:9e:
                    83:9f
                Exponent: 65537 (0x10001)
    Signature Algorithm: sha256WithRSAEncryption        #签名算法
         76:88:26:7e:5c:da:d2:5c:e3:22:cc:32:53:54:94:e2:c9:c2:
         8b:90:29:05:9f:c1:05:9e:53:da:0e:85:41:6a:97:b2:15:b5:
         20:c9:19:93:7e:ee:17:ce:d3:44:97:bd:d0:f6:6f:6f:ef:38:
         89:e8:02:23:5a:ff:75:62:c3:21:ec:6c:82:e3:c4:22:80:12:
         cc:86:6a:02:4e:8a:93:6f:64:19:61:48:77:31:27:87:59:18:
         93:a0:5e:0e:ef:7d:13:b8:87:2b:5c:51:b5:e8:8a:d5:54:d8:
         08:6f:77:a3:0c:7b:7a:1a:c2:36:5d:61:79:31:11:83:1e:96:
         f6:6f:17:98:7a:13:5f:47:73:ef:41:b0:15:a9:99:ae:b2:5f:
         85:cc:0b:2a:f2:61:a5:fd:11:34:79:30:7c:27:18:db:62:eb:
         c1:a6:43:c2:75:62:55:3f:38:8e:fe:ac:4c:f7:d2:e2:53:45:
         a0:a2:05:e1:93:52:0c:12:13:b7:30:ee:83:f9:33:0f:64:b7:
         52:45:f5:26:6c:fc:f9:d0:b1:28:b6:4c:dc:25:ee:b6:61:40:
         bc:42:6c:2a:18:5a:b0:04:91:d4:00:85:75:b0:ca:16:2d:3f:
         bc:a8:3e:6c:bd:6e:43:8c:fc:61:b7:65:82:d3:a4:25:fd:35:
         3f:69:e5:1d
[root@blog00 ca]# 
[root@blog00 myblog]# ls
myblog.com.crt  myblog.com.csr  myblog.com.key
#将crt文件格式转化为pem文件格式
[root@blog00 myblog]# openssl x509 -in myblog.com.crt -out myblog.com.pem -outform PEM
[root@blog00 myblog]# ls
myblog.com.crt  myblog.com.csr  myblog.com.key  myblog.com.pem
[root@blog00 myblog]# cat myblog.com.pem 
```
### 3.意义
公众认可CA机构，即也认可CA机构签发的证书，即也认可这个证书内的公钥是这个网站的真正公钥

