# Design Patterns, Elements of Reusable Object-Oriented Software
## 1 Introduction
* 設計一個可重複利用的 object-oriented software 很困難需要針對問題做設計但又要能夠應付未來可能的需求
* 有經驗的人會探索之前成功的設計來套用在問題上面，累積足夠經驗就可以知道哪些設計會很有彈性
* 設計如果遵循模式之後的設計選擇就跟著模式走就好了
* Design Pattern 紀錄常見的設計經驗幫助人可以快速做出正確的 design
### 1.1 What Is a Design Pattern?
* 每種 Pattern 都是對於重複出現的問題提供核心解法
* Pattern name 用來描述遇到的問題跟解法減少溝通成本
* Problem 描述符合哪種 pattern
* Solution 說明 class 跟 instances 的責任跟溝通方式來達成設計
* Consequences 說明使用 pattern 帶來的好壞用來考量是否使用
### 1.2 Design Patterns in Smalltalk MVC
* MVC 包含三種 object，Model 是 application object，View 決定螢幕顯示，Cotrol 決定回應使用者的輸入，MVC 把三項責任分開以增加彈性跟可重複利用
* MVC 使用 subscribe/notify protocol 來 decouple model 跟 view，model 有變動會通知 view，可以使用多種 view 而不需要改動 model
* Decoupling objects 讓 objects 改動的時候不會需要改動一堆東西可以在 Observer design pattern 找的類似概念
* MVC 的 Composite View 當成一般的 View 可以在 Composite design pattern 找到類似概念
* MVC View 跟 Controller 的關係像是 Strategy design pattern 的一種例子
### 1.3 Describing Design Patterns
* Pattern Name and Classification: 好的名字可以從設計的字典裡很快的找到需要的設計
* Intent: 設計的原理跟意圖，要解決哪種問題，如何解決
* Also Known As: 關於 pattern 的其他名字
* Motivation: class 跟 object 在 pattern 如何架構來解決問題，幫助以抽象化的方式來理解 pattern
* Applicability: 哪些情境可以套用這個 pattern，辨認可以使用的情境
* Structure: 圖像化的方式來表達 class 之間的關係
* Participants: class 跟 object 在 design pattern 扮演的腳色跟責任
* Collaborations: participants 之間如何溝通來達成他們的責任
* Implementation: 實作的技巧
* Sample Code: 使用 C++ 或 Smalltalk 的 code 片段來說明如何實作
* Known Uses: 真實世界的範例
* Related Pattern: 類似的 pattern
### 1.4 The Catalog of Design Patterns
* Abstract Factory: 提供一個介面可以不需要定義好 concrete class 就可以製造出一群 objects
* Adapter: 把 class interface 轉換成 client 期待的 interface，讓 class 可以一起工作
* Bridge: Decouple 實作跟 abstraction 讓兩者可以各自獨立
* Builder: 把建立的步驟分離開來讓我們可以使用相同的建立步驟製造出不同東西
* Chain of Responsibility: 只在需要處理 object 的時候才處理不然直接往下傳給其他人處理
* Command: 封裝 request 成物件讓 client 可以對不同 request 做處理
* Composite: 把 object 以樹狀結構組合再一起讓 client 可以當成一樣的 object 來使用
* Decorator: 動態的增加 object 的行為
* Facade: 提供一致性的介面讓 subsystem 容易使用
* Factory Method: 定義如何建立 object 的介面但讓 subclass 自己決定要用哪種 class 初始化
* Flyweight: 共享來有效率的管理大量類似 object
* Interpreter: 定義語言跟文法來解析
* Iterator: 提供讀取連續的 object 但不需要理解底下實作方式
* Mediator: 定義一個 object 來與多個 object 互動而不需要一一對 object 作互動
* Memento: 不知道 object 內部細節的情況下讀取目前狀態以便之後能回復跟狀態
* Observer: 定義 one-to-many 的依賴關係讓一個 object 變動的時候相關的 object 都可以收到通知
* Prototype: 使用已存在的 object 來建立新的 object
* Proxy: 提供對於其他 object 的控制權限
* Singleton: 確保 class 只有一個 instance 給予一個 global point 存取
* State: 根據內部狀態改變 object 行為
* Strategy: 將 algorithm 封裝起來並且可替換，讓不同 client 與 algorithm 彼此獨立
* Template Method: 定義 algorithm 架構讓 subclass 決定其中步驟，讓 subclass 只需透過改寫步驟而不需改動整個架構
* Visitor: 新增 operation 也不須改動 class 操作的地方
### 1.5 Organizing the Catalog
* Design pattern 有不同程度的 abstraction 所以分類可以幫助學習同個分類下的 pattern 更快速
* Purpose 根據目的分為 creational, structural, behavioral
    * Creational patterns 專注於 object 的建立過程
    * Structural patterns 處理 class 跟 object 之間的關係
    * Behavioral patterns 處理 class 跟 object 之間的責任
* Scope 說明是針對 class 還是 object
    * Class patterns 處理 class 跟 subclass 之間的關係
    * Object patterns 處理 object 之間的關係
### 1.6 How Design Patterns Solve Design Problems
#### Finding Appropriate Objects
* 用 object 組成系統需要考量很多面向所以很困難，encapsulation, granularity, dependency, flexibility, performance, evolution, reusability 之類的面向
* 通常都透過分析 model 來產出需要的 object 但是 object-oriented Design 最後產出的 object 有些無法直接對應的真實世界
* Desing pattern 幫助我們找出沒有這們明顯的 abstraction 例如 algorithm 跟 state 這種不然容易在早期設計發現的東西
#### Determining Object Granularity
* Design pattern 幫助判斷 object 的大小
* Facade pattern 描述如何用 object 描述整個系統
* Flyweight pattern 描述如何管理大量的 object
* 把 object 切分成更小的 object
* Abstract Factory 跟 Build pattern 切分出只負責產生 object 的 object
* Visitor 跟 Command pattern 切分出只負責處理 request 的 object
#### Specifying Object Interfaces
* Operation signature 包含 object operation 的名字、參數跟回傳值
* Object Interface 是該 object 所有的 operation signature
* Type 是指 object 符合所有 interface，subtype 代表符合一部分的 interface
* Dynamic binding 指送 request 給 object 要到 run-time 才能知道如何實作，這種可替代性稱為 polymorphism
* Design pattern 幫助我們定好 interface 決定 object 如何溝通
#### Specifying Object Implementations
* Class 定義 object 的實作，object 從 class 被初始化出來
* class 可以使用繼承從已定義好的 class 產生，有 parent class 的 data 跟 operation
* Abstract class 只定義好 interface 留給 subclass 定義實作，相反的為 concrete class
* Mixin class 提供額外的 interface 跟功能
#### Class versus Interface Inheritance
* Object 的 class 定義實作方式跟內部狀態，Object 的 type 定義對外溝通的 interface
* Class 繼承包含作用機制而 interface 繼承代表 object 可以用在哪些地方
#### Programming to an Interface, not an Implementation
* class 繼承可以讓我們很方便的從現有的 class 重用功能
* 繼承實作外還有介面，要抱持一樣的介面因為 polymorphism 依賴於這個介面
* 只對介面做事可以跟實作減少依賴，就可以不用知道實作方式，只要介面不變而 code 就不用動
#### Inheritance versus Composition
* class 繼承稱為 white-box reuse 因為 subclass 可以看到 parent class 的內部
* object composition 稱為 black-box reuse 因為透過 object interface 組出更複雜的功能但不知道 object 內部的實作
* 繼承缺點是只要 parent 有改動會影響到所有繼承的 class，會破壞 encasuplation
* Composition 會有很多的 object 著重在彼此的互動關係
* 盡量選擇 compostion 而不是繼承，但通常無法使用現有的 object 來組出需要的功能還是需要使用繼承來快速產生需要的 object
#### Delegation
* Delegation 讓 composition 像是繼承一樣方便使用
* Delegation 是有兩個 object 對 request 做處理，但繼承是透過 refer 到自己來做處理
* Delegation 優點是可以在 run-time 改變組成的 object，缺點是 dynamic highly parameterized 的軟體不好理解
* Delegation 是 composition 一個極端的例子用來說明永遠可以替換掉繼承來 reuse code
#### Inheritance versus Parameterized Types
* 透過 paremeterized type 可以重用功能，例如 List class 可以對 element 設定不同的 type 而不需要每種 type 都提供 List class
* Paremeterized types 跟繼承一樣無法再 run-time 改變
#### Relating Run-Time and Compile-Time Structures
* Code 架構在 compile-time 就已經決定了，例如 class 的繼承關係，run-time 的架構是包含 object 之間的改變
* Aggregation 是指 object 被其他 object 所需要
* Acquaintance 是指 object 有可能被其他 object 用到，關係比 aggregation 弱
* Aggregation 的關係比起 Acquaintance 固定，Acquaintance 比較 dynamic 所以會讓 code 更難讀懂
* Run-time structure 主要受設計所影響，所以要好好設計才能友好的 run-time structure
#### Designing for Change
* 知道可能有哪些 change 才能設計出可以很好接受這些 change 的系統，沒有把 change 考量進去的 design 通常在未來需要重新 design
* Design pattern 幫助系統可以接納以一定方式演化所需要的 change
#### Application Programs
* 可重用、維護性跟擴充性是撰寫 application program 最重要的事情，確保不需要做多餘的設計跟實作
* Design pattern 幫助減少 dependencies 來提高可重用性
* Design pattern 讓 application 更容易維護，減少coupling就可以提高extensibility
#### Toolkits
* Application 通常會使用其他的 library 的 predefined class 稱為 toolkit
* Toolkit 以提供功能為目的而不是針對 application 所設計
* Toolkit 比起 application 要難因為要提供足夠好用且泛用的功能
#### Frameworks
* 可以對特定面向的軟體客製化成自己所需要的
* 定義整個結構提供 design parameter 來讓人客製化，讓 design 可以 reuse 而不是 code reuse 但還是會提供 concrete subclasses 讓人可以快速使用
* Toolkit 是寫好主要部分後呼叫，而 framework 是寫會被呼叫到的 code
* Framework 缺點是少了一些自由因為都已經幫你做好 design decision
* Framwork 相對於 toolkit 跟 application 更難因為需要對該領域的 application 設計好架構，需要設計的有彈性跟擴充性
* Application 依賴於 framework 所以 framework 有改變就會影響到 application 需要跟著改變
* 透過學習 pattern 可以更快了解 framework 稍微降低學習曲線
### 1.7 How to Select a Design Pattern
* 思考 design pattern 如何幫助找出適當的 object 跟 granularity 跟 interface
* 看看 pattern 的目的可以找出跟現在問題有關的
* 研究 pattern 之間的關係可以直接找到正確的 pattern
* 研究三大類的 pattern 之間的異同
* 檢視為何需要重新設計來找到 pattern 可以避免
* 思考哪些是設計中可以變動的，
### 1.8 How to Use a Design Pattern
1. 快速研讀 pattern 的好壞處
2. 回頭讀 class 跟 object 之間的關係
3. 看 sample code 來幫助如何實作 pattern
4. 挑選 pattern participants 合適的名字因為 pattern 通常太泛用
5. 定義 classes，定好 interface、繼承關係
6. 為 pattern operation 定好跟 application 相關的名字
7. 實作 operation 來執行責任
## 2 A Case Study: Designing a Document Editor
### 2.1 Design Problems
* Document structure 會影響到所有 application 其他的部分，決定如何在 application 中組織這些 document
* Formatting 如何把圖片跟文字排好? 哪個 object 要負責各式各樣的 formatting? 如何跟 docuemen 溝通
* User Interface 的細節要如何能夠保持不斷演化而不影響其他部分
* 支援不同主題而不需修改太多
* 支援多種 window system 讓 design 能與 window system 彼此獨立
* User Operation 散落在各個畫面上，要如何提供一致的方式來存取或 undo 效果
* 支援檢查錯字跟決定斷行
### 2.2 Document Structure
* 把文字跟圖一視同仁
* 把單一 element 跟多個 element 一視同仁
#### Recursive Composition
* 透過簡單的 element 組成複雜的 element
* 透過把文字跟圖片當成一樣的 element 就可以不管怎麼呈現跟排版，未來加入新的字元也不影響其他功能
#### Glyphs
* Glyphs 是 abstract class 讓所有可能出現在 document 的人繼承
* 知道如何畫出來、知道佔據多少空間、知道 parent 跟 child
#### Composite Pattern
* 遞迴的組成比起單純的 document 要來的好，因為可以對未來有可能變複雜且階層式的架構建立彈性
### 2.3 Formatting
* 由於 formatting algorithm 有很多種變化，所以封裝起來方便替換不同 algorithm
* 封裝 algorithm 也是為了 Strategy pattern，設計好 interface 跟 contextr 就可以支援多種 algorithm
### 2.4 Embellishing the User Interface
* 修飾的部分不該讓 user interface object 才能夠隨時增加減少裝飾而不影響 user interface object
* 使用繼承來實作會有一堆 classes，因為需要對所有可能的組合建立 class 所以也不可行
* Composition 是比較有彈性跟可能性的作法，讓裝飾的 class 知道 Glyphs class 的存在才不需要修改原本的 Glyphs class
* 裝飾的 class 需要跟 Glyphs 的 interface 一樣因為 client 不需要知道有沒有裝飾的 class
* Transparent enclosure 有單一 child 跟相容性的 interface，可以直接 delegate operation 或者增加一些行為還可以增加狀態
* 透過不同的 transparent enclosure 可以嘗試不同組合的效果
* Decorator pattern 嘗試捕捉 transparent enclosure 的關係，為本來的 object 增加一點責任
### 2.5 Supporting Multiple Look-and-Feel Standards
* 軟體應該要能夠容易的在不同平台上移植並且根據平台有不同的呈現方式
* 根據不同平台使用不同的 style 但是每次移植平台都需要確認各處初始化 style 很難開發
* 需要有可以換掉整套風格的方法
* 透過 facctory 可以隱藏初始化特定風格來減少對風格的 dependency
* 從以上可以推導出 Abstract Factory Pattern，提出如何建立類似的東西但不需要直接初始化 class
### 2.6 Supporting Multiple Window Systems
* 無法使用 abstract factory 因為不同的 window system 都有不同的規格，需要有統一的 interface 才有辦法使用
* 可以使用大家的交集定成 interface 缺點是需要捨棄大部分都有的功能，或者只要有的功能都納入缺點是 interface 會過於龐大跟雜亂
* 根據不同 window system 實作會需要再編譯的時候選擇要用哪種版本，缺點是維護的時候會有一堆相同名稱但不同實作的 class，但如果加入 window system 在 class 名稱又會有太大量的 class 產生
* 把不一樣的地方封裝起來，把不同的實作方式封裝起來
* 使用 Implement class 繼承原本的隱藏不同 window system 的實作，讓原本的 class 保持簡單的繼承關係不被關於不同實作方式所汙染
* 實作 class 貼近於實際實作的東西而 Interface 就可以選擇要用那些 feature 提供給 application programmer 使用，這樣就有一定的彈性來選擇不同方式
* Bridge Pattern 由此問題發展出來，藉由把不同的 class hierarchies 讓不同面向的 class 可以各自演化又能彼此合作，像是一個面向 window 的 logic notion 而另外一個面向 window 的實作
### 2.7 User Operations
* 需要支援各種 user operation 但不能把 interface 跟 operation 綁再一起因為會有多個 interface 對應到相同的 operation 而且 interface 也有可能會改變
* 需要有 redo 跟 undo 功能在一部份的 operation 上面，還希望沒有限制次數的 redo 跟 undo
* 下拉式選單也是一種 Glyph 只是用於回應 request，但如果為每個 operation 都去建立處理 request 的 subclass 會太 coupling
* 使用 function 來 parameterize object 會無法支援 undo/redo、需要知道目前狀態跟不容易 reuse，所以應該要直接從 object parameterize
* 把 request 封裝到 command object，只有一個 execute method 而 subclass 各自去實作來迎合不同 request
* 需要 unexecute method 來支援 undo，為了處理沒有意義的 undo 再加入 reversible method 來判斷可不可以 undo
* 需要紀錄 command history 來支援 undo 跟 redo
* Command pattern 描述如何封裝 request 提供一個 common interface 根據設定來處理 request
### 2.8 Spelling Checking and Hyphenation
* 檢查拼寫跟斷行方式可能會有不同 algorithm 來提供各種 trade-offs 的選擇
* 不想要把功能綁在 document structure 上面因為會不斷的加入新功能
* 問題在於要能在散落各處的 glyphs 存取資訊來分析
* 存取機制要能套用在各種資料結構上面，加上會需要不同的存取方式
* Glyphs interface 加上各種存取跟 traversal 的方式，缺點是無法支援新的 traversal
* 把不同的地方封裝起來才是比較好的方法，使用 iterator class 來封裝 traversal 機制
* Iterator pattern 捕捉這種支援各種 data structure 的 access 跟 traversal 的技巧
* 文法檢查跟斷行在 traversal 中累績資訊來分析，把 traversal 跟之中所做的動作分開才方便 reuse
* 針對不同 Glyphs 做分析但是如果在每種 Glyphs 都增加分析功能之後只要新增一種分析就需要去改每種 Glyphs，所以將分析的能力封裝到不同 object 比較好
* 分析裡面要避免有一堆判斷哪種 Glyphs 的 code，為每個 Glyphs subclass 增加 CheckMe 就可以實作各種檢查的機制
* 新的分析方法如果要獨立 class 則需要繼續新增類似 CheckMe 但是可以將所有分析方法放在同一個 class 提供統一的 interface
* Visitor 指 traversal object 並做一些事情，之後新增加分析方法就只要新增 subclass 而不需要動到其他地方
* Visitor pattern 提供在不改變 traversal class 的 code 可以有各種不同的分析方式，需要注意適合使用在對固定的東西做各種不同的事情，因為只要多新增 structure 就需要改動所有 visitor 的 operation
### 2.9 Summary
* Composite 來表示文件結構
* Strategy 允許有不同格式
* Decorator 給 user interface 增加裝飾
* Abstract Factory 支援各種平台外觀標準
* Bridge 支援不同 window system
* Command 支援 undoable 操作
* Iterator 來讀取跟 traveral 文件
* Visitor 可以有不同分析方法
## 4 Structural Patterns
* Structural Patterns 關注於把 classes 跟 objects 組成更大的架構
* Structural class patterns 使用繼承來組合不同的 interface 或實作
* Structural object patterns 組合不同 object 做出新功能，可以在 run-time 改變組合的方式更有彈性
### Adapter
* 意圖: 把 class 的 interface 轉換成 client 希望的 interface 讓 classes 可以彼此合作
* 動機
    * 有時候 toolkit class 的 interface 不符合 domain-specific interface 所以無法 reuse
    * Adapter 需要補上原本沒有的功能
* 使用時機
    * 想要使用現存的 class 但是 interface 不符合需求
    * 想要建立 reusable class 可以跟不相關的 class 合作
    * 需要使用現有的多個 subclasses 但無法使用 subclassing 統一介面，用 object adapter adapt parent class interface
* 參與者
    * Target: 定義 client 使用的 interface
    * Client: 使用 Target interface 來合作
    * Adaptee: 需要被 adapat 的現存 interface
    * Adapter: adapt Adaptee 的 interface 到 Target 的 interface
* 合作方式: Client 呼叫 Adapter 後 Adapter 呼叫 Adaptee 來處理 request
* 後果
    * Class adapter
        * 如果想要 adapt 一個 class 跟他的所有 subclass 因為是藉由 implement Adapter class 的方式來 adapt
        * Adatper override Adaptee 的行為
        * 只有一個 object 不需要有額外的 pointer 去找到 adaptee
    * Object adapter
        * 一個 Adapter 跟多個 Adaptee 合作，可以一次加入新的功能到所有 adaptee 上面
        * 難以去 override Adaptee 的行為
    * Adapter 需要負多少責任
    * Pluggable adapter: 當 client 有越少的假設則 class 越容易被 reuse，藉由把 interface adaption 放到 class 就可以消除其他 class 使用的假設
    * Two-way adapter: 當 adaptee object 無法符合 adapter interface 就無法被使用，使用 Two-way adapter 來 adapt 彼此
* 實作
    * Adapter 是 Target 的 subtype 而非 Adaptee 因為繼承 Target 的 public 部分而 private 部分從 adaptee 繼承而來
    * Pluggable adapter 先找到最小的 interface
        * 定義 abstract operation 讓 subclass 實作後去 adapt
        * 使用 Delegate object 來 adapt 可以替換 delegate 方式來做不同的 adapt
### Bridge
### Composite
* 意圖: 將 object 組成樹狀結構來表達 part-whole hierarchy，讓 client 可以將單一或組合的 object 視為相同的 object
* 動機: Client 使用的時候都需要判斷是哪種 object 會使系統變得很複雜，composite pattern 使用 recursive composition 讓 client 不需要判斷 object 種類
* 應用時機: 使用 objects 來做出 part-whole hierarchy、client 不需要判斷 object 種類可以直接使用
* 參與者:
    * Componet: 定義 composite object 的 interface 並實作預設動作、定義如何存取管理 child components
    * Leaf: 定義基本 object 的行為，沒有 children
    * Composite: 定義有 child 的行為、儲存 child componet、實作 child 相關的操作
    * Client: 根據 component interface 使用 object
### Decorator
### Façade
### Flyweight
### Proxy
### Discussion of Structural Patterns
### 5 Behavioral Patterns
* Behavioral patterns 把複雜的 control flow 轉移到 object 如何互動
* Behavioral class patterns 使用繼承把 classes 的行為分開
* Behavioral object patterns 讓 object 互相合作達成獨自無法達成的功能
### Chain of Responsibility
### Command
### Interpreter
### Iterator
### Mediator
### Memento
### Observer
* 目的: 定義一對多的關係，當一個 object 改變狀態時候其他 object 要能被通知更新
* 動機
    * 不同 object 要維持一致性但不想讓 class 太過 coupled 導致 reusability 降低
    * subject 會有很多 observer 當 subject 改變狀態所有 observer 會被通知，observer 會去查詢 subject 狀態達成同步，這種互動稱為 publish-subscribe
* 適用情境
    * 封裝不同的概念在不同 object
    * 改變 object 狀態但不知道需要改變多少其他 object
    * 不用知道需要通知的 object 的模樣就可以通知避免太過 coupled
* 參與角色
    * Subject: 知道所有 observer 提供 attach 跟 detach 的功能來讓任意數量的 observers observe
    * Observer: 定義 update 的 interface 讓 subject 可以通知
    * ConcreteSubject: 儲存 ConcreteObserver objects 需要同步的狀態，當狀態改變通知 observers
    * ConcreteObserver: 儲存需要跟 subject 同步的狀態，實作 update interface 來與 subject 做同步
* 合作方式
    * ConcreteSubject 發生會跟 observers 的狀態不同的的改變時去通知 observers
    * ConcreteObserver 被通知後去查詢 subject 狀態來同步狀態
* 結果
    * Subjects 跟 observers 可以分開 reuse，增加 observers 也不需要改動 subject 跟其他 observers
    * Subject 跟 Observer 可以在不同的 layer 保持系統的 layer 完整
    * Subject 不需要送給特定的 receiver 而是自動送給有來訂閱的 observers，而 observer 可以自行決定怎麼處理收到的通知
    * Observer 不知道其他 observer 存在而盲目的改變 subject 狀態導致 observer 跟相關的 object 連鎖改變狀態，沒有額外的機制讓 observer 知道改變什麼東西會讓 observer 需要努力知道改變了什麼
* 實作
    * 對照 subject 跟 observer 的關係可以直接讓 subject 存好 observer 的位置但是如果當 subject 很多而 observer 比較少會浪費空間，這時候可以改用時間換空間使用 look-up 來記住關係但是會需要比較多的存取時間
    * Observer 可能會訂閱多個 subject 因此改變 update interface 讓 observer 知道是哪個 subject 通知
    * 有兩種做法來 trigger update，一個是當 subject 狀態改變就自動通知 observers 優點是 client 不需要主動要 subject 通知 observers 缺點是一連串的改變會導致一連串的通知沒有效率，另外可以讓 client 在適合的時間叫 subject 通知 observers
    * 刪除 subject 要注意 observer 的 reference 也要跟著刪除，可以在刪除 subject 的時候通知 observers 讓他們可以重設 reference
    * 確保 subject 自身狀態一致後才通知，可以使用 Template Method 把通知放在最後一行
    * 避免太過特定 observer 的更新方式，Subject 傳太多更新資訊給 observer 會讓 observer 無法被 reuse 但如果什麼都不給會讓 observer 自己需要找到改變了什麼導致沒有效率
    * 使用訂閱特定事件來增加效率
    * 當 subject 跟 observer 的關係變很複雜就需要有 object 來處理，責任在於確保 observer 所需要的 subject 都以變更狀態完成在通知增加效率
    * 在沒有多重繼承關係的語言裡把 subject 跟 observer 放在一起
### State
### Strategy
* 目的: 封裝類似的演算法可以互相替換，讓使用的人可以使用不同演算法
* 動機
    * 把演算法放進 class 後當要支援更多演算法的時候會變更複雜
    * 不同演算法適合不同時機使用並不想全部塞進去 class 但都沒用到
    * 增加演算法要跟現有的並存變得困難
* 適用情境
    * 很多相關的 class 只有行為不一樣，使用 strategies 提供設定 class 的方式來呈現不同行為
    * 需要使用不同的演算法，使用 class 繼承關係來實作不同的演算法
    * 演算法用到自己不該知道的資料，使用 Strategy pattern 來避免 expose 與演算法有關的資料結構
    * Class 在不同情況有不同行為，把 conditional branch 放到自己的 Strategy class
* 參與角色
    * Strategy: 定義使用演算法的 common interface，Context 用 interface 來使用 ConcreteStrategy 實作的演算法
    * ConcreteStrategy: 實作演算法
    * Context: 被 ConcreteStrategy 設定好、reference 到 Strategy、可能定義 interface 讓 Strategy 可以取用資料
* 合作方式
    * Context 傳演算法所需要的資料給 Strategy，或者把 context 直接傳給 strategy 讓他自己取用
    * Context 把 client 的 request 轉送給 Strategy，Client 會把 ConcreteStrategy 傳給 Context 之後就只對 Context 溝通
* 結果
    * 類似的演算法都放在繼承關係裡面
    * 使用 subclass 的方法為原本class給予不同行為會讓原本 class 變得複雜，也無法動態改變演算法，把演算法獨立封裝會更容易替換、理解跟擴充
    * Strategy 可以消除 conditional statements，當有不同行為在同一個 class 無可避免的需要使用 conditional statement，所以封裝行為到 Strategy class 可以消除 conditional statements
    * 可以使用不同的實作方式
    * Client 需要知道 Strategy 實作細節才知道要用哪個，所以只有不同行為跟 client 有關的時候才使用 Strategy pattern
    * Strategy interface 被所有人共用但如果 ConcreteStrategy 不需要那些資料就造成浪費導致沒有效率，需要解決這個問題只能讓 Strategy 跟 Context 更 coupled 一點
    * 增加 object 的數量，可以使用 stateless object 來共用，或者使用 Flyweight pattern
* 實作
    * Strategy 跟 Context 的 interface 要能夠讓 ConcreteStrategy 有效率的達到需要的資料
        * Context 透過 Strategy paramter 傳資料，可以維持 decoupled 但可以會傳不需要的資料
        * Context 直接把自己當成 arugment 讓 Strategy 去拿資料，可以獲取自己所需要的資料，但會造成 coupled
    * 可以不需要 strategy，當沒有 strategy 就用預設的行為讓 client 可以不需要知道 strategy，只有在不想要預設的行為才去研究 strategy
### Template Method
* 目的: 定義演算法的架構讓 subclass 可以自己替換某些步驟而不需要改變整個演算法架構
* 動機:
    * Application 有一定的步驟來處理 Document 而且有不同的 Application 可以處理不同 Document
    * Template method 定義好抽象步驟讓 subclass 可以替換不同實作
* 適用情境
    * 實作演算法固定的部分把會變動的地方留給 subclass 實作
    * Subclasses 有共同的部分可以抽出來形成 common class 避免 code duplication
* 參與角色
    * AbstractClass: 定義抽象步驟讓 subclass 實作步驟，實作演算法框架
    * ConcreteClass: 實作抽象步驟
* 合作方式
    * ConcreteClass 依賴於 AbstractClass 來實作步驟
* 結果
    * 基本的 code reuse 技巧，通常在 class libraries 因為都把共同行為放到 library classes
    * 提供 hook operation 給 subclass 用來擴充行為，hook 預設是不做任何事情
* 實作
    * 最小化需要實作的步驟，越多步驟需要實作越有可能產生詭異的行為
    * 需要實作的步驟加上共同的 prefix
### Visitor
### Discussion of Behavioral Patterns
