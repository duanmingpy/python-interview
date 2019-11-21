# 介绍   
我相信能够看这篇README的人都是已经具备python开发能力的程序员了，在所有的知识点中我只进行概要性的提示和简单例子的选举，具体内容相信读者都可以回忆出来，在参照整体的知识点拨之后，相信能够应对参加的python面试，祝好运！  
如果还是学习者，可以在整个学习的过程中，把这些点给提出来精学，这些都是python中的精华，当你整个学习阶段完成之后，并熟悉了这些设计方式和技巧之后，我相信你一定可以在这方面有所建树，同样，祝好运！

# 面试宝典结构   
## 第一部分是：[Python语言的要点](https://github.com/duanmingpy/python-interview/blob/master/1-Python%E8%AF%AD%E8%A8%80%E9%AB%98%E9%A2%91%E9%87%8D%E7%82%B9%E6%B1%87%E6%80%BB)   
## 第二部分是：操作系统的要点  
## 第三部分是：网络的要点
## 第四部分是：数据库的要点   
## 第五部分是：数据结构的重点  
## 第六部分是：经典算法题目汇总  
## 第七部分是：设计模式总结

#### 目前草稿版的第一部分、第二部分、第三部分、第四部分、第五部分都已经编写完毕，正在往这个仓库中整理第一部分，为了检查错误和复习，我会一个字一个字、一个例子一个例子的重写排好版传上来，草稿版过于杂乱就不直接上传了，望理解，可以⭐Star鼓励一下！   

# Python语言   
## 1. 函数-传参   
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
元类又叫metaclass，在python中我们使用`type(59)`就可以知道59是int类型，但是你考虑过int类的类型吗？这就是元类问题，python中一切皆对象，就像linux中的一切皆文件哲学那么彻底，所以类也是对象，既然是对象就有类型，所有新类型的缺省都是type类型，可以修改，在python中，当我们创建一个对象的时候，它会进行类型检查，如果我们没指定类型缺省就是type类了。可以参考下图：   
![元类](https://github.com/duanmingpy/python-interview/blob/master/images/yuanlei.png)
   
细节可以参考stack overflow的解答:[连接地址](http://stackoverflow.com/questions/100003/what-is-a-metaclass-in-python)    

## 3. @staticmethod和@classmethod两个装饰器    
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


## Python的自省   
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
其中%是类c语言的风格，现如今随着发展，学python的同学可能并不是很熟悉C语言，更多的使用的是format进行字符串格式化了，format函数不仅仅是占个位置那么简单了，它甚至可以进行进制转换、各种对齐方式等，功能堪称强大。   
以后建议使用format函数，它和print, str调用的都是实例的`__str__`方法。     
stack overflow参考：[String formatting: % vs .format](https://stackoverflow.com/questions/5082452/string-formatting-vs-format)    

## 9. 迭代器和生成器    
如果一个对象只有`__iter__`魔术方法，我们可以称它为可迭代对象，但是不是迭代器。   
如果一个对象拥有`__next__`方法，是迭代器。   

定义一个可迭代对象，要实现`__iter__`方法，定义一个迭代器则必须实现`__iter__`方法和`__next__`方法。因为迭代器也是可迭代对象，所以虽然迭代器的定义是拥有`__next__`方法，但是同时是可迭代对象所以必须有`__iter__`方法。   

`__iter__`方法返回的是迭代器类的实例，`__next__`方法返回的是自身，因为自身已经实现了`__iter__`方法（迭代器一定实现了）。   


生成器是一种特殊的迭代器，生成器自动实现了`迭代器协议`，即`__iter__`方法和next方法，不需要再手动实现了。   

在创建一个包含百万元素的列表，要占用很大的内存空间，我们可以采用生成器，能够边计算边循环。   
![可迭代对象](https://github.com/duanmingpy/python-interview/blob/master/images/iterable.png)
## 10. args和**kwargs   
### 更新中...

