# 安装virtualenvwrapper

在介绍`virtualenvwrapper`之前，我们先介绍一个`pip`这个东西

### 1.什么是pip

在我们的实际开发中，我们会使用到很多`依赖`，这些`依赖`都是别人开发好的python代码，我们可以直接把别人开发好的代码拿来使用。

> 可是怎么拿过来呢？

此时我们就需要用到pip命令了。

我这里有一个工作中写好的python模块，是一个和网络有关的模块，如果您在浏览pip源仓库的时候，看中了我的这个模块的功能，那么您可以通过如下的命令来安装它：

```
pip install tay-ping
```
> - pip install：从pip源仓库中下载包
> - tay-python：包名称，一般我自己发布的包都是以tay-开头的

安装好之后，您就可以在自己的代码中引入我的这个模块，使用我这个模块里被我封装好的功能。我把这种引用关系称之为`依赖`，您的代码`依赖`我的代码。

!> 补充：
当初我向开源社区贡献这个代码包的时候使用的是MIT开源许可证，任何人都可以无限制的使用。

### 2.virtualenvwrapper是什么呢？

上面简单的解释了`依赖`的概念，在我们开发大型项目的时候，一个项目里可能会有好几百个`依赖`。我们都可以用pip来把它下载到本地。可是如果这个项目开发完了之后，我们要准备开发第二个大型项目的时候怎么办呢？是删除第一个项目所留下的`依赖`？还是留着它和第二个项目里的`依赖`一起混用？


Python社区里在很早时候就已经给出了一个很不错的解决方案：`那就是创造一个又一个的虚拟环境，每个项目专属一个虚拟环境`，然后把这个项目的所有`依赖`都安装到这个虚拟环境里，如果不想要这个虚拟环境了，直接删了就是了。


随后一个名叫`virtualenv`的模块就此诞生了，您可以使用这个模块创造单独的Python虚拟环境，只需要如下的命令就可以安装它：
```
pip install virtualenv
```
`virtualenv`虽然很棒，但是没过多久就有一个基于`virtualenv`而诞生的更棒的虚拟环境工具，也是我一直使用到今天的工具，名字叫：`virtualenvwrapper`，您可以使用如下的命令安装它：
```
pip install virtualenvwrapper
```
!> 安装`virtualenvwrapper`的时候会包含着安装`virtualenv`，随意直接安装`virtualenvwrapper`就可以了 

### 3.开始安装virtualenvwrapper

windows环境，在命令行输入下面的命令安装
```
pip install virtualenvwrapper-win
```
<br><br><br>
MacOS环境，在命令行输入以下命令进行安装
```
pip install virtualenvwrapper
```
然后在您的~/.bashrc配置文件中插入以下内容：
```
#这一句的意思是说：虚拟环境会被创建在哪个目录里，您可以自己定义
export WORKON_HOME=/Users/tay/virtualenvs

# 这一句的意思是说：虚拟环境初始化脚本的位置
source /usr/local/bin/virtualenvwrapper.sh
```

<br><br><br>
Centos7环境，在命令行输入以下命令进行安装
```
pip install virtualenvwrapper
```
然后在您的~/.bashrc配置文件中插入以下内容：
```
#这一句的意思是说：虚拟环境会被创建在哪个目录里，您可以自己定义
export WORKON_HOME=/myenv/virtualenvs

# 这一句的意思是说：虚拟环境初始化脚本的位置
source /bin/virtualenvwrapper.sh
```

<br><br><br>

!> 我的安装步骤可能写的不是很全面，大家可以去网络上搜索`virtualenvwrapper`，然后查阅更详细的安装步骤。

### 4.如何使用virtualenvwrapper

打开命令行，随便在哪个目录下输入以下命令都行，下面的命令都是全局可用的。


```
#创建一个名为my_test的虚拟环境
mkvirtualenv -p python3 my_test

#创建一个名为my_test2的虚拟环境，该虚拟虚拟环境加载python2环境变量，前提是您电脑上要先安装的有Python2的版本
mkvirtualenv -p python2 my_test2
```

```
# 查看已经存在的虚拟环境
workon
```

```
# 进入名为my_test的虚拟环境
workon my_test
```

```
# 退出当前的虚拟环境
deactivate
```

```
# 删除名为my_test的这个虚拟环境
rmvirtualenv my_test
```

