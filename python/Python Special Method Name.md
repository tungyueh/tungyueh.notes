# Python Special Method Name
* object 可以執行特定動作如果有相對應的 special method
* 由於 operator overloading，我們可以自定義我們想要的行為

## Basic
* `object.__repr__(self)`: called by repr(), 必須回傳 string, 可以用來自己想要印出關於這個 object 的資訊
* `object.__str__(self)`: called by str(object) 跟 print() format()
* repr 會在程式想要表達 object 時使用，str 只會在 print 的時候用

``` python
>>> class A:
...     pass
>>> a = A()
>>> a
<A object at 0x037D6CB0>
>>> print(a)
>>> class B:
...   def __str__(self):
...       return 'this is B class overload __str__'   
>>> b = B()
>>> b
<B object at 0x037769D0>
>>> print(b)
this is B class overload __str__
>>> class C:
...   def __repr__(self):
...       return 'this is C class overload __repr__'   
>>> c = C()
>>> c
this is C class overload __repr__
>>> print(c)
this is C class overload __repr__
```

* `object.__lt__(self, other)`: x\<y 
* `object.__le__(self, other)`: x\<=y
* `object.__eq__(self, other)`: x==y
* `object.__ne__(self, other)`: x!=y
* `object.__gt__(self, other)`: x>y
* `object.__ge__(self, other)`: x>=y

## Customizing attribute access
* `object.__getattr__(self, name)`: 找不到 attribute 會被呼叫
* `object.__getattribute__(self, name)`: 只要存取 attribute 會被呼叫，不論存在與否
* 同時存在 getattr getattribute 會只呼叫 getattribute
* `object.__setattr__(self, name, value)`: 當想要 assign attribute 時會被呼叫
* `object.__delattr__(self, name)`: called by del object.name
* `object.__dir__(self)`: called by dir()

## \_\_slots\_\_
* class 預設用 dict 來保存 instance 的 property，允許在 runtime 設置新 property，但缺點是會用太多記憶體
* `__slots__`: explicit 定義 property，不允許其他新的 property 以減少記憶體使用量
 

## Emulating container types
* `object.__len__(self)`: Called to implement built-in `len()`
* `object.__getitem__(self, key)`: Called to implement evaluation of `self[key]`
* `object.__missing__(self, key)`: Called by `dict.__getitem__()` if key is not in dict
* `object.__setitem__(_self_, _key_, _value_)`: Called to implement assignment to `self[key]`
* `object.__delitem__(self, key)`: Called to implement deletion of `self[key]`
*  `object.__iter__(self)`: return 可以走過 container 中所有 object 的 iterator
*  `object.__reversed__(self)`: Called by `reversed()` built-in to implement reverse iteration
*  `object.__contains__(self, item)`: Called to implement membership test operators `in` or `not in`. 假設沒有定義 `__contains__` 會使用 `__iter__` 把所有 object iterate 出來看看已沒有一樣，假設連 `__iter__` 都沒有定義就會使用 `__getitem__` 把所有正數丟 index 後如果沒有 IndexError 就回傳 True

## Reference
[data model](https://docs.python.org/3.7/reference/datamodel.html#data-model)

###### tags:`python`
