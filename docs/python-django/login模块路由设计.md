# login模块路由设计

基本上可以在所有项目中通用了

URL|视图|模板|说明      
---|---|---|---         
/login/index/		|login.views.index|index.html|主页
/login/login/		|login.views.login|login.html|登录
/login/logout/      |login.views.logout|无|登出
/login/register/	|login.views.register|register.html|注册
/login/confirm/	|login.views.user_confirm|confirm.html|邮件激活