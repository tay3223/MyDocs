# npm国内加速

### 一、修改成淘宝镜像源

命令

```
npm config set registry https://registry.npm.taobao.org
```

验证命令

```
npm config get registry
```

如果返回`https://registry.npm.taobao.org`，说明镜像配置成功。

### 二、修改成华为云镜像源

命令

```
npm config set registry https://mirrors.huaweicloud.com/repository/npm/
```

验证命令

```
npm config get registry
```

如果返回`https://mirrors.huaweicloud.com/repository/npm/`，说明镜像配置成功。

### 三、通过使用淘宝cnpm安装

安装cnpm

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

使用cnpm

```
cnpm install xxx
```

### 参考链接
[https://npm.taobao.org/](https://npm.taobao.org/)

<br><br><br><hr><br><br><br>

#### 帮助作者改进文档
如果您喜欢这篇文档，想让它变得更好，您可以：

- 推荐这篇文档，让更多的人知道。
- 给作者反馈和建议：*_<tianye3223@gmail.com>_*

<br><br><br><br><br>