# django的部署（亲测可用）

### 安装uwsgi

```
pip install uwsgi
```

### 安装nginx

```
参见scritp3仓库里的nginx一键安装脚本

（把下文中的django_nginx.conf配置文件copy到/etc/nginx/conf.d/目录下）
```

### 其它操作
1.从git仓库拉取项目代码

2.在项目根目录内创建`django_uwsgi.ini`配置文件，可以参考下文的模板（请注意配置文件内的`项目路径`）

3.进入项目根目录执行如下命令：

```
python manage.py makemigrations
python manage.py migrate
python manage.py collectstatic #收集所有静态资源
nohup uwsgi --ini django_uwsgi.ini  & #启动uwsgi文件
```

4.uwsgi服务启动后，再去启动nginx服务`（可以参考下文中django_nginx.conf配置文件）`


<br><br><br>

### django_uwsgi.ini的配置

```
# pip install uwsgi (建议在虚拟环境和真实环境里都安装一遍)

[uwsgi]

# 项目路径（注意目录的权限，如果权限不足，依然会访问不到资源的，建议给一个chmod 777）
chdir           = /tianye/My_Work

# wsgi文件的位置（实质上是：My_Work/wsgi的文件结构）不过可以用.来代替/
module          = My_Work.wsgi

# python3虚拟环境的目录
home            = /virtualenvs/py3-My_Work

# process-related settings
# master
master          = true

# 开放几个进程
processes       = 10

# 开启socket端口，nginx会跟这个端口对接
# socket          = /tianye/My_Work/mysite.sock
socket          = 127.0.0.1:8001

# socket权限
chmod-socket    = 664

# clear environment on exit
vacuum          = true
```

<br><br><br>

### django_nginx.conf的配置

```
upstream django222 {
    server 127.0.0.1:8001;
}

server {
    listen      8000;
    server_name localhost;
    charset     utf-8;

    client_max_body_size 75M;

    location /media/  {
        alias /tianye/My_Work/media;
    }

    # 把/static/替换为/tianye/My_Work/static_cdn/
    # 因为django一旦关闭调试模式，就不会再解析静态文件了，这个时候就需要nginx来解析静态文件了，所以把/static/开头的请求都拦截下来
    location /static/ {
        # gzip_static on;
        # expires max;
        # add_header Cache-Control public;
        autoindex on;
        alias /tianye/My_Work/static_cdn/;
    }

    location / {
        uwsgi_pass  django222;
        include     /etc/nginx/uwsgi_params;
    }
}
```


<br><br><br><hr><br><br><br>

#### 帮助作者改进文档
如果您喜欢这篇文档，想让它变得更好，您可以：

- 推荐这篇文档，让更多的人知道。
- 给作者反馈和建议：*_<tianye3223@gmail.com>_*

<br><br><br><br><br>