# nginx配置文件（目录加密）



### 1.安装htpasswd
> htpasswd是Apache密码生成工具，Nginx支持auth_basic认证，因此我门可以将生成的密码用于Nginx中，输入一行命令即可安装：`yum install -y httpd-tools`，参数如下：
```
-c 创建passwdfile.如果passwdfile 已经存在,那么它会重新写入并删去原有内容.
-n 不更新passwordfile，直接显示密码
-m 使用MD5加密（默认）
-d 使用CRYPT加密（默认）
-p 使用普通文本格式的密码
-s 使用SHA加密
-b 命令行中一并输入用户名和密码而不是根据提示输入密码，可以看见明文，不需要交互
-D 删除指定的用户
```

### 2.生成密码

方式一：
```
# 创建一个存放密码文件的目录
mkdir /etc/nginx/conf.d/passwd
# 创建账号tianye，密码123456，保存在文件../tianye.db内
htpasswd -bc /etc/nginx/conf.d/passwd/tianye.db tianye 123456
```

方式二：
```
# 进入home目录
cd /home
# 生成密码
htpasswd -c ./passwd username
# 执行上命令后会要求输入两次密码，./passwd 是在当前目录下创建密码文件passwd ，username即为需要设置的账号
```

### 3.载入配置
接下来在Nginx配置文件中（通常是server段内），加入如下两行，并重载Nginx（`systemctl restart nginx`）即可生效。

```
auth_basic "Please input password";   #这里是验证时的提示信息
auth_basic_user_file /home/passwd;
```

如果想为某个目录加密，示例如下：

```
server {
    listen       7070;
    server_name  localhost;


    # 反向代理
    location / {
        proxy_pass http://127.0.0.1:6060;
    }

    # 加密目录
    location ^~ /taycc-private/ {
        auth_basic "Please input password";
        auth_basic_user_file /etc/nginx/conf.d/passwd/taycc.db;
        proxy_pass http://127.0.0.1:6060/taycc-private/;
    }



}
```

### 4.访问测试
再次访问站点时，就会提示需要输入密码！以后再补充截图




<br><br><br><hr><br><br><br>

#### 帮助作者改进文档
如果您喜欢这篇文档，想让它变得更好，您可以：

- 推荐这篇文档，让更多的人知道。
- 给作者反馈和建议：*_<tianye3223@gmail.com>_*

<br><br><br><br><br>