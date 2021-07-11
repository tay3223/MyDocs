# 搭建cobbler的步骤

### 1，准备实验环境（VMware版）
```
1) 准备一台服务器，安装Centos7系统
2) 该服务器上准备两块网卡，一块连接到正常的网段（192.168.88.0/24）一块连接到封闭网段（192.168.99.0/24）
3) 封闭的网段内：关掉dhcp，关闭外网连接
```
```markdown
我的服务器情况如下：

实验环境：VMware
操作系统：Centos7
IP地址：（192.168.88.100）（192.168.99.100）
初始化：关闭SELinux和iptables
```
### 2，配置阿里云的CentOS7和epel的yum源（比较快）
```
cd /etc/yum.repos.d/
mkdir backup
mv *.repo backup
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```
### 3，安装dhcp、cobbler、cobbler-web, 启动和启用服务
yum安装cobbler的时候，除了dhcp，其他依赖如：tftpd、httpd、syslinux（里面有pxelinux.0文件）会自动安装，所以只要装dhcp、cobbler、cobbler-web就好。
```
yum install dhcp cobbler cobbler-web
systemctl enable dhcpd httpd tftp cobblerd
systemctl start dhcpd httpd tftp cobblerd
```
这里启动dhcpd会报错，是因为配置文件还没弄好。不用管，忽略就行。

### 4，修改cobbler的配置文件
配置文件路径：`/etc/cobbler/settings`

备份配置文件：`cp /etc/cobbler/settings{,.bak}`

#### 第一处修改：

```
修改前：default_password_crypted: "$1$bfI7WLZz$PxXetL97LkScqJFxnW7KS1."
```
新打开一个终端，用openssl passwd -1命令来生成新的加密密码，这里设置的密码是cobbler123`(这个密码是linux安装好之后root用户的初始密码)`，加密后变成：`$1$2rPjTa.8$iDxUnssv2aGjLKUm/df2w1.`
```
openssl passwd -1
```
把加密密码填写到配置文件中：
```
修改后为：default_password_crypted: "$1$2rPjTa.8$iDxUnssv2aGjLKUm/df2w1."
```

#### 第二处修改：

```
# 这个选项是指cobbler server的ip地址，这里要指定一个网卡的ip，实验环境里指的是：192.168.99.100
server: 192.168.99.100
# 这个选项被用在DHCP/PXE上，用来作为DHCP和tftp的IP地址（待会在执行cobbler sync命令时会被设置到到dhcp和tftp配置文件中），一般和Cobbler服务端使用同一个IP
next_server: 192.168.99.100
```
#### 第三处修改：
```
# 0更改为1
manage_dhcp: 1
```
### 5，更改dhcp模板
由于上一节更改settings里的manage_dhcp为1了，所以dhcpd变成cobblerd来管理了。我们通过更改/etc/cobbler/dhcp.template里的内容，然后使用 cobbler sync同步到/etc/dhcp/dhcpd.conf
```
cp /etc/cobbler/dhcp.template{,.bak}
vim /etc/cobbler/dhcp.template
```
修改模板里的内容：
```
subnet 192.168.99.0 netmask 255.255.255.0 {
     option routers             192.168.99.2;           # 网关
     option domain-name-servers 192.168.99.2;
     option subnet-mask         255.255.255.0;          # 子网掩码
     range dynamic-bootp         192.168.99.120 192.168.99.200;    # IP分配范围：从120开始到200结束
     default-lease-time         21600;
     max-lease-time              43200;
     next-server                $next_server;           # 这一行及下面的内容千万不要修改
     class "pxeclients" {
          match if substring (option vendor-class-identifier, 0, 9) = "PXEClient";
          if option pxe-system-type = 00:02 {
                  filename "ia64/elilo.efi";
          } else if option pxe-system-type = 00:06 {
                  filename "grub/grub-x86.efi";
          } else if option pxe-system-type = 00:07 {
                  filename "grub/grub-x86_64.efi";
          } else if option pxe-system-type = 00:09 {
                  filename "grub/grub-x86_64.efi";
          } else {
                  filename "pxelinux.0";
          }
     }

}
```
备份原来的dhcpd.config，然后用cobbler sync一下`（因为上面修改了cobbler的配置，所以重启一下cobbler服务后，下面的cobbler sync命令才会生效）`
```
cp /etc/dhcp/dhcpd.conf{,.bak}
systemctl restart cobblerd
cobbler sync
```
同步完后，这时候去查看一下/etc/dhcp/dhcpd.conf，内容已经被覆盖了,我们也可以看到注释里写的是被Cobbler管理的dhcpd.conf：Cobbler managed dhcpd.conf file
![image](https://note.youdao.com/yws/res/9901/2EEFDBE00DD24949A16327FCAB2F5738)

重启cobblerd和dhcpd服务
```
systemctl restart cobblerd
systemctl restart dhcpd
```
### 6，同步配置，重启相关服务
```
cobbler sync
```
### 7，下载bootloader的加载程序
```
cobbler get-loaders
cobbler sync
```
### 8，配置cobbler-web
配置文件是/etc/cobbler/modules.conf。

修改cobbler的用户名和密码：
```
htdigest /etc/cobbler/users.digest "cobbler" cobbler
```
![image](https://note.youdao.com/yws/res/9914/8357DE89164F4EC1AEB942FECBF8378D)
```
systemctl restart cobblerd
cobbler sync
systemctl restart httpd
```
用修改过用户名密码登录测试：
```
https://192.168.99.100/cobbler_web
```
> 注意：   
> 这里会面临web页面打不开的情况，经过排查发现是python的问题，cobbler的一堆依赖中不兼容最新版的django，所以我们只要安装好pip，然后用pip安装1.8.9版本的django就可以顺利打开cobbler的web页面了

```
pip install django==1.8.9
```
> 如果此时仍然打不开页面，那么请输入`cobbler check`命令来查看cobbler的配置是正确，这个命令会列出影响cobbler运行的多处错误，我们只要看着错误一条一条解决掉就ok了。

![image](https://note.youdao.com/src/D41F52AB5EC24FCAB08F5EFFB6A416BD)

![image](https://note.youdao.com/src/2F83256C08F744A699A0D5C57A385166)


# 使用Cobbler的步骤
### 1，导入光盘
给虚拟机配置两个光盘，分别挂载CentOS6和CentOS7的光盘。

挂载光盘到目录：
```
mkdir /mnt/centos6
mkdir /mnt/centos7
mount /dev/sr0 /mnt/centos6
mount /dev/sr1 /mnt/centos7
```
如果是拷贝的iso文件到服务器，可以mount iso到目录：
```
mkdir /mnt/centos6
mkdir /mnt/centos7
mount CentOS-6.9-x86_64-bin-DVD1.iso /mnt/centos6
mount CentOS-7-x86_64-Everything-1611.iso /mnt/centos7
```
cobbler import导入光盘:
```
cobbler import --name=Centos6.9 --path=/mnt/centos6
cobbler import --name=Centos7.3 --path=/mnt/centos7
```

![image](https://note.youdao.com/src/227EB0ED0A334159BCD67CBEA692C99E)

如图我们可以看到，我们添加了两个发行版本到distros,也创建了两个profile(使用的是sample的ks文件），名字都是Centosx.x-x86_64，是cobblerd自动侦测了是x86_64的版本，自动添加到上面import命令的name后面。

查看distro/profile对，这两个list目前显示应该是一样的。（后期增加不同配置的ks文件，生成不同的profile，就不一样了，还需要把profile默认的两个例子删掉）
```
cobbler distro list

cobbler profile list
```

![image](https://note.youdao.com/src/00058ED82DDE4CA68893C47B8E624BB9)




查看导入的发行版操作系统信息(distro)：
```
# 导入的发行版操作系统信息
cobbler distro report  # 全部
cobbler distro report --name=Centos6.9-x86_64 # 单独一个
```

我们看到有两行信息：
```
Kickstart Metadata             : {'tree': 'http://@@http_server@@/cblr/links/Centos7.3-x86_64'}

Kickstart Metadata             : {'tree': 'http://@@http_server@@/cblr/links/Centos6.9-x86_64'}
```
这两行就是你将来ks文件里写的安装url，后续在ks文件就采用这个地址作为源的地址。

ks文件如果原来是cdrom安装方式，把cdrom换成url --url=http://xxxx：

CentOS 6:
```
url --url=http://192.168.111.1/cblr/links/Centos6.9-x86_64/
```
CentOS 7:
```
url --url=http://192.168.111.1/cblr/links/Centos7.3-x86_64/
```

### 重启cobblerd
```
systemctl restart cobblerd
cobblerd sync
```

### Cobbler自动化安装图示
新增加一台虚机，和cobbler在一个网段，打开运行，就会出现如下界面：

> 注意要使用：`BIOS引导`的方式启动

![image](https://note.youdao.com/src/9E1947F88C034093AB6C490629E77C96)

好了，可以尽情的使用了，如果要添加新的版本，按照上面步骤添加就可以了。

### Cobbler Web
图形界面，不感冒，感兴趣的可以自行研究!



<br><br><br><hr><br><br><br>

#### 帮助作者改进文档
如果您喜欢这篇文档，想让它变得更好，您可以：

- 推荐这篇文档，让更多的人知道。
- 给作者反馈和建议：*_<tianye3223@gmail.com>_*

<br><br><br><br><br>