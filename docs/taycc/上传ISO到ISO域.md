# 上传ISO到ISO域
### 说明：
ovirt使用命令行操作，完成上传iso文件到iso存储域

首先，复制需要上传的 ISO 文件到运行 OVIRT 管理平台的系统的临时目录下。

使用 root 用户登录到 OVIRT 管理平台系统。

<br><br><br>

### 步骤：

1.使用 `engine-iso-uploader` 命令查看存在的 ISO 存储域的名字：

```
$ engine-iso-uploader list
```

![-w847](http://img.taycc.com/15554012658104.jpg)


<br><br><br>

2.使用 `engine-iso-uploader` 命令上传 ISO 文件：

```
$ engine-iso-uploader --iso-domain=data-4---ISO upload /tmp/iso2/cn_windows_server_2016_vl_x64_dvd_11636695.iso
```

![-w1159](http://img.taycc.com/15554013473337.jpg)

该命令需要一个 ISO
存储域的名称作为参数，使用上一步的命令获取，还需要输入 ISO
文件的完整路径，该命令会提示用户输入 REST API 的密码。

<br><br><br>

### 结果：
ISO 上传完成并出现在命令中指定的 ISO 存储域中。同时也将出现在创建该 ISO
存储域所附加到的数据中心中的虚拟机时可用的启动媒介列表中。



<br><br><br><hr><br><br><br>

#### 帮助作者改进文档
如果您喜欢这篇文档，想让它变得更好，您可以：

- 推荐这篇文档，让更多的人知道。
- 给作者反馈和建议：*_<tianye3223@gmail.com>_*

<br><br><br><br><br>