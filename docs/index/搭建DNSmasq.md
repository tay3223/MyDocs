# 搭建DNSmasq

### 安装DNSmasq



```
# 如果是centos系统
yum install -y dnsmasq

# 如果是ubuntu系统
apt-get -y install dnsmasq
```

### 配置文件

```
# ==========================
# 原配置文件内容已删除
# 下文中是实际增加的内容
# 时间：2018-08-17
# 作者：tianye
# ==========================







# =======================================================
# 以下配置为线下生产环境专用
# =======================================================


# 指定额外的配置文件目录，/etc/dnsmasq.d/目录下的所有文件都被当做配置文件，除了.bak结尾的
conf-dir=/etc/dnsmasq.d,.bak


# *号表示包含，不加*号表示排除
#conf-dir=/etc/dnsmasq.d/,*.conf


# Include all files in /etc/dnsmasq.d except RPM backup files
#conf-dir=/etc/dnsmasq.d,.rpmnew,.rpmsave,.rpmorig


# 表示dnsmasq从哪里获取上游DNS服务器的地址，默认从/etc/resolv.conf获取
# resolv-file=/etc/resolv.conf


# 表示严格按照resolv-file文件中的顺序从上到下进行DNS解析，直到第一个解析成功为止
# strict-order


# 不使用/etc/resolv.conf这个配置文件，这里如果不使用resolv.conf，上面两个配置也就没有意义了
no-resolv


# 对以下设置的所有server发起查询，选择回应最快的一条作为查询结果
all-servers


# 指定上游DNS服务器
server=172.16.10.1
server=172.16.20.1
server=172.16.30.1


# server不但可以指定上游DNS服务器，还能为特殊规则指定特殊的DNS服务器去进行解析
# 指定dnsmasq程序使用哪个DNS服务器进行解析，相当于设置了某种规则，例如google.com结尾的域名，强制交给谷歌的DNS解析
# server=/google.com/8.8.8.8



# 开启DNS劫持，以最高的优先级，强制规定某个域名解析到指定的IP地址，相当于完成了a记录解析的任务
address=/www.dns.com/172.16.20.212


# 定义dnsmasq监听的地址，默认是监控本机的所有网卡上，局域网内主机若要使用dnsmasq服务时，指定本机IP地址
# 这里指定了两个配置文件是因为：该配置文件要每隔5分钟就同步到172.16.20.223主机上，让两台主机配置一致，所以多谢了一个
IP
listen-address=127.0.0.1,172.16.20.222,172.16.20.223


# 开启日志记录
log-queries
# 指定日志存放位置
log-facility=/var/log/dnsmasq.log


# 指定除/etc/hosts/之外的其它解析文件存放的位置
addn-hosts=/etc/tianye.hosts
```


`另一个环境的配置文件`

```
resolv-file=/etc/resolv.dnsmasq.conf
no-hosts
cache-size=20000
local-ttl=600
dns-forward-max=1500
addn-hosts=/etc/dnsmasq.hosts
log-facility=/var/log/dnsmasq/dnsmasq.log
log-queries
log-async=25
```