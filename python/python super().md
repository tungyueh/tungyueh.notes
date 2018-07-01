## Python super()
* super() 可以用來呼叫 parent 或 sibling class 的 function
* super([type[, object-or-type]])
  * class type(object)
  * class type(name, bases, dict)
  * 傳 objecy 給 type 會回傳 object 的型態
  * 傳 name, bases, dict 會回傳一個 type object，等於宣告 class 的意思，下面的兩段 code 意思一樣
  ``` python
  class X:
      a = 1
  ```
  ``` python
  X = type('X', (object,), dict(a=1))
  ```

### Usage
``` python
class C(B):
    def method(self, arg):
        super().method(arg)  # This does the same thing as:
                             # super(C, self).method(arg)
```

### Method Resolution Order(MRO)
* super() 本質上是根據 MRO 找出下一個 class
``` python
def super(cls, inst):
    mro = inst.__class__.mro()
    return mro[mro.index(cls) + 1]
```
``` python
class A(object):
    pass

class B(A):
    pass

b = B()
print b.__class__
# <class '__main__.B'>
print b.__class__.mro()
# [<class '__main__.B'>, <class '__main__.A'>, <type 'object'>]
print b.__class__.mro().index(A)
# 1
print b.__class__.mro().index(B)
# 0
```
* MRO 用 C3 linearization 來決定的
    * base 一定在 derived 後面
    * 若有多個 base 則順序不變
* C3 linearization
    * 自己 merge 後面的 class
    * 若 head OK 就拿出來並刪掉後面所有存在的
    * head 不 OK 就換下個 list
    * head OK 代表 head 不存在任何 list 的 tail
* Diamond inheritance
``` python
class Base(object):
    def __init__(self):
        print('Base.__init__')


class A(Base):
    def __init__(self):
        print('A.__init__ begin')
        Base.__init__(self)
        print('A.__init__ end')

class B(Base):
    def __init__(self):
        print('B.__init__ begin')
        Base.__init__(self)
        print('B.__init__ end')

class C(A,B):
    def __init__(self):
        print('C.__init__ begin')
        A.__init__(self)
        B.__init__(self)
        print('C.__init__ end')

c = C()
print C.__mro__
# Output:
# C.__init__ begin
# A.__init__ begin
# Base.__init__
# A.__init__ end
# B.__init__ begin
# Base.__init__
# B.__init__ end
# C.__init__ end
# (<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>, <class '__main__.Base'>, <type 'object'>)
```

### Another example in effective python item 25
``` python 
class MyBaseClass(object):
    def __init__(self, x):
        self.value = x

class PlusTwo(MyBaseClass):
    def __init__(self, x):
        super(TimesTwo, self).__init__(x)
        self.value += 2
        
class TimesFive(MyBaseClass):
    def __init__(self, x):
        super(TimeFive, self).__init__(x)
        self.value *= 5
class GoodWay(TimesFive. PlusTwo):
    def __init__(self, x):
        super(GoodWay, self).__init__(x)
x = GoodWay(5)
print x.value  # 35
```
因為 MRO 順序是 TimesFive, PlusTwo, MyBaseClass
所以 GoodWay 呼叫 super 則是 TimesFive, TimesFive 又呼叫 super 則是 PlusTwo, PlusTwo 又再一次呼叫 MyBaseClass, 所以將 value 初始化後就先加二再乘五。

### Pratical Use Cases
``` python
class LoggingDict(dict):
    def __setitem__(self, key, value):
        logging.info('Settingto %r' % (key, value))
        super().__setitem__(key, value)
```
* 這個 class 有跟 dict 一樣的功能，因為是繼承 dict 的 class
* 但是將 __setitem__ 做些改動，會先 log 在做原本 dict 的 __setitem__
* 用 super 去使用 dict 的 __setitem__ 的好處是可以用推論的找出要用誰的 __setitem__ 如果用 dict().__setitem__ 的方式，若想改變繼承的object 則需要將程式碼做改動
``` python
from collections import Counter, OrderedDict

class OrderedCounter(Counter, OrderedDict):
     'Counter that remembers the order elements are first seen'
     def __repr__(self):
         return '%s(%r)' % (self.__class__.__name__,
                            OrderedDict(self))
     def __reduce__(self):
         return self.__class__, (OrderedDict(self),)

oc = OrderedCounter('abracadabra')
```
* OrderedCounter(OrderedDict([('a', 5), ('b', 2), ('r', 2), ('c', 1), ('d', 1)]))
* OrderedCounter.__mro__: (<class 'OrderedCounter'>, <class 'collections.Counter'>, <class 'collections.OrderedDict'>, <class 'dict'>, <class 'object'>)
* __setitem__ -> <class 'collections.OrderedDict'>
* __getitem__ -> <class 'dict'>

### My test
* 覺得為什麼 super 第一個參數都要放自己 所以來嘗試不放自己會怎麼樣
``` python
class A(object):
    def __init__(self):
        print 'enter A'
        print 'leave A'

class B(A):
    def __init__(self):
        print 'enter B'
        super(C, self).__init__()
        print 'leave B'

class C(A):
    def __init__(self):
        print 'enter C'
        super(C, self).__init__()
        print 'leave C'

class D(B, C):
    pass
d = D()
print d.__class__.__mro_
# output:
# enter B
# enter A
# leave A
# leave B
# (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <type 'object'>)
```
* 證實 super 只是 MRO 順序的下一個 class，所以我在 B init 放 super(C, self).__init__() 就會找 Ｃ 的下一個 Ａ 使用 Ａ的 __init__() 而跳過 Ｃ


### Reference
[The Python Standard Library Built-in Functions super()](https://docs.python.org/3/library/functions.html#super)

[理解 Python super](https://laike9m.com/blog/li-jie-python-super,70/)

[The Python 2.3 Method Resolution Order](https://www.python.org/download/releases/2.3/mro/)

[Python’s super() considered super!](https://rhettinger.wordpress.com/2011/05/26/super-considered-super/)

###### tags:`python`
