# Python Metaclasses

## Type and Class
* Python 中所有東西包括 class 都是 object 所以 class 也有 type
* class 的 type 就是 type

``` python
>>> class Foo:
...     pass
>>> type(Foo)
<class 'type'>
```

* type 是 metaclass, type 的 instance 是 class
![](https://files.realpython.com/media/class-chain.5cb031a299fe.png)
    * x 是 Foo 的 instance
    * Foo 是 type 的 instance
    * type 也是 type 的 instance

## Defining a Class Dynamically
* built-in type() 只傳一個參數會回傳 object 的 type
* type(\<name\>, \<bases\>, \<dct\>) 會產生一個 class
    * name: string of class name will become \_\_name\_\_
    * bases: tuple of base class will become \_\_bases\_\_
    * dct: dictionary of class will become \_\_dict\_\_

## Custom Metaclasses

``` python
>>> class Foo:
...     pass
>>> f = Foo()
```

* `f = Foo()` 讓 Foo() 根據下面流程產生一個 instance
    * \_\_call\_\_() method 會被使用
    * \_\_call\_\_() 會用到 \_\_new\_\_() 跟 \_\_init\_\_()
    * Foo 可以藉由 define \_\_new\_\_() 跟 \_\_init\_\_() 來客製化
    
``` python
>>> def new(cls):
...     x = object.__new__(cls)
...     x.attr = 100
...     return x
>>> Foo.__new__ = new
>>> f = Foo()
>>> f.attr
100
```

* 根據上面的概念可以 assign 自己定義的 new function 給 object 的 \_\_new\_\_ 客製化 class 的 instance
* 以同樣概念想要客製化 class 就必須重新定義 type 的 \_\_new\_\_ 來產生客製化的 instance class

``` python
>>> def new(cls):
...     x = type.__new__(cls)
...     x.attr = 100
...     return x
...
>>> type.__new__ = new
Traceback (most recent call last):
  File "<pyshell#77>", line 1, in <module>
    type.__new__ = new
TypeError: can't set attributes of built-in/extension type 'type'
```

* 但 python 不允許修改 type metaclass 的 \_\_new\_\_() method
* 因此我們需要繼承 type 來自己定義 \_\_new\_\_() method

``` python
class Meta(type):
    def __new__(cls, name, bases, dct):
        x = super().__new__(cls, name, bases, dct)
        x.attr = 100
        return x

>>> class Foo(metaclass=Meta):
...     pass
...
>>> Foo.attr
100
```

* type 是個 metaclass 所以繼承它的 Meta 也是 metaclass


## Reference
https://realpython.com/python-metaclasses/

https://blog.ionelmc.ro/2015/02/09/understanding-python-metaclasses/

https://python-3-patterns-idioms-test.readthedocs.io/en/latest/Metaprogramming.html

###### tags: `python`
