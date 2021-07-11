# Centos7报错汇总

#### 报错：
```
systemctl restart zabbUnit session-108.scope could not be found.

systemctl restart nginUnit session-108.scope could not be found.

......

```
这个错误通常出现在Centos7安装了`bash-completion`命令补全工具之后。当某个命令输入一半然后按`Tab`键补全时，就会出现类似错误。

###### 解决办法：

删除`/run/systemd/system/`目录下的泄露文件

###### 永久解决办法：

在crontab中添加如下命令：

```
*/10 * * * * /usr/bin/rm -rf /run/systemd/system/*
```

###### 参考地址：
[https://github.com/systemd/systemd/issues/1961](https://github.com/systemd/systemd/issues/1961)