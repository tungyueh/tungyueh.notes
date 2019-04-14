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
        perim := geometry.Path{ {1, 1}, {5, 1}, {5, 4}, {1, 1} }
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

## Chapter 7: Interface
* Interface type 代表 type 行為的 abstraction, 隱藏了實作細節讓我們可以更彈性的寫 function

### 7.1 Interfaces as Contracts
* concrete type: 定義了準確的 representation 跟操作
* interface type: abstract type 只定義行為
* `fmt.Printf` 跟 `fmt.Sprintf` 使用 `fmt.Fprintf` reuse code
    ``` go
    package fmt
    
    func Fprintf(w io.Writer, format string, args ...interface{}) (int, error)
    
    func Printf(format string, args ...interface{}) (int, error) {
        return Fprintf(os.Stdout, format, args...)
    }
    
    func Sprintf(format string, args ...interface{}) string {
        var buf bytes.Buffer
        Fprintf(&buf, format, args...)
        return buf.String()
    }
    ```
* io.Writer 定義 contract 讓 caller 要傳 concrete type  有 Write method 進來
    ``` go
    package io
    // Writer is the interface that wraps the basic Write method.
    type Writer interface {
        // Write writes len(p) bytes from p to the underlying data stream.
        // It returns the number of bytes written from p (0 <= n <= len(p))
        // and any error encountered that caused the write to stop early.
        // Write must return a non-nil error if it returns n < len(p).
        // Write must not modify the slice data, even temporarily.
        //
        // Implementations must not retain p.
        Write(p []byte) (n int, err error)
    }
    ```
* Substitutability:`fmt.Fprintf`  不會預設要寫到檔案還是記憶體,只要有 Write method 就可以, 讓 `fmt.Fprintf` 可以自由傳入符合 interface 的變數

### 7.2 Interface Types
* Interface type 定義 concrete type 應該具有的 method
    * `io.Writer` 定義所有可以被 write 的 bytes
    * `io.Reader` 定義可以讀的 bytes
    * `io.Closer` 定義可以觀的東西
    ``` go
    package io

    type Reader interface {
        Read(p []byte) (n int, err error)
    }
    type Closer interface {
        Close() error
    }
    ```
    * 可以把 interface 組合起來稱為 embedding an interface
    ``` go
    type ReadWriter interface {
        Reader
        Writer
    }
    type ReadWriteCloser interface {
        Reader
        Writer
        Closer
    }
    ```

### 7.3 Interface Satisfaction
* 如果 type 滿足所有 interface 所需要的 method 代表 type 滿足 interface
* Assignability rules of interface
    ``` go
    var w io.Writer
    w = os.Stdout // OK: *os.File has Write method
    w = new(bytes.Buffer) // OK: *bytes.Buffer has Write method
    w = time.Second // compile error: time.Duration lacks Write method
    
    var rwc io.ReadWriteCloser
    rwc = os.Stdout // OK: *os.File has Read, Write, Close methods
    rwc = new(bytes.Buffer) // compile error: *bytes.Buffer lacks Close method
    
    w = rwc // OK: io.ReadWriteCloser has Write method
    rwc = w // compile error: io.Writer lacks Close method
    ```
* Compiler 會對 type method 的 receiver parameter 跟 argument 做 pointer 型態轉換但 type T 的 value 就不會，所以會符合比較少的 interface
    ``` go
    type IntSet struct { /* ... */ }
    func (*IntSet) String() string

    var _ = IntSet{}.String() // compile error: String requires *IntSet receiver

    var s IntSet
    var _ = s.String() // OK: s is a variable and &s has a String method

    var _ fmt.Stringer = &s // OK
    var _ fmt.Stringer = s // compile error: IntSet lacks String method
    ```
    * `var _ = s.String()`: 因為 s 是 addresable 所以 compiler 會幫忙做轉換
    * `var _ fmt.Stringer = s`: 因為 interaface 是 dynamic value 所以無法在 compile time 幫忙轉換
* Interface 把 concrete type 跟 data 封裝起來，儘管 concrete type 有 method 也不能使用
    ``` go
    os.Stdout.Write([]byte("hello")) // OK: *os.File has Write method
    os.Stdout.Close() // OK: *os.File has Close method

    var w io.Writer
    w = os.Stdout
    w.Write([]byte("hello")) // OK: io.Writer has Write method
    w.Close() // compile error: io.Writer lacks Close method
    ```
* Empty interface: 因為不需要符合任何條件代表可以 assign 任意的值
    ``` go
    var any interface{}
    any = true
    any = 12.34
    any = "hello"
    any = map[string]int{"one": 1}
    any = new(bytes.Buffer)
    ```
* Interface satisfaction 是看兩個 type 的 method 所以不需要特別宣告 concrete type 跟 satisfy 的 interface
    * 有時候也會特別 assert relationship 作為 document 讓 compiler 抓的出錯誤來
    ``` go
    // *bytes.Buffer must satisfy io.Writer
    var w io.Writer = new(bytes.Buffer)
    ```
    * 不需要 allocate variable 因為不需要任何 value 所以用 nil
    * 永遠不會用到 w 所以可以用 blank identifier
    ``` go
    // *bytes.Buffer must satisfy io.Writer
    var _ io.Writer = (*bytes.Buffer)(nil)
    ```

### 7.4. Parsing Flags with flag.Value
``` go
import (
	"flag"
	"fmt"
	"time"
)

var period = flag.Duration("period", 1*time.Second, "sleep period")

func main() {
	flag.Parse()
	fmt.Printf("Sleeping for %v...", *period)
	time.Sleep(*period)
	fmt.Println()
}
```
* fmt package 呼叫 time.Duration 的 String() method 印出
    ```shell
    $ go build gopl.io/ch7/sleep
    $ ./sleep
    Sleeping for 1s...
    ```
* -period 參數控制 sleep 時間
    ``` shell
    $ ./sleep period
    50ms
    Sleeping for 50ms...
    $ ./sleep period
    2m30s
    Sleeping for 2m30s...
    $ ./sleep period
    1.5h
    Sleeping for 1h30m0s...
    $ ./sleep period
    "1 day"
    invalid value "1 day" for flag period:
    time: invalid duration 1 day
    ```
* 只要符合 flag.Value interface 就可以使用新的 flag
    ``` go
    package flag
    // Value is the interface to the value stored in a flag.
    type Value interface {
        String() string
        Set(string) error
    }
    ```
    * String format flag value 當作 command-line help messages
    * Set: parse string argument 跟 update flag value
* `celsiusFlag` type: 可以用 Celsius 或者 Fahrenheit 表示溫度
    ``` go
    // *celsiusFlag satisfies the flag.Value interface.
    type celsiusFlag struct{ Celsius }

    func (f *celsiusFlag) Set(s string) error {
        var unit string
        var value float64
        fmt.Sscanf(s, "%f%s", &value, &unit) // no error check needed
        switch unit {
        case "C", "°C":
            f.Celsius = Celsius(value)
            return nil
        case "F", "°F":
            f.Celsius = FToC(Fahrenheit(value))
            return nil
        }
        return fmt.Errorf("invalid temperature %q", s)
    }
    ```
    * fmt.Sscanf parse floating-point number(value) and string(unit)
    ``` go
    // CelsiusFlag defines a Celsius flag with the specified name,
    // default value, and usage, and returns the address of the flag variable.
    // The flag argument must have a quantity and a unit, e.g., "100C".
    func CelsiusFlag(name string, value Celsius, usage string) *Celsius {
        f := celsiusFlag{value}
        flag.CommandLine.Var(&f, name, usage)
        return &f.Celsius
    }
    ```
    * return pointer to the Celsius field embedded within celsiusFlag f
    * Celsius field 會被 Set update
    * Var 把 flag 加進 application 原本的 flag
    * 呼叫 Var 把 celsiusFlag argument assign 到 flag.Value 的 parameter 讓 compiler 檢查是否有符合 interface
    ``` go
    var temp = tempconv.CelsiusFlag("temp", 20.0, "the temperature")

    func main() {
        flag.Parse()
        fmt.Println(*temp)
    }
    ```
    ``` shell
    $ go build gopl.io/ch7/tempflag
    $ ./tempflag
    20°C
    $ ./tempflag temp -18C
    -18°C
    $ ./tempflag temp 212°F
    100°C
    $ ./tempflag temp 273.15K
    invalid value "273.15K" for flag temp: invalid temperature "273.15K"
    Usage of ./tempflag:
        -temp value
            the temperature (default 20°C)
    $ ./tempflag help
    Usage of ./tempflag:
        -temp    value
            the temperature (default 20°C)
    ```
### 7.5. Interface Values
* Interface value 包含 concrete type 跟該 type 的 value
* type 是 compile-time 的概念所以 type 不是 value
* type descriptor 提供 type 的資訊包含 name 跟 methods
``` go
var w io.Writer
w = os.Stdout
w = new(bytes.Buffer)
w = nil
```
* `w` 有三種 value 其中第一個跟最後一個是一樣的
* Go 會將宣告的變數初始化好所以 interface 也是一樣的
* `var w io.Write` 將 type 跟 value 都設為 `nil`
* `w = os.Stdout` 把 `*os.File` type 的 value assign 給 w，另外 compiler 會做隱性轉換 `io.Writer(os.Stdout)`
* `w.Write([]byte("Hello"))` 由於 interface value 有 `*os.File` 所以會呼叫 `(*os.File).Write`
* Dynamic dispatch: compiler time 不知道 interface value 的 type 是什麼，所以 receiver argument 會複製 interface 的 dynamic value 
* `w = new(bytes.Buffer)` assign `*bytes.Buff` type 的 value 給 w
* Interface 可以用 `==` 或 `!=` 如果兩者的 Interface value 的 dynamic type 跟 dynamic value 都一樣就代表一樣
* Interface 的 dynamic type 如果是不可比較就會 panic

#### 7.5.1. Caveat: An Interface Containing a Nil Pointer Is NonNil
* Interface 有可能 dynamic type 有形態但是 dynamic value 是 nil

### 7.6. Sorting with sort.Interface
* 一般程式語言的 sort 都是 sequence of data type 然後 order data type 的 value
* Go 的 `sort.Sort` function 不管 sequence 或 element 的 representation
* Go 使用 interface `sort.Interface` 定義一般 sorting algorithm 會使用到的 method
``` go
package sort
type Interface interface {
    Len() int
    Less(i, j int) bool // i, j are indices of sequence elements
    Swap(i, j int)
}
```
* 要 sort 任何 sequence 必須先定義好有sort.Interface 的 method 的 type
``` go
type StringSlice []string

func (p StringSlice) Len() int { return len(p) }
func (p StringSlice) Less(i, j int) bool { return p[i] < p[j] }
func (p StringSlice) Swap(i, j int) { p[i], p[j] = p[j], p[i] }
```
* sorting slice of string

### 7.7. The http.Handler 
``` go
package http
type Handler interface {
    ServeHTTP(w ResponseWriter, r *Request)
}
func ListenAndServe(address string, h Handler) error
```
* `ListenAndServe` 需要 address (localhost:8888) 跟一個 dispatch 所有 requests 的 Handler interface
``` go
package main

import (
	"fmt"
	"log"
	"net/http"
)

func main() {
	db := database{"shoes": 50, "socks": 5}
	log.Fatal(http.ListenAndServe("localhost:8000", db))
}

type dollars float32

func (d dollars) String() string { return fmt.Sprintf("$%.2f", d) }

type database map[string]dollars

func (db database) ServeHTTP(w http.ResponseWriter, req *http.Request) {
	for item, price := range db {
		fmt.Fprintf(w, "%s: %s\n", item, price)
	}
}
```
* site 可以顯示商品的價格
* `database` 是個 map type
* `database` 有 ServeHTTP method 滿足 http.Handler interface
``` go
func (db database) ServeHTTP(w http.ResponseWriter, req *http.Request) {
	switch req.URL.Path {
	case "/list":
		for item, price := range db {
			fmt.Fprintf(w, "%s: %s\n", item, price)
		}
	case "/price":
		item := req.URL.Query().Get("item")
		price, ok := db[item]
		if !ok {
			w.WriteHeader(http.StatusNotFound) // 404
			fmt.Fprintf(w, "no such item: %q\n", item)
			return
		}
		fmt.Fprintf(w, "%s\n", price)
	default:
		w.WriteHeader(http.StatusNotFound) // 404
		fmt.Fprintf(w, "no such page: %s\n", req.URL)
	}
}
```
* /list 列出所有商品價格
* /price?item=socks 找出特定商品價格
``` go
func main() {
	db := database{"shoes": 50, "socks": 5}
	mux := http.NewServeMux()
	mux.Handle("/list", http.HandlerFunc(db.list))
	mux.Handle("/price", http.HandlerFunc(db.price))
	log.Fatal(http.ListenAndServe("localhost:8000", mux))
}

type database map[string]dollars

func (db database) list(w http.ResponseWriter, req *http.Request) {
	for item, price := range db {
		fmt.Fprintf(w, "%s: %s\n", item, price)
	}
}

func (db database) price(w http.ResponseWriter, req *http.Request) {
	item := req.URL.Query().Get("item")
	price, ok := db[item]
	if !ok {
		w.WriteHeader(http.StatusNotFound) // 404
		fmt.Fprintf(w, "no such item: %q\n", item)
		return
	}
	fmt.Fprintf(w, "%s\n", price)
}
```
* `net/http` `ServeMux` 可以簡化 URLs 跟 Handler 的關聯
* Go 不需要 framework 因為使用基本 library 可以組出 framework 的效果
* `db.list` 是個 method value 也就是使用 receiver vale 是 db 的  database.list method
* `db.list` 是個 function 沒有 method 也就不滿足 http.Handler interface 所以不能直接傳給 mux.Handler
* `http.HandlerFunc(db.list)` 是 conversion 不是 functino call
    ``` go
    package http
    type HandlerFunc func(w ResponseWriter, r *Request)
    func (f HandlerFunc) ServeHTTP(w ResponseWriter, r *Request) {
        f(w, r)
    }
    ```
    * `HandlerFunc` 是個有 method 且滿足 `Http.Handler` interface 的 function type
    * `HandlerFunc` 是個 adapter 將 function value 滿足 interface
    * 因為太常需要這種轉換所以 mux.HandleFunc 簡化寫法
    ``` go
    mux.HandleFunc("/list", db.list)
    mux.HandleFunc("/price", db.price)
    ```
``` go
func main() {
	db := database{"shoes": 50, "socks": 5}
	http.HandleFunc("/list", db.list)
	http.HandleFunc("/price", db.price)
	log.Fatal(http.ListenAndServe("localhost:8000", nil))
}
```
* `net/http` 提供 `ServeMux` global instance 名字叫 `DefaultServeMux` 跟 package-level function `http.Handle` 跟 `http.HandleFunc`
* 使用 `DefaultServeMux` 只要傳 `nil` 給 `ListenAndServe` 就可以了

### 7.8. The error Interface
``` go
type error interface {
    Error() string
}
```
* error interface 只有一個 method 會回傳 error message
``` go
package errors
func New(text string) error { return &errorString{text} }
type errorString struct { text string }
func (e *errorString) Error() string { return e.text }
```
* `error.New` return 新的 error 
* `errorString` 是個 struct 而不是 string 為了隱藏 representation 和以後 update
* 使用 pointer type 的 method 是為了滿足 interface error 和每次 New 出來的 error 都是不一樣的
    ``` go
    fmt.Println(errors.New("EOF") == errors.New("EOF")) // "false"
    ```
* 不常使用到 `errors.New` 因為有 wrapper function `fmt.Errorf` 可以 formaatting
* Go 的 syscall 提供 API 給 low-level system `Errorno` 滿足 `Error`
    ``` go
    package syscall

    type Errno uintptr // operating system error code

    var errors = [...]string{
        1: "operation not permitted",   // EPERM
        2: "no such file or directory", // ENOENT
        3: "no such process",           // ESRCH
        // ...
    }

    func (e Errno) Error() string {
        if 0 <= int(e) && int(e) < len(errors) {
            return errors[e]
        }
        return fmt.Sprintf("errno %d", e)
    }
    ```
    ``` go
    var err error = syscall.Errno(2)
    fmt.Println(err.Error()) // "no such file or directory"
    fmt.Println(err) // "no such file or directory"
    ```
    * `Errno` 有效率的對應出 system 的錯誤並且滿足 error interface

### 7.9. Example: Expression Evaluator
* Build arithmetic expression evaluator
* 包含 floating-point literal, binary operation(+, -, *, /), unary operation(-x, +x), function call(pow(x, y), sin(x), sqrt(x))
``` go
// A Var identifies a variable, e.g., x.
type Var string

// A literal is a numeric constant, e.g., 3.141.
type literal float64

// A unary represents a unary operator expression, e.g., -x.
type unary struct {
	op rune // one of '+', '-'
	x  Expr
}

// A binary represents a binary operator expression, e.g., x+y.
type binary struct {
	op   rune // one of '+', '-', '*', '/'
	x, y Expr
}

// A call represents a function call expression, e.g., sin(x).
type call struct {
	fn   string // one of "pow", "sin", "sqrt"
	args []Expr
}
```
* 需要 evaluate 有 variable 的 expression 所以需要 map variable name 到 values 的 `environment`
``` go
type Env map[Var]float64
```
* `Eval` 用來 計算 expression 的值，因為每個 expression 都需要這個 method 所以加進 `Expr` interface
``` go
type Expr interface {
    // Eval returns the value of this Expr in the environment env.
    Eval(env Env) float64
}
```
* package eval 只有 export `Expr`, `Env`, `Var` 讓 client 不需要讀取其他 expression type 就可以 evaluate
``` go
func (v Var) Eval(env Env) float64 {
	return env[v]
}

func (l literal) Eval(_ Env) float64 {
	return float64(l)
}
```
* `Var` 從 environment 找 varaible 的值並回傳，如果找不到就回傳 0
* `literal` 單純回傳 literal 的 value
``` go
func (u unary) Eval(env Env) float64 {
	switch u.op {
	case '+':
		return +u.x.Eval(env)
	case '-':
		return -u.x.Eval(env)
	}
	panic(fmt.Sprintf("unsupported unary operator: %q", u.op))
}

func (b binary) Eval(env Env) float64 {
	switch b.op {
	case '+':
		return b.x.Eval(env) + b.y.Eval(env)
	case '-':
		return b.x.Eval(env) - b.y.Eval(env)
	case '*':
		return b.x.Eval(env) * b.y.Eval(env)
	case '/':
		return b.x.Eval(env) / b.y.Eval(env)
	}
	panic(fmt.Sprintf("unsupported binary operator: %q", b.op))
}

func (c call) Eval(env Env) float64 {
	switch c.fn {
	case "pow":
		return math.Pow(c.args[0].Eval(env), c.args[1].Eval(env))
	case "sin":
		return math.Sin(c.args[0].Eval(env))
	case "sqrt":
		return math.Sqrt(c.args[0].Eval(env))
	}
	panic(fmt.Sprintf("unsupported function call: %s", c.fn))
}
```
* unary 跟 binary 都是先決定 operation 在去 eval value 後進行運算
* 不考慮除 0 或者除無限等問題，因為還是會產生結果
* 有些 method 也會錯誤像是 function name 給錯或者只使用不允許的 operation 這些錯誤都會造成 panic
``` go
func TestEval(t *testing.T) {
	tests := []struct {
		expr string
		env  Env
		want string
	}{
		{"sqrt(A / pi)", Env{"A": 87616, "pi": math.Pi}, "167"},
		{"pow(x, 3) + pow(y, 3)", Env{"x": 12, "y": 1}, "1729"},
		{"pow(x, 3) + pow(y, 3)", Env{"x": 9, "y": 10}, "1729"},
		{"5 / 9 * (F - 32)", Env{"F": -40}, "-40"},
		{"5 / 9 * (F - 32)", Env{"F": 32}, "0"},
		{"5 / 9 * (F - 32)", Env{"F": 212}, "100"},
		//!-Eval
		// additional tests that don't appear in the book
		{"-1 + -x", Env{"x": 1}, "-2"},
		{"-1 - x", Env{"x": 1}, "-2"},
		//!+Eval
	}
	var prevExpr string
	for _, test := range tests {
		// Print expr only when it changes.
		if test.expr != prevExpr {
			fmt.Printf("\n%s\n", test.expr)
			prevExpr = test.expr
		}
		expr, err := Parse(test.expr)
		if err != nil {
			t.Error(err) // parse error
			continue
		}
		got := fmt.Sprintf("%.6g", expr.Eval(test.env))
		fmt.Printf("\t%v => %s\n", test.env, got)
		if got != test.want {
			t.Errorf("%s.Eval() in %v = %q, want %q\n",
				test.expr, test.env, got, test.want)
		}
	}
}
```
* 每次測試不同的 expression 跟 environment
* Check syntax for static error 可以在跑程式起來之前就檢查到錯誤
``` go
type Expr interface {
	// Eval returns the value of this Expr in the environment env.
	Eval(env Env) float64
	// Check reports errors in this Expr and adds its Vars to the set.
	Check(vars map[Var]bool) error
}
```
* Check method 檢查 expression 的 static error 
``` go
func (v Var) Check(vars map[Var]bool) error {
	vars[v] = true
	return nil
}

func (literal) Check(vars map[Var]bool) error {
	return nil
}

func (u unary) Check(vars map[Var]bool) error {
	if !strings.ContainsRune("+-", u.op) {
		return fmt.Errorf("unexpected unary op %q", u.op)
	}
	return u.x.Check(vars)
}

func (b binary) Check(vars map[Var]bool) error {
	if !strings.ContainsRune("+-*/", b.op) {
		return fmt.Errorf("unexpected binary op %q", b.op)
	}
	if err := b.x.Check(vars); err != nil {
		return err
	}
	return b.y.Check(vars)
}

func (c call) Check(vars map[Var]bool) error {
	arity, ok := numParams[c.fn]
	if !ok {
		return fmt.Errorf("unknown function %q", c.fn)
	}
	if len(c.args) != arity {
		return fmt.Errorf("call to %s has %d args, want %d",
			c.fn, len(c.args), arity)
	}
	for _, arg := range c.args {
		if err := arg.Check(vars); err != nil {
			return err
		}
	}
	return nil
}

var numParams = map[string]int{"pow": 2, "sin": 1, "sqrt": 1}
```
* Check Var 跟literal 不可能 fail 所以都是 return nil
* Check unary 跟 binary 先檢查 operator 再來檢查 operand
* Check call 先檢查 function name 在檢查 argument 的數量 在檢查 argument 本身
* Check argument 是 Var 的 set 在 check expression 途中會增加 variable name，如果這些 variable name 都有在 environment 裡面代表成功

### 7.10. Type Assertions
* type assertion 使用在 interface value 的 operation
* `x.(T)` x 是 expression of interface type, T 是 type
* type assertion 檢查 dynamic type 跟 asserted type 一樣
* 如果 T 是 concrete type，type assertion 會檢查 dynamic type 是否和 T 一樣
    * 如果一樣 type assertion 結果就是 dynamic value
    * 如果不一樣就 panic
    ``` go
    var w io.Writer
    w = os.Stdout
    f := w.(*os.File) // success: f == os.Stdout
    c := w.(*bytes.Buffer) // panic: interface holds *os.File, not *bytes.Buffer
    ```
* 如果 T 是 interface type，type assertion 檢查 dynamic type 是否滿足 type T
    * 如果滿足則結果還是 interface value 但 dynamic type 換成 type T
    ``` go
    var w io.Writer
    w = os.Stdout
    rw := w.(io.ReadWriter) // success: *os.File has both Read and Write
    w = new(ByteCounter)
    rw = w.(io.ReadWriter) // panic: *ByteCounter has no Read method
    ```
    * rw 多了 Read method
* 如果 operand 是 nil 則 type assertion 一定會 panic
* 想測試 dynamic type 是否為 type T 又不想 panic 可以使用兩個 result 來接收結果
    ``` go
    var w io.Writer = os.Stdout
    f, ok := w.(*os.File) // success: ok, f == os.Stdout
    b, ok := w.(*bytes.Buffer) // failure: !ok, b == nil
    ```

### 7.11. Discriminating Errors with Type Assertions
* 有很多可能的錯誤會在 file operation 的時候發生，os package 提供 file exist, file not found, file permission denied 的 helper function
    ``` go
    package os

    func IsExist(err error) bool
    func IsNotExist(err error) bool
    func IsPermission(err error) bool
    ```
* 最簡單的實作方式就是確認 error message 但是不同平台會有不同 message 而且錯誤也不一定每次都是相同的 message
``` go
package os

// PathError records an error and the operation and file path that caused it.
type PathError struct {
	Op   string
	Path string
	Err  error
}

func (e *PathError) Error() string {
	return e.Op + " " + e.Path + ": " + e.Err.Error()
}
```
* 使用專用的 type 來描述 error
* `PathError` 描述 file operation 的錯誤像是 create or delete file
* 處理 error 都是統一用呼叫 Error method 來處理
* Client 需要分辨不同 error 需要使用 type assertion
``` go
var ErrNotExist = errors.New("file does not exist")

// IsNotExist returns a boolean indicating whether the error is known to
// report that a file or directory does not exist. It is satisfied by
// ErrNotExist as well as some syscall errors.
func IsNotExist(err error) bool {
	fmt.Println(err)
	if pe, ok := err.(*PathError); ok {
		err = pe.Err
		fmt.Println(err)
	}
	fmt.Println(err)
	return err == syscall.ENOENT || err == ErrNotExist
}
```
* error 是 `syscall.ENOENT` 或者 `os.ErrorNotExist` 或者 `*PathError` 都回傳 True

### 7.12. Querying Behaviors with Int erface Type Assertions
``` go
func writeHeader(w io.Writer, contentType string) error {
	if _, err := w.Write([]byte("ContentType:")); err != nil {
		return err
	}
	if _, err := w.Write([]byte(contentType)); err != nil {
		return err
	}
	// ...
}
```
* `io.Writer w` 代表 HTTP response
* `Write` method 需要 byte slice 但是我們希望寫入的是 string 因此需要做轉換 `[]byte(...)`
* `[]byte(...)` 需要 allocate 記憶體但是又馬上丟棄，假使這是核心部分就會造成速度緩慢
* `net/http` package 有些 type 有 `WriteString` 讓人方便的寫入 string 而不需要 allocate memory
* 無法確定 `io.Writer` 到底有沒有 `WriteString` method 但我們可以定義新的 type 只有 `WriteString` 使用 type assertion 確認 dynamic type 是否滿足新的 type
    ``` go
    // writeString writes s to w.
    // If w has a WriteString method, it is invoked instead of w.Write.
    func writeString(w io.Writer, s string) (n int, err error) {
        type stringWriter interface {
            WriteString(string) (n int, err error)
        }
        if sw, ok := w.(stringWriter); ok {
            return sw.WriteString(s) // avoid a copy
        }
        return w.Write([]byte(s)) // allocate temporary copy
    }
    func writeHeader(w io.Writer, contentType string) error {
        if _, err := writeString(w, "ContentType:"); err != nil {
            return err
        }
        if _, err := writeString(w, contentType); err != nil {
            return err
        }
        // ...
    }
    ```
    * 為了避免重複的 check code 所以搬到 function 裡面
    * type assertion 檢查 general interface type 是否有更符合特定的 interface type，如果有就使用
``` go
package fmt

func formatOneValue(x interface{}) string {
	if err, ok := x.(error); ok {
		return err.Error()
	}
	if str, ok := x.(Stringer); ok {
		return str.String()
	}
	// ...all other types...
}
```
* `fmt.Fprintf` 檢查使否滿足 error 或 Stringer interface

### 7.13. Type Switches
* Interface 使用方式有兩種
    * 專注在 Interface 的 method 表示 concrete type 滿足行為而隱藏 concrete type 的實作細節
    * Discriminated union
        * interface value 可以相容於各種 concrete type 把 interface 視為 concrete type 的集合
        * 注重 concreate type 本身，而不是 interface  method，不隱藏實作細節
* type switch 簡化 if-else chain of type assertion
``` go
func sqlQuote(x interface{}) string {
	switch x := x.(type) {
	case nil:
		return "NULL"
	case int, uint:
		return fmt.Sprintf("%d", x) // x has type interface{} here.
	case bool:
		if x {
			return "TRUE"
		}
		return "FALSE"
	case string:
		return sqlQuoteString(x) // (not shown)
	default:
		panic(fmt.Sprintf("unexpected type %T: %v", x, x))
	}
}
```
* case 的順序有影響，一旦符合就執行
* `switch x := x.(type)` reuse x 因為 switch 會創造新的 lexical block 所以不會 conflict
* case 內容相同可以 combine 
* 雖然 x 是 interface{} 但可以視為 discriminated union of int, uint, bool, nil, string

### 7.14. Example: Token-Based XML Decoding
* `encoding/xml` 提供與 `encoding/json` 用來 encode/decode JSON 的Marshal 跟 Unmarshal
* `encoding/xml` 提供 low-level  token-based API  來 decode xml 不會建構整個 document tree
* token-based 會 consume input 後產生 stream of tokens 每一個都是 concrete type: `StartElement`, `EndElement`, `CharData`, `Comment`
``` go
package xml

type Name struct {
	Local string // e.g., "Title" or "id"
}
type Attr struct { // e.g., name="value"
	Name  Name
	Value string
}

// A Token includes StartElement, EndElement, CharData,
// and Comment, plus a few esoteric types (not shown).
type Token interface{}
type StartElement struct { // e.g., <name>
	Name Name
	Attr []Attr
}
type EndElement struct{ Name Name } // e.g., </name>
type CharData []byte                // e.g., <p>CharData</p>
type Comment []byte                 // e.g., <!Comment

type Decoder struct { /* ... */}

func NewDecoder(io.Reader) *Decoder
func (*Decoder) Token() (Token, error) // returns next Token in sequence

```
* Token interface 沒有 method 也就是一個 discriminated union 的例子
    * 一般的 interface 隱藏 concrete type 的實作細節，每一種 concrete type 都當成一樣的東西
    * the set of concrete type 滿足 discriminated union，藉由 type switch 來決定要使用何種 method
``` go
// Xmlselect prints the text of selected elements of an XML document.
package main

import (
	"encoding/xml"
	"fmt"
	"io"
	"os"
	"strings"
)

func main() {
	dec := xml.NewDecoder(os.Stdin)
	var stack []string // stack of element names
	for {
		tok, err := dec.Token()
		if err == io.EOF {
			break
		} else if err != nil {
			fmt.Fprintf(os.Stderr, "xmlselect: %v\n", err)
			os.Exit(1)
		}
		switch tok := tok.(type) {
		case xml.StartElement:
			stack = append(stack, tok.Name.Local) // push
		case xml.EndElement:
			stack = stack[:len(stack)-1] // pop
		case xml.CharData:
			if containsAll(stack, os.Args[1:]) {
				fmt.Printf("%s: %s\n", strings.Join(stack, " "), tok)
			}
		}
	}
}

// containsAll reports whether x contains the elements of y, in order.
func containsAll(x, y []string) bool {
	for len(y) <= len(x) {
		if len(y) == 0 {
			return true
		}
		if x[0] == y[0] {
			y = y[1:]
		}
		x = x[1:]
	}
	return false
}
```

### 7.15. A Few Words of Advice
* 初學者當設計新的 package 時候先是定義了很多 interface 接著才定義滿足於 interface 的 concrete type
    * 壞處: 導致很多 interface 只有一種實作，這些 interface 就是多餘的還會造成 run-time 的 cost
    * 改善: 使用 export 的方式決定 type 的 method 或 field 可以給其他 package 看見
* interface 適用於多種 concrete type 需要同樣處理方式使用
    * 例外: interface 只有一種 concrete type 滿足但是因為相依性而無法在同一個 package 存在，所以可以用 interface 來 decouple two package
* interface 的設計原則是只定義需要的 method: ask only for what you need

## Chapter 8: Goroutines and Channels
* Concurrent programming 是指多個 autonomous activities 所組合起來的程式
    * Web server 同時處理多個 client 的 request
    * 手機跟平板同時 render 畫面動畫跟處理網路 request 跟計算
    * Batch problem 用 concurrency 隱藏 I/O operation latency
* Go 支援兩種 concurrent programming
    * Goroutines and Channels 支援 communicating sequential processes(GSP): independent activity 互相傳 value
    * Shared memory multithreading

### 8.1 Goroutiness
* 每一個 executing activity 稱為 goroutine
* Goroutine 近似於 thread
* 程式一開始一個 goroutine 呼叫 main 稱之為 main goroutine
* go statement create new goroutine
    * go statement 是由一般的 function 或 method 前面加上 go
    * functino 會在新的 goroutine 中被呼叫
``` go
func main() {
	go spinner(100 * time.Millisecond)
	const n = 45
	fibN := fib(n) // slow
	fmt.Printf("\rFibonacci(%d) = %d\n", n, fibN)
}

func spinner(delay time.Duration) {
	for {
		for _, r := range `-\|/` {
			fmt.Printf("\r%c", r)
			time.Sleep(delay)
		}
	}
}

func fib(x int) int {
	if x < 2 {
		return x
	}
	return fib(x-1) + fib(x-2)
}
```
* 計算結果需要花很久，使用畫面輸出讓使用者知道程式沒有當掉
* main function return 後所有 goroutine 馬上停止接著程式 exit
* Goroutine 無法停止另外一個 goroutine
* spinning 跟 fibonacci computing 是兩個 autonomous activities

### 8.2 Example: Concurrent Clock Server
* Networking 很自然使用 concurrency programming 因為 server handle 不同獨立 client 的 request 
* net package 提供打造使用 TCP, UDP, Unix domain socket 溝通的 network client 跟 server program 的 component
* sequential clock server write current time per second to client
    ``` go
    func main() {
        listener, err := net.Listen("tcp", "localhost:8000")
        if err != nil {
            log.Fatal(err)
        }
        for {
            conn, err := listener.Accept()
            if err != nil {
                log.Print(err) // e.g., connection aborted
                continue
            }
            handleConn(conn) // handle one connection at a time
        }
    }

    func handleConn(c net.Conn) {
        defer c.Close()
        for {
            _, err := io.WriteString(c, time.Now().Format("15:04:05\n"))
            if err != nil {
                return // e.g., client disconnected
            }
            time.Sleep(1 * time.Second)
        }
    }
    ```
    * net.Conn 符合 io.Writer interface 所以可以直接使用
    * time.Time.Format method 使用 example 顯示 date and time information
``` go
func main() {
	conn, err := net.Dial("tcp", "localhost:8000")
	if err != nil {
		log.Fatal(err)
	}
	defer conn.Close()
	mustCopy(os.Stdout, conn)
}

func mustCopy(dst io.Writer, src io.Reader) {
	if _, err := io.Copy(dst, src); err != nil {
		log.Fatal(err)
	}
}
```
* Go version of netcat
* Read data from connection and write data to standard output
``` go
	for {
		conn, err := listener.Accept()
		if err != nil {
			log.Print(err) // e.g., connection aborted
			continue
		}
		go handleConn(conn) // handle connections concurrently
	}
```
* 使用 go 呼叫 handleConn 可以同時 handle 多個 client

### 8.3 Example: Concurrent Echo Server
* Echo server 使用多個 goroutine 在一個 connection 
``` go
func echo(c net.Conn, shout string, delay time.Duration) {
	fmt.Fprintln(c, "\t", strings.ToUpper(shout))
	time.Sleep(delay)
	fmt.Fprintln(c, "\t", shout)
	time.Sleep(delay)
	fmt.Fprintln(c, "\t", strings.ToLower(shout))
}

func handleConn(c net.Conn) {
	input := bufio.NewScanner(c)
	for input.Scan() {
		echo(c, input.Text(), 1*time.Second)
	}
	// NOTE: ignoring potential errors from input.Err()
	c.Close()
}
```
* Reverberation echo server: echo "HELLO!", "Hello!", "hello!" with delay
``` go
func main() {
	conn, err := net.Dial("tcp", "localhost:8000")
	if err != nil {
		log.Fatal(err)
	}
	defer conn.Close()
	go mustCopy(os.Stdout, conn)
	mustCopy(conn, os.Stdin)
}
```
* 讓 client 可以印出 server 的 response
* main goroutine read standard input and send it to the server
* second goroutine read and print server response
``` go
func handleConn(c net.Conn) {
	input := bufio.NewScanner(c)
	for input.Scan() {
		go echo(c, input.Text(), 1*time.Second)
	}
	// NOTE: ignoring potential errors from input.Err()
	c.Close()
}
```
* 改良 echo 只會按照順序 reverberation

### 8.4 Channels
* Channel 是 goroutine 之前的 connection
* Channel 讓 goroutine 可以 send value 到其他 goroutine
* 每個 channel 都是傳導特定 type 的 value 稱為 channel 的 element type
``` go
ch = make(chan int)
```
* channel 的 element type 是 `int` 則 channel type 是 `chan int` 
* `make` create a new channel
* channel reference 到 make create 的 data structure
* copy 或傳入 function 都是 copy reference
* zero value of channel is `nil`
* 相同 type 的 channel 可以比較，如果 reference 到相同的 data structure 則代表一樣的，也可以跟 nil 比較
* channel operation: `send` and `receive` 統稱 communication
    * send statement 傳送 value 到另外的 goroutine
    * send 跟 receive 都是使用 `<-` operator
    ``` go
    ch <- x   // a send statement
    x = <- ch // a receive expression in an assignment statement
    <- ch     // a receive statement; result is discarded
    ```
* channel operation: `close`
    * Set flag 告知不能傳 value 給該 channel
    * send value to closed channel will panic
    * receive value from closed channel 會 yield 之前收到的 value，把收到的 value yield 完之後會 yield zero value
* Unbuffered channel: 沒有 capacity 的 channel
    ``` go
    ch = make(chan int) // unbuffered channel
    ch = make(chan int, 0) // unbuffered channel
    ch = make(chan int, 3) // buffered channel with capacity 3
    ```

#### 8.4.1. Unbuffered Channels
* Send operation on the unbuffered channel 會 block goroutine 直到另外一個 goroutine recieve 這個 channel
* Recieve 一個 unbuffered channel 也會 block 直到有另外一個 goroutine send
* Unbuffered channel 機制讓 sending goroutine 跟 recieving goroutine 可以 synchronize 所以又稱為 synchronous channel
* netcat 可以等 background goroutine 結束才結束
    ``` go
    func main() {
        conn, err := net.Dial("tcp", "localhost:8000")
        if err != nil {
            log.Fatal(err)
        }
        done := make(chan struct{})
        go func() {
            io.Copy(os.Stdout, conn) // NOTE: ignoring errors
            log.Println("done")
            done <- struct{}{} // signal the main goroutine
        }()
        mustCopy(conn, os.Stdin)
        conn.Close()
        <-done // wait for background goroutine to finish
    }
    ```
    * done channel sychronize main goroutine 跟 background goroutine
    * user close input stream 會讓 main 把 conn close
        * close write 讓 seerver 收到 end-of-file
        * close read 讓 background goroutine io.Copy 會出現 read from closed connection 錯誤
    * Message 不需要 value 只用來 synchronize 稱為 event

#### 8.4.2. Pipelines
* 使用 channel 把 goroutine 的輸入輸出串再一起成為 pipeline
``` go
func main() {
	naturals := make(chan int)
	squares := make(chan int)

	// Counter
	go func() {
		for x := 0; ; x++ {
			naturals <- x
		}
	}()

	// Squarer
	go func() {
		for {
			x := <-naturals
			squares <- x * x
		}
	}()

	// Printer (in main goroutine)
	for {
		fmt.Println(<-squares)
	}
}
```
``` go
func main() {
	naturals := make(chan int)
	squares := make(chan int)

	// Counter
	go func() {
		for x := 0; x < 100; x++ {
			naturals <- x
		}
		close(naturals)
	}()

	// Squarer
	go func() {
		for x := range naturals {
			squares <- x * x
		}
		close(squares)
	}()

	// Printer (in main goroutine)
	for x := range squares {
		fmt.Println(x)
	}
}
```
* 輸出有限數字，使用 close 關閉 channel
* 不一定結束 channel 都要 close，但需要告知其他 goroutine 已經送完所有東西就需要 close
* close 一個已經 close 的 channel 會 panic

#### 8.4.3. Unidirectional Channel Types
* 隨著程式成長會需要將 goroutine 拆開成 function
    ``` go
    func counter(out chan int)
    func squarer(out, in chan int)
    func printer(in chan int)
    ```
    * `out` 跟 `in` 都是 `chan int` type 但是只能 receive from`in`channel 跟 send to `out` channel
    * 無法阻止 send to `in` channel 跟 receive from `out` channel
* Unidirectional channel type 規定 chane type 只能 send 或 receive
    * `chan<- int` type 是 send-only channel of int
    * `<-chan int` type 是 receive-only channel of int
    * close receive-only hannel 會 compile error
``` go
func counter(out chan<- int) {
	for x := 0; x < 100; x++ {
		out <- x
	}
	close(out)
}

func squarer(out chan<- int, in <-chan int) {
	for v := range in {
		out <- v * v
	}
	close(out)
}

func printer(in <-chan int) {
	for v := range in {
		fmt.Println(v)
	}
}

func main() {
	naturals := make(chan int)
	squares := make(chan int)

	go counter(naturals)
	go squarer(squares, naturals)
	printer(squares)
}
```
* `counter(naturals)` 跟 `printer(squares)` compiler 會隱性轉換將 bidirectional channel type 轉成 unidirectional channel

#### 8.4.4. Buffered Channels
* Buffered channel has a queue of elements
* Create buffered channel 就要決定 queue 大小
* Send to full channel will block goroutine
* Receive from empty channel will block goroutine
* `cap(ch)` 得知 queue 的大小
* `len(ch)` 得知目前 element 數量
* 有時候會因為容易撰寫 buffered channel 的關係而在一個 goroutine 裡面當成 queue，但沒有使用好會讓整個程式暴露在 blocking 的情況下
* goroutine leak: send one the channel which no goroutine will ever receive
* 選擇使用 unbuffered channel 或 buffered channel 會影響到程式正確性
    * unbuffered channel 能確保每個 send 會 synchronize 相對應的 receive
    * buffered channel decouple send and receive operation
    * 如果知道有多少 value 會被 send 到 channel 就會使用 unbufferd channel，因為不會想要把所有 value send 完才開始處理
    * buffered channel 的 capacity 給的不足夠會發生 deadlock
* Buffered channel 的 capacity 會影響程式效能

### 8.5 Looping in Parallel
* Embarrassingly parallel: 分割出來的小問題彼此獨立，可以使用 concurrency 增加效能
``` go
func makeThumbnails(filenames []string) {
	for _, f := range filenames {
		if _, err := thumbnail.ImageFile(f); err != nil {
			log.Println(err)
		}
	}
}
```
* 一次執行一個縮圖
``` go
// NOTE: incorrect!
func makeThumbnails2(filenames []string) {
	for _, f := range filenames {
		go thumbnail.ImageFile(f) // NOTE: ignoring errors
	}
}
```
* 使用 goroutine 處理每個縮圖
* 不正確因為不會等 goroutine 結束才結束
``` go
// makeThumbnails3 makes thumbnails of the specified files in parallel.
func makeThumbnails3(filenames []string) {
	ch := make(chan struct{})
	for _, f := range filenames {
		go func(f string) {
			thumbnail.ImageFile(f) // NOTE: ignoring errors
			ch <- struct{}{}
		}(f)
	}

	// Wait for goroutines to complete.
	for range filenames {
		<-ch
	}
}
```
* 因為知道總共有多少檔案需要處理所以使用 channel 來計算是否處理完畢
* literal function 使用 explicit 的 f 而不是 range 出來的 f
* 使用 range assign 的 f 會讓所有 goroutine 都使用最後一個來處理
``` go
// makeThumbnails4 makes thumbnails for the specified files in parallel.
// It returns an error if any step failed.
func makeThumbnails4(filenames []string) error {
	errors := make(chan error)

	for _, f := range filenames {
		go func(f string) {
			_, err := thumbnail.ImageFile(f)
			errors <- err
		}(f)
	}

	for range filenames {
		if err := <-errors; err != nil {
			return err // NOTE: incorrect: goroutine leak!
		}
	}

	return nil
}
```
* 第一個 create file error 就會停止
* 遇到 error 就 return 導致沒有人去 receive err channel 讓其他 goroutine block 也就是 goroutine leak
``` go
// makeThumbnails5 makes thumbnails for the specified files in parallel.
// It returns the generated file names in an arbitrary order,
// or an error if any step failed.
func makeThumbnails5(filenames []string) (thumbfiles []string, err error) {
	type item struct {
		thumbfile string
		err       error
	}

	ch := make(chan item, len(filenames))
	for _, f := range filenames {
		go func(f string) {
			var it item
			it.thumbfile, it.err = thumbnail.ImageFile(f)
			ch <- it
		}(f)
	}

	for range filenames {
		it := <-ch
		if it.err != nil {
			return nil, it.err
		}
		thumbfiles = append(thumbfiles, it.thumbfile)
	}

	return thumbfiles, nil
}
```
* 使用 buffered channel 避免錯誤出現後造成 goroutine leak
``` go
// makeThumbnails6 makes thumbnails for each file received from the channel.
// It returns the number of bytes occupied by the files it creates.
func makeThumbnails6(filenames <-chan string) int64 {
	sizes := make(chan int64)
	var wg sync.WaitGroup // number of working goroutines
	for f := range filenames {
		wg.Add(1)
		// worker
		go func(f string) {
			defer wg.Done()
			thumb, err := thumbnail.ImageFile(f)
			if err != nil {
				log.Println(err)
				return
			}
			info, _ := os.Stat(thumb) // OK to ignore error
			sizes <- info.Size()
		}(f)
	}

	// closer
	go func() {
		wg.Wait()
		close(sizes)
	}()

	var total int64
	for size := range sizes {
		total += size
	}
	return total
}
```
* return total bytes of new files
* 使用 channel 接收 filename
* `sync.WaitGroup` count goroutine
    * `Add`: goroutine 啟動之前呼叫，如果再 goroutine 裡面呼叫可能會在 closer goroutine 呼叫 wait 之後才 Add
    * `Done`: 等於 `Add(-1)` 使用 defer 確保發生錯誤還是會呼叫 Done
*  size channel 計算總共的 file size 
*  wait 跟 close 必須平行處理
    *  wait 在 main goroutine 的 loop 之前會造成永遠不停止因為沒有 goroutine receive size channel
    *  wait 在 main goroutine 的 loop 之後會造成沒有人 close sizes channel 所以 loop 不會停止

### 8.6. Example: Concurrent Web Crawler
* 使用 concurrent 方式建立 link graph of web
``` go
func main() {
	worklist := make(chan []string)

	// Start with the command-line arguments.
	go func() { worklist <- os.Args[1:] }()

	// Crawl the web concurrently.
	seen := make(map[string]bool)
	for list := range worklist {
		for _, link := range list {
			if !seen[link] {
				seen[link] = true
				go func(link string) {
					worklist <- crawl(link)
				}(link)
			}
		}
	}
}
```
* worklist 使用 channel 同時處理 URLs
* crawl goroutine 用 explicit parameter 避免 loop variable problem
* 一開始要使用 goroutine 把 command-line arguments 送給 worklist ,不然會 deadlock, main goroutine 跟 crawler goroutine 都會想送給 ｗorklist 但沒有人會 receive
* 因為太平行處理導致容易超過網路連線數量而使 DNS lokup 跟 `net.Dial` 失敗
    * 沒有上限的平行處理容易超過系統的限制，例如: CPU cores for compute-bound workloads, local disk I/O operation, network bandwidth
* 所以需要控管超越某個數量的呼叫時候就要停止
* 使用 buffered channel 來限制平行處理的程度，稱為 couting semaphore
* channel 中空的 slot 像是 token,send value 給 channel 像是 acquires token, receive value from channel 像是 release token
``` go
// tokens is a counting semaphore used to
// enforce a limit of 20 concurrent requests.
var tokens = make(chan struct{}, 20)

func crawl(url string) []string {
	fmt.Println(url)
	tokens <- struct{}{} // acquire a token
	list, err := links.Extract(url)
	<-tokens // release the token

	if err != nil {
		log.Print(err)
	}
	return list
}
```
* 另外的問題是即使找到了所有 link 程式無法停止
* 當 worklist 已經空了而且沒有 crawl goroutine 還在跑就需要 break main loop
``` go
func main() {
	worklist := make(chan []string)
	var n int // number of pending sends to worklist

	// Start with the command-line arguments.
	n++
	go func() { worklist <- os.Args[1:] }()

	// Crawl the web concurrently.
	seen := make(map[string]bool)
	for ; n > 0; n-- {
		list := <-worklist
		for _, link := range list {
			if !seen[link] {
				seen[link] = true
				n++
				go func(link string) {
					worklist <- crawl(link)
				}(link)
			}
		}
	}
}
```
* 使用 counter n 來計算 worklist 的數量
``` go
func main() {
	worklist := make(chan []string)  // lists of URLs, may have duplicates
	unseenLinks := make(chan string) // de-duplicated URLs

	// Add command-line arguments to worklist.
	go func() { worklist <- os.Args[1:] }()

	// Create 20 crawler goroutines to fetch each unseen link.
	for i := 0; i < 20; i++ {
		go func() {
			for link := range unseenLinks {
				foundLinks := crawl(link)
				go func() { worklist <- foundLinks }()
			}
		}()
	}

	// The main goroutine de-duplicates worklist items
	// and sends the unseen ones to the crawlers.
	seen := make(map[string]bool)
	for list := range worklist {
		for _, link := range list {
			if !seen[link] {
				seen[link] = true
				unseenLinks <- link
			}
		}
	}
}
```
* 另一種方法解決過度平行化的問題
* 使用原本的 crawl function，使用 20 long-lived crawler goroutine 確保 20 HTTP requests active concurrently
* crawler goroutine 都是使用相同的 channel `unseenLinks`
* main goroutine 負責去除 worklist 重複的網址

### 8.7. Multiplexing with select
``` go
func main() {
	fmt.Println("Commencing countdown.")
	tick := time.Tick(1 * time.Second)
	for countdown := 10; countdown > 0; countdown-- {
		fmt.Println(countdown)
		<-tick
	}
	launch()
}
```
* `time.Tick` return 會週期性 send event 的channel
``` go
func main() {
	abort := make(chan struct{})
	go func() {
		os.Stdin.Read(make([]byte, 1)) // read a single byte
		abort <- struct{}{}
	}()
	launch()
}
```
* 增加可以取消發射火箭的功能
* 需要 select 來 multiplex 正常與不正常的情況
``` go
	fmt.Println("Commencing countdown.  Press return to abort.")
	select {
	case <-time.After(10 * time.Second):
		// Do nothing.
	case <-abort:
		fmt.Println("Launch aborted!")
		return
	}
	launch()
```
* select 等待可以處理 case 一旦開始處理其他 case 就不會同時處理
* `select{}`: 沒有 case  的 select 會永遠等待
* `time.After`: 馬上 return channel 跟 start new goroutine 等待特定時間後 send value 給 channel
``` go
func main() {
	ch := make(chan int, 1)
	for i := 0; i < 10; i++ {
		select {
		case x := <-ch:
			fmt.Println(x) // "0" "2" "4" "6" "8"
		case ch <- i:
		}
	}
}
```
* `ch`  是個 buffer size 是 1 的 channel, 所以會在 empty 跟 full 的狀態交替
* 因為不是空的就是滿的所以只會有一個 case 會執行
* 多個 case 都 ready 則 select 會隨機挑一個來執行，所以當增加 channel 的 size 會導致每次結果不一定會一樣
``` go
func main() {
	// ...create abort channel...
	fmt.Println("Commencing countdown.  Press return to abort.")
	tick := time.Tick(1 * time.Second)
	for countdown := 10; countdown > 0; countdown-- {
		fmt.Println(countdown)
		select {
		case <-tick:
			// Do nothing.
		case <-abort:
			fmt.Println("Launch aborted!")
			return
		}
	}
	launch()
}
```
* `countdown` function return 後就沒有人 receive `time.Tick` 造成 goroutine leak
``` go
ticker := time.NewTicker(1 * time.Second)
<-ticker.C // receive from the ticker's channel
ticker.Stop() // cause the ticker's goroutine to terminate
```
* 如果不是使用 `Tick` 在整個 application lifetime 就用這種 
* 如果只是想嘗試 send 或 receive channel 而不 block 可以使用 selct default
``` go
select {
case <- abort:
    fmt.Printf("Launch aborted!\n")
    return
default:
    // do nothing
}
```
* 只 receive abort channel 稱為 polling a channel

### 8.8. Example: Concurrent Directory Traversal
* 回報多個資料夾的硬碟使用量
``` go
// walkDir recursively walks the file tree rooted at dir
// and sends the size of each found file on fileSizes.
func walkDir(dir string, fileSizes chan<- int64) {
	for _, entry := range dirents(dir) {
		if entry.IsDir() {
			subdir := filepath.Join(dir, entry.Name())
			walkDir(subdir, fileSizes)
		} else {
			fileSizes <- entry.Size()
		}
	}
}

// dirents returns the entries of directory dir.
func dirents(dir string) []os.FileInfo {
	entries, err := ioutil.ReadDir(dir)
	if err != nil {
		fmt.Fprintf(os.Stderr, "du1: %v\n", err)
		return nil
	}
	return entries
}
```
* `wakDir` 遞迴找出所有檔案的大小
    * `walkDir` send message 到 `fileSizes` channel
 * `dirents` 列舉 directory entries
    * `ioutil.ReadDir` 回傳 slice of `os.FileInfo`
``` go
// The du1 command computes the disk usage of the files in a directory.
package main

import (
	"flag"
	"fmt"
	"io/ioutil"
	"os"
	"path/filepath"
)

func main() {
	// Determine the initial directories.
	flag.Parse()
	roots := flag.Args()
	if len(roots) == 0 {
		roots = []string{"."}
	}

	// Traverse the file tree.
	fileSizes := make(chan int64)
	go func() {
		for _, root := range roots {
			walkDir(root, fileSizes)
		}
		close(fileSizes)
	}()

	// Print the results.
	var nfiles, nbytes int64
	for size := range fileSizes {
		nfiles++
		nbytes += size
	}
	printDiskUsage(nfiles, nbytes)
}

func printDiskUsage(nfiles, nbytes int64) {
	fmt.Printf("%d files  %.1f GB\n", nfiles, float64(nbytes)/1e9)
}
```
* main function 使用兩個 goroutines
    * backgroud goroutine 呼叫 `walkDir` 最後 close `fileSizes` channel
    * main goroutine 計算 file sizes 總和後印出結果
* 如果能即時讓使用者知道進度會更好，但用 loop 不斷呼叫 `printDiskUsage` 畫面會被洗版
``` go
func main() {
	// ...start background goroutine...
	//!+
	// Print the results periodically.
	var tick <-chan time.Time
	if *verbose {
		tick = time.Tick(500 * time.Millisecond)
	}
	var nfiles, nbytes int64
loop:
	for {
		select {
		case size, ok := <-fileSizes:
			if !ok {
				break loop // fileSizes was closed
			}
			nfiles++
			nbytes += size
		case <-tick:
			printDiskUsage(nfiles, nbytes)
		}
	}
	printDiskUsage(nfiles, nbytes) // final totals
}
```
* 當使用者加上 `-v` flag 時候會週期性的印出結果
* main goroutine 使用 ticker 每 500ms 會產生 event
* select 會等待 file size message 或者 tick event
* `-v` flag 沒有被 set 的時候 `tick` channel 是 `nil` 所以 select 的 case 會被 disable
* 第一個 select case 會測試 `fileSizes` channel 是否已經 close
    * 如果 `fileSizes` 已經 close 就 break loop
    * label break 會跳出 select 跟 for loop
    * unlabeled break 只會跳出 select
* 使用平行處理加快處理速度
``` go
func main() {
	// ...determine roots...
	// Traverse each root of the file tree in parallel.
	fileSizes := make(chan int64)
	var n sync.WaitGroup
	for _, root := range roots {
		n.Add(1)
		go walkDir(root, &n, fileSizes)
	}
	go func() {
		n.Wait()
		close(fileSizes)
	}()
    // ...select loop...
}

func walkDir(dir string, n *sync.WaitGroup, fileSizes chan<- int64) {
	defer n.Done()
	for _, entry := range dirents(dir) {
		if entry.IsDir() {
			n.Add(1)
			subdir := filepath.Join(dir, entry.Name())
			go walkDir(subdir, n, fileSizes)
		} else {
			fileSizes <- entry.Size()
		}
	}
}
```
* 會產生上千的 goroutine 所以需要用 couting semaphore 避免同時開啟太多檔案
    ``` go
    // sema is a counting semaphore for limiting concurrency in dirents.
    var sema = make(chan struct{}, 20)

    // dirents returns the entries of directory dir.
    func dirents(dir string) []os.FileInfo {
        sema <- struct{}{}        // acquire token
        defer func() { <-sema }() // release token
        // ...
    }
    ```
### 8.9. Cancellation
* 有時候需要停止正在跑的 goroutine，例如 web server 計算途中 client 就斷線了
* goroutine 無法直接停止另外一個 goroutine 因為會使 shared variable 處在 undefined state
* 因為 goroutine 從 channel receive value 時候其他 goroutine 無法知道，所以需要有一個 broadcast 機制讓 goroutine 知道 cancellation event
* 使用 channel close 並消耗完 send value 後，receive 只會得到 zero value 的特性製作 broadcast mechanism
``` go
var done = make(chan struct{})

func cancelled() bool {
	select {
	case <-done:
		return true
	default:
		return false
	}
}
```
* 使用沒有 goroutine 會 send value 的 cancellation channel
* cancellation channel 被 close 之後就可以 receive value
* `cancelled` function 被呼叫的時候會 check cancellation state
``` go
// Cancel traversal when input is detected.
go func() {
    os.Stdin.Read(make([]byte, 1)) // read a single byte
    close(done)
}()
```
* 使用 goroutine 去讀 standard input
* 如果有 input 就 close `done` channel 通知所有 goroutine cancellation
``` go
for {
    select {
    case <-done:
        // Drain fileSizes to allow existing goroutines to finish.
        for range fileSizes {
            // Do nothing.
        }
        return
    case size, ok := <-fileSizes:
        // ...
    }
}
```
* select 增加 receive `done` channel 的 case
* cacellation 後必須將 `fileSizes` channel 消耗完確保 `walkDir` 不會因為無法 send value 到 `fileSizes` 而無法跑完
``` go
func walkDir(dir string, n *sync.WaitGroup, fileSizes chan<- int64) {
	defer n.Done()
	if cancelled() {
		return
	}
	for _, entry := range dirents(dir) {
		// ...
	}
}
```
* `walkDir` 一開始先檢查 cancellation status
* 雖然在 loop 裡面也檢查 cancellation 可以避免繼續產生 goroutine，但檢查的越精細會越需要頻繁的改動 code
* `dirents` acquire semaphore token 會有 bottleneck
``` go
func dirents(dir string) []os.FileInfo {
	select {
	case sema <- struct{}{}:
	// acquire token
	case <-done:
		return nil // cancelled
	}
	defer func() {
		<-sema
	}() // release token
	// ...read directory...
}
```
* cancellation 發生後所有 backgroud goroutine 會停止而 main fuction return
* main function return 後程式就 exit 所以難以分辨 main function 有沒有在其他人都 clean up 後才結束
* 測試的時候可以使用 panic 使 runtime dump goroutine stack

### 8.10. Example: Chat Server
* chat server 讓多個使用者可以 broadcast 訊息給彼此
* main goroutine
    ``` go
    func main() {
        listener, err := net.Listen("tcp", "localhost:8000")
        if err != nil {
            log.Fatal(err)
        }

        go broadcaster()
        for {
            conn, err := listener.Accept()
            if err != nil {
                log.Print(err)
                continue
            }
            go handleConn(conn)
        }
    }
    ```
    * listen 與 accept client network connection
    * 為每一個 connection 建立 `handleConn` goroutine
* broadcaster goroutine
    ``` go
    type client chan<- string // an outgoing message channel

    var (
        entering = make(chan client)
        leaving  = make(chan client)
        messages = make(chan string) // all incoming client messages
    )

    func broadcaster() {
        clients := make(map[client]bool) // all connected clients
        for {
            select {
            case msg := <-messages:
                // Broadcast incoming message to all
                // clients' outgoing message channels.
                for cli := range clients {
                    cli <- msg
                }

            case cli := <-entering:
                clients[cli] = true

            case cli := <-leaving:
                delete(clients, cli)
                close(cli)
            }
        }
    }
    ```
    * `clients` 紀錄目前連線的 clients
    * `entering` 跟 `leaving` 表示抵達與離開的 client，會 update `clients`
    * `message` channel 收到 message 後轉送給每個 client
* handleConn goroutine and clientWriter goroutine
    ``` go
    func handleConn(conn net.Conn) {
        ch := make(chan string) // outgoing client messages
        go clientWriter(conn, ch)

        who := conn.RemoteAddr().String()
        ch <- "You are " + who
        messages <- who + " has arrived"
        entering <- ch

        input := bufio.NewScanner(conn)
        for input.Scan() {
            messages <- who + ": " + input.Text()
        }
        // NOTE: ignoring potential errors from input.Err()

        leaving <- ch
        messages <- who + " has left"
        conn.Close()
    }

    func clientWriter(conn net.Conn, ch <-chan string) {
        for msg := range ch {
            fmt.Fprintln(conn, msg) // NOTE: ignoring network errors
        }
    }
    ```
    * `handleConn` 建立 outgoing client message channel send 給 entering 表示 client 已連線
    * 讀取 input 後加上 prefix send 給 `messages` channel
    
## Chapter 9: Concurrency with Shared Variables
### 9.1. Race Conditions
* 多個 goroutine 同時運行時很能得知 statement 執行的順序
* function 在 concurrency 的情形下也可以正確執行就稱為 concurrency-safe
* 不需要讓每個 concrete type 都 concurrency-safe 才能打造 concurrency-safe 的程式
    * goroutine 只存取 concurrency-safe 的 type
    * 單個 goroutine 存取
    * 使用 mutual exclusion
* package-level function 通常被認為是 concurreny-safe，因為 package-level variable 並無法限制在單一 goroutine 使用
* deadlock, livelock 跟 resource starvation 都可能使 function 無法再 concurrently 下正常使用
* race condition 是當程式的多個 goroutine 以不同的 operation 執行順序而產生不同結果，因為不頻繁出現或者只在特定情況下才出現所以很能重製跟分析
* date race: 兩個 goroutine 同時操作 variable 而且至少一個是做修改的操作
* 避免 data race 的方法
    * 不去 write variable
    * 多個 goruotine 不讀取同一個 variable，限制一個 goroutine 讀取一個 variable
    * 
``` go
// Package bank provides a concurrency-safe bank with one account.
package bank

var deposits = make(chan int) // send amount to deposit
var balances = make(chan int) // receive balance

func Deposit(amount int) { deposits <- amount }
func Balance() int       { return <-balances }

func teller() {
	var balance int // balance is confined to teller goroutine
	for {
		select {
		case amount := <-deposits:
			balance += amount
		case balances <- balance:
		}
	}
}

func init() {
	go teller() // start the monitor goroutine
}
```
* serial confinement: goroutine 之前用 pipeline 的方式 share variable，依序讀取 share variable

### 9.2. Mutual Exclusion: sync.Mutex
* 使用 buffered channel 當作 counting semaphore 確保一定個數的 goroutine 能執行，當限制 buffered size 為一就可以限制只有一個 goroutine 可以存取 share variable
* 只允許一個 goroutine 的 semaphore 稱為 binary semaphore
``` go
var (
	sema    = make(chan struct{}, 1) // a binary semaphore guarding balance
	balance int
)

func Deposit(amount int) {
	sema <- struct{}{} // acquire token
	balance = balance + amount
	<-sema // release token
}

func Balance() int {
	sema <- struct{}{} // acquire token
	b := balance
	<-sema // release token
	return b
}
```
* 這種方式的 mutual exclusion 很常使用所以 sync package 的 Mutex type 有直接支援
``` go
import "sync"

var (
	mu      sync.Mutex // guards balance
	balance int
)

func Deposit(amount int) {
	mu.Lock()
	balance = balance + amount
	mu.Unlock()
}

func Balance() int {
	mu.Lock()
	b := balance
	mu.Unlock()
	return b
}
```
* `Lock` acquire token
* `Unlock` release token
* 通常被 mutux 保護的變數要皆在 mutex 後面宣告
* function 裡面先去 Lock 後才去存取變數稱為 monitor
* 使用 `defer` 避免忘記 `Unlock`，function 結束時就會自動 `Unlock`
    ``` go
    func Balance() int {
        mu.Lock()
        defer mu.Unlock()
        return balance
    }
    ```
* 使用 `defer` `Unlock` 可以確保就算 critical section panic 也是會 `Unlock` 讓程式有機會 recover
``` go
// NOTE: not atomic!
func Withdraw(amount int) bool {
    Deposit(amount)
    if Balance() < 0 {
        Deposit(amount)
        return false // insufficient funds
    }
    return true
}
```
* `balance` 會暫時處於小於 0 的數字，會導致 Bob 買跑車時 Alice 卻無法買咖啡
* `Withdraw` 不是 atomic operation，而是三個 atomic operation 所組成的 (Deposit, Balance, Deposit)
``` go
// NOTE: incorrect!
func Withdraw(amount int) bool {
	mu.Lock()
	defer mu.Unlock()
	Deposit(amount)
	if Balance() < 0 {
		Deposit(amount)
		return false // insufficient funds
	}
	return true
}
```
* `Withdraw` 先 acquire mutex lock 後因為 lock 不是 re-entrant 所以 `Deposit` or `Balance` 就無法 acquire mutex lock 形成 deadlock
* golang 沒有 re-entrant lock 是因為 lock 不只有保證單一 goroutine 存取還有其他保證，但是 re-entrant lock 只保證單一 goroutine 存取並不確保其他保證
``` go
func Withdraw(amount int) bool {
	mu.Lock()
	defer mu.Unlock()
	deposit(amount)
	if balance < 0 {
		deposit(amount)
		return false // insufficient funds
	}
	return true
}

func Deposit(amount int) {
	mu.Lock()
	defer mu.Unlock()
	deposit(amount)
}
func Balance() int {
	mu.Lock()
	defer mu.Unlock()
	return balance
}

// This function requires that the lock be held.
func deposit(amount int) { balance += amount }
```
* 新增 unexported function `deposit` 預設已經 lock 完成
* 確保需要保護的變數都沒有 export

### 9.3. Read/Write Mutexes: sync.RWMutex
* 如果不斷的確認存款會導致 deposit 跟 withdraw 變慢因為 balance 會 acquire lock 導致其他 goroutine block
* 確認存款只需要 read 所以只要 deposit 跟 withdraw goroutine 沒有再跑應該要允許多個 balance goroutine 同時跑
``` go
var mu sync.RWMutex
var balance int

func Balance() int {
	mu.RLock() // readers lock
	defer mu.RUnlock()
	return balance
}
```
* 只允許多個 goroutine 進行 read-only operation，write operation 還是 exclusive access 則稱為 multiple reader, single writer lock
* `RWMutx` 最好只用在大部分的 goroutine 只需要讀取而且 lock 經常被 acquire 讓其他 gorouinte 需要定期等待 acquire lock，因為 `RWMutex` 比起一般 mutex 會更耗時間在確認 lock 的狀態

### 9.4. Memory Synchronization
* `Balance` function 需要 mutex 是因為要確保不會再類似 `Withdraw` 途中執行 `Balance`，另外就是同步不只是 goroutine 之間的執行順序還有 memory 的同步
* 現代電腦都有多個 processor 都各自的 cache 只有在需要的時候才會 flush 到 main memory，而 mutex 跟 channel 的作用就是讓 processor 去 flush cache 到 main memory

### 9.5. Lazy Initialization: sync.Once
* 將 expensive initialization 推遲到真正需要的那一刻是個不錯的實作方式
* 程式一開始就 initialize variable 可能會讓啟動變慢，還有可能根本不會用到那個 variable
``` go
var icons map[string]image.Image

func loadIcons() {
	icons = map[string]image.Image{
		"spades.png":   loadIcon("spades.png"),
		"hearts.png":   loadIcon("hearts.png"),
		"diamonds.png": loadIcon("diamonds.png"),
		"clubs.png":    loadIcon("clubs.png"),
	}
}

// NOTE: not concurrencysafe!
func Icon(name string) image.Image {
	if icons == nil {
		loadIcons() // onetime initialization
	}
	return icons[name]
}
```
* 多個 goroutine 呼叫會有問題，因為 Icon 裡面有多個步驟，確認 icons 是否為空、loadIcons、update icons
* 直覺來說最糟的情況也不過是 loadIcons 被呼叫多次但是這是錯誤的，因為少可能 memory 的問題
    ``` go
    func loadIcons() {
        icons = make(map[string]image.Image)
        icons["spades.png"] = loadIcon("spades.png")
        icons["hearts.png"] = loadIcon("hearts.png")
        icons["diamonds.png"] = loadIcon("diamonds.png")
        icons["clubs.png"] = loadIcon("clubs.png")
    }
    ```
    * 有可能 icons 先變成 empty map 後其他 goroutine 就以為已經 load 完成
``` go
var mu sync.Mutex // guards icons
var icons map[string]image.Image

// Concurrencysafe.
func Icon(name string) image.Image {
	mu.Lock()
	defer mu.Unlock()
	if icons == nil {
		loadIcons()
	}
	return icons[name]
}
```
* 雖然用 mutex 可以確保 initialize icons 成功，但是會讓以後 goruotine 無法同時讀取 Icon
``` go
var mu sync.RWMutex // guards icons
var icons map[string]image.Image

// Concurrencysafe.
func Icon(name string) image.Image {
	mu.RLock()
	if icons != nil {
		icon := icons[name]
		mu.RUnlock()
		return icon
	}
	mu.RUnlock()
	// acquire an exclusive lock
	mu.Lock()
	if icons == nil { // NOTE: must recheck for nil
		loadIcons()
	}
	icon := icons[name]
	mu.Unlock()
	return icon
}
```
* 先用 reader lock 確認 map 有沒有結果然後 release
* 假設 map 是空的就 acquire writer lock 去 loadIcons
* 雖然提供更好的 concurrency 但是 code 變很複雜
``` go
var loadIconsOnce sync.Once
var icons map[string]image.Image

// Concurrencysafe.
func Icon(name string) image.Image {
	loadIconsOnce.Do(loadIcons)
	return icons[name]
}
```
* `sync.Once` 提供 one-time initialization
* `sync.One` 包含一個 mutex 跟 boolean 表示是否 initialize 狀態
* `Do` accept initialization function
* 每一次呼叫 `Do(loadIcons)` 都會 lock muteux 跟 check boolean
    * 第一次呼叫 `Do(loadIcons)` 會呼叫 `loadIcons` 跟把 variable 設成 True
    * 之後的呼叫就不做任何事

### 9.6. The Race Detector
* `go build`, `go run` or `go test` 加上 `-race` flag 來分析 concurrency 的 bug
    * compiler 在 execution 的時候建立額外的 shared variable record 
    * compiler 紀錄 shared variable 被那些 goroutine read 跟 wrote
    * compiler 紀錄 go, channel operation 等等的資訊

### 9.7. Example: Concurrent Non-Blocking Cache
* 存下只需要計算一次就可以的 function result 
    ``` go
    func httpGetBody(url string) (interface{}, error) {
        resp, err := http.Get(url)
        if err != nil {
            return nil, err
        }
        defer resp.Body.Close()
        return ioutil.ReadAll(resp.Body)
    }
    ```
    * 呼叫這個 function 的成本很昂貴所以希望可以存下結果避免重複計算
    * `ReadAll` return `[]bytes` and `error`
``` go
// Package memo provides a concurrency-unsafe
// memoization of a function of type Func.
package memo

// A Memo caches the results of calling a Func.
type Memo struct {
	f     Func
	cache map[string]result
}

// Func is the type of the function to memoize.
type Func func(key string) (interface{}, error)

type result struct {
	value interface{}
	err   error
}

func New(f Func) *Memo {
	return &Memo{f: f, cache: make(map[string]result)}
}

// NOTE: not concurrency-safe!
func (memo *Memo) Get(key string) (interface{}, error) {
	res, ok := memo.cache[key]
	if !ok {
		res.value, res.err = memo.f(key)
		memo.cache[key] = res
	}
	return res.value, res.err
}
```
* `Memo` instance 紀錄 `f` function 跟 `cache`
    * `f` date type 是 `Func`
    * `cache` 是 mapping string 到 `result`
        * `result` 紀錄 呼叫 `f` 回傳的 value 跟 error
``` go
m := memo.New(httpGetBody)
for url := range incomingURLs() {
    start := time.Now()
    value, err := m.Get(url)
     err != nil {
        log.Print(err)
    }
    fmt.Printf("%s, %s, %d bytes\n",
    url, time.Since(start), len(value.([]byte)))
}
```
* 紀錄 request 的時間
``` go
m := memo.New(httpGetBody)
var n sync.WaitGroup
for url := range incomingURLs() {
    n.Add(1)
    go func(url string) {
        start := time.Now()
        value, err := m.Get(url)
        if err != nil {
            log.Print(err)
        }
        fmt.Printf("%s, %s, %d bytes\n",
            url, time.Since(start), len(value.([]byte)))
        n.Done()
    }(url)
}
n.Wait()
```
* HTTP request 通常都是同時一起跑
* 雖然速度加快但是有時候結果有錯，cache miss, hit cache 但是回傳結果錯誤、crash，但是有時候是正確無誤
* 使用 `-race` flag 可以找出 `Get` function 不是 concurrency-safe
``` go
type Memo struct {
	f     Func
	mu    sync.Mutex // guards cache
	cache map[string]result
}

// Get is concurrency-safe.
func (memo *Memo) Get(key string) (value interface{}, err error) {
	memo.mu.Lock()
	res, ok := memo.cache[key]
	if !ok {
		res.value, res.err = memo.f(key)
		memo.cache[key] = res
	}
	memo.mu.Unlock()
	return res.value, res.err
}
```
* 使用 monitor-based synchronization 讓 `Get` 變成 concurrency-safe
* 雖然沒有 data race 但是 `Get` 無法 parallel 執行導致速度變慢
``` go
func (memo *Memo) Get(key string) (value interface{}, err error) {
	memo.mu.Lock()
	res, ok := memo.cache[key]
	memo.mu.Unlock()
	if !ok {
		res.value, res.err = memo.f(key)

		// Between the two critical sections, several goroutines
		// may race to compute f(key) and update the map.
		memo.mu.Lock()
		memo.cache[key] = res
		memo.mu.Unlock()
	}
	return res.value, res.err
}
```
* acquire lock twice
    * lookup
    * update if lookup return nothing
* 速度變快但是如果同時去 Get 會發現 cache 沒有結果而同時去 Get 所以會 fetch URLs 兩次
``` go
type entry struct {
	res   result
	ready chan struct{} // closed when res is ready
}

func New(f Func) *Memo {
	return &Memo{f: f, cache: make(map[string]*entry)}
}

type Memo struct {
	f     Func
	mu    sync.Mutex // guards cache
	cache map[string]*entry
}

func (memo *Memo) Get(key string) (value interface{}, err error) {
	memo.mu.Lock()
	e := memo.cache[key]
	if e == nil {
		// This is the first request for this key.
		// This goroutine becomes responsible for computing
		// the value and broadcasting the ready condition.
		e = &entry{ready: make(chan struct{})}
		memo.cache[key] = e
		memo.mu.Unlock()

		e.res.value, e.res.err = memo.f(key)

		close(e.ready) // broadcast ready condition
	} else {
		// This is a repeat request for this key.
		memo.mu.Unlock()

		<-e.ready // wait for ready condition
	}
	return e.res.value, e.res.err
}
```
* duplicate suppression: 避免 redundant work
* map element 換成 `entry`
* `entry` 多了 channel `ready` broadcast 其他 goroutine 可以安全讀取結果
* 第一次讀取發現沒有 entry 就創造 entry 塞進 map 後就 release lock
* 如果有 entry 但還沒有結果就先 release lock 等到 channel ready close 後拿結果
``` go
// A request is a message requesting that the Func be applied to key.
type request struct {
	key      string
	response chan<- result // the client wants a single result
}

type Memo struct{ requests chan request }

// New returns a memoization of f.  Clients must subsequently call Close.
func New(f Func) *Memo {
	memo := &Memo{requests: make(chan request)}
	go memo.server(f)
	return memo
}

func (memo *Memo) Get(key string) (interface{}, error) {
	response := make(chan result)
	memo.requests <- request{key, response}
	res := <-response
	return res.value, res.err
}

func (memo *Memo) Close() { close(memo.requests) }
```
* `Memo` 有 `requests` channel 讓 `Get` 可以與 monitor goroutine 溝通
* `requests` channel 的 element type 是 `request`
* 呼叫 `Get` 的人 send `key` 跟 `response` 給 `requests`
``` go
func (memo *Memo) server(f Func) {
	cache := make(map[string]*entry)
	for req := range memo.requests {
		e := cache[req.key]
		if e == nil {
			// This is the first request for this key.
			e = &entry{ready: make(chan struct{})}
			cache[req.key] = e
			go e.call(f, req.key) // call f(key)
		}
		go e.deliver(req.response)
	}
}

func (e *entry) call(f Func, key string) {
	// Evaluate the function.
	e.res.value, e.res.err = f(key)
	// Broadcast the ready condition.
	close(e.ready)
}

func (e *entry) deliver(response chan<- result) {
	// Wait for the ready condition.
	<-e.ready
	// Send the result to the client.
	response <- e.res
}
```
* `cache` 只在 monitor goroutine 使用
* monitor read request from loop until channel closed
* `call` and `deliver` 必須用自己的 goroutine 確保 monitor goroutine 不會停止處理新的 rquest

### 9.8. Goroutines and Threads

#### 9.8.1. Growable Stacks
* OS thread
    * 有固定大小 block (2MB) 的 memory 在 stack 上面，stack 處理 function 呼叫暫存 local variable 的工作區
    * 2MB stack 對於工作量小的 goroutine 顯得太浪費記憶體
    * 2MB stack 對於 Go program 很常產生上千的 goroutine 顯得太小
    * 固定大小的 stack 也不利於複雜的 recursive function
* Goroutine
    * 2KB stack
    * goroutine's stack 不是固定大小，會自動增減大小

#### 9.8.2. Goroutine Scheduling
* OS threads 由 OS kernel schedule
    * 每幾個 milliseconds hardware timer interrupt the processor 讓 kernel function 呼叫 scheduler 暫停所有目前執行的 thread 並存好運行的 thread 的狀態到 memory，然後挑選下一個 thread 回復狀態並執行
    * OS threads 轉換需要 context switch，而這個 operation 特別慢
* Go runtime 有自己的 scheduler: `m:n scheduling`
    * m 個 goroutine 在 n 個 OS thread
* Go scheduler 不是定時去檢查該執行哪個 gorouinte 而是用 Go language construct
    * 如果 goroutine call `time.Sleep` 或者 block channel 或者 mutex operation 的時候 scheduler 就會讓 goroutine sleep 讓其他 goroutine 跑起來
    * 因為不需要 switch kernel context 所以 reschedule goroutine 比較快

#### 9.8.3. GOMAXPROCS
* Go scheduler 使用 `GOMAXPROCS` parameter 決定 OS thread 的數量
* 預設為 CPU 的個數
* Goroutine sleep 或 block in communication 不需要 thread
* Goroutine block in I/O 或者 system call 或者互叫 non-Go function 雖然需要 OS thread 但不算入 `GOMAXPROCS`
* 修改環境變數 `GOMAXPROCS` 或者使用 `runtime.GOMAXPROCS` function 控制 `GOMAXPROCS` 數字
``` go
for {
    go fmt.Print(0)
    fmt.Print(1)
}
$ GOMAXPROCS=1 go run hackercliché.go
111111111111111111110000000000000000000011111...
$ GOMAXPROCS=2 go run hackercliché.go
010101010101010101011001100101011010010100110...
```
* 限制同時只能跑一個 goroutine 讓 main goroutine 先不斷印出 1 後才被 Go scheduler 叫去 sleep 後喚醒印出 0 的 goroutine 不斷印出 0
* 限制同時可以跑兩個 goroutine 讓 OS thread 可以有兩個所以兩個 goroutine 就可以同時跑。

#### 9.8.4. Goroutines Have No Identity
* 許多作業系統與程式語言支援 multithreading
    * 每個 thread 有 identity 使用 interger 或 pointer 來表示
    * thread-local storage 是個 global map 紀錄每個 thread 可以存取的 value
* Goroutine 沒有類似的 identity 因為設計上認為 thread-local storage 會讓 function 需要多看 thread identity 而造成行為容易混肴
    * Go 希望影響 function 行為的變因只有 parameter，不只容易閱讀而且使用 function 在 subtask 裡面也不需要顧慮 identity

## Chapter 10: Packages and the Go Tool
* package 讓 code 可以被 reuse
* http://godoc.org 可以找到 go community 的 package
* go 提供工具方便管理 package

### 10.1. Introduction
* package system 是為了設計大型程式的時候讓有相同 feature 的東西形成一個單位，這些單位會容易被理解、改變、並且獨立於其他單位，讓大型程式容易維護
* 模組化讓 package 有機會讓 package 重複被利用
* package 都需要有於其他人不同的名字作為 identifier，名字盡量簡短清楚讓之後使用更加方便
* package 支援 encapsulation，藉由決定哪寫名稱可以被其他 package 看見
    * 隱藏 package 的 helper function 跟 type 可以讓 maintainer 改變實作方式而不會影響到外部程式
    * 隱藏內部變數可以只讓外部 package 透過 exported function 來改變這些變數確保內部的假設是正確的跟 concurrent program 的 mutual exclusion
* Go compile 很快有三個原因
    * import 都必須在 file 的開頭就列出，所以 compiler 不需要全部看過檔案
    * dependencies of package 會形成 direct acyclic graph，所以 package 分開或甚至平行 compile
    * Go compile 完的 object file 紀錄 exported 資訊，不僅是自己的還有 dependencies package，所以 compile package 就只需要看一個 object file

### 10.2. Import Paths
* package 的 identify string 就是 import path
* Import path 是出現在 package `import` declaration 裡面
* Go spec 沒有規定 import path 的意義跟如何定義 package import path 而是留給 tool 來決定
* 公開發行的 package import path 必須是 global unique，起頭必須是  internet domain name of the organization

### 10.3. The Package Declaration
* package declaration 必須出現在檔案開頭作為 package identifier
* package name 照慣例就是 import path 最後一個 segment
    * package main 是例外情形，package main 是為了讓 go build 知道需要去 invoke linker 建立 executable file
    * 結尾是 \_test package name，是 external test package 讓 go test 知道要額外多 build package，
    * 為了 dependency management 後面會加上版本像是 "gopkg.in/yaml.v2"，所以 package name 還是 yaml

### 10.4. Import Declarations
* Go source file 在 package declaration 後會 import declaration 在第一個不是 import declaration 之前
* import order 通常是字典排序 gofmt 跟 goimport 都會幫忙排好
* renaming import: import 相同 package name 需要將其中一個 rename 避免 conflict
    * rename 的 package 只影響該 file 不會影響其他 file 包含自己 package 的 file 也是不會被影響
    * rename 不定要只用在 conflict 情形下，如果想要簡化 package name 或者避免與 local variable conflict 都可以使用
    ``` go
    import (
        "crypto/rand"
        mrand "math/rand" // alternative name mrand avoids conflict
    )
    ```
* import declaration 建立 從 package 到 imported package 的 dependency，go build tool 會 report circular import

### 10.5. Blank Import
* Go 無法 import 沒有用到的 package，但是有時候我們需要 import package side effect 而不用到 package 像是 package `init` function
* 為了避免 unused import error 所以需要 renaming import 成 blank identifier 無法讓人 reference
    ``` go
    import _ "image/png" // register PNG decoder
    ```
``` go
package main

import (
	"fmt"
	"image"
	"image/jpeg"
	_ "image/png" // register PNG decoder
	"io"
	"os"
)

func main() {
	if err := toJPEG(os.Stdin, os.Stdout); err != nil {
		fmt.Fprintf(os.Stderr, "jpeg: %v\n", err)
		os.Exit(1)
	}
}

func toJPEG(in io.Reader, out io.Writer) error {
	img, kind, err := image.Decode(in)
	if err != nil {
		return err
	}
	fmt.Fprintln(os.Stderr, "Input format =", kind)
	return jpeg.Encode(out, img, &jpeg.Options{Quality: 95})
}
```
* `image` package export `Decode` 可以 read from `io.Reader` 找出 image format 使用正確的 decoder 後回傳 `image.Image`
* 少了 blank import `image/png` 還是可以正常 compile 但是無法知道輸入是 PNG 格式
    * Go standard library 提供 GIF, PNG, JPEG 多種 decoder 但為了讓 executable 小只有在特別要求時才會加入 decoder
    * `image.Decode` 使用 table 決定 supported format，table entry 有四種東西，format name, prefix string, Decode function, DecodeConfig
    * `image.RegisterFormat` 可以加入 support format
    * blank import 讓 `image.Decode` 可以 decode PNG
    ``` go
    package png // image/png
    func Decode(r io.Reader) (image.Image, error)
    func DecodeConfig(r io.Reader) (image.Config, error)
    func init() {
        const pngHeader = "\x89PNG\r\n\x1a\n"
        image.RegisterFormat("png", pngHeader, Decode, DecodeConfig)
    }
    ```

### 10.6. Packages and Naming
* package name 需要短
* package name 需要有描述性且不混淆，避免使用常用到的變數名稱
* package name 通常都是單數，但像 `bytes`, `errors`, `strings` 是為了不跟 keyword 或者 predeclared type conflict
* package name 不要使用有大家都常用的意義
    * 溫度轉換 package 一開始用 temp 會跟所有表示 temporary 的東西 conflict
    * 改成 temperature 顯得太長又不知道 package 行為
    * 最後改成 tempconv 可以跟 strconv 互相輝映
* package member 命名要跟 package name 一起思考，不需要重複 package name 已有的概念，`fmt.Println`, `bytes.Equal`, `flag.Int`, `http.Get`, `json.Marshal`
* Single-type package 代表 package 只有 export 一個 principle type 與 method 通常是 `New`，可能造成 package name 與 package member name 造成重複

### 10.7. The Go Tool
* Go tool 用來download, query, foramt, build, test, install package
* Go tool 集 package manager, build system, test driver 於一身

#### 10.7.1. Workspace Organization
* `GOPATH` 決定 workspace root，要切換不同 workspace 只要改變 `GOPATH`
* `GOPATH` 有三個 subdirectory
    * `src` 放 source code，每個 package 放在這邊，import path 就是相對於 `$GOPATH/src`
    * `pkg` 放 build tool 產生的 compile package
    * `bin` 放 excutable program
* `GOROOT` 決定 Go distribution 放 standard library 所有 package 的地方
* `go env` command 顯示重要的環境變數
    * `GOOS`: target operating system (android, linux, darwin, windows)
    * `GOARCH`: target processor architecture(amd64, 386, arm)

#### 10.7.2. Downloading Packages
* import path 不只代表 local workspace 也有需要從 internet 找到，`go get` command 會 retrieve 並且 update
* `go get` command 除了可以下載一個 package 外也可以加上 `...` notation 下載整個 repository
* `go get` 會下載所有 init package dependency package
* `go get` 下載 package 後會 build 跟 install library 跟 command
* `go get` 支援各種知名 version-control system 像是 GitHub, Bitbucket，如果使用不知名的網站需要提供 version protocol (Git or Mercurial)
* `go get` 建立完整 remote repository 而不是單純複製檔案，所以可以看 diff 或者用不同 revision
* `go get` command 加上 `-u` flag 確保之前 build 且 compiled 或的所有 package 都會被 update 到最新 version

#### 10.7.3. Building Packages
* `go build` command compile package
    * 如果 package 是 library 會把結果丟棄只確認沒有 compile error
    * 如果 package 是 main invoke linker 創造 executable 在目前的資料夾
* `go build` 後面沒有改參數預設是目前資料夾
* 如果是 main package，executable name 就是 go file 的名字
* `go run` 用在想要 build 完馬上跑的時候
* `go build` 預設 build 所有 dependency package 後丟棄只保留 excutable，雖然分析 dependency 跟 compilation 非常快但是如果 project 場的非常大的時候就會發現 recompile dependency 變慢
* `go install` 跟 `go build` 很像，差在會把 package 的 compiled code 跟 command 結果存在 `pkg` 跟 `bin` 資料夾下面
* `go install` 不會重新 compile 當 package 沒變的時候
* `go build -i` 會 install build target 的 dependency package
* compiled package 在不同平台跟架構上面都不一樣，`go install` 根據 `GOOS` 跟 `GOARCH` 存在相關資料夾底下
* 只要設好想要的 `GOOS` 跟 `GOARCH` 就可以 cross-compile Go program
    ``` go
    func main() {
        fmt.Println(runtime.GOOS, runtime.GOARCH)
    }
    ```
* package 需要 compile 不同版本在特別的 platform 或 processor 以增加效能
    * file name 含有 operating system name 或者 processor architecture name 時候，GO 只會 compile 該 file 在相對應的環境
    * build tag: 決定要不要 build 這個 file
        * `// +build linux dawwin`: 只在 Linux 或 Mac OS X 才 build
        * `// +build ignore`: 永遠不 compile 這個 file

#### 10.7.4. Documenting Packages
* Go style 鼓勵 package API 要有目的及用法的註解，package declaration 與 package member 前面都要有註解
* Go doc comment 都是完整的句子，第一句話用 function name 開頭概括，其中提到的 function parameter 都不需要特別 markup 或 quotation
* package 前面的註解被認為是整個 package 的註解
* 過長的註解會放在另外的檔案 `doc.go`
* `go doc` 印出 declaration 與 doc comment

#### 10.7.5. Internal Packages
* Unexported identifier 只在相同的 package 看的到
* exported identifier 則是其他 package 都看的到
* 有時候想要讓一部分的 package 看的到，像是把 large package 重構成可管理的部分，但又不想洩漏這些部份的 interface 給其他 package 知道
* `go build` 會將 import path 中間有 internal 當成 internal package

#### 10.7.6. Querying Packages
* `go list` 測試 package 存不存在以及 import path
* `go list` 加上 `...` argument 代表 wildcard
* `go list` 加上 `-json` argument 完整列出 package 詳細資訊

## Chapter 11: Testing
* 使用 peer review program 與 testing 管理 program 的複雜度
* Testing 通常都是只自動化的測試，藉由測試程式測試 code 是否輸出預期的結果
* Go 用 `go test` command 與寫測試程式的 convention 讓 `go test` 可以運作
* 寫測試程式與一般程式一樣，每個 function 都只負責一小塊功能，必須注意 boundary condition

### 11.1. The go test Tool
* package directory 裡面檔案名稱結尾是 \_test.go 在 go build 不會被包進去，但是 go test 會
* \*\_test.go 檔案裡面有三種類型的 function
    * tests function: 名稱是 Test 開頭，測試程式邏輯與行為，go test 回報 PASS 或 FAIL
    * benchmark function: 名稱是 Benchmark 開頭，測試 operation 的 performance，go test 回報平均的執行時間
    * example function: 名稱是 Example 開頭，提供 machine-checked documentation
* go test 掃描 \*\_test.go 的檔案，建立暫時的 main package，結束後會 clean up

### 11.2. Test Functions
* test file 必須 import testing package
* test function signature
    ``` go
    func TestName(t *testing.T) {}
    ```
    * Name 需要以大寫開頭
    * t parameter 提供 method 回報 failure 跟額外的 log 資訊
* 檢查字串是否向後讀與向前讀一樣
    ``` go
    // Package word provides utilities for word games.
    package word

    // IsPalindrome reports whether s reads the same forward and backward.
    // (Our first attempt.)
    func IsPalindrome(s string) bool {
        for i := range s {
            if s[i] != s[len(s)-1-i] {
                return false
            }
        }
        return true
    }
    ```
* IsPalindrome 測試程式
    ``` go
    package word

    import "testing"

    func TestPalindrome(t *testing.T) {
        if !IsPalindrome("detartrated") {
            t.Error(`IsPalindrome("detartrated") = false`)
        }
        if !IsPalindrome("kayak") {
            t.Error(`IsPalindrome("kayak") = false`)
        }
    }

    func TestNonPalindrome(t *testing.T) {
        if IsPalindrome("palindrome") {
            t.Error(`IsPalindrome("palindrome") = true`)
        }
    }
    ```
    * `t.Error` 回報錯誤
* go test 後面沒有接 package 就是用目前所在位置的 package
* 使用回報的錯誤作為新的 test case
    ``` go
    // The tests below are expected to fail.
    // See package gopl.io/ch11/word2 for the fix.
    func TestFrenchPalindrome(t *testing.T) {
        if !IsPalindrome("été") {
            t.Error(`IsPalindrome("été") = false`)
        }
    }

    func TestCanalPalindrome(t *testing.T) {
        input := "A man, a plan, a canal: Panama"
        if !IsPalindrome(input) {
            t.Errorf(`IsPalindrome(%q) = false`, input)
        }
    }
    ```
    * Errorf 避免過長的 input 重複寫兩遍
* go test -v 印出每個測試的執行時間與名稱
* go test -run="French|Canal" 指定 test case
* 修正使用 bytes sequence 而不是使用 rune 來做檢查與沒有忽略不是 letter 的問題
    ``` go
    // Package word provides utilities for word games.
    package word

    import "unicode"

    // IsPalindrome reports whether s reads the same forward and backward.
    // Letter case is ignored, as are non-letters.
    func IsPalindrome(s string) bool {
        var letters []rune
        for _, r := range s {
            if unicode.IsLetter(r) {
                letters = append(letters, unicode.ToLower(r))
            }
        }
        for i := range letters {
            if letters[i] != letters[len(letters)-1-i] {
                return false
            }
        }
        return true
    }
    ```
* 新增許多測試案例
    ``` go
    func TestIsPalindrome(t *testing.T) {
        var tests = []struct {
            input string
            want  bool
        }{
            {"", true},
            {"a", true},
            {"aa", true},
            {"ab", false},
            {"kayak", true},
            {"detartrated", true},
            {"A man, a plan, a canal: Panama", true},
            {"Evil I did dwell; lewd did I live.", true},
            {"Able was I ere I saw Elba", true},
            {"été", true},
            {"Et se resservir, ivresse reste.", true},
            {"palindrome", false}, // non-palindrome
            {"desserts", false},   // semi-palindrome
        }
        for _, test := range tests {
            if got := IsPalindrome(test.input); got != test.want {
                t.Errorf("IsPalindrome(%q) = %v", test.input, got)
            }
        }
    }
    ```
    * table-driven test 常見於 Go，加入新的 test case 很容易，assertion logic 不會重複，可以產出更好的 error message
    * 錯誤結果不會包含 stack trace，一發生錯誤也不會馬上停止，會把整個 test case 跑完
    * t.Fatal 可以中斷 test case 馬上停下來
    * failure message 通常都是 "f(x) = y, want z" 這種形式
        * f(x) 代表嘗試操作與 input
        * y 則是實際結果
        * z 則是期望結果
        * 測試 boolean function 就不需要 want z

#### 11.2.1. Randomized Testing
* table-driven test 使用特定的 input 來檢查 function logic 但 randomized testing 使用隨機組合的 input 讓測試涵蓋性可以變廣
* 使用 randomized testing 就無法提供預期結果，但有兩種方式可以製造出預期結果
    * 使用不同實作方法的 function，使用該 function 輸出作為預期結果
    * 根據 input 的 pattern 決定預期結果
* 由於 ramdomized testing 每次都不一樣所以保存 fail test record 非常重要，如果 input 很複雜應該只要保存 seed 就可以重新複製問題

#### 11.2.2. Testing a Command
* go test 用來測試 library packate 很好用，但也可以拿來測試 command
* echo program
    ``` go
    // Echo prints its command-line arguments.
    package main

    import (
        "flag"
        "fmt"
        "io"
        "os"
        "strings"
    )

    var (
        n = flag.Bool("n", false, "omit trailing newline")
        s = flag.String("s", " ", "separator")
    )

    var out io.Writer = os.Stdout // modified during testing

    func main() {
        flag.Parse()
        if err := echo(!*n, *s, flag.Args()); err != nil {
            fmt.Fprintf(os.Stderr, "echo: %v\n", err)
            os.Exit(1)
        }
    }

    func echo(newline bool, sep string, args []string) error {
        fmt.Fprint(out, strings.Join(args, sep))
        if newline {
            fmt.Fprintln(out)
        }
        return nil
    }
    ```
    * echo 多了參數減少 dependency
    * out global variable 讓 echo write through 而不是直接 write os.Stdout 讓測試可以寫到不同 Writer
* echo 測試程式
    ``` go
    package main

    import (
        "bytes"
        "fmt"
        "testing"
    )

    func TestEcho(t *testing.T) {
        var tests = []struct {
            newline bool
            sep     string
            args    []string
            want    string
        }{
            {true, "", []string{}, "\n"},
            {false, "", []string{}, ""},
            {true, "\t", []string{"one", "two", "three"}, "one\ttwo\tthree\n"},
            {true, ",", []string{"a", "b", "c"}, "a,b,c\n"},
            {false, ":", []string{"1", "2", "3"}, "1:2:3"},
        }

        for _, test := range tests {
            descr := fmt.Sprintf("echo(%v, %q, %q)",
                test.newline, test.sep, test.args)

            out = new(bytes.Buffer) // captured output
            if err := echo(test.newline, test.sep, test.args); err != nil {
                t.Errorf("%s failed: %v", descr, err)
                continue
            }
            got := out.(*bytes.Buffer).String()
            if got != test.want {
                t.Errorf("%s = %q, want %q", descr, got, test.want)
            }
        }
    }
    ```
    * test code 與 production code 在同個 package
    * 雖然 package 是 main 也有 main function 但是在測試 package 會忽略 main function 只會 expose TestEcho function

#### 11.2.3. White-Box Testing
* White-box test 對於 package internal working 某種程度的了解，可以 access internal function or data structure，所以可以觀察與改變原本 client 無法辦到的事情
* Black-box test 對於 package 的理解只靠 expose API 與 document
* White-box test 與 black-box test 是互補關西，black-box test 不太需要隨著 software 發展而改變，幫助測試人員專注在 client of package 與 API design 的缺陷， white-box 可以對於細節做測試提高 coverage
* TestIsPalindrome 只有呼叫 exported function 所以是 black-box test
* Testecho 呼叫 unexported function echo 跟改變 global variable out 所以是 white-box test
* 替換某些 production code 讓 code 變得容易測試
* quota-checking logic，大於 90% 用量就寄信通知用戶
    ``` go
    package storage

    import (
        "fmt"
        "log"
        "net/smtp"
    )

    var usage = make(map[string]int64)

    func bytesInUse(username string) int64 { return usage[username] }

    // Email sender configuration.
    // NOTE: never put passwords in source code!
    const sender = "notifications@example.com"
    const password = "correcthorsebatterystaple"
    const hostname = "smtp.example.com"

    const template = `Warning: you are using %d bytes of storage,
    %d%% of your quota.`

    func CheckQuota(username string) {
        used := bytesInUse(username)
        const quota = 1000000000 // 1GB
        percent := 100 * used / quota
        if percent < 90 {
            return // OK
        }
        msg := fmt.Sprintf(template, used, percent)
        auth := smtp.PlainAuth("", sender, password, hostname)
        err := smtp.SendMail(hostname+":587", auth, sender,
            []string{username}, []byte(msg))
        if err != nil {
            log.Printf("smtp.SendMail(%s) failed: %s", username, err)
        }
    }
    ```
* 測試的時候不想要真的寄信，所以把寄信的 function 獨立出來存在一個 unexported variable
    ``` go
    var notifyUser = func(username, msg string) {
        auth := smtp.PlainAuth("", sender, password, hostname)
        err := smtp.SendMail(hostname+":587", auth, sender,
            []string{username}, []byte(msg))
        if err != nil {
            log.Printf("smtp.SendMail(%s) failed: %s", username, err)
        }
    }

    func CheckQuota(username string) {
        used := bytesInUse(username)
        const quota = 1000000000 // 1GB
        percent := 100 * used / quota
        if percent < 90 {
            return // OK
        }
        msg := fmt.Sprintf(template, used, percent)
        notifyUser(username, msg)
    }
    ```
* 不實際送信的測試
    ``` go
    package storage

    import (
        "strings"
        "testing"
    )

    func TestCheckQuotaNotifiesUser(t *testing.T) {
        var notifiedUser, notifiedMsg string
        notifyUser = func(user, msg string) {
            notifiedUser, notifiedMsg = user, msg
        }

        const user = "joe@example.org"
        usage[user] = 980000000 // simulate a 980MB-used condition

        CheckQuota(user)
        if notifiedUser == "" && notifiedMsg == "" {
            t.Fatalf("notifyUser not called")
        }
        if notifiedUser != user {
            t.Errorf("wrong user (%s) notified, want %s",
                notifiedUser, user)
        }
        const wantSubstring = "98% of your quota"
        if !strings.Contains(notifiedMsg, wantSubstring) {
            t.Errorf("unexpected notification message <<%s>>, "+
                "want substring %q", notifiedMsg, wantSubstring)
        }
    }
    ```
    * 下個程式的 CheckQuota 的 notifyUser 還是被換過的，所以需要使用 defer 將改過的 global variable 在 test function 結束將原本的值換回來

#### 11.2.4. External Test Packages
* 測試 low-level package 可能會需要 import high-level package，但是 Go 禁止 cycle import
* external test package 可以解決測試程式造成 cycle import 的問題
    * 例如在 net/url package 的檔案使用 package url_test 來暗示 go test 需要將這些檔案多新增一個 package 只用來跑 test
    * external test package 邏輯上是比其他 package 更高 level 所以不會有 cycle import 的問題
* go list 可以列出 package 裡的檔案哪些是 production code, in-package tests, external tests
    * production code
    ```
    $ go list f={{.GoFiles}} fmt
    [doc.go format.go print.go scan.go]
    ```
    * in-package tests
    ```
    $ go list f={{.TestGoFiles}} fmt
    [export_test.go]
    ```
    * external tests
    ```
    $ go list f={{.XTestGoFiles}} fmt
    [fmt_test.go scan_test.go stringer_test.go]
    ```
* 有時候 external test package 需要讀取 internal package，我們可以在 in-package test 宣告將 external 需要用到的 internal expose，通常檔案名稱都是 export_test.go
    * fmt package 需要用到 Unicode.IsSpace 當作 fmt.Scanf 的一部分
    * 為了不造成 cycle import 所以 fmt 不能 import unicode package 跟龐大的 table 要使用簡單一點的 isSpace function
    * external package 無法使用 isSpace 所以 fmt 開個 back door 在 export_test.go 宣告 exported variable 其實就是 isSpace
    ``` go
    package fmt
    
    var IsSpace = isSpace
    ```
    * export_test.go 單純宣告 exported symbol fmt.IsSpace 只給 external test 使用
    * export_test.go 不包含任何測試

#### 11.2.5. Writing Effective Tests
* 其他語言可能提供很多關於測試的功能但是 failure message 卻時常令人費解
* 好的 test 需要再失敗的時候清楚描述問題，最理想情況維護者可以不需要看 source code 才知道 fail message 是什麼意思
* 測試不應該失敗一次就停止，應該跑完整個 test 找出多個錯誤，因為錯誤或許有相似的 pattern 可以提供線索

#### 11.2.6. Avoiding Brittle Tests
* 新的但是合法的輸入導致程式失敗稱為 buggy
* 程式改動後導致程式失敗稱為 brittle
* brittle test 每次程式改動都會失敗也稱為 change detector 或 status quo tests
* 花在處理 brittle test 的時間所帶來的壞處會大於 test 的好處
* 最簡單避免 brittle test 的方式就是只檢查重要的 properties，檢查 substring 而不是要求整個 string 都要符合

### 11.3. Coverage
* test 顯示已存在的 bug 而不是未發現的 bug，任何 test 都無法證明程式沒有任何錯誤，最多保證在重要的情況下都可以正常運作
* test coverage 顯示程式的被測試的程度，可以指引我們在有用的地方進行測試
* statement coverage 是指至少有跑過一次的 statement 與全部 statement 的比率
* -coverprofile flag: 每個 statement 都有 boolean 跑過就會變成 true
* -covermode=count flag: 每個 statement 都會算跑過幾次，就可以看出測試的熱點

### 11.4. Benchmark Functions
* benchmark 是指在固定 workload 上面測試程式的效能
* Go benchmark function 在前面加上 Benchmark 跟用 \*testing.B
``` go
import "testing"
func BenchmarkIsPalindrome(b *testing.B) {
    for i := 0; i < b.N; i++ {
        IsPalindrome("A man, a plan, a canal: Panama")
    }
}
```
* go test -bench=.
    * -bench flag 選擇 benchmark 去跑
    * 一開始先做點測試來決定到底要跑多少次
* benchmark 除了用來得知 operation 的速度外，也常常需要用來比較不同設定的速度差異用來找出最合適的設定

### 11.5. Profiling
* profiling 可以找出程式中 critical code 的部分來增加效能
* CPU profile 找出哪個 function 用了最多 CPU time
* heap profile 找出哪個 statement 用了做多 memory
* blocking profile 找出哪個 operation 讓 goroutine block 最久
* 盡量不使用一種以上的 profile，因為有可能互相混亂
* go tool pprof 用來分析 profile
* profile 為了節省空間及增加效率不會包含 function name 只會有 function address，所以 pprof 需要executable

### 11.6. Example Functions
* 開頭是 Example 沒有 parameter 跟 result
* example 是為了 documentation，一個好的例子可以很容易解釋 library 的行為
* example 是會被 go test 執行，如果 example 最後有 // Output: comment，test drive 就會執行並確認結果
* example 會被 godoc server 變成可以讓使用者在 go playground 修改做測試，這是最快的方式讓人知道 function 或語言的特性

## Chapter 12: Reflection
* Go 提供可以在 run time 時候更新、查看 value，呼叫 method 而不需要知道型態稱為 reflection
* Reflection 可以把 types 當成 first-class value

### 12.1. Why Reflection?
* 有時候需要寫一個處理通用的 value of types 的 function 但是不滿足於單一 interface，或不知道 representation
    * `fmt.Fprintf` 印出任何型態的 value
    * 使用 type switch 來判斷型態後印出值，但是型態是無限的，所以需要 reflection 來解決問題

### 12.2. reflect.Type and reflect.Value
* Reflection 在 Go package reflect
* Type represent a Go type
* Type 是個 interface 有很多 method 用來判斷 types 跟 inspect component
* `reflect.TypeOf` 接受任何 interface{} 然後 return dynamic type
    ``` go
    t := reflect.TypeOf(3) // a reflect.Type
    fmt.Println(t.String()) // "int"
    fmt.Println(t) // "int"
    ```
    * `Type(3)` assign value 3 到 interface{}，進行隱性轉換
    * interface value 的 dynamic type 是 int 跟 dynamic value 是 3
* `reflect.TypeOf` return interface value dynamic type 所以都會 return concrete type 同時也是 reflect.Type
    ``` go
    var w io.Writer = os.Stdout
    fmt.Println(reflect.TypeOf(w)) // "*os.File"
    ```
* `reflect.Type` 滿足 `fmt.Stringer` 因為印出 interface value 的 dynamic type 對於 debugging 跟 logging 很有用，`fmt.Printf` 的 %T 內部是使用 `reflect.Type`
* `reflect.ValueOf` 接受任何 interface{} 然後 return `reflect.Value` 含有 dynamic value
* `reflect.ValueOf` 回傳的都是 concrete 但是 `reflect.Value` 含有 interface value
    ``` go
    v := reflect.ValueOf(3) // a reflect.Value
    fmt.Println(v) // "3"
    fmt.Printf("%v\n", v) // "3"
    fmt.Println(v.String()) // NOTE: "<int Value>"
    ```
    * `reflect.Value` 也滿足 `fmt.Stringer`，但除非 value 是 string 才會印出不然只會印出 type，`%v` 會讓 fmt 特殊對待 `reflect.Value`
    * Type method 回傳 type
        ```
        t := v.Type() // a reflect.Type
        fmt.Println(t.String()) // "int"
        ```
* 與 `reflect.ValueOf` 相反操作是 `reflect.Value.Interface` 會回傳有著相同 concrete value 的 Interface{}
    ``` go
    v := reflect.ValueOf(3) // a reflect.Value
    x := v.Interface() // an interface{}
    i := x.(int) // an int
    fmt.Printf("%d\n", i) // "3"
    ```
* `reflect.Value` 與 Interface{} 有相同的 value 但是 empty interface 隱藏 representation 與相關 operation 並且沒有 expose 任何 method，所以除非知道 dynamic type 跟 type assertion 才能知道怎麼使用，Value 有很多 method 可以在不知道 type 的情況去看內容。
* 使用 `reflect.Value` 的 `Kind` method 區分 case，雖然型態是無限的但是種類是有限的
    ``` go
    package format

    import (
        "reflect"
        "strconv"
    )

    // Any formats any value as a string.
    func Any(value interface{}) string {
        return formatAtom(reflect.ValueOf(value))
    }

    // formatAtom formats a value without inspecting its internal structure.
    func formatAtom(v reflect.Value) string {
        switch v.Kind() {
        case reflect.Invalid:
            return "invalid"
        case reflect.Int, reflect.Int8, reflect.Int16,
            reflect.Int32, reflect.Int64:
            return strconv.FormatInt(v.Int(), 10)
        case reflect.Uint, reflect.Uint8, reflect.Uint16,
            reflect.Uint32, reflect.Uint64, reflect.Uintptr:
            return strconv.FormatUint(v.Uint(), 10)
        // ...floating-point and complex cases omitted for brevity...
        case reflect.Bool:
            return strconv.FormatBool(v.Bool())
        case reflect.String:
            return strconv.Quote(v.String())
        case reflect.Chan, reflect.Func, reflect.Ptr, reflect.Slice, reflect.Map:
            return v.Type().String() + " 0x" +
                strconv.FormatUint(uint64(v.Pointer()), 16)
        default: // reflect.Array, reflect.Struct, reflect.Interface
            return v.Type().String() + " value"
        }
    }
    ```
    * 對於 aggregate type(array, struct, interface) 只有印出 type 的 value
    * 對於 refernece type 只會印出 reference type 跟 reference address in hexidecimal
        ``` go
        var x int64 = 1
        var d time.Duration = 1 * time.Nanosecond
        fmt.Println(format.Any(x)) // "1"
        fmt.Println(format.Any(d)) // "1"
        fmt.Println(format.Any([]int64{x})) // "[]int64 0x8202b87b0"
        fmt.Println(format.Any([]time.Duration{d})) // "[]time.Duration 0x8202b87e0"
        ```
### 12.3. Display, a Recursive Value Printer
* 改善 display composite type 為了不想直接抄 `fmt.Sprint` 所以做一個 `Display` function 可以 debugging，給一個 complex value `x`，印出完整的 structure 標示每個 element
    ``` go
    e, _ := eval.Parse("sqrt(A / pi)")
    Display("e", e)
    ```
    ``` go
    Display e (eval.call):
    e.fn = "sqrt"
    e.args[0].type = eval.binary
    e.args[0].value.op = 47
    e.args[0].value.x.type = eval.Var
    e.args[0].value.x.value = "A"
    e.args[0].value.y.type = eval.Var
    e.args[0].value.y.value = "pi"
    ```
* 不應該 expose reflection 在 package 的 API
* 定義 unexported 的 display function 做 recursion 然後 Display 只是個 wrapper 接受 interface{} parameter
    ``` go
    func Display(name string, x interface{}) {
        fmt.Printf("Display %s (%T):\n", name, x)
        display(name, reflect.ValueOf(x))
    }
    ```
* `display` 裡面用 `formatAtom` 印出 elementary value 然後用 `reflect.Value` 去遞迴每個 component
    ``` go
    func display(path string, v reflect.Value) {
        switch v.Kind() {
        case reflect.Invalid:
            fmt.Printf("%s = invalid\n", path)
        case reflect.Slice, reflect.Array:
            for i := 0; i < v.Len(); i++ {
                display(fmt.Sprintf("%s[%d]", path, i), v.Index(i))
            }
        case reflect.Struct:
            for i := 0; i < v.NumField(); i++ {
                fieldPath := fmt.Sprintf("%s.%s", path, v.Type().Field(i).Name)
                display(fieldPath, v.Field(i))
            }
        case reflect.Map:
            for _, key := range v.MapKeys() {
                display(fmt.Sprintf("%s[%s]", path,
                    formatAtom(key)), v.MapIndex(key))
            }
        case reflect.Ptr:
            if v.IsNil() {
                fmt.Printf("%s = nil\n", path)
            } else {
                display(fmt.Sprintf("(*%s)", path), v.Elem())
            }
        case reflect.Interface:
            if v.IsNil() {
                fmt.Printf("%s = nil\n", path)
            } else {
                fmt.Printf("%s.type = %s\n", path, v.Elem().Type())
                display(path+".value", v.Elem())
            }
        default: // basic types, channels, funcs
            fmt.Printf("%s = %s\n", path, formatAtom(v))
        }
    }
    ```
    * Slices and arrays: `Len` 得知有幾個 element，`Index(i)` 拿取第 i 個 element
    * Structs: `NameField` 得知有幾個 field，`Field(i)` 得知第 i 個 field 的值，`v.Type().Field(i).Name` 知道 field 的名字
    * Maps: `MapKeys` 回傳 slice of reflect.Value，`MapIndex(key)` 回傳 key 的 value
    * Pointers: `Elem` 回傳 pointer 指向的 value，雖然遇到 nil 也使可以處理好但是 display 會當成 invalid，為了更精確的顯示所以明確處理 pointer 是 nil 的情況
    * Interfaces: 跟 pointer 一樣先檢查是否為 nil，再來印出 dynamic value 的 type 跟 value

### 12.4. Example: Encoding S-Expressions
* `Display` 顯示 structured data 但是不夠短到可以 encode 或 marshal Go object 在 inter-process communication
* Go standard library 支援 JSON, XML and ASN.1 但是沒有支援 S-expressions 因為沒有廣泛接受的定義
* Encode type of Go
    * Nil -> nil
    * Array and slices -> list notation
    * Struct -> list of field binding, 每個 field binding 是兩個 element 的 list，第一個是 field name，第二個是 field value
    * Map -> list of pairs, 每個 pair 有 key 跟 value
* Encoding 遞迴 encode function 來完成
    ``` go
    func encode(buf *bytes.Buffer, v reflect.Value) error {
        switch v.Kind() {
        case reflect.Invalid:
            buf.WriteString("nil")

        case reflect.Int, reflect.Int8, reflect.Int16,
            reflect.Int32, reflect.Int64:
            fmt.Fprintf(buf, "%d", v.Int())

        case reflect.Uint, reflect.Uint8, reflect.Uint16,
            reflect.Uint32, reflect.Uint64, reflect.Uintptr:
            fmt.Fprintf(buf, "%d", v.Uint())

        case reflect.String:
            fmt.Fprintf(buf, "%q", v.String())

        case reflect.Ptr:
            return encode(buf, v.Elem())

        case reflect.Array, reflect.Slice: // (value ...)
            buf.WriteByte('(')
            for i := 0; i < v.Len(); i++ {
                if i > 0 {
                    buf.WriteByte(' ')
                }
                if err := encode(buf, v.Index(i)); err != nil {
                    return err
                }
            }
            buf.WriteByte(')')

        case reflect.Struct: // ((name value) ...)
            buf.WriteByte('(')
            for i := 0; i < v.NumField(); i++ {
                if i > 0 {
                    buf.WriteByte(' ')
                }
                fmt.Fprintf(buf, "(%s ", v.Type().Field(i).Name)
                if err := encode(buf, v.Field(i)); err != nil {
                    return err
                }
                buf.WriteByte(')')
            }
            buf.WriteByte(')')

        case reflect.Map: // ((key value) ...)
            buf.WriteByte('(')
            for i, key := range v.MapKeys() {
                if i > 0 {
                    buf.WriteByte(' ')
                }
                buf.WriteByte('(')
                if err := encode(buf, key); err != nil {
                    return err
                }
                buf.WriteByte(' ')
                if err := encode(buf, v.MapIndex(key)); err != nil {
                    return err
                }
                buf.WriteByte(')')
            }
            buf.WriteByte(')')

        default: // float, complex, bool, chan, func, interface
            return fmt.Errorf("unsupported type: %s", v.Type())
        }
        return nil
    }
    ```
* Marshal function wrap the encoder in API
    ``` go
    // Marshal encodes a Go value in S-expression form.
    func Marshal(v interface{}) ([]byte, error) {
        var buf bytes.Buffer
        if err := encode(&buf, reflect.ValueOf(v)); err != nil {
            return nil, err
        }
        return buf.Bytes(), nil
    }
    ```

### 12.5. Setting Variables with reflect.Value
* variable 是 addressable storage location 含有值則可以透過 address 更新值
    ``` go
    x := 2 // value type variable?
    a := reflect.ValueOf(2) //2 int no
    b := reflect.ValueOf(x) //2 int no
    c := reflect.ValueOf(&x) // &x *int no
    d := c.Elem() //2 int yes (x)
    ```
    * a 無法取得 address 只是複製 integer 2，b 也是一樣
    * c 無法取得 address 只是複製 &x
    * 所有 `reflect.ValueOf(x)` 回傳的 `reflect.Value` 都是無法取 address
    * d dereference c 就可以得到 address
    * `reflect.ValueOf(&x).Elem()` 就可以得到 addressable value
* `reflect.Value` 的 `CanAddr` method 可以判斷是否 addressable
* 從 addressable `reflect.Value` 還原 variable 需要三步驟
    ``` go
    x := 2
    d := reflect.ValueOf(&x).Elem() // d refers to the variable x
    px := d.Addr().Interface().(*int) // px := &x
    *px = 3 // x = 3
    fmt.Println(x) // "3"
    ```
    1. 呼叫 `Addr()` 回傳 pointer
    2. 呼叫 `Interface()` 回傳包含 pointer 的 interfaceP{}
    3. 使用 type assertion 取的 interface 的 content 當成原本的 pointer
* 直接使用 `reflect.Value.Set` method 更新
    ``` go
    d.Set(reflect.ValueOf(4))
    fmt.Println(x) // "4"
    ```
    * run time 會確認是否可以 assign，如果不行就 panic
        ``` go
        d.Set(reflect.ValueOf(int64(5))) // panic: int64 is not assignable to int
        ```
    * 無法 address 也會 panic
        ``` go
        x := 2
        b := reflect.ValueOf(x)
        b.Set(reflect.ValueOf(3)) // panic: Set using unaddressable value
        ```
* `Set` method 有為基本型態做特化的 method: `SetInt`, `SetUint`, `SetString`, `SetFloat` 等等
    ``` go
    d := reflect.ValueOf(&x).Elem()
    d.SetInt(3)
    fmt.Println(x) // "3"
    ```
* `CanSet` 判斷是否可以更新 `reflect.Value`

### 12.6. Example: Decoding S-Expressions
* `lexer` 用 `text/scanner` package 的 `Scanner` type 分割 input stream 成為多個 token
    ``` go
    type lexer struct {
        scan  scanner.Scanner
        token rune // the current token
    }

    func (lex *lexer) next()        { lex.token = lex.scan.Scan() }
    func (lex *lexer) text() string { return lex.scan.TokenText() }

    func (lex *lexer) consume(want rune) {
        if lex.token != want { // NOTE: Not an example of good error handling.
            panic(fmt.Sprintf("got %q, want %q", lex.text(), want))
        }
        lex.next()
    }
    ```
* parser 有兩個重要 function 一個是 `read`
    ``` go
    func read(lex *lexer, v reflect.Value) {
        switch lex.token {
        case scanner.Ident:
            // The only valid identifiers are
            // "nil" and struct field names.
            if lex.text() == "nil" {
                v.Set(reflect.Zero(v.Type()))
                lex.next()
                return
            }
        case scanner.String:
            s, _ := strconv.Unquote(lex.text()) // NOTE: ignoring errors
            v.SetString(s)
            lex.next()
            return
        case scanner.Int:
            i, _ := strconv.Atoi(lex.text()) // NOTE: ignoring errors
            v.SetInt(int64(i))
            lex.next()
            return
        case '(':
            lex.next()
            readList(lex, v)
            lex.next() // consume ')'
            return
        }
        panic(fmt.Sprintf("unexpected token %q", lex.text()))
    }
    ```
    * S-expression 使用 identifier 為了兩個不同的目的
        * struct field name
        * nil pointer，把 value 使用 `reflect.Zero` 設成 zero value
    * `readList` 看到 ( token 代表接下來處理 list 有可能是 map, struct, slice, array，最後用 `endList` 判斷是否結束了
    * 在遇到 ')' token 之前都會不斷遞迴呼叫 `read`
``` go
func readList(lex *lexer, v reflect.Value) {
    switch v.Kind() {
    case reflect.Array: // (item ...)
        for i := 0; !endList(lex); i++ {
            read(lex, v.Index(i))
        }

    case reflect.Slice: // (item ...)
        for !endList(lex) {
            item := reflect.New(v.Type().Elem()).Elem()
            read(lex, item)
            v.Set(reflect.Append(v, item))
        }

    case reflect.Struct: // ((name value) ...)
        for !endList(lex) {
            lex.consume('(')
            if lex.token != scanner.Ident {
                panic(fmt.Sprintf("got token %q, want field name", lex.text()))
            }
            name := lex.text()
            lex.next()
            read(lex, v.FieldByName(name))
            lex.consume(')')
        }

    case reflect.Map: // ((key value) ...)
        v.Set(reflect.MakeMap(v.Type()))
        for !endList(lex) {
            lex.consume('(')
            key := reflect.New(v.Type().Key()).Elem()
            read(lex, key)
            value := reflect.New(v.Type().Elem()).Elem()
            read(lex, value)
            v.SetMapIndex(key, value)
            lex.consume(')')
        }

    default:
        panic(fmt.Sprintf("cannot decode list into %v", v.Type()))
    }
}

func endList(lex *lexer) bool {
    switch lex.token {
    case scanner.EOF:
        panic("end of file")
    case ')':
        return true
    }
    return false
}
```
* Exported function `Unmarshal` wrap parser
    ``` go
    // Unmarshal parses S-expression data and populates the variable
    // whose address is in the non-nil pointer out.
    func Unmarshal(data []byte, out interface{}) (err error) {
        lex := &lexer{scan: scanner.Scanner{Mode: scanner.GoTokens}}
        lex.scan.Init(bytes.NewReader(data))
        lex.next() // get the first token
        defer func() {
            // NOTE: this is not an example of ideal error handling.
            if x := recover(); x != nil {
                err = fmt.Errorf("error at %s: %v", lex.scan.Position, x)
            }
        }()
        read(lex, reflect.ValueOf(out).Elem())
        return nil
    }
    ```

### 12.7. Accessing Struct Field Tags
* struct field tag 修改 JSON encode Go struct value
    * json field tag 可以換成不同的 field name 跟避免 empty field
* HTTP handler 的 search function
    ``` go
    import "gopl.io/ch12/params"

    // search implements the /search URL endpoint.
    func search(resp http.ResponseWriter, req *http.Request) {
        var data struct {
            Labels     []string `http:"l"`
            MaxResults int      `http:"max"`
            Exact      bool     `http:"x"`
        }
        data.MaxResults = 10 // set default
        if err := params.Unpack(req, &data); err != nil {
            http.Error(resp, err.Error(), http.StatusBadRequest) // 400
            return
        }

        // ...rest of handler...
        fmt.Fprintf(resp, "Search: %+v\n", data)
    }
    ```
    * data 是個沒有名字的 struct， field 跟 HTTP request parameter 有關聯
* Unpack function
    ``` go
    // Unpack populates the fields of the struct pointed to by ptr
    // from the HTTP request parameters in req.
    func Unpack(req *http.Request, ptr interface{}) error {
        if err := req.ParseForm(); err != nil {
            return err
        }

        // Build map of fields keyed by effective name.
        fields := make(map[string]reflect.Value)
        v := reflect.ValueOf(ptr).Elem() // the struct variable
        for i := 0; i < v.NumField(); i++ {
            fieldInfo := v.Type().Field(i) // a reflect.StructField
            tag := fieldInfo.Tag           // a reflect.StructTag
            name := tag.Get("http")
            if name == "" {
                name = strings.ToLower(fieldInfo.Name)
            }
            fields[name] = v.Field(i)
        }

        // Update struct field for each parameter in the request.
        for name, values := range req.Form {
            f := fields[name]
            if !f.IsValid() {
                continue // ignore unrecognized HTTP parameters
            }
            for _, value := range values {
                if f.Kind() == reflect.Slice {
                    elem := reflect.New(f.Type().Elem()).Elem()
                    if err := populate(elem, value); err != nil {
                        return fmt.Errorf("%s: %v", name, err)
                    }
                    f.Set(reflect.Append(f, elem))
                } else {
                    if err := populate(f, value); err != nil {
                        return fmt.Errorf("%s: %v", name, err)
                    }
                }
            }
        }
        return nil
    }
    ```
    * 呼叫 `req.ParseForm()` parse request 讓 `req.Form` 有所有的 parameter
    * `Unpack` 建立 effective name 與 field name 的 mapping
    * `Unpack` iterate 整個 HTTP request 然後 update struct field
* `populate` 負責設好 single field
    ``` go
    func populate(v reflect.Value, value string) error {
        switch v.Kind() {
        case reflect.String:
            v.SetString(value)

        case reflect.Int:
            i, err := strconv.ParseInt(value, 10, 64)
            if err != nil {
                return err
            }
            v.SetInt(i)

        case reflect.Bool:
            b, err := strconv.ParseBool(value)
            if err != nil {
                return err
            }
            v.SetBool(b)

        default:
            return fmt.Errorf("unsupported kind %s", v.Type())
        }
        return nil
    }
    ```

### 12.8. Displaying the Methods of a Type
* 使用 `reflect.Type` 印出 type 的 value 與 method
    ``` go
    // Print prints the method set of the value x.
    func Print(x interface{}) {
        v := reflect.ValueOf(x)
        t := v.Type()
        fmt.Printf("type %s\n", t)

        for i := 0; i < v.NumMethod(); i++ {
            methType := v.Method(i).Type()
            fmt.Printf("func (%s) %s%s\n", t, t.Method(i).Name,
                strings.TrimPrefix(methType.String(), "func"))
        }
    }
    ```
    * `reflect.Type` 跟 `reflect.Value` 都有 `Method` 
    * 呼叫 `t.Method(i)` 回傳一個 `reflect.Method` instance 描述 method 的 name 跟 type
    * 呼叫 `v.Method(i)` 回傳 `reflect.Value` 的 method value

### 12.9. A Word of Caution
* 使用 reflect 的 code 很脆弱，每個錯誤都會讓 compiler 噴 type error，reflection error 只會在執行的時候發現
    * 使用 reflection 修改變數需要特別注意 addressability 跟 settability
    * 避免的方法是確保只在自己的 package 使用 reflection，不要使用 `reflect.Value` 當作 parameter 在 API，如果無法做到就要在做危險的操作之前先檢查
    * reflection 減少安全性跟 automated refactoring 的精確度與分析工具的精確度，因為無法藉由 type information 來判斷
* type 是一種 document 的用處，reflection 的 operation 無法使用 static type checking，使用太多 reflect 的 code 也不容易讀，所以使用 reflect 時候需要清楚標示期望的 type
* 使用 reflect 的 code 會比一般的 function 慢

## Chapter 13: Low Level Programming
* Go 設計時有許多原則避免 Go 壞掉，compilation 時候會檢查 operation 是否型態，避免直接存取內部型態像是 strings, maps, slices, channels
* Go 對於無法靜態找出錯誤的時候讓程式馬上停止並提供錯誤訊息
* Go 自動記憶體管理避免 use after free bug 跟 memory leak
* 以上這些特性讓 Go 成為可預測性跟少些神秘感的程式
* 有時候為了 performance 會選擇捨棄這些特性
* `unsafe` package 讓我們不用遵從這些特性
* `cgo` tool 建立 C library 跟 operating system call 與 Go 的 binding
* `unsafe` package 提供存取 built-in language feature，但這些不會再正常情況使用因為 expose Go memory layout
* `unsafe` package 通常使用在與 operating system 頻繁互動的 package 像是 runtime, os, syscall, net

### 13.1. unsafe.Sizeof, Alignof, and Offsetof
* `unsafe.Sizeof` 回報 operand 使用幾個 bytes
    * 只會回報固定部分的 data structure 例如 pointer 或者字串長度
    * 不同平台的 word 會不一樣，4 bytes 在 32-bit, 8 bytes 在 64-bit
* 電腦從記憶體讀取值的時候只要都有適當對齊就最有效率
    * 不同 size 的型態的 address 都需要是各自 size 的倍數
    * 對於 aggregate type 會有一些 hole 來對齊所以會占用比實際需要大的空間
* 沒有規定宣告 field 的順序要與在記憶體的一致所以 compiler 可以重新排列
    * 對於 struct 中 field 有不同 size 排序方式會造成不同空間使用量
    * 不需要太擔心 struct 的對齊方式但有效率的 pack struct 可以讓配置 data struct 更快速
* `unsafe.Alignof` 回報需要對齊的大小，通常 boolean 跟 numeric type 會對齊到 size 而其他都是 word-aligned
* `unsafe.Offsetof` 回報 field 與 struct 起點的距離

### 13.2. unsafe.Pointer
* `unsafe.Pointer` 是一種特殊的 pointer 可以存任何變數的 address
* 一般的 pointer 可以轉換成 `unsafe.Pointer` 也可以轉換回來
* `unsafe.Pointer` 讓我們能夠直接改動記憶體的值但是會顛覆 type system
* 很多 `unsafe.Pointer` 都是當成中間人把原本的 pointer 轉成 raw numeric address 接著再轉回來
    ``` go
    var x struct {
        a bool
        b int16
        c []int
    }

    // equivalent to pb := &x.b
    pb := (*int16)(unsafe.Pointer(
        uintptr(unsafe.Pointer(&x)) + unsafe.Offsetof(x.b)))
    *pb = 42

    fmt.Println(x.b) // "42"
    ```
* 不要把 uintptr 先暫存下來
    ``` go
    // NOTE: subtly incorrect!
    tmp := uintptr(unsafe.Pointer(&x)) + unsafe.Offsetof(x.b)
    pb := (*int16)(unsafe.Pointer(tmp))
    *pb = 42
    ```
    * 有些 garbage collector 會移動變數在記憶體的位置稱為 moving GCs
    * 當變數移動的時候需要更新所有指向這個變數的 pointer
    * `unsafe.Pointer` 是個 pointer 所以會更新但是 uintptr 只是個數字
    * 所以如果把 pointer 數字藏在 tmp 底下就不知道要更新這個數字了
    * 最後也就會把不該改的記憶體位置寫 42 下去
* 最保險方式是把所有 uintptr 的值當成舊的 address，減少轉換過程的操作


### 13.3. Example: Deep Equivalence
* `DeepEqual` 在 `reflect` package 裡面判斷兩個 value 是否相等
    * 比較 basic type 就用內建的 == operator
    * 比較 composite type 會遞迴的比較每個 element
    * 如果無法使用內建的 == operator 還是可以比較
    * 通常使用在 test 中
    * 沒有考慮 nil map 等於 non-nil empty map 跟 nil slice 等於 non-nil empty slice 的情況
* 定義 `Equal` function 可以比較任意 value 跟 `DeepEqual` 但另外會考慮 nil slice(map) 跟 non-nil empty slice(map) 的情況
    ``` go
    func equal(x, y reflect.Value, seen map[comparison]bool) bool {
        if !x.IsValid() || !y.IsValid() {
            return x.IsValid() == y.IsValid()
        }
        if x.Type() != y.Type() {
            return false
        }

        // ...cycle check omitted (shown later)...
        switch x.Kind() {
        case reflect.Bool:
            return x.Bool() == y.Bool()

        case reflect.String:
            return x.String() == y.String()

        // ...numeric cases omitted for brevity...

        case reflect.Chan, reflect.UnsafePointer, reflect.Func:
            return x.Pointer() == y.Pointer()

        case reflect.Ptr, reflect.Interface:
            return equal(x.Elem(), y.Elem(), seen)

        case reflect.Array, reflect.Slice:
            if x.Len() != y.Len() {
                return false
            }
            for i := 0; i < x.Len(); i++ {
                if !equal(x.Index(i), y.Index(i), seen) {
                    return false
                }
            }
            return true

        // ...struct and map cases omitted for brevity...

        panic("unreachable")
    }
    ```
    * 使用 unexported function `equal` 進行遞迴
    * 對於 `x` 跟 `y` 檢查是否 valid 跟同型態
* 不透露使用 reflect 所以用 export `Equal`
    ``` go
    func Equal(x, y interface{}) bool {
        seen := make(map[comparison]bool)
        return equal(reflect.ValueOf(x), reflect.ValueOf(y), seen)
    }

    type comparison struct {
        x, y unsafe.Pointer
        t    reflect.Type
    }
    ```
    * 為了避免再 cyclic data structure 重複比較所以需要紀錄比較過那些
    * `comparison` struct 有兩個變數的 address 跟 type of comparison
    * 需要額外紀錄 type 因為兩個不同變數可能是同一個 address 像是 array x 跟 x[0]
* `Equal` 一開始先檢查是否已經檢查過
    ``` go
    if x.CanAddr() && y.CanAddr() {
        xptr := unsafe.Pointer(x.UnsafeAddr())
        yptr := unsafe.Pointer(y.UnsafeAddr())
        if xptr == yptr {
            return true // identical references
        }
        c := comparison{xptr, yptr, x.Type()}
        if seen[c] {
            return true // already seen
        }
        seen[c] = true
    }
    ```

### 13.4. Calling C Code with cgo
* Go program 或許需要使用 C implement 的 hardware driver，查詢 C++ 實作的 embedded database，C 已經是程式的通用語所以 package 想要被廣泛使用需要 export C-compatible API
* `cgo` 是個可以在 Go 建立 C function 的 binding，這類工具稱為 foreign-function interfaces(FFIs)，也有其他可以做到相同功能像是 SWIG 提供對於 C++ classes 更多 feature
* Standard library 中 compress/... 提供常見的壓縮與解壓縮演算法像是 LZW 跟 DEFLATE，皆提供 wrapper `io.Writer` 讓壓縮完的資料寫入跟 `io.Reader` 讀取解壓縮完成的資料
* bzip2 演算法雖然比 gzip 慢但是壓縮率更好，但 Go 只有提供 decompressor 而沒有提供 compressor，不過可以使用 open-source C implementation
* 如果 C library 很小則會直接轉成 pure Go
* 如果 C library Performance 不重要就會使用 subprocess 去跑
* 如果需要使用複雜的 C library 跟 performance 需要很好，就需要使用 `cgo` wrap
* 為了使用 libbzip2 C package 
    * `bz_stream` struct 存放 input 跟 output buffer
    * `BZ2_bzCompressInit` 配置 stream buffer
    * `BZ2_bzCompress` 從 input buffer 壓縮資料到 output buffer
    * `BZ2_bzCompressEnd` 釋放 buffer
* 直接呼叫 `BZ2_bzCompressInit` 跟 `BZ2_bzCompressEnd` 然後定義 wrapper function `BZ2_bzCompress`
    ``` go
    /* This file is gopl.io/ch13/bzip/bzip2.c,         */
    /* a simple wrapper for libbzip2 suitable for cgo. */
    #include <bzlib.h>

    int bz2compress(bz_stream *s, int action,
                    char *in, unsigned *inlen, char *out, unsigned *outlen) {
      s->next_in = in;
      s->avail_in = *inlen;
      s->next_out = out;
      s->avail_out = *outlen;
      int r = BZ2_bzCompress(s, action);
      *inlen -= s->avail_in;
      *outlen -= s->avail_out;
      s->next_in = s->next_out = NULL;
      return r;
    }
    ```
* `import "C"` Go 沒有 C package 這是讓 go build 處理檔案之前先用 `cgo`
    ``` go
    // Package bzip provides a writer that uses bzip2 compression (bzip.org).
    package bzip

    /*
    #cgo CFLAGS: -I/usr/include
    #cgo LDFLAGS: -L/usr/lib -lbz2
    #include <bzlib.h>
    #include <stdlib.h>
    bz_stream* bz2alloc() { return calloc(1, sizeof(bz_stream)); }
    int bz2compress(bz_stream *s, int action,
                    char *in, unsigned *inlen, char *out, unsigned *outlen);
    void bz2free(bz_stream* s) { free(s); }
    */
    import "C"

    import (
        "io"
        "unsafe"
    )

    type writer struct {
        w      io.Writer // underlying output stream
        stream *C.bz_stream
        outbuf [64 * 1024]byte
    }

    // NewWriter returns a writer for bzip2-compressed streams.
    func NewWriter(out io.Writer) io.WriteCloser {
        const blockSize = 9
        const verbosity = 0
        const workFactor = 30
        w := &writer{w: out, stream: C.bz2alloc()}
        C.BZ2_bzCompressInit(w.stream, blockSize, verbosity, workFactor)
        return w
    }
    ```
    * `cgo` 產生暫時的 package 包含所有檔案用到所宣告 C function 跟 type
    * `NewWriter` 呼叫 C function `BZ2_bzCompressInit` 初始化 stream 到 buffer
* `Write` method 把未壓縮的資料給 compressor 呼叫 `bz2compress`，也有用到許多 C types 像是 bz_stream, char, uint 跟 C function 像是 bz2compress 跟 BZ_RUN 
    ``` go
    func (w *writer) Write(data []byte) (int, error) {
        if w.stream == nil {
            panic("closed")
        }
        var total int // uncompressed bytes written

        for len(data) > 0 {
            inlen, outlen := C.uint(len(data)), C.uint(cap(w.outbuf))
            C.bz2compress(w.stream, C.BZ_RUN,
                (*C.char)(unsafe.Pointer(&data[0])), &inlen,
                (*C.char)(unsafe.Pointer(&w.outbuf)), &outlen)
            total += int(inlen)
            data = data[inlen:]
            if _, err := w.w.Write(w.outbuf[:outlen]); err != nil {
                return total, err
            }
        }
        return total, nil
    }
    ```
    * 每次 loop `bz2compress` 傳入剩下的資料的 address 跟長度還有 `w.outbuf` 的 address 跟長度，
    * `inlin` 跟 `outlen` 傳入 address 所以 C function 可以改動這些值，所以就能知道消耗了多少資料跟多少壓縮完成的資料
* `Close` method 使用 loop flush 剩下壓縮完成的資料
    ``` go
    // Close flushes the compressed data and closes the stream.
    // It does not close the underlying io.Writer.
    func (w *writer) Close() error {
        if w.stream == nil {
            panic("closed")
        }
        defer func() {
            C.BZ2_bzCompressEnd(w.stream)
            C.bz2free(w.stream)
            w.stream = nil
        }()
        for {
            inlen, outlen := C.uint(0), C.uint(cap(w.outbuf))
            r := C.bz2compress(w.stream, C.BZ_FINISH, nil, &inlen,
                (*C.char)(unsafe.Pointer(&w.outbuf)), &outlen)
            if _, err := w.w.Write(w.outbuf[:outlen]); err != nil {
                return err
            }
            if r == C.BZ_STREAM_END {
                return nil
            }
        }
    }
    ```
    * 完成後呼叫 `BZ2_bzCompressEnd` 釋放 stream buffer 使用 defer 確保所有 path 都會執行
    * `w.stream` 結束後就不應該被 dereference 所以設成 nil 同時 一開始就先檢查避免呼叫 `Close` 之後還去使用 `w.stream`
* `Write` 跟 `Close` method 都不是 concurrent-safe 所以 concurrent call to `Write` 跟 `Close` method 會讓 C function crash
* `bzipper` 是 bzip2 compressor command
    ``` go
    package main

    import (
        "io"
        "log"
        "os"

        "gopl.io/ch13/bzip"
    )

    func main() {
        w := bzip.NewWriter(os.Stdout)
        if _, err := io.Copy(w, os.Stdin); err != nil {
            log.Fatalf("bzipper: %v\n", err)
        }
        if err := w.Close(); err != nil {
            log.Fatalf("bzipper: close: %v\n", err)
        }
    }
    ```
### 13.5. Another Word of Caution
* 高階語言把程式設計師從電腦細節分離出來讓程式可以安全且穩定的跑在各種不同的機器上面
* `unsafe` package 讓程式設計師可以穿越隔離直接操作電腦細節用以獲得更好的效率，但代價是可攜性與安全性。仔細評估後如果使用 `unsafe` 是最好 solution 才使用，盡量侷限在小範圍

### Reference
Golang website: https://golang.org/

Go example: https://gobyexample.com/

The Go Play Space: https://goplay.space/

Book: https://www.amazon.com/Programming-Language-Addison-Wesley-Professional-Computing/dp/0134190440

Book code example: https://github.com/adonovan/gopl.io/

###### tags: `go`
