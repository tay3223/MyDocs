# 如何使用openvpn命令行版

<br>

### 作用范围：
- 操作系统：Centos7     
- 提前说明：这里仅描述`openvpn客户端`如何使用，`openvpn服务端`的搭建不在本文讨论范围之内。

<br><br><br>

### 安装客户端：

Linux服务器安装OpenVPN相对简单一些，为了方便安装，我们用yum直接安装，具体过程如下：

```
yum -y install epel-release
yum -y install openvpn
```

<br>

OpenVPN 安装完成后会在`/etc/openvpn`生成对应的文件，具体如下：

```
[root@ns1 ~]# ll /etc/openvpn/     
drwxr-x--- 2 root openvpn  34 Jul 26 15:06 client
drwxr-x--- 2 root openvpn   6 Apr 26 23:04 server
```

<br>

在使用openvpn的过程中我们会得到一个名为`xxx.ovpn`的文件，我们把该文件放在`/etc/openvpn`目录下，目录结构如下:

```
root@continental-2 openvpn $ tree
.
├── client
│   ├── passwd              #账号密码文件，需要新建，第一行是账号，第二行是密码
│   └── passwd_huawei       #账号密码文件，需要新建，第一行是账号，第二行是密码
├── client.ovpn             #连接腾讯云的ovpn文件，由服务器生成
├── server
├── tuhu_work.ovpn          #连接华为云的ovpn文件，由服务器生成
└── update-resolv.sh

```

<br>

因为这台服务器上同时启动了两个`openvpn客户端`所以，我这里会有两套的配置文件：

腾讯云：
> /etc/openvpn/client.ovpn       
> /etc/openvpn/clinet/passwd

华为云：
> /etc/openvpn/tuhu_work.ovpn       
> /etc/openvpn/client/passwd_huawei

<br><br><br>

### 进行连接
相关配置文件导入后，我们来进行测试连接：

连接腾讯云

```
openvpn --daemon --cd /etc/openvpn --config client.ovpn --auth-user-pass /etc/openvpn/client/passwd --log-append /var/log/openvpn.log
```

连接华为云

```
openvpn --daemon --cd /etc/openvpn --config tuhu_work.ovpn --auth-user-pass /etc/openvpn/client/passwd_huawei --log-append /var/log/openvpn_huawei.log
```

<br>

参数说明：

```
--daemon           # 后台运行
--cd               # 配置文件目录路径
--config           # 配置文件名称
--auth-user-pass   # 指定账号密码文件
--log-append       # 日志文件
```

<br>

命令执行完后，可以用以下命令查看相关日志：

```
tail -f /var/log/openvpn.log
```

<br>

当日志末尾出现类似如下内容说明正常连接了：
```
Thu Jul 26 15:19:43 2018 /sbin/ip addr add dev tun0 local 10.6.0.226 peer 10.6.0.225
Thu Jul 26 15:19:43 2018 /sbin/ip route add 172.16.1.0/24 via 10.6.0.225
Thu Jul 26 15:19:43 2018 /sbin/ip route add 10.0.0.0/8 via 10.6.0.225
Thu Jul 26 15:19:43 2018 /sbin/ip route add 10.6.0.0/24 via 10.6.0.225
Thu Jul 26 15:19:43 2018 Initialization Sequence Completed
```

<br>
<br>

`工作比较多时间比较仓促，先把客户端的写完，后续有时间再写服务端的部署。。。`


<br><br><br><hr><br><br><br>

#### 帮助作者改进文档
如果您喜欢这篇文档，想让它变得更好，您可以：

- 推荐这篇文档，让更多的人知道。
- 给作者反馈和建议：*_<tianye3223@gmail.com>_*

<br><br><br><br><br>
