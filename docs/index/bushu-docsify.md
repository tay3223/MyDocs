# 部署docsify

## 1.安装nodejs

```
yum install -y nodejs
```

## 2.安装docsify

```
npm i docsify-cli -g
```

> docsify官网地址：https://docsify.js.org/#/zh-cn/quickstart

## 3.拉取我的docsify项目

```
yum install -y git
git clone https://gitee.com/tay3223/MyDocsify.gi
```

## 4.测试项目是否正常启动

```
cd ./MyDocsify
docsify serve ./docs
```

默认启动的是3000端口，如果要修改端口，使用参数`-p`，例如：
> docsify serve ./docs -p 3000

## 5.使用nginx部署

### 5.1.安装nginx

```
这里安装nginx或者tengine都行，因为有现成的脚本所有就不在多做赘述
```

### 5.2.配置反向代理

配置文件内容如下：

```
server {
    listen       7070;
    server_name  localhost;


    # 反向代理
    location / {
        proxy_pass http://127.0.0.1:3000/;
    }
    
    
}
```

### 5.3.重启nginx

```
nginx -t
nginx -s reload
```


### 5.4.使用nginx静态部署docsify
使用nginx部署docsify时，不需要使用`docsify serve ./docs`这样的方式了，直接使用nginx解析静态页面即可。

配置文件如下：

```
server {
    listen       6060;
    server_name  localhost;


    # 网站根目录
    location / {
        root    /git/docs;
        index  index.html;
    }



}
```








<br><br><br><hr><br><br><br>

#### 帮助作者改进文档
如果您喜欢这篇文档，想让它变得更好，您可以：

- 推荐这篇文档，让更多的人知道。
- 给作者反馈和建议：*_<tianye3223@gmail.com>_*

<br><br><br><br><br>