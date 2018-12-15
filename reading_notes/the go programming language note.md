# # The Go Programming Language by Alan A. A. Donovan, Brian W. Kernighan  
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

## Chapter 4 Composite Types
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
    

### Reference
https://golang.org/
https://gobyexample.com/

https://www.amazon.com/Programming-Language-Addison-Wesley-Professional-Computing/dp/0134190440

###### tags: `go`
