# zabbix-server配置文件

常用参数

```
# 日志文件存放的位置
LogFile=/var/log/zabbix/zabbix_server.log

# 日志文件的大小，单位为MB，设置为0时，表示不进行日志轮询
LogFileSize=0

# pid文件存放的位置
PidFile=/var/run/zabbix/zabbix_server.pid

# 指定存放zabiix数据的数据库名
DBName=zabbix

# 指定连接数据库的用户名
DBUser=zabbix

# 用户连接数据库时使用的密码
DBPassword=123456

# JavaGateway的ip地址或主机名
JavaGateway=127.0.0.1

# JavaGateway的端口号
JavaGatewayPort=10052

# 开启连接javagatey的进程数
StartJavaPollers=5

# SNMPTrapServer临时文件，必须和zabbix_trap_receiver.pl的名字相同
SNMPTrapperFile=/var/log/snmptrap/snmptrap.log

# 缓存的大小
CacheSize=6144M

# 等待Agent的时间，单位为秒
Timeout=4

# 自定义告警脚本的路径，取决于编译时候的datadir参数
AlertScriptsPath=/usr/lib/zabbix/alertscripts
#AlertScriptsPath=/root/mail


# 外部脚本的路径，取决于编译时候的datadir参数
ExternalScripts=/usr/lib/zabbix/externalscripts


# 慢查询日志，设置为0时表示不记录
LogSlowQueries=3000
```