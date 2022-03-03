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
