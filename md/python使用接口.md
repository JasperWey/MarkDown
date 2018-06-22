​	python tips





## `__init__`

## `__call__`

​	

## `__new__`

##`self`与`cls`

## classmethod与staticmethod

## setter与getter

## 元类

编码与解码

**lambda存在的意义**: 最大的好处是匿名,不用费神去想函数名

函数调用后使用装饰器，思路实现class A的`__exit__`方法，采用with A:修饰funB

```python
class A(object):
	def __enter__(self):
        pass
    
    def __exit__(self):
        """some codes"""
        pass
    
    
def funcB(*args, **kwarg):
    """logical code"""
    
    
with A:
    funcB()
```



Q:协程为什么效率比线程高？

A:不用加锁。因为同一时间，只有一个协程在运行，所以无需加锁，省去了频繁的加锁解锁操作，自然效率高



`__enter__`与`__exit__`

with用法



##map用法

![map](/home/jiangwei/桌面/md/images/map.png)





1. 获取dir

   ```python
   ts = time.time()
   ts = datetime.datetime.fromtimestamp(ts).strftime('%Y-%m-%d_%H'-%M-%S_)
   self.temp_dir = tempfile.mkdtemp(prefix='kolla-' + ts)
   self.working_dir = os.path.join(self.temp_dir, 'docker')
   ```

##迭代器是一个对象，而生成器是一个函数

迭代器和生成器是python中两个非常强大的特性，编写程序时你可以不使用生成器达到同样的效果，但是生成器让你的程序更加pythonic。创建生成器非常简单，只要在函数中加入yield语句即可。函数中每次使用yield产生一个值，函数就返回该值，然后停止执行，等待被激活，被激活后继续在原来的位置执行。下边的例子实现了同样的功能：

```python
#!/usr/bin/env python
#coding=utf-8
def fib():
    a,b = 0,1
    while 1:
        a,b = b,a+b
        yield a
for f in fib():
    if f < 10000:
        print f
    else:
        break
```

​    



- [x] 读取文件使用readlines()优势
- [x] python类
- [ ] 类加载
- [x] 多线程
- [ ] 与系统环境交互

### `__call__`方法

 想让类的*实例*变成可调动的对象，需要实现`__call__`方法



```python
class A(object):
    def __init__(self, a, b):
        self.__a = a
        self.__b = b
    def myprint(self):
        print 'a=', self.__a, 'b=', self.__b

    def __call__(self, num, num2):
        print 'call:', num + num2
    
a1=A(10, 20)
a1.myprint()
a1(80, 2)
```

### `__new__` 和 `__init__`方法

`__new__`方法是创建对象之前调用，用于设计模式的单例和工厂模式

`__init__`是创建对象时调用的，在`__new__`调用之后

#getattr

方法**getattr**只有当没有定义的方法调用时，才是调用他。当fn1方法传入参数时，我们可以给mydefault方法增加一个*args不定参数来兼容。

```python
class A(object):
    def __init__(self, a, b):
        self.a1 = a
        self.b1 = b
        print 'init'
    def mydefault(self):
        print 'default'
    def __getattr__(self, name):
        return self.mydefault

a1 = A(10,20)
a1.fn1()
a1.fn2()
a1.fn3()
```



## 静态方法与类方法  普通实例方法

####从被调用的角度看:

1. 实例方法属于实例拥有,仅能有类的实例发起调用,**可以定义一些差异化的属性,而类方法和静态方法,其属性被所有实例共有,所以无法设置差异化的属性**
2. 类方法,由@classmethod修饰,第一个参数为类,在方法中,无法调用实例方法,因为它是属于实例的而不是类的.但是可以调用静态方法
3. 静态方法由@staticmethod修饰,虽然它属于类,可以被**实例或者类直接调用**,但无法使用与类或者实例相关的任何属性,因为定义时就没有把类cls或者实例self作为参数传进去.

#### 从实例或类的角度看:

1. 类可以直接调用类方法和静态方法
2. 实例可以调用三者



```python
class A(object):
    #普通类方法,或实例方法
    def foo(self):
        self.faa()
        self.fbb()
        print "common class method"
    
    #静态方法
    @staticmethod
    def faa():
        print "static method"

    #类方法
    @classmethod
    def fbb(cls):
        cls.faa()
      	print "class method"
        
        
a = A()
a.foo()
a.faa()
a.fbb()
A.faa()
A.fbb()
#A.foo()  错误
```



#闭包

```python
def mulby(num):
    def gn(val):
        return num * val

    return gn


zw = mulby(7)
print zw
print(zw(9))
```

闭包=函数块+定义函数时的环境

### 使用闭包是注意事项

闭包中不能修改外部作用域的局部变量，具体查看https://www.cnblogs.com/JohnABC/p/4076855.html



1. `__del__`
   1. 对象生命周期结束时，del会被调用，可以理解我析构函数
   2. 可以作为垃圾回收机制使用





# 并发编程



#### python多进程编程

https://blog.csdn.net/u014556057/article/details/61616902

采用多进程加速计算pi的值

```python
# coding: utf-8
import os
import sys
import math
import redis
def slice(mink, maxk):
    s = 0.0
    for k in range(mink, maxk):
        s += 1.0/(2*k+1)/(2*k+1)
    return s
def pi(n):
    pids = []
    unit = n / 10
    client = redis.StrictRedis()
    client.delete("result")  # 保证结果集是干净的
    del client  # 关闭连接
    for i in range(10):  # 分10个子进程
        mink = unit * i
        maxk = mink + unit
        pid = os.fork()
        if pid > 0:
            pids.append(pid)
        else:
            s = slice(mink, maxk)  # 子进程开始计算
            client = redis.StrictRedis()
            client.rpush("result", str(s))  # 传递子进程结果
            sys.exit(0)  # 子进程结束
    for pid in pids:
        os.waitpid(pid, 0)  # 等待子进程结束
    sum = 0
    client = redis.StrictRedis()
    for s in client.lrange("result", 0, -1):
        sum += float(s)  # 收集子进程计算结果
    return math.sqrt(sum * 8)
print pi(10000000)
```



## python多线程编程

###有两种实现方式，thread和threading

1. thread应避免使用
   1. 其不支持守护线程，主线程退出，所有的线程都会被强制退出
   2. 支持的同步原语少

#### 守护线程

1. 如果你设置一个线程为守护线程，则表示该线程是不重要的，当进程退出时，不用等待这个线程退出，
2. 设置守护线程，```thread.setDaemon(True)```, 查看是否是守护线程: ```thread.isDaemon()```
3. 新的子线程会继承父线程的Deamon属性


GreenThead GileThread

# 浅拷贝与深拷贝

1. 赋值种类
   1. 引用赋值
   2. 浅拷贝
   3. 深拷贝

### 浅拷贝

对象进行浅拷贝，该过程会重新生成一个新对象，对于对象中的元素，浅拷贝只会使用原始元素的引用

使用下面的操作的时候，会产生浅拷贝的效果：

- 使用工厂函数（如list/dir/set）
- 使用copy模块中的copy()函数


###深拷贝

通过copy模块里面的深拷贝函数deepcopy()，对will指向的对象进行深拷贝，然后深拷贝生成的新对象赋值给wilber变量

- 跟浅拷贝类似，深拷贝也会创建一个新的对象，这个例子中**"wilber is not will"**
- 但是，对于对象中的元素，深拷贝都会重新生成一份（有特殊情况，下面会说明），而不是简单的使用原始元素的引用（内存地址）



####对象的赋值和拷贝，以及它们之间的差异：

- Python中对象的赋值都是进行对象引用（内存地址）传递

- 使用copy.copy()，可以进行对象的浅拷贝，它复制了对象，但对于对象中的元素，依然使用原始的引用.

- 如果需要复制一个容器对象，以及它里面的所有元素（包含元素的子元素），可以使用copy.deepcopy()进行深拷贝

- 对于非容器类型（如数字、字符串、和其他'原子'类型的对象）没有被拷贝一说

- 如果元祖变量只包含原子类型对象，则不能深拷贝

  ​

  使用a = b[:]赋值时，会重新开辟空间存储该值，对a修改也不会影响到b的值

```
>>> a = range(1,10)
>>> a
[1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> b=a
>>> b
[1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> b[2]=11
>>> b
[1, 2, 11, 4, 5, 6, 7, 8, 9]
>>> a
[1, 2, 11, 4, 5, 6, 7, 8, 9]
>>> c = b[:]
>>> c[4] = 223
>>> c
[1, 2, 11, 4, 223, 6, 7, 8, 9]
>>> b
[1, 2, 11, 4, 5, 6, 7, 8, 9]
>>> a
[1, 2, 11, 4, 5, 6, 7, 8, 9]

```





## 上下文管理器

```python
from time import time

def run2():
    l = []
    for i in range(5000000):
        l.extend([i])
    return len(l)

class ElapsedTime():
    def __enter__(self):
        self.start_time = time()
        return self

    def __exit__(self, exception_type, exception_value, traceback):
        self.end_time = time()
        print('run time: {:.6f}s'.format(self.end_time - self.start_time))

with ElapsedTime():
    run2()
```



## Slots使用

在类中，如果要限制类属性的定义，可以使用`__slots__`属性，具体使用如下：

```python
class Student(object):
    __slots__ = ( "name", "age", "score")

s = Student()
s.name = 100
print(s.name)

```

说明：

1. 类的属性定义只能采用slots中指定的名字
2. 采用slots后，类中就没有dict了，在创建数量庞大的类时，采用slots可以有效减少内存消耗


## 动态规划问题



## pdb调试方法



##### 可以直接在运行时,指定pdb调试

```shell
python -m pdb test.py
```

\1. where(w) 找出当前代码运行位置

\2. list(l) 显示当前代码的部分上下文

\3. list <line number> 显示指定行的上下文

\4. list <line number1, line number2> 显示指定开始行到结束行的代码

\5. up(u) 返回上个调用点

\6. down(d) 返回下个调用点

\7. args(a) 显示当前所有变量

\8. print(p) 打印表达式结果

9 ! 运行python命令，比如!test='hello' 将会把test变量的值改变为hello

\10. pp 打印美化过的表达式结果

\11. step 步进运行至下行代码（如果是调用函数，则运行至所调用函数的第一行）

\12. next 运行至下行代码（如果是调用函数，会直接运行完此函数）

\13. until 运行至当前代码端底部

\14. return 运行至return代码处

\15. break <line number> 运行时设置断点

\16. continue 运行程序直至遇到下一个断点

\17. break <file name:line number> 运行时设置另一个文件的断点

\18. break 显示断点情况

\19. disable <break number> 将指定的断点失效（但存在）

\20. enable <break number> 将指定的断点生效

\21. clear <break number> 删除断点

\22. tbreak <line number> 运行时设置临时断点（运行一次后自动删除）

\23. break <line number> <condition> 运行时设置断点，当满足condition条件时触发断点，ex: break 11 i > 10 表示在第11行代码处，当变量i大于10时，触发断点

\24. condition <break number> <condition> 设置指定断点的触发条件

\25. ignore <break number> <n> 忽略指定断点n次

\26. commands <break number> ... end 对指定断点编写脚本，当运行到该断点时自动执行





## yield用法

- 通常的for...in...循环中，in后面是一个数组，这个数组就是一个可迭代对象，类似的还有链表，字符串，文件。它可以是mylist = [1, 2, 3]，也可以是mylist = [x*x for x in range(3)]。

  它的缺陷是所有数据都在内存中，如果有海量数据的话将会非常耗内存。


- 生成器是可以迭代的，但只可以读取它一次。因为用的时候才生成。比如 mygenerator = (x*x for x in range(3))，注意这里用到了()，它就不是数组，而上面的例子是[]。


- 我理解的生成器(generator)能够迭代的关键是它有一个next()方法，工作原理就是通过重复调用next()方法，直到捕获一个异常。可以用上面的mygenerator测试。


- 带有 yield 的函数不再是一个普通函数，而是一个生成器generator，可用于迭代，工作原理同上。


- yield 是一个类似 return 的关键字，迭代一次遇到yield时就返回yield后面(右边)的值。重点是：下一次迭代时，从上一次迭代遇到的yield后面的代码(下一行)开始执行。


- 简要理解：yield就是 return 返回一个值，并且记住这个返回的位置，下次迭代就从这个位置后(下一行)开始。


- 带有yield的函数不仅仅只用于for循环中，而且可用于某个函数的参数，只要这个函数的参数允许迭代参数。比如array.extend函数，它的原型是array.extend(iterable)。


- send(msg)与next()的区别在于send可以传递参数给yield表达式，这时传递的参数会作为yield表达式的值，而yield的参数是返回给调用者的值。——换句话说，就是send可以强行修改上一个yield表达式值。比如函数中有一个yield赋值，a = yield 5，第一次迭代到这里会返回5，a还没有赋值。第二次迭代时，使用.send(10)，那么，就是强行修改yield 5表达式的值为10，本来是5的，那么a=10


- send(msg)与next()都有返回值，它们的返回值是当前迭代遇到yield时，yield后面表达式的值，其实就是当前迭代中yield后面的参数。


- 第一次调用时必须先next()或send(None)，否则会报错，send后之所以为None是因为这时候没有上一个yield(根据第8条)。可以认为，next()等同于send(None)。