---
title: python 装饰器
date: 2020-02-29 11:52:07
tags:
   - Python
---

# Python装饰器模式

## Decorator in Design Pattern

打开设计模式课的PPT，看到了当初老师讲的老虎的例子
> Tiger属于Animals类，继承了Eat方法，目前的需求是Tiger中有一种TigerA在Eat前后额外的动作，比如Eat前后有Drink，为尽可能地重用代码、降低耦合、可替换等设计原则，将新的类用装饰器模式封装，类图如下

![](https://s2.ax1x.com/2020/02/29/3yV8HJ.jpg)

### 老师总结的优缺点
- 优点：

1. 保持抽象层上关系的稳定；
2. 比单独使用组合或继承灵活； 
3. 随时扩展功能或职责；
- 不足：
1. 可能产生许多更小的子类；
2. Component类过大时，效率较低，此时可考虑使用策略模式
3. 嵌套很多比较复杂（多层装饰器）



## Decorator in Python

### Aspect-Oriented Programming

面向切面编程，在函数进入或退出时不断加入新方法。此前遇到的case：js常用编程方法callback以实现加入新的相应

### 装饰器介绍

​	python中有函数装饰器和类装饰器，此外functool中也有wraps

> 装饰器**本质上是一个Python函数**,它可以让其他函数在**不需要做任何代码变动的前提下增加额外功能**,装饰器的**返回值也是一个函数对象** 它经常用于有**切面需求**的场景,比如:**插入日志 性能测试 事务处理 缓存 权限校验**等场景 装饰器是解决这类问题的绝佳设计,有了装饰器,我们就可以抽离出大量与函数功能本身无关的雷同代码并继续重用 概括的讲,**装饰器的作用就是为已经存在的对象添加额外的功能**。

## Usage

### 函数装饰器

**原始代码**

```python
def foo():
    print('i am foo')
```

新需求：在foo函数中追加日志需求，并且这种日志需求后续可能在其他函数中也要用到

**改动v1**  -------   本人常做的：）

```python
def foo():
    print('i am foo')
	logging.info("foo is running")
```

存在的问题：1. 后续无法重用 2. 改变了原有代码

**改动v2** ------- 简单的装饰器

```python
def use logging(func): 
    logging.warn("%s is running" % func. name ) 
    func() 
    
def bar(): 
    print('i am bar')

use_logging(bar)
```

 存在的问题：1. 改变了上层的调用

**改动v3** ------- 真正的python装饰器，python为其添加了语法糖@

```python
def use_logging(func): 
	def wrapper(*args, **kwargs): 
    	logging.warn("%s is running" % func. name ) 
   		return func(*args) 
 	return wrapper 
 	
@use_logging 
def foo(): 
	print("i am foo")
	
@use_logging 
def bar(): 
	print("i am bar")
    
    
bar()
```

执行`bar()`，就相当于执行`use_logging(bar)()`，此时原来的代码内部我们没有做改动，上层的调用也不需要改

#### 带参数装饰器

装饰器变成三层函数，最外面一层接受装饰器的参数，中间层接受被装饰的函数，最里面层接受被装饰的函数的参数（通常用`*args`和`**kwargs`接受）

```python
import functools

def log_with_param(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            print('call %s():' % func.__name__)
            print('args = {}'.format(*args))
            print('log_param = {}'.format(text))
            return func(*args, **kwargs)

        return wrapper

    return decorator
    
@log_with_param("param")
def test_with_param(p):
    print(test_with_param.__name__)
```



#### 多装饰器

```python
@wrap1
@wrap2
def func():
	pass
```



#### functools.wraps

目的：原来的装饰器在打印函数的`__name__`等内置变量时都显示的是装饰器的；使用functools.wraps可以解决这个问题。

相当于functools.wraps来将这些内置变量改写成被装饰函数的内置变量

```python
import functools


def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print('call %s():' % func.__name__)
        print('args = {}'.format(*args))
        return func(*args, **kwargs)

    return wrapper
```



### 类装饰器

- 类装饰器的意思是用一个装饰器类来装饰一个函数，这个类必须实现`__init__`和`__call__`内置函数

- `__init__ `：接收被装饰函数
- `__call__ `：实现装饰逻辑

```python
class logger(object):
  def __init__(self, level='INFO'):
    self.level = level
 
  def __call__(self, func): # 接受函数
    def wrapper(*args, **kwargs):
      print("[{level}]: the function {func}() is running..."\
        .format(level=self.level, func=func.__name__))
      func(*args, **kwargs)
    return wrapper #返回函数
 
@logger(level='WARNING')
def say(something):
  print("say {}!".format(something))
 
say("hello")
```

> 绝大多数装饰器都是基于函数和闭包实现的，但这并非制造装饰器的唯一方式。
>
> 事实上，Python 对某个对象是否能通过装饰器（ @decorator）形式使用只有一个要求：decorator 必须是一个“可被调用（callable）的对象。
>
> 对于这个 callable 对象，我们最熟悉的就是函数了。
>
> 除函数之外，类也可以是 callable 对象，只要实现了__call__ 函数，还有比较少人使用的偏函数也是 callable 对象。

## Appendix

### *args, **kwargs

> **`*args`**表示任何多个**无名参数**，它是一个**tuple**；`**kwargs`表示**关键字参数**，它是一个**dict**。并且同时使用`*args`和`**kwargs`时，必须`*args`参数列要在`**kwargs`前，像`foo(a=1, b='2', c=3, a', 1, None, )`这样调用的话，会提示语法错误`“SyntaxError: non-keyword arg after keyword arg”`。

1. args 是一个tuple，`*args`是多个参数；同理kwargs是dict，`**kwargs`是多个关键字参数
2. 关键字参数就是在调用时标记形参名的参数，如 `func(3,b=1,c='hello')`，这里3是无名参数，b和c是关键字参数
3. args和kwargs常用在装饰器模式等切面编程中以及一些参数个数任意的函数

## Reference

1. [如何理解Python装饰器](https://www.zhihu.com/question/26930016)

## Gain
1. 设计模式真的是需要自己再认真学习的一个topic，初次在课堂上学习的时候只是认识了每个模式是什么，属于哪个型，判断这段代码用的是什么模式，优缺点；没有去想这种模式提出是为了解决什么问题，这个模式的独特性在哪里；当自己要写代码的时候，脑子里没有设计模式的思想。
