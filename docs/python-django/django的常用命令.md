# django的常用命令

创建一个app

```
python manage.py startapp students
```

其它：

```
python manage.py makemigrations
python manage.py migrate

python manage.py createsuperuser         #创建超级用户

python manage.py collectstatic           #收集所有静态资源
nohup uwsgi --ini django_uwsgi.ini  &    #启动uwsgi文件
```