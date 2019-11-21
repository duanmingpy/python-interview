# 介绍   
我相信能够看这篇README的人都是已经具备python开发能力的程序员了，在所有的知识点中我只进行概要性的提示和简单例子的选举，具体内容相信读者都可以回忆出来，在参照整体的知识点拨之后，相信能够应对参加的python面试，祝好运！  
如果还是学习者，可以在整个学习的过程中，把这些点给提出来精学，这些都是python中的精华，当你整个学习阶段完成之后，并熟悉了这些设计方式和技巧之后，我相信你一定可以在这方面有所建树，同样，祝好运！

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
     
     
       
       
### 更新中....


