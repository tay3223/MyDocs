# django路由配置

全局配置：

```
from django.urls import path
from django.urls import include
from django.views.generic.base import RedirectView as ChongDingXiang

# 跟restapi有关的导入
from rest_framework import routers
from quickstart.views import UserViewSet, GroupViewSet, LoginUserViewSet

import xadmin

# rest_api的规则列表
router = routers.DefaultRouter()
router.register(r'users', UserViewSet)
router.register(r'groups', GroupViewSet)
router.register(r'login_user', LoginUserViewSet)

# ========================
# 整个项目的路由入口
# ========================
urlpatterns = [
    # 网站入口，重定向到指定的url
    path('', ChongDingXiang.as_view(url='index/')),

    # 匹配不同app的url，此处只拦截开头部分，剩余的url转发给指定的app内的urls来处理
    path('dns/', include('dns.urls')),
    path('login/', include('login.urls')),
    path('demo2/', include('demo2.urls')),
    path('index/', include('index.urls')),
    path('dns_and_zabbix/', include('dns_and_zabbix.urls')),

    # 后台
    path('xadmin/', xadmin.site.urls),

    # 验证码
    path('captcha', include('captcha.urls')),

    # api专用
    path('api/', include(router.urls)),
    # path('api-auth/', include('rest_framework.urls', namespace='rest_framework')),
]
```

模块内配置：

apps/login/urls.py

```
from django.urls import path
from django.views.generic.base import RedirectView as ChongDingXiang  # 重定向

from . import views

# 设定这个app的url的名字
app_name = 'login'

# 设定这个app内的url的规则
urlpatterns = [
    path('', ChongDingXiang.as_view(url='index/')),
    path('index/', views.index, name='index'),
    path('login/', views.login, name='login'),
    path('logout/', views.logout, name='logout'),
    path('register/', views.register, name='register'),
    path('confirm/', views.user_confirm),  # 邮箱确认码

]
```