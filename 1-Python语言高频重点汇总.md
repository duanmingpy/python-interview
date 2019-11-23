# Python语言高频重点汇总   
[**回到首页**](https://github.com/duanmingpy/python-interview)   
### 目录：   
  * [Python语言高频重点汇总](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)
  * [目录：](#%E7%9B%AE%E5%BD%95)
  * [1\. 函数\-传参](#1-%E5%87%BD%E6%95%B0-%E4%BC%A0%E5%8F%82)
  * [2\. 元类](#2-%E5%85%83%E7%B1%BB)
  * [3\. @staticmethod和@classmethod两个装饰器](#3-staticmethod%E5%92%8Cclassmethod%E4%B8%A4%E4%B8%AA%E8%A3%85%E9%A5%B0%E5%99%A8)
  * [4\. 类属性和实例属性](#4-%E7%B1%BB%E5%B1%9E%E6%80%A7%E5%92%8C%E5%AE%9E%E4%BE%8B%E5%B1%9E%E6%80%A7)
  * [5\. Python的自省](#5-python%E7%9A%84%E8%87%AA%E7%9C%81)
  * [6\. 列表、集合、字典推导式](#6-%E5%88%97%E8%A1%A8%E9%9B%86%E5%90%88%E5%AD%97%E5%85%B8%E6%8E%A8%E5%AF%BC%E5%BC%8F)
  * [7\. Python中单下划线和双下划线](#7-python%E4%B8%AD%E5%8D%95%E4%B8%8B%E5%88%92%E7%BA%BF%E5%92%8C%E5%8F%8C%E4%B8%8B%E5%88%92%E7%BA%BF)
  * [8\. 格式化字符串中的%和format](#8-%E6%A0%BC%E5%BC%8F%E5%8C%96%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%E5%92%8Cformat)
  * [9\. 迭代器和生成器](#9-%E8%BF%AD%E4%BB%A3%E5%99%A8%E5%92%8C%E7%94%9F%E6%88%90%E5%99%A8)
  * [10\. args和\*\*kwargs](#10-args%E5%92%8Ckwargs)
  * [11\. 面向切面编程AOP和装饰器](#11-%E9%9D%A2%E5%90%91%E5%88%87%E9%9D%A2%E7%BC%96%E7%A8%8Baop%E5%92%8C%E8%A3%85%E9%A5%B0%E5%99%A8)
  * [12\. 鸭子类型](#12-%E9%B8%AD%E5%AD%90%E7%B1%BB%E5%9E%8B)
  * [13\. Python中的重载](#13-python%E4%B8%AD%E7%9A%84%E9%87%8D%E8%BD%BD)
  * [14\. 新式类和旧式类](#14-%E6%96%B0%E5%BC%8F%E7%B1%BB%E5%92%8C%E6%97%A7%E5%BC%8F%E7%B1%BB)
  * [15\. \_\_new\_\_和\_\_init\_\_的区别](#15-__new__%E5%92%8C__init__%E7%9A%84%E5%8C%BA%E5%88%AB)
  * [16\. Python中的作用域](#16-python%E4%B8%AD%E7%9A%84%E4%BD%9C%E7%94%A8%E5%9F%9F)
  * [17\. GIL线程全局锁](#17-gil%E7%BA%BF%E7%A8%8B%E5%85%A8%E5%B1%80%E9%94%81)
  * [18\. 协程](#18-%E5%8D%8F%E7%A8%8B)
  * [19\. 闭包](#19-%E9%97%AD%E5%8C%85)
  * [20\. lambda匿名函数](#20-lambda%E5%8C%BF%E5%90%8D%E5%87%BD%E6%95%B0)
  * [21\. Python中函数式编程](#21-python%E4%B8%AD%E5%87%BD%E6%95%B0%E5%BC%8F%E7%BC%96%E7%A8%8B)
  * [22\. Python中的拷贝](#22-python%E4%B8%AD%E7%9A%84%E6%8B%B7%E8%B4%9D)
  * [23\. Python的垃圾回收机制](#23-python%E7%9A%84%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E6%9C%BA%E5%88%B6)
  * [24\. List](#24-list)
  * [25\. Python中的is](#25-python%E4%B8%AD%E7%9A%84is)
  * [26\. read, readline和readlines](#26-read-readline%E5%92%8Creadlines)
  * [27\. Python2和Python3的区别](#27-python2%E5%92%8Cpython3%E7%9A%84%E5%8C%BA%E5%88%AB)
  * [28\. super init](#28-super-init)

## 1. 函数-传参   
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)    
在python中，给一个函数传递参数其实是把实参这个变量对应的地址复制了一份，然后把复制的这个地址传递给函数中局部变量形参，此时实参和对应的形参都指向内存中这一个实际的对象。   
`第一个例子：`   
```python   
a = 500

def function1(a):
  print("函数中局部变量a的地址：", id(a))
  a = 100  # 更改之后
  print("指向100后的局部a 和 100的地址：", id(a), id(100))

print("全局a和500的地址：", id(a), id(500))
function1(a)
print("函数调用完之后全局a的地址：", id(a))

# 输出结果：
# 全局a和500的地址： 2272322100976 2272322100976
# 函数中局部变量a的地址： 2272322100976
# 指向100后的局部a 和 100的地址： 140730892516720 140730892516720
# 函数调用完之后全局a的地址： 2272322100976
```    
我们可以看到，全局a指向500这个实体的地址，我们在调用function1函数的时候，把全局a的地址复制了一份，给了局部a，虽然全局a指向500，局部a也指向500，但是全局a和局部a是两个变量，即便名字相同，因为都指向500,所以可以看到此刻的全局a还有局部a还有500的内存地址都是同一块内存；接着在函数中局部a不再指向500了，局部a重新指向了100所在的内存地址了，这时候可以看到局部a和100指向的是同一块内存，因为整个函数调用的过程中，全局a一直指向500这个内存地址，在函数调用结束之后，局部a消亡，全局a依旧指向500，内存地址从来没有变化过。    

`第二个例子：`   
```python   
lst = []


def function2(lis: list):
  print("lis的地址：", id(lis))
  lis.append(200)
  print("append200之后lis的地址：", id(lis))

print("lst的地址", id(lst))
function2(lst)
print("函数调用之后的lst:", lst)

# 输出结果：
# lst的地址 2110929195592
# lis的地址： 2110929195592
# append200之后lis的地址： 2110929195592
# 函数调用之后的lst: [200]
```  
这个例子是同样的道理，lst指向[]所在的内存地址，接着调用函数，lst所指向的内存地址复制一份给了lis，然后lis在函数中通过接收到的内存地址把[]增加了一个元素200，然后调用结束lis局部变量消亡，所以lst还是指向[]这个对象所在的地址，因为在函数中，lis把这个列表修改了，所以lst通过地址找到这个列表是被修改过的。   
我们可以把所有变量理解为内存中一个对象的`引用`，或者可以看作是C++ 中的指针类型。每一个变量记住的都是对象的地址，而对象又可以分为`可变的mutable`和`不可变的immutable`，在python中，string、tuple、数值是不可变的，list、set、dict是可修改的对象，可以通过多个变量都记住它的地址，然后通过不同变量去修改这些可修改的对象。   
这是stack overflow上的解答：[连接地址](http://stackoverflow.com/questions/986006/how-do-i-pass-a-variable-by-reference)    

## 2. 元类   
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)         
元类又叫metaclass，在python中我们使用`type(59)`就可以知道59是int类型，但是你考虑过int类的类型吗？这就是元类问题，python中一切皆对象，就像linux中的一切皆文件哲学那么彻底，所以类也是对象，既然是对象就有类型，所有新类型的缺省都是type类型，可以修改，在python中，当我们创建一个对象的时候，它会进行类型检查，如果我们没指定类型缺省就是type类了。可以参考下图：   
![元类](https://github.com/duanmingpy/python-interview/blob/master/images/yuanlei.png)
   
细节可以参考stack overflow的解答:[连接地址](http://stackoverflow.com/questions/100003/what-is-a-metaclass-in-python)    

## 3. @staticmethod和@classmethod两个装饰器     
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)         
在python类方法中有三中类型的方法，第一种是最普通的方法，在实例进行调用的时候会主动绑定调用的实例作为第一个参数，第二个是通过@classmethod装饰的方法，无论是类还是实例进行调用，都会自动绑定当前类作为第一个参数，第三个是通过@staticmethod装饰的方法，这种方法调用就像普通函数一样，不会进行传参数的自动绑定。    
其实这三种方法的本质都是用类写一个装饰器，然后通过描述器的方法对类方法的参数进行约束，使用描述器实现的还有@property，其中前面的都是`非数据描述器non-data descriper`, @property则是`数据描述器的实现data descriper`。记得在刚学习python描述器的时候我还自己手写了@staticmethod、@classmethod还有@property这三个装饰器。   
例子：    
```python   
class Student:
  def __init__(self, name, age):
    self.name = name
    self.age = age

  @classmethod
  def play(cls):
    print(cls.__name__)

  @staticmethod
  def eating(student, food):
    print(f"{student} eat {food}")

tom = Student("tom", 17)
tom.play()
Student.eating("tom", "apples")

# 输出：
# Student
# tom eat apples
```   
如果实在回忆不上来了（不可能的），可以参考real python 和stack overflow的解读：    
real python：[Instance, Class, and Static Methods — An Overview](https://realpython.com/instance-class-and-static-methods-demystified/)      
stack overflow:[What is the difference between @staticmethod and @classmethod?](https://stackoverflow.com/questions/136097/what-is-the-difference-between-staticmethod-and-classmethod)      

## 4. 类属性和实例属性      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)         
类属性：通俗的讲是挂在类上的属性，所以只要是这个类的实例就可以访问，比如人类有马克思哲学这个属性，所有人都可以去学习使用马克思哲学，所有人都可以访问；从专业角度讲类属性就是放在类的`__dict__`这个字典中的属性。   
实例属性：同理，实例属性就是每个实例私有的，比如每个人的房子，这就是私有的，别人是不能够拥有的，从专业角度讲实例属性就是放在实例的字典`__dict__`中的。   
例子：   
```python   
class Server:  # Server类
  protocol = "TCP/IP"  # TCP/IP协议是所有服务器共有的资源
  
  def __init__(self, ip, port):
    self._ip = ip  # ip和端口则是每台服务器自己的
    self._port = port  # 即使端口相同也不是一台计算机的端口
```     
这里的协议protocol就是类属性，IP+port就是实例属性。    


## 5. Python的自省      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)        
**什么是自省？**   
在日常生活中，自省（introspection）是一种自我检查行为。   
在计算机编程中，自省是指这种能力：检查某些事物以确定它是什么、它知道什么以及它能做什么。自省向程序员提供了极大的灵活性和控制力。    
说的更简单直白一点：自省就是面向对象的语言所写的程序在运行时，能够知道对象的类型。简单一句就是，运行时能够获知对象的类型。   
例如python, buby, object-C,C++ 都有自省的能力，这里面的c++的自省的能力最弱，只能够知道是什么类型，而像python可以知道是什么类型，还有什么属性。     
python中的自省方法：   
`type()`   
`dir()`   
`getattr()`   
`hasattr()`   
`isinstance()`等     
也是插件化开发技术的依赖之一。
```python   
class Server:  # Server类
  protocol = "TCP/IP"
  
  def __init__(self, ip, port):
    self._ip = ip 
    self._port = port 
    

print(hasattr(Server, "protocol"))  # True
```

## 6. 列表、集合、字典推导式       
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)         
推导式是python开发过程中非常常用的技术，简单但是绝对是好用的，在2.7版本之前并没有字典推导式，由于太好用了，社区一直建议增加，在2.7之后增加了字典推导式。   
例子：    
```python   
# 100以内所有的奇数 —— 列表解析式
a = [i for i in range(100) if i & 1]

# 100以内3的倍数 —— 集合解析式
b = {i for i in range(100) if i % 3 == 0}

import random
import string

# 生成100个name和对应的id —— 字典解析式
name_id = {"".join([random.choice(string.ascii_letters) for i in range(4)]):random.randint(1000, 9999) for j in range(100)}

```   
## 7. Python中单下划线和双下划线      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
如：服务器的addr是需要大家知道的，不隐藏，`_socket`是服务器的监听socket不必让别人知道，可以隐藏一下，start是服务器的启动方法，需要让别人知道，而用来接收连接的`__accept`则不需要别人知道。   
```python  
import socket
import threading

class Server:
  def __init__(self, ip, port):  # 魔术方法
    self.addr = ip, port   # 服务器的地址和端口
    self._socket = socket.socket()  # 服务器的监听socket
    
  def start(self):  # 启动服务器的接口
    self._socket.bind(self.addr)
    self._socket.listen()
    threading.Thread(target=self.__accept, name="接收连接").start()
    
  def __accept(self):  # 监听socket用来接收连接的方法
    new_socket, raddr = self._socket.accept()
    pass
    
print(Server.__dict__)
```   
在python的类中，单下划线被约定为隐藏变量，分为两种，一种是开头短下划线`_socket`，另一种是开头长下划线`__accept`；其中短下划线的标识符在类字典中是不更改名称的，而长下滑线的在类属性字典中更改了名称，如：`_Server__accept`,但是由于python的黑魔法太过容易破解，如果我们真的想访问对应的属性，只需要把类字典拿出来看一下名称就可以调用了。   
总之，`防君子不防小人把！`   
双下滑下，即两端都有下划线的是一些特殊的魔术方法，以及特殊方法，比如类或实例的字典使用`__dict__`访问，还有上下文使用`__enter__`和`__exit__`来控制。    

回忆不上来的时候可以查阅知乎和stack overflow:   
stack overflow:[What is the meaning of a single and a double underscore before an object name?](https://stackoverflow.com/questions/1301346/what-is-the-meaning-of-a-single-and-a-double-underscore-before-an-object-name)   
知乎：[Python的类的下划线命名有什么不同？](https://www.zhihu.com/question/19754941)     
闲扯：为什么知乎选择用python写？我觉得可能是开发效率高把，现在听说转到go语言了。   

## 8. 格式化字符串中的%和format      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
其中%是类c语言的风格，现如今随着发展，学python的同学可能并不是很熟悉C语言，更多的使用的是format进行字符串格式化了，format函数不仅仅是占个位置那么简单了，它甚至可以进行进制转换、各种对齐方式等，功能堪称强大。   
以后建议使用format函数，它和print, str调用的都是实例的`__str__`方法。     
stack overflow参考：[String formatting: % vs .format](https://stackoverflow.com/questions/5082452/string-formatting-vs-format)    

## 9. 迭代器和生成器      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
如果一个对象只有`__iter__`魔术方法，我们可以称它为可迭代对象，但是不是迭代器。   
如果一个对象拥有`__next__`方法，是迭代器。   

定义一个可迭代对象，要实现`__iter__`方法，定义一个迭代器则必须实现`__iter__`方法和`__next__`方法。因为迭代器也是可迭代对象，所以虽然迭代器的定义是拥有`__next__`方法，但是同时是可迭代对象所以必须有`__iter__`方法。   

`__iter__`方法返回的是迭代器类的实例，`__next__`方法返回的是自身，因为自身已经实现了`__iter__`方法（迭代器一定实现了）。   


生成器是一种特殊的迭代器，生成器自动实现了`迭代器协议`，即`__iter__`方法和next方法，不需要再手动实现了。   

在创建一个包含百万元素的列表，要占用很大的内存空间，我们可以采用生成器，能够边计算边循环。   
![可迭代对象](https://github.com/duanmingpy/python-interview/blob/master/images/iterable.png)
## 10. args和**kwargs      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)        
在我们定义形式参数的时候，`*args`和`**kwargs`是一个很方便的选择，但是也可以不使用，比如我们在函数形参中定义时不知道用户会传多少参数时可以使用可变参数，`*args`称为可变位置参数，`**kwargs`称为可变关键字传参，接收的参数分别封成了元组和字典，还有放置在函数参数列表中的位置也要注意，**kwargs放在最后， `*args`必须在`**kwargs`之前。   
args：   
```python   
def average_score(*args): # args是一个元组
    """计算所有学科的平均分"""
    return sum(args) / len(args)

print(average_score(10, 20))
```      
kwargs：   
```python   
def make_tab(**kwargs):
    return kwargs   # 返回的是字典    

print(make_tab(name="tom", grade=100, age=20))

# 输出结果：  {'name': 'tom', 'grade': 100, 'age': 20}

```    
我们同样可以使用`*`进行解包，但是参数要对应整齐。   
stack overflow参考：[Use of \*args and \*\*kwargs](https://stackoverflow.com/questions/3394835/use-of-args-and-kwargs)   
## 11. 面向切面编程AOP和装饰器       
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
AOP和OOP一样，是一种编程范式，这种在运行时，动态地将代码切入到类的指定方法、指定位置上的编程思想就是面向切面的编程。简单理解我认为AOP是OOP的补充，OOP从横向上区分出一个个类来，而AOP则从纵向上向对象中加入特定的代码，有了AOP之后，OOP就变得立体了。   
装饰器就是这种思路了，有AOP的编程经验，理解Python的装饰器就是分分钟的事。既然是装饰器，那么对被装饰的对象来说，一定是功能得到了增强，按方法能增强的地方进行划分，又可以分为以下四类：     
1. 方法调用前； 
2. 方法调用后； 
3. 方法调用前后(环绕)； 
4. 方法调用异常；   
```python   
# 方法调用前：   
def before(func):
    def check(a, *args):
        # 如果小于0，抛出异常
        if a < 0:  # id肯定是大于等于0的
            raise Exception('a is less than zero!')
        else:
            return func(a, *args)
    # 记住，返回的一定是函数            
    return check

@before
def id(*args):
    return args  
    
# -------------------------
# 方法调用后
def afterProxy(func):
    # 修改返回结果
    def add_more (*args):
        result = func(* args)
        return result + 100  # 调用后修改
    return add_more
    
# -------------------------

# 方法调用前后
def afterProxy(func):
    # 修改返回结果
    def add_more (*args):
        #  对结果进行包装
        for value in args:  # 调用前检查
            if value < 0:
                raise ValueError
        result = func(* args)
        return result + 100  # 调用后修改
    return add_more
    
# ------------------------
# 方法调用异常
# 方法调用前后
def afterProxy(func):
    # 修改返回结果
    def add_more (*args):
        #  对结果进行包装
        try:
            result = func(* args)
        except Exception:  # 捕获异常
            return "run error"
        return result + 100  
    return add_more
``` 
StackOverflow参考：[How to make a chain of function decorators?](https://stackoverflow.com/questions/739654/how-to-make-a-chain-of-function-decorators)   

## 12. 鸭子类型      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)        
理解：当我们看到远远的一只鸟走起来像鸭子，游泳也像鸭子，叫声也像鸭子，那么我们就可以称这只鸟为鸭子。     
在编程中：   
我们并不关心对象是什么类型，到底是不是鸭子，只关心行为。   
比如在`python`中，有很多`file-like`的东西，比如`StringIO,GzipFile,socket`。它们有很多相同的方法，我们把它们当作文件使用。    
又比如`list.extend()`方法中,我们并不关心它的参数是不是`list`,只要它是可迭代的,所以它的参数可以是`list/tuple/dict/`字符串/生成器等.   
鸭子类型在动态语言中经常使用，非常灵活，使得`python`不想`java`那样专门去弄一大堆的设计模式。     

## 13. Python中的重载      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
函数重载的目的是解决两个问题。   
1. 可变参数类型；   
2. 可变参数个数。     
设计原则：    
两个函数的功能是相同的，但是传入的参数类型是不同的，此时可以采用函数重载。     
在python中对于`函数功能相同，参数类型不同`这种情况并不需要重载，因为python本身就可以接收各种类型的参数到函数中，但是我们也可以说天生的实现了重载。   
对于`函数功能相同，但是参数个数不同`这种情况我们想到的肯定就是可变或者缺省参数了，这里是函数功能相同，但是如果函数功能不同那么缺省参数也就可以用得上了。    
分析过这两种情况之后我们发现，python就根本不需要单独提出来一个重载的方法，因为天生能够实现。    
知乎参考：[为什么 Python 不支持函数重载？其他函数大部分都支持的？](https://www.zhihu.com/question/20053359)    

## 14. 新式类和旧式类       
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
旧式类没有共同的祖先object，新式类是从`python2.2`版本出现的，到了python3来之后，所有的类都是新式类了，python2版本采用了兼容模式，分为古典类（旧式类）和新式类，新式类中可以使用super。   
在2.2之前python的MRO遵循的是经典算法，2.2版本采用的是新式类算法，到了2.2之后采用了C3算法，能够保证多继承的单一性。   
stack overflow参考：[What is the difference betweeen old style and new style classes in Python？](https://stackoverflow.com/questions/54867/what-is-the-difference-between-old-style-and-new-style-classes-in-python)   
博客园参考：[新式类和经典类](https://www.cnblogs.com/btchenguang/archive/2012/09/17/2689146.html#WizKMOutline_1347874388282497)    

## 15. `__new__`和`__init__`的区别      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
这两个都是类的魔术方法，都是在创建实例的时候使用的，其中`__new__`是一个静态方法，而`__init__`是一个实例方法，在调用`__new__`方法的时候会返回一个创建的实例，然后才进行调用`__init__`进行对实例的实例化。      
例子：   
```python   
class School:
    def __new__(cls, *args, **kwargs):
        obj = super().__new__(School)
        obj.student_number = 10000  # 在new的时候就偷偷的增加一个属性
        return obj

    def __init__(self, name, city):
        self.name = name
        self.city = city


Tsinghua = School('清华', "北京")
print(Tsinghua.student_number)  # 10000  
```   
`__metaclass__`是创建类时起作用.所以我们可以分别使用`__metaclass__`,`__new__`和`__init__`来分别在类创建,实例创建和实例初始化的时候做一些小手脚.  

stack overflow参考：[Why is `__init__()` always called after `__new__()`?](https://stackoverflow.com/questions/674304/why-is-init-always-called-after-new)     
## 16. Python中的作用域      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
Python中，一个变量的作用域总是由代码中被赋值的地方所决定的，如[1\. 函数\-传参](#1-%E5%87%BD%E6%95%B0-%E4%BC%A0%E5%8F%82)中也能体现这样一个作用域的思想，在Python中遇到一个变量的搜索顺序是：    
本地作用域（Local）→ 当前作用域被嵌入的本地作用域（Enclosing locals） → 全局/模块作用域（Global）→ 内置作用域（Built-in）。    

## 17. GIL线程全局锁      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
线程全局锁(Global Interpreter Lock),即Python为了保证线程安全而采取的独立线程运行的限制,说白了就是一个核只能在同一时间运行一个线程.对于io密集型任务，python的多线程起到作用，但对于cpu密集型任务，python的多线程几乎占不到任何优势，还有可能因为争夺资源而变慢。      
可以参考开源中国的翻译文章：[Python最难的问题](https://www.oschina.net/translate/pythons-hardest-problem)      

## 18. 协程      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
简单点说协程是进程和线程的升级版,进程和线程都面临着内核态和用户态的切换问题而耗费许多切换时间，而协程就是用户自己控制切换的时机，不再需要陷入系统的内核态。    
Python里最常见的yield就是协程的思想!    

## 19. 闭包      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
闭包(closure)是函数式编程的重要的语法结构。闭包也是一种组织代码的结构，它同样提高了代码的可重复使用性。   
当一个内嵌函数引用其外部作作用域的变量,我们就会得到一个闭包. 总结一下,创建一个闭包必须满足以下几点:    
1. 必须有一个内嵌函数   
2. 内嵌函数必须引用外部函数中的变量
3. 外部函数的返回值必须是内嵌函数

重点是函数运行后并不会被撤销，就像[16\. Python中的作用域](#16-python%E4%B8%AD%E7%9A%84%E4%BD%9C%E7%94%A8%E5%9F%9F)的instance字典一样,当函数运行完后，instance并不被销毁，而是继续留在内存空间里，这个功能类似类里的类变量，只不过迁移到了函数上。  
闭包就像个空心球一样，你知道外面和里面，但你不知道中间是什么样。     
## 20. lambda匿名函数      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
lambda函数叫做匿名函数的原因是当我们想要再次调用这个函数的时候，我们必须重写一遍，也就是重新定义一遍，虽然可以有标识符记住它，但是我们一般不这样做，真的是用来复用的函数我们会使用def关键字进行定义，注意的是lambda函数中不能出现`return`和`等号`。        
```python   
print((lambda a, b: a + b)(3, 4))  

res = lambda : 100

print(res())  # 可以记住，但是一般不这样做

result = (lambda a, b: a + b)(3, 4)
```   
详细内容参考知乎：[Lambda 表达式有何用处？如何使用？](https://www.zhihu.com/question/20125256)

## 21. Python中函数式编程      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
支持`filter`、`map`、`reduce`三个高阶函数。   
```python   
a = [i for i in range(10) if i & 1]

result = filter(lambda x: x > 5, a)
print(result)  # <filter object at 0x00000204412ECC88>
print(list(result))  # [7, 9]  
```   
```python   
a = [i for i in range(10) if i & 1]

result = map(lambda x: str(x), a)
print(result)  # <map object at 0x0000022BEB4BCAC8>
print(list(result))  # ['1', '3', '5', '7', '9']
```     
```python
from functools import reduce
a = [i for i in range(10) if i & 1]

result = reduce(lambda x, y: x + y, a)
print(result)  # 25
```      
从上面可以看到，`filter`和`map`的结果都是惰性的，`reduce`的结果不是惰性的。      

## 22. Python中的拷贝      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
copy()我们称为浅拷贝，deepcopy()我们称为深拷贝；看下面的例子：   
```python   
import copy
lst = [1, 2, [5, 6]]
print("修改前的lst：", lst)
new_lst1 = lst.copy()
new_lst2 = copy.copy(lst)
new_lst3 = copy.deepcopy(lst)

lst[2][0] = 100  # 把lst的元素修改了,引用类型

print("修改后的lst：", lst)
print("内置的函数copy():", new_lst1)
print("copy模块的函数copy():", new_lst2)
print("copy模块的函数deepcopy():", new_lst3)

# 输出结果：   
修改前的lst： [1, 2, [5, 6]]
修改后的lst： [1, 2, [100, 6]]
内置的函数copy(): [1, 2, [100, 6]]
copy模块的函数copy(): [1, 2, [100, 6]]
copy模块的函数deepcopy(): [1, 2, [5, 6]]
```
从结果中我们可以看到，如果是内置的copy还是copy模块的copy对于列表中存的地址都是复制一份地址过来，所以导致在修改地址背后的数据所有的copy都被修改了；而deepcopy则会顺着地址，把地址后面的对象也复制一份，这样在修改了lst之后，new_lst3没有被修改。    

## 23. Python的垃圾回收机制      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
Python GC主要使用引用计数（reference counting）来跟踪和回收垃圾。在引用计数的基础上，通过“标记-清除”（mark and sweep）解决容器对象可能产生的循环引用问题，通过“分代回收”（generation collection）以空间换时间的方法提高垃圾回收效率。    
**一：引用计数**       
PyObject是每个对象必有的内容，其中ob_refcnt就是做为引用计数。当一个对象有新的引用时，它的ob_refcnt就会增加，当引用它的对象被删除，它的ob_refcnt就会减少.引用计数为0时，该对象生命就结束了。

优点:   
1. 简单   
2. 实时性   

缺点:   
1. 维护引用计数消耗资源   
2. 循环引用   

**二：标记-清除机制**   
基本思路是先按需分配，等到没有空闲内存的时候从寄存器和程序栈上的引用出发，遍历以对象为节点、以引用为边构成的图，把所有可以访问到的对象打上标记，然后清扫一遍内存空间，把所有没标记的对象释放。    

**三：分代技术**    
分代回收的整体思想是：将系统中的所有内存块根据其存活时间划分为不同的集合，每个集合就成为一个“代”，垃圾收集频率随着“代”的存活时间的增大而减小，存活时间通常利用经过几次垃圾回收来度量。

Python默认定义了三代对象集合，索引数越大，对象存活时间越长。

举例： 当某些内存块M经过了3次垃圾收集的清洗之后还存活时，我们就将内存块M划到一个集合A中去，而新分配的内存都划分到集合B中去。当垃圾收集开始工作时，大多数情况都只对集合B进行垃圾回收，而对集合A进行垃圾回收要隔相当长一段时间后才进行，这就使得垃圾收集机制需要处理的内存少了，效率自然就提高了。在这个过程中，集合B中的某些内存块由于存活时间长而会被转移到集合A中，当然，集合A中实际上也存在一些垃圾，这些垃圾的回收会因为这种分代的机制而被延迟。    

## 24. List       
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
List是python的内置数据结构，在标准库中一句pass带过，Cpython是用C语言写的List，下面是C中List的结构：   
**结构定义：**
```C
typedef struct {
    PyObject_VAR_HEAD
    PyObject **ob_item;
    Py_ssize_t allocated;
} PyListObject;
```   
**初始化：**   
假定是空列表[]   
```C  
arguments: size of the list = 0
returns: list object = []
PyListNew:
    nbytes = size * size of global Python object = 0
    allocate new list object
    allocate list of pointers (ob_item) of size nbytes = 0
    clear ob_item
    set list's allocated var to 0 = 0 slots
    return list object 
```   
非常重要的是知道list申请内存空间的大小（后文用allocated代替）的大小和list实际存储元素所占空间的大小(ob_size)之间的关系，ob_size的大小和len(L)是一样的，而allocated的大小是在内存中已经申请空间大小。通常你会看到allocated的值要比ob_size的值要大。这是为了避免每次有新元素加入list时都要调用realloc进行内存分配。接下来我们会看到更多关于这些的内容。    

**追加：**   
使用Append函数会调用内部的C函数app1()   
```C   
arguments: list object, new element
returns: 0 if OK, -1 if not
app1:
    n = size of list
    call list_resize() to resize the list to size n+1 = 0 + 1 = 1
    list[n] = list[0] = new element
    return 0
```     
`list_resize()`会申请多余的空间以避免调用多次`list_resize()`，list的增长模型是: 0, 4, 8, 16, 25, 35, 46, 58, 72, 88, ...    
```C   
arguments: list object, new size
returns: 0 if OK, -1 if not
list_resize:
    new_allocated = (newsize >> 3) + (newsize < 9 ? 3 : 6) = 3
    new_allocated += newsize = 3 + 1 = 4
    resize ob_item (list of pointers) to size new_allocated
    return 0
```   
还有其他对应的函数可以参考网上的解读。    
推荐简书上的解答：[Python中list的实现](https://www.jianshu.com/p/J4U6rR)     

## 25. Python中的is      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
在python中我们经常会有判断两个值或两个对象是否相等或是同一个，在两个对象使用`==`进行比较的时候会调用相应的实例魔术方法`__eq__`，而使用is进行比较的时候会比较两个对象的内存地址。   
```python   
class MyClass1:
    def __init__(self):
        self.num = 1
    def __eq__(self, other):
        return self.num == other

class MyClass2:
    def __init__(self):
        self.num = 1

    def __eq__(self, other):
        return other == self.num

print(MyClass1() == MyClass2())  # True
print(MyClass1() is MyClass2())  # False
```   
看上面的例子，真正比较的是两个对象的num属性，而is比较的是对象的地址；如果没有定义`__eq__`则`==`会比较内存地址，一般容器`==`比较的是大小，非容器的`==`比较的是地址。    

## 26. read, readline和readlines      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
read是读取整个文件；   
readline是读取一行，使用生成器方法；   
readlines是读取整个文件到一个迭代器供我们遍历。   

## 27. Python2和Python3的区别      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)       
print函数的变化：   
```python   
# Python2
print 'Python', python_version()
print 'Hello, World!'
print('Hello, World!')
print "text", ; print 'print more text on the same line'

 
run result:
Python 2.7.6
Hello, World!
Hello, World!
text print more text on the same line

# Python3
print('Python', python_version())
print('Hello, World!')
print("some text,", end="") 
print(' print more text on the same line')


run result:
Python 3.7.4
Hello, World!
some text, print more text on the same line
```     

整除的变化：   
```python  
# python2
print 'Python', python_version()
print '3 / 2 =', 3 / 2
print '3 // 2 =', 3 // 2
print '3 / 2.0 =', 3 / 2.0
print '3 // 2.0 =', 3 // 2.0

# 输出：   
run result:
Python 2.7.6
3 / 2 = 1
3 // 2 = 1
3 / 2.0 = 1.5
3 // 2.0 = 1.0

# python3
print('Python', python_version())
print('3 / 2 =', 3 / 2)
print('3 // 2 =', 3 // 2)
print('3 / 2.0 =', 3 / 2.0)
print('3 // 2.0 =', 3 // 2.0)

# 输出：
run result:
Python 3.4.1
3 / 2 = 1.5
3 // 2 = 1
3 / 2.0 = 1.5
3 // 2.0 = 1.0
```    
具体可以参考ShinChan的博客：[Python 2.x 与 Python 3.x的主要差异](http://chenqx.github.io/2014/11/10/Key-differences-between-Python-2-7-x-and-Python-3-x/)    

## 28. super init      
[**回到顶部**](#python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)        
在前面[14\. 新式类和旧式类](#14-%E6%96%B0%E5%BC%8F%E7%B1%BB%E5%92%8C%E6%97%A7%E5%BC%8F%E7%B1%BB)中也提了因为MRO的原因，python3可以直接使用`super().__init__()`，这是因为C3算法把搜索路径确定了，而在python2.2之前并不能确定，所以必须要使用`super(ChildB, self).__init__()`来确定。    
stack overflow参考：[Understanding Python super() with `__init__()` methods \[duplicate\]](https://stackoverflow.com/questions/576169/understanding-python-super-with-init-methods)    
CSDN参考：[
Python2.7中的super方法浅见](https://blog.csdn.net/mrlevo520/article/details/51712440)
