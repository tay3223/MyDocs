# xadmin配置

模块内配置：

apps/loginx/admin.py

```python
from django.contrib import admin

from xadmin import views
import xadmin
from .models import User, ConfirmString

"""
修改xadmin的logo和底部标签
"""


# 修改xadmin的logo和底部标签
class GlobalSetting(object):
    site_title = "大千世界观望者"
    site_footer = "大千世界观望者"
    # menu_style = "accordion"


# 添加主题按钮（因为我没有导入主题素材，所以很难看）
class BaseSetting(object):
    # xadmin能修改主题
    enable_themes = True
    use_bootswatch = True


# 绑定上面的两个内容
xadmin.site.register(views.CommAdminView, GlobalSetting)
# xadmin.site.register(views.BaseAdminView, BaseSetting)

"""
下面是我添加的表单和字段
"""


# 设定xadmin上显示那几个字段
class lalala3(object):
    list_display = ('id', 'name', 'password', 'role', 'email', 'sex', 'create_time', 'has_confirmed',)

    # 设定xadmin上显示那几个字段


class lalala4(object):
    list_display = ('id', 'code', 'user', 'create_time',)


# 把显示字段和表绑定在一起
xadmin.site.register(User, lalala3)
xadmin.site.register(ConfirmString, lalala4)
```