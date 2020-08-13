# NetScaler语法

<br>

#### 语法
lb配置：以xxx开头，同时 以xxx结尾

```
HTTP.REQ.URL.PATH.SET_TEXT_MODE(IGNORECASE).STARTSWITH("/Utility/")&&HTTP.REQ.URL.PATH.SET_TEXT_MODE(IGNORECASE).ENDSWITH(".svc")
```

lb精确匹配

```
HTTP.REQ.URL.PATH.SET_TEXT_MODE(IGNORECASE)=="/MemberGroup/UserFilter.svc"
```

lb模糊匹配

```
HTTP.REQ.URL.PATH.SET_TEXT_MODE(IGNORECASE).STARTSWITH("/Activity/")
```




<br><br><br><hr><br><br><br>

#### 帮助作者改进文档
如果您喜欢这篇文档，想让它变得更好，您可以：

- 推荐这篇文档，让更多的人知道。
- 给作者反馈和建议：*_<tianye3223@gmail.com>_*

<br><br><br><br><br>