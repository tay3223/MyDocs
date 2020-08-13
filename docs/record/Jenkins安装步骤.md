# Jenkins安装步骤 

### 1.安装JDK

```
略。。。
```
可以直接使用我的一键安装脚本：[Git地址](https://gitee.com/tay3223/script3/blob/master/install/801---JDK%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85___oss%E5%8F%AF%E7%94%A8.sh)



### 2.安装Jenkins的yum源

```
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
```



### 3.安装Jenkins服务

```
yum install -y jenkins
```

服务安装好后，配置文件位置：`/etc/sysconfig/jenkins`打开这个配置文件设置一下jenkins的`工作目录`
```
vim /etc/sysconfig/jenkins
```

配置文件内默认的工作目录是：`JENKINS_HOME="/var/lib/jenkins"`     
我们把它修改为：`JENKINS_HOME="/git"`，请提前把/git目录创建好.

> 一定要在这一步就把工作目录确定好，不然后期更改工作目录的话，所有的插件什么的都要重装了，说多了都是眼泪......（`不过经过一番折腾，作者决定不修改工作空间的目录了，大不了把nginx中root指向的目录修改一下，折腾了一大圈重新安装插件啊、/git目录内生成一大堆临时文件啊、什么什么的`，让人心累！）



### 4.启动服务

```
systemctl restart jenkins.service
ss -antp|grep 8080 # 默认启动8080端口，查看端口是否存在
```

查看页面是否能打开：http://localhost:8080（如下图）

![-w1440](http://img.taycc.com/15568720867359.jpg)

如果上面这个界面可以打开，就代表Jenkins安装成功了，剩下的初始化的步骤，我们去页面上完成！

---


### 5.在浏览器上完成剩余步骤

当打开这个页面的时候，Jenkins提示我们需要输入一个密码，我们可以通过查看`/var/lib/jenkins/secrets/initialAdminPassword`这个文件来获得初始密码。

![-w1440](http://img.taycc.com/15568720867359.jpg)

获取初始密码：`cat /var/lib/jenkins/secrets/initialAdminPassword`如下图所示

![-w1322](http://img.taycc.com/15568721892903.jpg)



正确的密码输入后，将会计入到下面这个页面。因为是初次使用并不清楚有什么插件，所以就选择安装官方推荐的插件，点击`左侧按钮`进入下一步。


![-w1440](http://img.taycc.com/15568722480949.jpg)


然后我们慢慢等待插件安装即可，下图列表中的插件都是官方推荐的


![-w1440](http://img.taycc.com/15568723614509.jpg)


如下图所示，继续等待。。。

![-w1440](http://img.taycc.com/15568724410749.jpg)


继续等待，感觉快好了，到现在为止已经等待了5分钟了。。。

![-w1440](http://img.taycc.com/15568725245451.jpg)


插件中午安装完成了，现在设置一个新用户，当新用户设置成功之后，最开始登录的那个临时密码就会失效了！

![-w1440](http://img.taycc.com/15568725518982.jpg)




设置jenkins服务的地址，便于后期webhook发送请求的时候可以找到jenkins服务


![-w1440](http://img.taycc.com/15568726255934.jpg)


jenkins算是彻底安装完成了！

![-w1440](http://img.taycc.com/15568726351235.jpg)



到这一步位置，大家就可以正常使用了！

![-w1440](http://img.taycc.com/15568726499827.jpg)






<br><br><br><hr><br><br><br>

#### 帮助作者改进文档
如果您喜欢这篇文档，想让它变得更好，您可以：

- 推荐这篇文档，让更多的人知道。
- 给作者反馈和建议：*_<tianye3223@gmail.com>_*

<br><br><br><br><br>