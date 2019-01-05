# The Go Programming Language by Alan A. A. Donovan, Brian W. Kernighan  
![image alt](https://d1q6f0aelx0por.cloudfront.net/product-logos/81630ec2-d253-4eb2-b36c-eb54072cb8d6-golang.png)

## Preface
* Go 的表面與 C 很相似
* Go 拿掉那些在 C 會讓 code 變得複雜而且不可靠的 feature
* Go 追求 simplicity
* Go 容易撰寫 concurrency code
* Go 有現代語言擁有的 feature
    * garbage collection
    * package system
    * first-class function
    * lexical scope
    * system call interface
    * immutable string which encoded in UTF-8
* Go 沒有下面這些 feature
    * no implicit numeric conversion
    * no constructor or destructor
    * no operator overloading
    * no default parameter values
    * no inheritance
    * no generic ???
    * no exceptions
    * no macros
    * no function annotations
    * no thread-local storage
* Go 的 type system 提供足以檢查 type 但又沒有強到像其他 typed language

## Chapter 1: Tutorial
### 1.1. Hello, World
* Go is compiled language
    * Run: `go run helloworld.go`
    * Compile: `go build helloworld.go`
    * Execute: `./helloworld`
* Go code is organized into packages
    * Package `main` defined standalone executable program 
    * 程式不會 compile 如果少 import 或者 import 不需要的 package
    * `import` 要寫在 `package` 後面
* Go 對於 format 有嚴格限制，可以用 gofmt tool 自動 format
### 1.2 Command-Line Arguments
``` go
// Echo1 prints its command-line arguments.
package main
import (
    "fmt"
    "os"
)
func main() {
    var s, sep string
    for i := 1; i < len(os.Args); i++ {
        s += sep + os.Args[i]
        sep = " "
    }
    fmt.Println(s)
}
```
* `os.Args` 讀取 command-line arguments
* comment start with `//`
* `var s, sep string`
    * `var` 宣告變數 
    * `string` 定義型態
    * 沒有給值就用預設
* `s += sep + os.Args[i]`
    * `+=` assignment operator
    * 等於 `s = s + sep + os.Args[i]`
* `for i := 1; i < len(os.Args); i++`
    * `:=` short variable declaration
    * `i++` 等於 `i=i+1`
    * `i++` 是 statement 不是 expression
    * `++i` is not legal
* Go 只有 for loop
    * traditional loop: `for initialization; condition; post {}`
    * while loop: `for condition {}`
    * Infinite loop: `for {}`

### 1.3 Finding Duplicate Lines
### 1.4 Animated GIFs
### 1.5 Fetching a URL
### 1.6 Fetching URLs Concurrently
### 1.7 A Web Server
### 1.8 Loose Ends

## Chapter 2: Program Structure
### 2.1 Names
* a name begins with a letter or an underscore
* may have any number of a additional letter,  digits and underscores
* Case matters
* 25 keywords: `break`, `case`, `chan`, `const`, `continue`, `default`, `defer`, `else`, `fallthrough`, `for`, `func`, `go`, `goto`, `if`, `import`, `interface`, `map`, `package`, `range`, `return`, `select`, `struct`, `switch`, `type`, `var`
* Pre-declared name: Package builtin
    * Constants: `true`, `false`, `iota`, `nil`
    * Types: `int`, `int8`, `int16`, `int32`, `int64`, `uint`, `uint8`, `uint16`, `uint32`, `uint64`, `uintptr`, `float32`, `float64`, `complex128`, `complex64`, `bool`, `byte`, `rune`, `string`, `error`
    * Functions: `make`, `len`, `cap`, `new`, `append`, `copy`, `close`, `delete`, `complex`, `real`, `imag`, `panic`, `recover`
* scope
    * 宣告在 function 的變數就只在 function 看的到
    * 宣告在 function 外面的變數在同屬於 package 的檔案都看得到
    * 變數開頭是大寫則所有 package 都看得到
* 變數長度
    * 沒有限制
    * Go 建議越短的名字 scope 越小，scope 越大變數名稱越長且越有意義
* Style: Camel case

### 2.2 Declarations
* Four major declarations: `var`, `const`, `type`, `func`
* `package`: file 開頭都有 `package` 宣告自己所屬的 `package`

### 2.3 variables
* `var name type = expression`
    * `var` 宣告變數 
    * `type` 表示型態
    * `exporession`: 可以省略，如果省略就使用該型態的預設值
    * `type` 跟 `expression` 其中一個可以省略但不能都省略
* 每種型態都有預設值確保不會有未初始化的變數出現

#### 2.3.1 Short Variable Declarations
* `name := expression`
* 變數型態由 expression 來決定
* 通常用來宣告 local variables
* `var` 通常搭配型態宣告後之後才給值得情況
* 只有在增加可讀性的時候才使用 `i, j := 0,1` 像是 `for` loop 裡面
* 接收多個回傳值得時候 `f, err := os.Open(name)`
* 假設 variable 已經被宣告過則 short variable declaration 其實就是 assignment
* short variable declaration 一定要有新變數,不然會 compile error

#### 2.3.2 Pointers
* variable: 儲存 value 的一塊空間，使用 name(`x`) 或者 expression(`x[i]`) 得到 value
* pointer: variable 的 address
    * value 不一定有 address 但所有 variable 都有 address
    * 使用 pointer 可以在不需要知道變數名稱的狀況下間接的讀取或更新 value
* expression `&x`(address of x) yield a pointer to int
* zero value of pointer is `nil`
* compiler 自動判斷 local variable 要在 stack or heap，如果 function 回傳 address 就會長在 heap
    ``` go
    var p = f()
    func f() *int {
         v := 1
        return &v 
    }
    ```

#### 2.3.4 The new Function
* `new(T)`: create an unnamed variable of type T, init to the zero value of T, and returns its address
* syntactic convenience
    ``` go
    func newInt() *int {
        var dummy int
        return &dummy
    }
    ```
* rarely used: 通常需要 unnamed variable 都是 struct types 而這些有其他的 syntax
* pre-declared function: 可以 redefine

#### 2.3.4 Lifetime of Variables
* 變數在程式存活的時間
* package-level variable 的 lifetime 是整個程式執行時間
* local variable lifetime: 從宣告開始後到被回收的時候
* function parameter 也是 local variable

### 2.4 Assignments
* assignment 更新 variable 的 value

#### 2.4.1 Tuple Assignment
* allow several variables to be assigned at once
* `x, y = y, x`: 右邊的 expression 會先都 evaluate 後才更新左邊變數的值
* trivial assignment 使用 tuple assignment 讓程式更簡潔 `i, j, k = 2, 3, 5`

#### 2.4.2 Assignability
* Implicit assignment
    * argument value implicitly assign corresponding parameter value
    * `return` 自動 assign 給相關的 variable
* 左右兩邊的 type 一樣就可以 assign
* Constants 有比較寬鬆的限制避免太多 explicit conversion
* `==` or `!=` 左右邊的值要能互相 assign 才能比較

### 2.5 Type Declarations
* type 表示 value 的特徵
* 程式中不同概念會使用相同 type 像是 `int` 可以表示 timestamp, month
* `type name underlying-type`: 宣告表示新的型態，但背後還是都是用現存的 type
* 通常出現在 package level，讓同屬於這個 package 都看得到
* 使用新的 type 不同概念但使用相同的 type 屬於不同的 type 以減少錯誤
* 每個 type 都有 conversion operation `T(x)`
* named type 是一種為了方便對基本 type 下註解的特色，如果太複雜的時候應該要使用 `struct` 

### 2.6 Packages and Files
* support modularity, encapsulation, separate compilation, reuse
* 每個 package 有各自的 namespace
* 只有開頭大寫的 identifier 全部的人都看得到

### 2.7 scope
* compile-time property
* `block`: 在 function or loop {} 裡面的 statements
* `lexical block`: 沒有特別被 {} 包起來的 statements
* `universe block`: 整個 package 都看的見

## Chapter 3: Basic Data Types
* Go's types
    * basic types: numbers, strings and booleans
    * aggregate types: arrays, structs
    * reference types: pointers, slices, maps, functions, channels
    * interface types

### 3.1 Integer
* sizes: 8, 16, 32, 64 bits
* signed and unsigned
* `rune`: `int32` 的同義詞，通常代表 Unicode code point
* `bytes`: `uint32` 的同義詞，但強調是 raw data
* `uintptr`: 不限長度的 unsigned integer，通常用來處理低階程式
* `int` 與 `int32` 是不同 type 雖然兩個都是代表 32 bit integer
* overflows: 當數字超過自己的 bits 所能代表的範圍後會自動捨棄最前面的
* integer, floating-point number and strings are ordered by comparison operators. What is ordered? (53 page)
* 雖然 Go 提供 unsigned numbers，但是 built-in function 像是 `len` 會回傳 signed integer 是為了使用 for loop 不會陷入無窮迴圈

### 3.2 Floating-Point Numbers
* 遵守 IEEE 754: 二進位浮點數算數標準
* `float32` and `float64`
* Max value: `math.MaxFloat32` and `math.MaxFloat64`
* `float32`: 6 decimal precision
* `float64`: 15 decimal precision
* `fmt.Printf` use `%g` to print float
* `math` package 可以 create 或檢查 IEEE 754 的特殊數值
    ``` go
    var z float64
    fmt.Println(z, z, 1/z, 1/z, z/z) // "0 0 +Inf Inf NaN"
    ```

### 3.3 Complex Numbers
``` go
var x complex128 = complex(1, 2) // 1+2i  
var y complex128 = complex(3, 4) // 3+4i  
fmt.Println(x*y) // "(-5+10i)"  
fmt.Println(real(x*y)) // "-5"  
fmt.Println(imag(x*y)) // "10"
```

### 3.4 Booleans
* Boolean value combined with && (AND) and || (OR) have short-circuit behavior
* if statment 需要是個 boolean 不能是 int 需要做成 boolean 才能使用

### 3.5 Strings
* Immutable sequence of bytes
* UTF-8-encoded
* `len(s)` return string length
* `s[i]`: return `i`-th byte of string `s`
* `s[i:j]`: `i` 不寫代表從頭，`j` 不寫代表到尾
* `+` operator 可以 concate two strings
* `==` or `<`: 比較兩個 string byte by byte
* string 內容不能改但是可以 concate
    * 確保 copy 的 string 不會被改動

#### 3.5.1 String Literals
* string value 是 squence of bytes enclosed in double quotes
* escape sequences: begin with backslash `\`
* hexadecimal escape: `\xhh`
* octal escape: `\ooo`
* raw string literal: \`...\` backquotes instead of double quotes
    * regular expression
    * HTML template
    * JSON literals
    * command usage messages
    * multiple lines messages

#### 3.5.2 Unicode
* American Standard Code for Information Interchange(ASCII)
    * use 7 bits to represent 128 characters
    * upper- and lower-case letters of English
    * digits
    * punctuation
    * decive-control characters
* Unicode: all of the characters in all of the world's writing system
* Unicode code point: standard number for each unicode
* rune: Go terminology of unicode code point
* single rune is `int32`
* 都使用 32 bit 可能會浪費空間，因為通常還是使用 65536 個字只要 16 bit 就足夠了

#### 3.5.3 UTF-8
* 不定長度 unicode code points encoding
* use 1~4 bytes 表示每個 `rune`
    * ASCII characters 用 1 bytes
    * 其他用 2-4 bytes
* high-order bit 表示會用到幾個 bytes
* Go source files are always encoded in UTF-8
* `\uhhhh`: 16 bit value code point
* `\uhhhhhhhh`: 32 bit value code point
* `utf8.RuneCountInString(s)`: 回傳使用幾個 code point 也就是有幾個 unicode character
    * `len(s)`: 回傳用了多少 bytes
* `[]rune`: return sequence of Unicode code point

#### 3.5.4 Strings and Byte Slices
* `strings`: searching, replacing, comparing, trimming, splitting and joining strings
* `bytes`
    * `Buffer` type: efficient manipulate byte slice
* `strconv`
    * convert boolean, integer, and floating-point value to and from string representations
    * quote and unquote strings
* `unicode`
    * `IsDigit`, `IsLetter`, `IsUpper`, `IsLower` for clssifying runes
    * `ToUpper` and `To Lower` convert rune into given case if it is letter
* A string contains an array of bytes is immutable
* The elements of a byte slice can be modified

#### 3.5.5 Conversions between Strings and Numbers
* Convert an integer to a string
    * `fmt.Sprintf`: `y := fmt.Sprintf("%d", x)`
    * `strconv.Itoa(123)`:
    * `strconv.FormatInt(int64(x), 2)`: convert to different base
    * `fmt.Printf`: `%b`, `%d`, `%u`, `%x` 可以更方便轉成 string
* Parse a string representing an integer
    * `x, err := strconv.Atoi("123")`
    * `y, err := strconv.ParseInt("123", 10, 64)`: third arument give the size of integer type

### 3.6 Constants
* compile time 就 evaluate 完成
* 避免在 runtime 的時候被不小心修改
* 讓原本在 runtime 的 error 變成 compile error 
* boolean, string, or number 可以使用 const
* 如果宣告 const 沒有指定 type 會自動從 RHS 推論 type
* constants declared in group: 如果沒有給值會從上一個 declared 取值跟 type
    ``` go
    const (
		a = 1
		b
		c = 2
		d
	)
    fmt.Println(a, b, c, d) // "1 1 2 2"
    ```

#### 3.6.1 The Constant Generator iota
* `iota`: 自動產生一連串相關的值而不需要每一個都實際寫出來
* `const` 使用 iota 會從 0 開始每次增加一

#### 3.6.2 Untyped Constants
* compiler 讓 untyped constant 擁有更多的精準度，可以假設有 256 bit 的精準度
    * untyped boolean: `true` or `flase`
    * untyped integer: 0
    * untyped rune: '\u0000'
    * untyped floating-point: 0.0
    * untyped conplex: 0i
    * untyped string: string literals
* untyped constant 可以不需要 convertion
    * untyped constant assign 給 var 時候會自動做型態轉換

## Chapter 4: Composite Types
* Arrays and structs
    * Aggregate type: value 記憶體都是相連的
    * Fixed size
* Slices and maps: element 可以放不同的 type，大小隨著 element 增減而變化

### 4.1 Arrays
* 包含相同 data type 的固定長度陣列
* 通常不會用但為了理解 slice 需要先了解 array
* `var my_array [3]int`: my_array is an array of 3 integers
* `var q [3]int = [3]int{1, 2, 3}`: init array q with 1, 2, 3
* `q := [...]int{1, 2, 3}`: ... 會自動從 {} 裡面的數量知道 array 長度
* array 的 size 也是 type 的一部份，所以 `[3]int` 跟 `[4]int` 是不同的型態
* 初始化 array 可以指定 index 的 value
    ``` go 
    type Currency int
    const (
         USD Currency = iota
         EUR
         GBP
         RMB
    )
    symbol := [...]string{USD: "$", EUR: "9", GBP: "!", RMB: """}           
    fmt.Println(RMB, symbol[RMB]) // "3 ""
    ```
* 如果 array 的 element 可以比較則 array 就可以比較
* 傳 array 給 function 會複製一份給 argument，所以沒有效率
* 使用 pointer 傳 array 比較有效率而且 function 就可以改動原本的 array
    ``` go
    func zero(ptr *[32]byte) {
         *ptr = [32]byte{}
    }
    ```

### 4.2 Slices
* 動態增減 size 的 array
* 讀取 array subsequence 的 lighweight data structure
* Slice 有三個部分
    * pointer: 指向 slice 第一個 element
    * length: slice element 的數量，用 `len` 得到數量
    * capacity: 從 slice 頭到 array 尾的數量，用 `cap` 得到數量
* array 可以被 slice 成不同大小的 array，彼此交錯也可以
    * slice 超過 cap 會 panic
    * slice 超過 len 會 extend slice
* slice 有 pointer 所以傳 slice 給 function 讓 function 可以改動 array 裡面的 element
* slice 不能直接用 `==` 比較，需要使用 `bytes.Equal`
* slice 只能跟 `nil` 比較
* test slice empty using `len(s) == 0` rather than `s == nil`
    ``` go
     var s []int    // len(s) == 0, s == nil
     s = nil        // len(s) == 0, s == nil
     s = []int(nil) // len(s) == 0, s == nil
     s = []int{}    // len(s) == 0, s != nil
    ```
* `make([]T, len, cap)`: create a slice with specified element type, length, and capacity

#### 4.2.1 The append Function
* append item to slice
* 因為不知道什麼時候 append slice 需要 reallocate 所以都寫成 `runes = append(runes, r)`
* append 可能會改動 `cap`
* slice 像是一個 struct
    ``` go
    type IntSlice struct {
        ptr *int
        len, cap int
    }
    ```
* add more than one elements
    * `x = append(x, 1,2,3)`
    * `x = append(x, x...)` // append the slice x

#### 4.2.2 In-Place Slice Techniques
``` go
// Nonempty is an example of an inplace slice algorithm.
package main
import "fmt"
// nonempty returns a slice holding only the nonempty strings.
// The underlying array is modified during the call.
func nonempty(strings []string) []string {
    i := 0
    for _, s := range strings {
        if s != "" {
            strings[i] = s
            i++
        }
    }
    return strings[:i]
}
```
* input strings and output strings shared the same array
* avoid allocate another array
* original array is partly overwritten
### 4.3 Maps
* unordered collection of key/value pairs
* `map[K]V`
    * K is type of key, V is type of value
    * All key type are the same type
    * All value type are the same type
    * type K must be comparable using `==` for key testing

* Create a map
    * `ages := make(map[string]int) // mapping from strins to ints`
    * map literal: `ages := map[string]int{"alice": 31, "charlie": 34,}`
* Access value: `ages["alice"]`
* Remove value: `delete(ages, "alice")`
* 如果沒有 key 就會 return value type 的 zero value
* Enumerate all the key/value pairs in the map
    ``` go 
    for name, age := range ages {
        fmt.Printf("%s\t%d\n", name, age)
    }
    ```
* Enumverate map in order
    ``` go
    import "sort"
    var names []string
    for name := range ages {
        names = append(names, name)
    }
    sort.Strings(names)
    for _, name := range names{
        fmt.Printf("%s\t%d\n", name, ages[name])
    }
    ```
* zero value of map is `nil`
* allocate the empty map before store a element
* 檢查是 key 存不存在: `if age, ok := ages[""bob]; !ok { /*..*/}`
* a set of strings: `map[string]bool`
* Build a map with slice as key
    * slice is not comparable
    ``` go
     var m = make(map[string]int)
     func k(list []string) string { return fmt.Sprintf("%q", list) }
     func Add(list []string)       { m[k(list)]++ }
     func Count(list []string) int { return m[k(list)] }
    ```
    * helper function k that k(x) == k(y) iff x and y are quivalent
    * 可以應用在任何無法比較的資料型態上面

### 4.4 Structs
* Aggregate data type: 把零或許多有特定名字的型態聚集在一起
* 每一個 value 稱為 field
* Example:
    ``` go
    type Employee struct {
         ID        int
         Name      string
         Address   string
         DoB       time.Time
         Position  string
         Salary    int
         ManagerID int
    }
    
    var dilbert Employee
    ```
    * fields: ID, name, address, date of birth, position, salary, manager id
    * 所有 field 聚集在一起成為 employee 就可以作為單位來複製一份，傳給 function，function 回傳值，當作 array 的 element 等等。
    * dilbert 是 Employee 的 instance
    ``` go 
    dilbert.Salary -= 5000 // demoted, for writing too few lines of code
    ```
    * dot notation 讀取 field
    ``` go 
    position := &dilbert.Position
    *position = "Senior " + *position // promoted, for outsourcing to Elbonia
    ```
    * 用 pointer 讀取 field
* struct 相同 field 但順序不同是不同的 type
* empty struct: `struct{}`

#### 4.4.1 Struct Literals
* specifies values for struct field
    ``` go
    type Point struct{ X, Y int}
    p := Point{1,2}
    ```
* 需要為每一個 field 都設好值
* 需要記得每一個 field 的意義
* struct field 變動需要重新改動
* 通常使用在小的 struct 上面具有顯而易見的 field 順序像是 `image.Point{x, y}`
* init some struct fields
    ``` go 
    anim := gif.GIF{LoopCount: nframes}
    ```
    * 沒有出現的 field 就用 zero value 初始化
    * 順序不重要
* field 沒有 export 就不能再別的 package 使用
* 

#### 4.4.2 Comparing Structs
* struct 所有 field 都可以 compare 則 struct 就可以 compare
* 因為 struct 可以比較所以可以拿來當 map 的 key 

#### 4.4.3 Struct Embedding and Anonymous Fields
* syntactic shortcut: `x.f` == `x.d.e.f`

### 4.5 JSON
* `encoding/json`, `encoding/xml`, `encoding/asn1`
``` go
type Movie struct {
    Title  string
    Year   int  `json:"released"`
    Color  bool `json:"color,omitempty"`
    Actors []string
}

var movies = []Movie{
	{Title: "Casablanca", Year: 1942, Color: false,
		Actors: []string{"Humphrey Bogart", "Ingrid Bergman"}},
	{Title: "Cool Hand Luke", Year: 1967, Color: true,
		Actors: []string{"Paul Newman"}},
	{Title: "Bullitt", Year: 1968, Color: true,
		Actors: []string{"Steve McQueen", "Jacqueline Bisset"}},
}

data, err := json.Marshal(movies)
if err != nil {
    log.Fatalf("JSON marshaling failed: %s", err)
}
fmt.Printf("%s\n", data)
//[{"Title":"Casablanca","released":1942,"Actors":["Humphrey Bogart","Ingrid Bergman"]},{"Title":"Cool Hand Luke","released":1967,"color":true,"Actors":["Paul Newman"]},{"Title":"Bullitt","released":1968,"color":true,"Actors":["Steve McQueen","Jacqueline Bisset"]}]
data, err := json.MarshalIndent(movies, "", "    ")
if err != nil {
    log.Fatalf("JSON marshaling failed: %s", err)
}
fmt.Printf("%s\n", data)
/*
[
    {
        "Title": "Casablanca",
        "released": 1942,
        "Actors": [
            "Humphrey Bogart",
            "Ingrid Bergman"
        ]
    },
    {
        "Title": "Cool Hand Luke",
        "released": 1967,
        "color": true,
        "Actors": [
            "Paul Newman"
        ]
    },
    {
        "Title": "Bullitt",
        "released": 1968,
        "color": true,
        "Actors": [
            "Steve McQueen",
            "Jacqueline Bisset"
        ]
    }
]
*/
```
* field tag
    ``` go
    Year  int  `json:"released"`
    Color bool `json:"color,omitempty"`
    ```
    * raw string literal
* Unmarshaling: decoding JSON
    ``` go
    var titles []struct{ Title string }
    if err := json.Unmarshal(data, &titles); err != nil {
        log.Fatalf("JSON unmarshaling failed: %s", err)
    }
    fmt.Println(titles) // "[{Casablanca} {Cool Hand Luke} {Bullitt}]"
    ```

### 4.6 Text and HTML Templates
* `text/template` or `html/template`
* template 是指 string 中含有一個或多個 {{...}} actions
    ``` go
    const templ = `{{.TotalCount}} issues:
    {{range .Items}}----------------------------------------
    Number: {{.Number}}
    User:   {{.User.Login}}
    Title:  {{.Title | printf "%.64s"}}
    Age:    {{.CreatedAt | daysAgo}} days
    {{end}}`
    ```
    * dot is notation of current value
    * `{{.TotalCount}}` expand value of the TotalCound field
    * `{{range .Items}}` and `{{end}} actions create a loop
    ``` go
    report, err := template.New("report").
        Funcs(template.FuncMap{"daysAgo": daysAgo}).
        Parse(templ)
    if err != nil {
         log.Fatal(err)
    }
    ```
* template fixed at compiler time
* `template.Must` 提供 error handling

## Chapter 5: Functions
* function 將一連串的 statements 包裝成一個單位
* function 允許讓複雜問題切割成小問題
* function 隱藏內部的細節

### 5.1 Function Declarations
* Declarations
    ```
    func name(parameterlist) (resultlist) {
        body
    }
    ```
    * a name
    * a list of parameters
    * an optional list of results
        * function return value type
        * 如果只有 return 一個變數或不 return 則 () 可以省略
        * result 也可以有名字，使用 type 的 zero value 初始化: `func sub(x, y int) (z int) { z = x - y; return }`
        * 一定要有 return 除非確定跑不到 function 底部
    * a body
* Type of a function: 有相同的 parameter list 跟 result list 則 function 為相同型態
* Parameters
    * local variable 
    * initial value given by caller arguments
    * Arguments passed by value: 複製一份給 function parameter，修改無法影響 caller 給的 argument
    * 若想要修改 caller 給的 argument 則需要傳 address 讓 function 可以用 indirect 的方式修改
* function 沒有 body 代表是用其他語言寫的

### 5.2 Recursion
* Go use variable-size stacks
* Do not worry stack overflow

### 5.3 Multiple Return Values
* go standard library 通常使用第一個回傳值當計算結果第二個當錯誤訊息
* 用 blank identifier 忽略錯誤: `links, _ = findLinks(url)`
* 通常不會直接使用多個回傳值當成 argument 傳給 function，只會用在 debug 而已: `log.Println(findLinks(url))`
* 使用多個回傳值的 function name 很重要，要很容易讓讓人看得出來這個 function 是回傳多個結果
* bare return: function 有 named result 可以直接寫 return，function 會自動補齊該 return 的東西
    * reduce code duplication
    * 很少會讓 code 更容易讀，所以需要謹慎的使用

### 5.4 Errors
* 通常在最後一個回傳值表達成功與否的狀態
    * ok: boolean 失敗原因只有一種
    * error: 多種不同原因造成的失敗
* built-in type error is interface type
    * nil 代表成功
    * non-nil 代表失敗
    * non-nil error 有 error message
* 通常只要失敗其他結果需要忽略
* go 不使用 exception 代表失敗
    * go 使用 exception 回報沒有預期到的錯誤
    * exception 需要 control flow 去處理 error 讓事情複雜化
    * exception 讓 end user 收到不想要的結果
    * excpetion 雖然有 full stack trace 但是難以找到哪邊出錯
* go 使用 `if` 跟 `return` 去處理錯誤，強迫寫程式的人面對 error handling 的邏輯

#### 5.4.1 Error-Handling Strategies
* Caller 需要負責處理 function 回傳的錯誤
* Propagate the error: 將 error 加上必要資訊後回傳
    * `fmt.Errorf`: format error message and return new error value
    * Prefix 描述問題的文字，讓人容易理解問題出在哪邊
    * error message 通常都是 chain 在一起所以不要換行跟大寫
* Retry: 針對短暫出現的 error 或者 retry 會成功的 error
* Stop program: 當 error 嚴重到無法繼續進行
    * 通常在 main package 使用
    * 暫停程式: `os.exit(0)` or `log.Fatalf('shut down...')`
* Log the error and continue

#### 5.4.2 End of File (EOF)
* io package 保證所有 EOF 都會回傳 `io.EOF`

### 5.5 Function Values
* first-class value
    * 像是其他 value 一樣
    * function value 有 type
    * 可以被 assign 到變數
    * 當參數傳給 function
    * 可以回傳 function
* zero value is nil
* function value 只能跟 nil 比較，function 彼此不能比較

### 5.6 Anonymous Functions
* 有名字的 function 只能宣告在 package level
* 使用 function literal 在任何 expression 製造出一個沒有名字的 function
`strings.Map(func(r rune) rune { return r + 1 }, "HAL9000"`
* anonymous function 可以讀取在 scope 裡面的變數
* closure: 使用 inner function 可以讓 function 有 state
* anonymous function 需要使用 recursion 的時候要先宣告在 assign

#### 5.6.1 Cavet: Capturing Iteration Variables
* loop variable 是 share address 所以之後 anonymous function 去找 loop variable 時候那個 address 已經換成別的 value 了

### 5.7 Variadic Function
* 可以給不同數量 arguments 的 function
``` go
func sum(vals ...int) int {
    total := 0
    for _, val := range vals {
        total += val
    }
    return total
}
```
* 在最後一個 parameter 的 type 前面加上 `...` 代表可以接受不同數量的 arguments
* vals type is []int slice
* caller 先 allocate array 後把 argument 搬進去，接著傳整個 slice 給 function
* 如果 argument 已經都在 slice 裡面就在名稱後面加上 `...`
    ``` go
    values := []int{1, 2, 3, 4}
    fmt.Println(sum(values...)) // "10"
    ```
* 雖然 variadic function 跟 slice 很像但是是不同 type
* 通常使用在 string formatting

### 5.8 Deffered Function Calls
* 處理遇到錯誤時候需要 clean up 的問題
* function 或 method 前面加上 `defer`
* 當執行到 statement 去 evaluated
* 實際執行是在有 defer 的 function 結束時候
* defer 通常使用在有互相對應的 operation 像是 open 跟 close
* 可以用來 debug function
    ``` go
    func bigSlowOperation() {
        defer trace("bigSlowOperation")() // don't forget the extra parentheses
        // ...lots of work...
        time.Sleep(10 * time.Second) // simulate slow operation by sleeping
    }
    func trace(msg string) func() {
        start := time.Now()
        log.Printf("enter %s", msg)
        return func() { log.Printf("exit %s (%s)", msg, time.Since(start)) }
    }
    ```
    * 紀錄 function 開始與結束時間

### 5.9 Panic
* 在 runtime 發現的錯誤使用 panic 來讓程式盡早失敗
* panic 發生後，執行中止，呼叫所有 deferred function，程式 crash 並寫下 log message
* panic 用來代表認為絕對不可能發生的情形
* 檢查 function 需要的 precondition 讓錯誤盡早發生
* 能預期的錯誤應該要用 error 來優雅的處理
* built-in function 前面是 Must 代表只要一有錯誤就噴 panic 出來
* panic 發生後 deferred function 反序執行

### 5.10 Recover
* 遇到 panic 就直接放棄是很合理的選擇但有時候還有機會 recover 
* 只能在 defer 裡面呼叫 recover()，其他地方呼叫只會回傳 nil
* recover 中止 panic 狀態並且回傳 panic value
* recover 後的 package variable 沒有很好的文件跟規範
* 不應該 recover 別的 package 的 panic
## Chapter 6: Methods
* Object-oriented program: 使用 method 來表達跟操作 data structure 讓使用者不用直接讀取內部的細節
    * encapsulation
    * composition
* object: 就是有 method 的 value 或 variable
* method: 特定型態的 function

### 6.1 Method Declarations
* 在 function name 前面加上 parameter 跟 parameter type
    ``` go
    package geometry
    
    import "math"
    
    type Point struct{ X, Y float64 }
    
    // traditional function
    func Distance(p, q Point) float64 {
        return math.Hypot(q.X-p.X, q.Y-p.Y)
    }
    
    // same thing, but as a method of the Point type
    func (p Point) Distance(q Point) float64 {
        return math.Hypot(q.X-p.X, q.Y-p.Y)
    }
    ```
    * 宣告兩個 `Distance` function 並不衝突，因為第一個是 package-level function 第二個是 `Point` 的 method
    * `p` 是 method receiver: 因為以前把呼叫 method 當成送訊息給 object
    * 不使用 `this` 或 `self` 代表 receiver 而是用 name
    * 因為 receiver name 會很常被用到，所以盡量選擇短的名字並且所有 methods 保持一致
    * 通常使用 type 的第一個字母，像是用 `p` 當成 `Point` 的 receiver name
* 呼叫 method 的時候 receiver argument 會在 method name 之前
    ``` go
    p := Point{1, 2}
    q := Point{4, 6}
    fmt.Println(Distance(p, q)) // "5", function call
    fmt.Println(p.Distance(q))  // "5", method call
    ```
    * `p.Distance` 稱為 selector，因為挑選適當的 `Distance` method 給 receiver p
    * `p.X` 也是用 selector 來找出 struct 的 field
    * struct field 不能跟 method name 同名，不然會有 compile error: `prog.go:14:6: type Point has both field and method named Y`
* 每種 type 都有自己的 name space 所以不同 type 可以用相同的 method
    ``` go
    // A Path is a journey connecting the points with straight lines.
    type Path []Point

    // Distance returns the distance traveled along the path.
    func (path Path) Distance() float64 {
	    sum := 0.0
	    for i := range path {
		    if i > 0 {
			    sum += path[i-1].Distance(path[i])
		    }
	    }
	    return sum
    }
    ```
    * `type Path` 表示一連串的線段
    * `Distance`: `Path` 的總長度
    * 因為 `Path` 的 type 與 `Point` 的 type 不一樣所以可以 define `Distance`
* 相同型態的 method 需要不同的 name，但是不同型態可以使用相同的 method name
* 好處是可以讓 method name 比較短也不用寫出 package name
    ``` go
    perim := geometry.Path{{1, 1}, {5, 1}, {5, 4}, {1, 1}}
	fmt.Println(geometry.PathDistance(perim)) // "12", standalone function
	fmt.Println(perim.Distance())             // "12", method of geometry.Path
    ```
    
### 6.2 Methods with a Pointer Receiver
* 使用 pointer 傳 variable 的 address 給 method 避免複製每一個 argument value 或者需要 update variable 的時候
    ``` go 
    func (p *Point) ScaleBy(factor float64) {
        p.X *= factor
        p.Y *= factor
    }
    ```
* 通常如果 method 有 pointer receiver 則該 type 的所有 method 都應該要有 pointer receiver
* receiver argument 與 receiver parameter 有相同型態
    ``` go 
    Point{1, 2}.Distance(q) //  Point
    pptr.ScaleBy(2)         // *Point
    ```
* receiver argument is variable of type `T`， receiver paramter has type `*T`
    ``` go
    p.ScaleBy(2) // implicit (&p)
    ```
* receiver argument has type `*T` and receiver parameter has type `T`
    ``` go
    pptr.Distance(q) // implicit (*pptr)
    ```
* 如果所有 method 都有 reciver type `T` 就可以放心複製 instance of that type

#### 6.2.1 Nil Is a Valid Receiver Value
* 有些 method 可以使用 `nil` 當 receiver, 特別是 `nil` 可以有意義的代表該型態的 zero value
``` go 
// An IntList is a linked list of integers.
// A nil *IntList represents the empty list.
type IntList struct {
    Value int
    Tail  *IntList
}

// Sum returns the sum of the list elements.
func (list *IntList) Sum() int {
    if list == nil {
        return 0
    }
    return list.Value + list.Tail.Sum()
}
```
* 使用 `nil` 當成 receiver value 是值得特別處理這個情形 
``` go
package url

// Values maps a string key to a list of values.
type Values map[string][]string

// Get returns the first value associated with the given key,
// or "" if there are none.
func (v Values) Get(key string) string {
	if vs := v[key]; len(vs) > 0 {
		return vs[0]
	}
	return ""
}

// Add adds the value to key.
// It appends to any existing values associated with key.
func (v Values) Add(key, value string) {
	v[key] = append(v[key], value)
}
```
* multimap: value 是 slice of string
* Get method 讀取 key 的第一個 value，如果沒有就回傳空字串
* Add method: 把 value append 到提供的 key
``` go
m := url.Values{"lang": {"en"}} // direct construction
m.Add("item", "1")
m.Add("item", "2")
fmt.Println(m.Get("lang")) // "en"
fmt.Println(m.Get("q")) // ""
fmt.Println(m.Get("item")) // "1" (first value)
fmt.Println(m["item"]) // "[1 2]" (direct map access)

m = nil
fmt.Println(m.Get("item")) // ""
m.Add("item", "3") // panic: assignment to entry in nil map
```
* `m = nil` 代表 m 是個 empty map
* `Value(nil).Get("item")` 會回傳 empty string
* `nil.Get("item")` 會 compile error: use of untyped nil，因為 nil 沒有型態不知道要用誰的 Get method

### 6.3. Composing Types by Struct Embedding
``` go
import "image/color"

type Point struct{ X, Y float64 }
type ColoredPoint struct {
	Point
	Color color.RGBA
}
```
* Embedded `Point` 讓 `ColoredPoint` 有 `X`, `Y` fields
    ``` go
    var cp ColoredPoint
	cp.X = 1
	fmt.Println(cp.Point.X) // "1"
	cp.Point.Y = 2
	fmt.Println(cp.Y) // "2"
    ```
* Embedded struct 的用法也可以套用在 method 上面
    ``` go
	red := color.RGBA{255, 0, 0, 255}
	blue := color.RGBA{0, 0, 255, 255}
	var p = ColoredPoint{Point{1, 1}, red}
	var q = ColoredPoint{Point{5, 4}, blue}
	fmt.Println(p.Distance(q.Point)) // "5"
	p.ScaleBy(2)
	q.ScaleBy(2)
	fmt.Println(p.Distance(q.Point)) // "10"
    ```
    * `Point` 的 method 都被 promoted 到 `ColoredPoint`
    * 讓複雜的型態可以分割成很多的簡單型態在組合起來，就是 OOP 中的 composition 概念
* `ColoredPoint` 與 `Point` 不是 class 繼承的關係 (is-a relationship)
    ``` go 
    p.Distance(q) // compile error: cannot use q (ColoredPoint) as Point
    ```
    * Distance 接受的參數型態是 `Point` 而不是 `ColoredPoint`
* `ColoredPoint` 與 `Point` 是 composition (has-a relationship)
* Implementation details
    ``` go
    func (p ColoredPoint) Distance(q Point) float64 {
	    return p.Point.Distance(q)
    }
    func (p *ColoredPoint) ScaleBy(factor float64) {
	    p.Point.ScaleBy(factor)
    }
    ```
    * compiler 產生 wrapper function 
* Anonymous filed 可以是 pointer 指向一個 named type
    ``` go
    type ColoredPoint struct {
    	*Point
	    Color color.RGBA
    }
    
    p := ColoredPoint{&Point{1, 1}, red}
	q := ColoredPoint{&Point{5, 4}, blue}
	fmt.Println(p.Distance(*q.Point)) // "5"
	q.Point = p.Point                 // p and q now share the same Point
	p.ScaleBy(2)
	fmt.Println(*p.Point, *q.Point) // "{2 2} {2 2}"
    ```
    * method 會被 promote 成 pointed-to object
    * 因為是 indirect 所以可以 share structure
* struct 可以有多個 anonymous field
    ``` go
    type ColoredPoint struct {
        Point
        color.RGBA
    }
    ```
    * 找 method 的順序
        1. 直接宣告的 method
        2. promote 一次的 method
        3. promote 兩次的 method
        4. 以此類推
    * https://goplay.space/#BNVg0jTui2r
* 宣告 method 一定要使用 named type 或者 point to named type 但是托 embedded 的福可以讓 unamed struct type 有 method
    ``` go
    var (
        mu sync.Mutex // guards mapping
        mapping = make(map[string]string)
    )

    func Lookup(key string) string {
        mu.Lock()
        v := mapping[key]
        mu.Unlock()
        return v
    }
    ```
    * mu 跟 mapping 是分開的兩個變數
    * mu 跟 mapping 都一定一起使用所以封裝再一起比較好
    ``` go
    var cache = struct {
	    sync.Mutex
	    mapping map[string]string
    }{
	    mapping: make(map[string]string),
    }

    func Lookup(key string) string {
	    cache.Lock()
	    v := cache.mapping[key]
	    cache.Unlock()
	    return v
    }
    ```
    * cache 把 sync.Mutex 跟 mapping 合再一起變得更有意義的名字
    * sync.Mutex field 是 embedded
    * cache 有 Lock 跟 Unlock 這兩個 method 從 sync.Mutex promote 來的
    * 不用特別宣告 cache 使用的 struct 就可以讓 cache 用 Lock 跟 Unlock method

### 6.4. Method Values and Expressions
* 通常都會在一個 expression 中同時 select 跟 call method
* select 跟 call 可以分成兩步驟
    ``` go
    p := Point{1, 2}
	q := Point{4, 6}
	distanceFromP := p.Distance        // method value
	fmt.Println(distanceFromP(q))      // "5"
	var origin Point                   // {0, 0}
	fmt.Println(distanceFromP(origin)) // "2.23606797749979", ;5

	scaleP := p.ScaleBy // method value
	scaleP(2)           // p becomes (2, 4)
	scaleP(3)           //      then (6, 12)
	scaleP(10)          //      then (60, 120)
    ```
    * selector `p.Distance` 產生 method value，是一個 function 綁定 method (Point.Distance) 跟 receiver value \(p\)
    * Function 只需要 non-receiver arguments
* method value 在 package API 需要傳入 function value 時候很好用
    * Without method value
    ``` go
    type Rocket struct { /* ... */ }
    func (r *Rocket) Launch() { /* ... */ }
    r := new(Rocket)
    time.AfterFunc(10 * time.Second, func() { r.Launch() })
    ```
    * Using method value
    ``` go
    time.AfterFunc(10 * time.Second, r.Launch)
    ```
* Method expression
    ``` go
    p := Point{1, 2}
	q := Point{4, 6}
	distance := Point.Distance   // method expression
	fmt.Println(distance(p, q))  // "5"
	fmt.Printf("%T\n", distance) // "func(Point, Point) float64"
	scale := (*Point).ScaleBy
	scale(&p, 2)
	fmt.Println(p)            // "{2 4}"
	fmt.Printf("%T\n", scale) // "func(*Point, float64)"
    ```
    * `T.f` or `(*T).f` yield  a function value
    * 第一個 parameter 會被當成 receiver
* Method expression 可以用在當需要從多個 method 中挑選特定 method 並且使用不同 receiver 去呼叫
    ``` go
    type Point struct{ X, Y float64 }

    func (p Point) Add(q Point) Point { return Point{p.X + q.X, p.Y + q.Y} }
    func (p Point) Sub(q Point) Point { return Point{p.X - q.X, p.Y - q.Y} }

    type Path []Point

    func (path Path) TranslateBy(offset Point, add bool) {
        var op func(p, q Point) Point
        if add {
            op = Point.Add
        } else {
            op = Point.Sub
        }
        for i := range path {
            // Call either path[i].Add(offset) or path[i].Sub(offset).
            path[i] = op(path[i], offset)
        }
    }
    ```
    * `op` 代表 `Point` addition 或者 subtraction
    * `Path.TranslateBy` 對於每個 point 做 operation

### 6.5 Example: Bit Vector Type
* Sets in GO: `map[T]bool`
* 雖然用 map 很彈性但在特定問題使用特製的 representation 會更好
    * Set elements 都是小的非負整數，常常會用到 union 跟 intersection
    * 用 bit vector 比較理想
* Bit vector
    * slice of unsigned integer
    * 每一個 bit 代表可能的 element
    * 如果 i-th bit is set 則 set 包含 i
    ``` go
    // An IntSet is a set of small non-negative integers.
    // Its zero value represents the empty set.
    type IntSet struct {
        words []uint64
    }

    // Has reports whether the set contains the non-negative value x.
    func (s *IntSet) Has(x int) bool {
        word, bit := x/64, uint(x%64)
        return word < len(s.words) && s.words[word]&(1<<bit) != 0
    }

    // Add adds the non-negative value x to the set.
    func (s *IntSet) Add(x int) {
        word, bit := x/64, uint(x%64)
        for word >= len(s.words) {
            s.words = append(s.words, 0)
        }
        s.words[word] |= 1 << bit
    }

    // UnionWith sets s to the union of s and t.
    func (s *IntSet) UnionWith(t *IntSet) {
        for i, tword := range t.words {
            if i < len(s.words) {
                s.words[i] |= tword
            } else {
                s.words = append(s.words, tword)
            }
        }
    }
    ```
    * 每一個 word 有 64 bits
    * `x/64` 找出在第幾個 word
    * `x%64` 找出在 word 的 bit index
* Print an IntSet as a string
    ``` go
    // String returns the set as a string of the form "{1 2 3}".
    func (s *IntSet) String() string {
        var buf bytes.Buffer
        buf.WriteByte('{')
        for i, word := range s.words {
            if word == 0 {
                continue
            }
            for j := 0; j < 64; j++ {
                if word&(1<<uint(j)) != 0 {
                    if buf.Len() > len("{") {
                        buf.WriteByte(' ')
                    }
                    fmt.Fprintf(&buf, "%d", 64*i+j)
                }
            }
        }
        buf.WriteByte('}')
        return buf.String()
    }
    ```
* Demonstrate IntSet in action
    ``` go
    var x, y IntSet
    x.Add(1)
    x.Add(144)
    x.Add(9)
    fmt.Println(x.String()) // "{1 9 144}"
    y.Add(9)
    y.Add(42)
    fmt.Println(y.String()) // "{9 42}"
    x.UnionWith(&y)
    fmt.Println(x.String()) // "{1 9 42 144}"
    fmt.Println(x.Has(9), x.Has(123)) // "true false"
    ```
    * 把 `String` 跟 `Has` 不需要使用 pointer type 宣告，只是為了一致性
    ``` go
    fmt.Println(&x) // "{1 9 42 144}"
    fmt.Println(x.String()) // "{1 9 42 144}"
    fmt.Println(x) // "{[4398046511618 0 65536]}"
    ```
    * 第一個印出 `*IntSet` Pointer，有 `String` method
    * 第二個用 `String()` 所以 compiler 會隱性加上 & operation，所以也有 `String` method
    * 第三個 IntSet value 沒有 `String` method 所以 `fmt.Println(x)` 會直接印出 struct 的 representation
    

### 6.6 Encapsulation
* 如果 object 的 variable 或者 method 無法被外部直接存取就稱為 encapsulated
* 有時候也稱為 Information hiding
* Go 只有一種方法可以控制 name 的 scope 就是大小寫
    * 大寫開頭可以被其他 package 看到
    * 小寫開頭只有該 package 看的到
    * 相同原則也適用 struct 的 field 跟型態的 method
    * 所以要 encapsulate object 就要用 struct
    * IntSet 之所以要宣告成 struct 也是這個原因
    ``` go
    type IntSet struct {
        words []uint64
    }
    ```
    ``` go
    type IntSet []uint64
    ```
    * package level 的 encapsulation
* Encapsulation benefit
    * client 不能直接修改 object 的 variable 所以可以 client 看比較少的 code 就可以理解 variable 可能的值
    * 隱藏實作細節避免 client 依賴可能隨時變動的東西，讓設計者可以在原有設計界面上自由改變實作細節
        * bytes.Buffer: 用來 accumulate short string
        ``` go
        type Buffer struct {
            buf     []byte
            initial [64]byte
            /* ... */
        }

        // Grow expands the buffer's capacity, if necessary,
        // to guarantee space for another n bytes. [...]
        func (b *Buffer) Grow(n int) {
            if b.buf == nil {
                b.buf = b.initial[:0] // use preallocated space initially
            }
            if len(b.buf)+n > cap(b.buf) {
                buf := make([]byte, b.Len(), 2*cap(b.buf)+n)
                copy(buf, b.buf)
                b.buf = buf
            }
        }
        ```
        * 增加小寫開頭的 initial 讓 client 無法察覺
        * client 感覺到的只有效能提升
    * 避免 client 直接干涉 object variable，只能透過 function 改變 object variable 讓設計者可以確保 object 內部的一致性
* 單純讀取或修改型態的 internal value 稱為 getter 跟 setter
    * 命名原則: 忽略 Get, Find, Lookup, Fetch 等等 prefix
    ``` go
    package log
    type Logger struct {
        flags int
        prefix string
        // ...
    }
    func (l *Logger) Flags() int
    func (l *Logger) SetFlags(flag int)
    func (l *Logger) Prefix() string
    func (l *Logger) SetPrefix(prefix string)
    ```
* Go 沒有禁止 struct 的 field export
    * export field 後如果後悔會造成 API 不相容
    * export field 前要先考慮 code 複雜度是否容易維護
* 把所有東西通通 encapsulate 起來不一定是好的
    * `time.Duration` 只用 int64 宣告讓我們可以使用各種運算
    ``` go
    const day = 24 * time.Hour
    fmt.Println(day.Seconds()) // "86400" 
    ```
    * `geometry.Path` 也沒有 encapsulate 讓 client 可以使用 slice literal syntax 來 iterate loop
* 為什麼 `IntSet` 要 encapsulate 而 `geometry.Path` 不用
    * `geometry.Path` 本質上就是一連串的 Point，未來應該不會需要增加新的 field
    * `IntSet` 目前是用 `[]uint64` 作為內部表示但是也可以用 `[]uint` 來表示或者更多不同的東西來表示，而且可能增加新 feature 後需要新增 field 來紀錄 element 數量，所以 `IntSet` 需要 encapsulate
* 本章節學習了如何使用 named type 的 method 但還需要 interface 搭配才能實現 object-oriented programming 的全貌

### Reference
Golang website: https://golang.org/
Go example: https://gobyexample.com/
The Go Play Space: https://goplay.space/
Book: https://www.amazon.com/Programming-Language-Addison-Wesley-Professional-Computing/dp/0134190440
Book code example: https://github.com/adonovan/gopl.io/

###### tags: `go`
