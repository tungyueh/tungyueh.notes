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
