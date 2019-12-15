

```python 
from django.template import loader
from django.template.backend import Template
from django.shortcuts import render, render_to_response
loader.get_template("index")   # 返回一个Template对象
loader.get_template("index").render()  # 返回选然后的html字符串SafeStringString
# django.shortcuts.render和django.shortcuts.render_to_string都是快捷渲染函数
# 其中django.shortcuts.render_to_string比render少一个request参数，但是在django3.0中前者没了
```



```python
from django.http import HttpRequest, HttpResponse, JsonResponse
# 视图函数的接收的参数request就是HttpRequest的实例
# 试图函数返回的要是HttpResponse的实例或者JsonResponse的实例
```

```python
from django.urls import path, include
# path是映射路径和函数的
# include是映射路径和app中的路由的
```

```python
from django.views.decorators.http import require_http_methods, require_POST, required_GET
# @require_http_methods是限定请求的方式, 接收一个列表作为参数
# @require_POST是限定必须使用POST请求方式
```

```python
from django.contrib.auth.admin import User  # 这种导入其实就是下面这个User
from django.contrib.auth.models import User
from django.contrib.auth import authenticate  # 验证账号密码是否正确
from django.contrib.auth import login, logout
from django.contrib.auth.decorators import login_required  # @login_required
```

```python
class Middleware:
    def __ini__(self, get_response):
        self.get_response = get_response
    
    def __call__(self, request):
        # 在执行process_view函数(路径映射之前)之前的操作
        try:
            # 这两个可能报错，看Middleware的位置
            print(1, request.session)
        	print(1, request.user)
        except Exception as e:
            pass
        
        response = self.get_response(request)
        
        # 在执行view函数之后
        print(2, response)
        
        return response
    
    def process_view(self, request, view_func, view_args, view_kwargs):
        # 此时已经执行了路径映射, 但是还没有执行view函数
        print(view_func)
      	
```

```python
request.session是类字典结构，可以使用*request.session.items()进行解构；
request.user.is_authenticated验证用户
```

