# django2怎么使用xadmin

### 1.新建项目

使用Django2.x版框架创建好一个新项目，此时是一个空的项目

### 2.执行下面的命令安装xadmin

```
pip install https://codeload.github.com/sshwsfc/xadmin/zip/django2
（当然也可以采用其它办法，请自行搜索）
```

### 3.编写django的全局配置文件

在apps中添加如下内容

```
# ------------------------------
'xadmin',     
'crispy_forms',     
'reversion',     
# ------------------------------
```

### 4.编写mysql的配置

> 注意：`pymysql`需要在__init__文件中初始化一下，初始化内容如下：

```
import pymysql
pymysql.install_as_MySQLdb()
```

不过现在mysqlclient用的比较多!

Django中MySQL的配置如下：

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': "py3-mywork",
        'USER': 'lalala',
        'PASSWORD': '123456',
        'HOST': 'mysql.taycc.com',
        'PORT': '3306',
        'OPTIONS': {
            "init_command": "SET sql_mode='STRICT_TRANS_TABLES'",
        },
    }
}
```

### 5.编写django的全局url配置文件

```
import xadmin
path('xadmin/', xadmin.site.urls),
```

### 6.让Django初始化与数据库的连接

```
makemigrations
migrate
```

### 7.创建超级用户

```
python manage.py createsuperuser
```

### 8.启动django项目

输入：`http://localhost:8000/xadmin`然后进行测试

### 9.注意保存项目的依赖

```
pip freeze > pip3.txt
```

<br><br><br><hr><br><br><br>

#### 帮助作者改进文档
如果您喜欢这篇文档，想让它变得更好，您可以：

- 推荐这篇文档，让更多的人知道。
- 给作者反馈和建议：*_<tianye3223@gmail.com>_*

<br><br><br><br><br>