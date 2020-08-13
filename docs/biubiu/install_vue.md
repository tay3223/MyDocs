# vue及vue-cli傻瓜式安装教程（windows版）

<br>

### 第一步：安装node.js

下载node.js安装包（建议下载LTS版）
[官网地址](https://nodejs.org/)


安装包下载好后，狂点下一步，完成安装（全都遵循默认配置）


打开windows的开始菜单，输入`cmd`命令，打开windows命令行，输入以下命令进行验证：
```
node -v
```
如果输出版本信息，就代表安装成功。
node.js安装成功，就代表npm这个工具也安装成功了。


<br><br><br>


### 第二步：npm国内加速

`以下命令都是在windows电脑的cmd命令行内执行`

通常我们使用如下的命令来安装对应的模块：

```
npm install vue
```

但是这样是直接从国外的源拉取数据，超级慢，所以参见以下两条：



临时配置npm的源指向国内的淘宝的数据源（但是电脑重启后会清空掉）

```
npm config set registry https://registry.npm.taobao.org
```

永久使用淘宝的npm源（不过就要用cnpm来代替npm了）

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

参考地址：[淘宝npm开源站点](https://npm.taobao.org/)


<br><br><br>

### 第三步：安装vue-cli

参考地址：[官方文档-中文版](https://cli.vuejs.org/zh/guide/#%E8%AF%A5%E7%B3%BB%E7%BB%9F%E7%9A%84%E7%BB%84%E4%BB%B6)

> 提醒一下：
> 这个工具在2.0版时名字叫：`vue-cli`
> 但是现在最新的应是3.0版以上了，新的名字叫：`@vue/cli`


安装命令：
```
npm install -g @vue/cli
```

<br><br><br>


### 第四步：如何使用vue-cli

我个人推荐小白用户使用vue-cli自带的web界面进行操作

输入以下命令启动web界面：

```
vue ui
```
该命令会在本机启动一个web管理界面，监听本机的8000端口。启动后界面如下：

![](http://img.taycc.com/15602641312780.jpg)



新建项目

![](http://img.taycc.com/15602643828430.jpg)


选择第一个
![](http://img.taycc.com/15602644167021.jpg)


你在命令行上操作的每一个按钮，其本质上就是在命令行里跑的命令，只是考虑到很多前端开发人员不会使用命令，所以才有了这个web页面诞生。
![](http://img.taycc.com/15602644462429.jpg)


项目创建好了，基本上前期你只会使用下图中的这两块
![](http://img.taycc.com/15602645164845.jpg)


安装官方自带的路由插件
![](http://img.taycc.com/15602645435302.jpg)



然后开始启动项目
![](http://img.taycc.com/15602649144746.jpg)


### 第五步：示例代码

路由标签的使用
```
<!-- router-view标签有点像一个占位符，后面匹配到的路由视图会出现在这里 -->

<router-view></router-view>
```

如何导入自定义的插件

```
import BottomNav from './components/BottomNav.vue';

import test1 from '@/views/test1.vue';
```

如何配置路由

```json
{
    path: '/daohang',
    name: 'daohang',
    component: () => import( /* webpackChunkName: "about" */ '@/views/heard.vue')
},
```

如何写一个完整的vue组件

```vue
<template>
    <div id="DaoHang"></div>
</template>

<script>
    export default{
        name:'heard'
    }
</script>

<style>
    #DaoHang{
        width: 100vw;
        height: 60px;
        position: fixed;
        left: 0;
        top: 0;
        background-color: antiquewhite;
    }
</style>

```



<br><br><br><hr><br><br><br>

#### 帮助作者改进文档
如果您喜欢这篇文档，想让它变得更好，您可以：

- 推荐这篇文档，让更多的人知道。
- 给作者反馈和建议：*_<tianye3223@gmail.com>_*

<br><br><br><br><br>