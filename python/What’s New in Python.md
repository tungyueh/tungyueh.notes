# What’s New in Python
## What’s New In Python 3.7
### dataclasses
* 可以想成具有 default 的 namedtuple
* 自動產生特殊 function 可以節省撰寫這些 function 的時間
* decorate class 會自動產生 \_\_init\_\_, \_\_repr\_\_, \_\_eq\_\_ 特殊 function 加入到目前的 class
* Example
``` python
@dataclass
class InventoryItem:
    '''Class for keeping track of an item in inventory.'''
    name: str
    unit_price: float
    quantity_on_hand: int = 0

    def total_cost(self) -> float:
        return self.unit_price * self.quantity_on_hand
```
* 會自動產生 \_\_init\_\_
``` python
def __init__(self, name: str, unit_price: float, quantity_on_hand: int = 0) -> None:
    self.name = name
    self.unit_price = unit_price
    self.quantity_on_hand = quantity_on_hand

```

* ref: https://docs.python.org/release/3.7.0/library/dataclasses.html#dataclasses.dataclass

## What’s New In Python 3.6
### Formatted string literals
* python 支援 %-formatting 跟 str.format 的方式，但實際上使用還是沒有這麼方便
* f-string 提出可以用 f prefix 來表示要 format string
* string 裡面用 {} 表示要將這個 format
* 要 format 的變數可以用 !r, !s, !a 或者 : format specifier 來做的格式
* Exmaple
``` python
>>> name = "Fred"
>>> f"He said his name is {name!r}."
"He said his name is 'Fred'."
>>> today = datetime(year=2017, month=1, day=27)
>>> f"{today:%B %d, %Y}"  # using date format specifier
'January 27, 2017'
```

* ref: https://docs.python.org/release/3.7.0/reference/lexical_analysis.html#formatted-string-literals

## What’s New In Python 3.5
### Coroutines with async and await syntax
* TBD: https://www.python.org/dev/peps/pep-0492/

## What’s New In Python 3.4
### enum
* 讓用特定數字或字串代表某些意義的情況，可以換用 enum 讓可讀性提高
``` python
from enum import Enum
class Color(Enum):
  RED = 1
  GREEN = 2
  BLUE = 3
```

* Color.RED type 是 ```<enum 'Color'>```
* Color.RED 不等於 1，若想要讓 Color.RED 可以當成 1 使用 class 需要繼承 IntEnum
* ref: https://docs.python.org/release/3.7.0/library/enum.html#module-enum
### pathlib
* 相較於單純的 string，pathlib 針對不同系統提供各種易懂的 function
* ref: https://docs.python.org/release/3.7.0/library/pathlib.html#module-pathlib
### tracemalloc
* debug tool for memory usage
* ref: https://docs.python.org/release/3.7.0/library/tracemalloc.html#module-tracemalloc

## What’s New In Python 3.3
## Reworking the OS and IO exception hierarchy
* OSError, IOError, EnvironmentError, WindowsError, mmap.error, socket.error or select.error 都變成 OSError
* 舊有的 code 遇到 IOError 需要判斷 errno 才能知道是哪種 exception
``` python
from errno import ENOENT, EACCES, EPERM

try:
    with open("document.txt") as f:
        content = f.read()
except IOError as err:
    if err.errno == ENOENT:
        print("document.txt file is missing")
    elif err.errno in (EACCES, EPERM):
        print("You are not allowed to read document.txt")
    else:
        raise
```

* New exception: BlockingIOError, ChildProcessError, ConnectionError, FileExistsError, FileNotFoundError, InterruptedError, IsADirectoryError, NotADirectoryError, PermissionError, ProcessLookupError, TimeoutError
* 使用新 exception 可以讓 code 可讀性變高，行數減少
``` python
try:
    with open("document.txt") as f:
        content = f.read()
except FileNotFoundError:
    print("document.txt file is missing")
except PermissionError:
    print("You are not allowed to read document.txt")
```

### Syntax for Delegating to a Subgenerator
* ```yield from```: 可以直接 yield from 後面的 generator 的值
* 通常使用在需要使用 send() throw() 的 generator，讓 code 可以重複利用
``` python
def accumulate():
    tally = 0
    while 1:
        next = yield
        if next is None:
            return tally
        tally += next

def gather_tallies(tallies):
    while 1:
        tally = yield from accumulate()
        tallies.append(tally)

tallies = []
acc = gather_tallies(tallies)
next(acc)  # Ensure the accumulator is ready to accept values
for i in range(4):
    acc.send(i)

acc.send(None)  # Finish the first tally
for i in range(5):
    acc.send(i)

acc.send(None)  # Finish the second tally
tallies # [6, 10]
```

* accumulate 會將 yield 的值加總然後遇到 None 回傳
* gather_tallies 將結果存在 list 上面
* acc.send(i) 將 i 的值 assign 給 tally
* 第一次 acc.send() 0~3 把 0~3 加總後存到 tallies 所以是 6
* 使用 ```yield from``` 可以讓 accumulate 獨立成一個 function 增加可讀性跟重複使用

### Suppressing exception context
* raise XXX from None 減少重複的 traceback

## What’s New In Python 3.0
### Function argument and return value annotations
* 對於參數或回傳值給予型態建議
``` python
def foo(a: 'x', b: 5 + 6, c: list) -> max(2, 9):
  pass
```
* object 型態記錄在 \_\_annotations\_\_
``` python
>>> print(foo.__annotations__)
{'a': 'x', 'b': 11, 'c': <class 'list'>, 'return': 9}
```

### Keyword-only arguments
* 在 \*arg 後面的 keyword argument 一定要給 key
``` python
>>> def compare(a, b, *, key=None):
...   pass
>>> compare(1,2,3)
TypeError: compare() takes 2 positional arguments but 3 were given
```

### nonlocal
* 直接存取 outer scope 的變數
### Extended Iterable Unpacking
* ```(a, *rest, b) = range(5)```

## Reference
https://docs.python.org/release/3.7.0/whatsnew/index.html
###### tags: `python`
