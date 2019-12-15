# 目录

1. 变量
2. 标签
3. 注释
4. 过滤器

### 1. 变量

语法格式：`{{variable}}`，变量名由字母、数字、下划线、点号组成；点号的使用规则中例如`foo.bar`的查找原则是：

1. 字典查找，把foo当做字典，bar当作key，等价于`foo['bar']`;
2. 属性或方法查找，把foo当作对象，bar当作属性；
3. 数字索引查找，把foo当作列表，bar当作索引，等价于`foo[bar]`.

视图函数如下：

```python	
def test(request):
    dict1 = {
        'a': "这是我的第一个django模板：",
        'b': "这里有一个列表：",
        'c': [i for i in range(10)],
    }
    context = {"head": "hey，", 'my_dict': dict1}
    return render(request, 'index.html', context)
```

模板内容如下：

```html	
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
欢迎来到duanmingpy的Django！<br>
{{head}}{{my_dict.a}}<br>
{{my_dict.b}}{{my_dict.c}}<br>
</body>
</html>

网页读取到对应的变量值的效果如下：
    欢迎来到duanmingpy的Django！
    hey，这是我的第一个django模板：
    这里有一个列表：[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

如果变量未能找到，则缺省插入`空字符串`在模板中调用方法，不能加小括号，自然也不能传递参数;

`{{my_dict.a}}`符合第一条，当做字典的key就可以访问到了；`{{ my_dict.keys }}` 这样是对的，符合第二条，当做my_dict对象的属性或
方法。



### 2. 标签

教程可以参考[Django内置的标签和过滤器](https://docs.djangoproject.com/en/2.0/ref/templates/builtins/)。

循环的条件也支持`and`、`or`、`not`
注意，因为这些标签是断开的，所以不能像Python一样使用`缩进`就可以表示出来，必须有个`结束标签`，
例如`endif`、`endfor`。

**`if/else`标签**

格式：

```django
{% if condition %}
	...
{% endif %}

或者：
{% if condition1 %}
	...
{% elif condition2 %}
	...
...
{% else %}
	...
{% endif %}
```



**`for`标签**

格式：

```django
<ul>
{% for athlete in athlete_list %}
<li>{{ athlete.name }}</li>
{% endfor %}
</ul>
```

**常用的变量**

|        变量         |                说明                |
| :-----------------: | :--------------------------------: |
|   forloop.counter   |        当前循环从1开始计数         |
|  forloop.counter0   |        当前循环从1开始计数0        |
| forloop.revcounter  |     从循环的末尾开始倒计数到1      |
| forloop.revcounter0 |     从循环的末尾开始到计数到0      |
|    forloop.first    |           第一次进入循环           |
|    forloop.last     |          最后一次进入循环          |
| forloop.parentloop  | 循环嵌套时，内层当前循环的外层循环 |

给标签增加一个`reversed`使得该列表被反向迭代：

```django
{% for athlete in athlete_list reversed %}
...
{% empty %}
... 如果被迭代的列表是空的或者不存在，执行empty
{% endfor %}
```

可以嵌套使用`{% for %}`标签：

```python
{% for athlete in athlete_list %}
	<h1>{{ athlete.name }}</h1>
	<ul>
		{% for sport in athlete.sports_played %}
			<li>{{ sport }}</li>
		{% endfor %}
	</ul>
{% endfor %}
```

变量、`if/else`、`for`的综合使用：

定义函数：

```python
from django.contrib import admin
from django.urls import path
from django.shortcuts import render
def test(request):
    context = {"dct": {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}}
    return render(request, 'index.html', context)

from django.urls import include, path

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', test),  # 映射到根路径，因为是首页
]
```

注意，在模板中只能访问`context`内的元素，所以必须在`dct`字典外层再套一层字典，是不是叫`context`无所谓，传参正确即可。

模板内容：

```django
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>

字典是dict(zip('abcde', range(1, 6)))
{{dct.items}}   显示dct的items
<ul>
    {% if 1 %}
        <li>第一个标签</li>
    {% endif %}
    {% for k,v in dct.items %}
        <li> {{k}} {{v}} </li>
    {% endfor %}
</ul>

<hr>

<ul>
    {% for k, v in dct.items %}
        <li>{{forloop.counter0}} {{k}} {{v}}</li>
    {% endfor %}
</ul>

<hr>

<ul>
    {% for k, v in dct.items %}
        {{forloop.first}}  # True或False
        {{forloop.last}}  # True或False
        <li>{{forloop.revcounter0}} {{k}} {{v}}</li>
    {% endfor %}
</ul>

</body>
</html>

```

**`ifequal/ifnotequal`标签**

```dango
{% ifequal user currentuser %}
	<h1>Welcome!</h1>
{% endifequal %}
```

和 `{% if %}`类似， `{% ifequal %}` 支持可选的 `{% else%}` 标签：

```django
{% ifequal section 'sitenews' %}
	<h1>Site News</h1>
{% else %}
	<h1>No News Here</h1>
{% endifequal %}
```

**`{% csrf_token %}`** 

`csrf_token` 用于跨站请求伪造`保护`，防止跨站攻击的。

**注释标签** 

单行注释 `{# #}`；
多行注释 `{% comment %} ... {% endcomment %}`.

```django
{# 这是一个注释 #}
{% comment %}
	这是多行注释
{% endcomment %}
```

**过滤器** 

语法
： `{{ 变量|过滤器 }}`；

模板过滤器可以在变量被显示前修改它，过滤器使用管道字符 `|` ，例如`{{ name|lower }}` ， `{{ name }}` 变量被过滤器 `lower` 处理后，文档大
写转换文本为小写。
过滤管道可以被套接 ，一个过滤器管道的输出又可以作为下一个管道的输入。
例如`{{ my_list|first|upper }} `，将列表第一个元素并将其转化为大写。

过滤器传参：

过滤器传参
有些过滤器可以传递参数，过滤器的`参数`跟随`冒号之后`并且`总是以双引号包含`。
例如：` {{ bio|truncatewords:"30" }}` ，截取显示变量 `bio` 的前30个词。` {{ my_list|join:"," }}` ，将`my_list`的所有元素使用`,`逗号连接起来。

常见过滤器：

| 过滤器            | 说明                                                         | 举例                                                         |
| ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `cut`             | 切掉字符串中的指定字                                         | `{{ value|cut:"b"}}`                                         |
| `lower`           | 转换为小写                                                   | `{{value|lower}}`                                            |
| `upper`           | 转换为大写                                                   | `{{value|upper}}`                                            |
| `truncatewords`   | 保留指定个数的字母                                           | `{{bio|truncatewords:"30"}}`                                 |
| `join`            | 对序列拼接                                                   | `{{d.e|join:"//"}}`                                          |
| `first`           | 取序列第一个元素                                             |                                                              |
| `last`            | 取序列最后元素                                               |                                                              |
| `yesno`           | 变量可以是`True`、`False`、`None`;<br> `yesno`的参数给定逗号分隔的三个值，返回三个值中的一个；<br>`True`对应第一个；<br>`False`对应第二个；<br>`None`对应第三个；<br>如果参数只有两个，`None`等效`False`处理。 | `{{value|yesno:"yeah, no, none"}}`                           |
| `add`             | 加法，参数是负数就是减法                                     | 数字加`{{value|add"100"}}`<br>列表合并`{{mylist|add:newlist}}` |
| `divisibleby`     | 能否被整除                                                   | `{{value:divisibleby:"3"}}`能否被3整除返回`True`             |
| `addslashes`      | 在反斜杠、单引号或双引号前加反斜杠                           | `{{value|addslashes}}`                                       |
| `length`          | 返回变量的长度                                               | `{% if my_list | length >1 %}`                               |
| `default`         | 变量等价`False`则使用缺省值                                  | `{{value|default:"nothing"}}`                                |
| `default_if_none` | 变量为`None`时使用的缺省值                                   | `{{value|default_if_none:"nothing"}}`                        |
| date              | 格式化`date`或者`datetime`对象                               |                                                              |

时间的格式字符查看[Django内置时间格式](https://docs.djangoproject.com/en/2.2/ref/templates/builtins/#date)<br> 过滤器参考 [Django参考文档](https://docs.djangoproject.com/en/2.2/ref/templates/builtins/#)

### 练习题

###### 1. 奇偶行列表输出

使用下面字典my_dict的c的列表，在模板网页中列表ul输出多行数据；要求：

奇偶行颜色不同<br>每行有行号（从1开始）<br>
列表中所有数据都增大100

```python
from django.http import HttpResponse, HttpRequest
from django.shortcuts import render
def index(request:HttpRequest):
"""视图函数：请求进来返回响应"""
    my_dict = {
        'a':100,
        'b':0,
        'c':list(range(10,20)),
        'd':'abc',
        'date':datetime.datetime.now()
    }
    context = {'content':'www.magedu.com', 'my_dict':my_dict}
    return render(request, 'index.html', context)
```

###### 2. 打印九九方阵（可以是九九乘法表）

```
1*1=1 1*2=2 ... 1*9=9
...
9*1=1 9*2=18... 9*9=81
```

把上面所有的数学表达式放到HTML表格对应的格子中。如果可以，请实现奇偶行颜色不同。

