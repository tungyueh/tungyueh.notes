# 1. A Pragmatic Philosophy
## The Cat Ate My Source Code
* 犯錯就承認後提供解法選項來補救而非責怪他人
## Software Entropy
* 當發現問題就盡早修復而非讓洞越破越大
## Stone Soup and Boiled Frogs
* 提供催化劑讓大家樂於進步
* 記住目標不被周而復始功能所迷惑
## Good-Enough Software
* 越早讓使用者嘗試可以盡早得到回饋而非花很長時間做出完美的產品後發現沒人要用
## Your Knowledge Portfolio
* 規律的累積知識的廣度與深度
* 用批判性的思考去檢視學習到的東西
## Communicate!
## Summary

# 2. A Pragmatic Approach
## The Evils of Duplication
* 維護並非 release 才開始而是面對不斷變動的現況採去維護知識完整的樣貌
* 有多種語言代表相同一俟可以用 code generator 產生 code
* 先寫文件在寫 code 在緊急的時候就會跳過文件，可以用規格產生測試 code 確保程式符合規格
* 讓東西容易使用才會讓人想用
## Orthogonality
* 彼此獨立可以增加工程師的生產力
    * 可以減少開發跟測試時間
    * 更容易 reuse
    * 可以組合成更多功能的結果
* 減少風險
    * 壞掉的部分不回影響其他部分
    * 受損部分會被侷限
    * 容易被測試就可以更完整測試
    * 不被特定第三方綁住
* 合作可以明確分工不須開會討論如何處理 overlay 的地方
* 可以根據當改變需要開會有多少人要參加來判斷系統各部分獨立的程度
## Reversibility
* 不該假設某項東西不會變而當成基礎去設計
* 採用的技術之後可能因為效能或成本因素而被要求替換，所以沒有任何決定是永久的
## Tracer Bullets
* 盡快弄出一個完整的產品可以讓使用者回饋需求、開發者有環境可以開發整合、可以 demo 給人看跟對於掌握進度更有感覺
* Prototype 用來摸索產品可能的樣貌，過程中產出的 code 是可以被丟棄的
* Tracer code 用來找出系統的架構
## Prototypes and Post-it Notes
* Prototype 不一定需要寫 code 才能做也可以用便利貼的方式來思考，目的只是回答產品需求的問題
* 使用 prototype 來學習不確定的事物
## Domain Languages
* 讓程式語言貼近問題的語言
## Estimating
* 能夠預估程式的能力才有辦法知道可以處理多大維度的問題
* 不同的問題有不同的精確程度

# 3. The Basic Tools
## The Power of Plain Text
* 直接將知識用基本格式保留起來後續可以持續使用
## Shell Games
* 使用 GUI 雖然很方便但也被限制在設計者的思考下，通常想要更多功能就無法達到
* Shell 可以提供更快速的方法達成需求
* 熟悉 shell 的各項指令的用法可以使生產力大增
## Power Editing
## Source Code Control
## But My Team Isn't Using Source Code Control
## Source Code Control Products
## Debugging
## Text Manipulation
## Exercises
## Code Generators

# 4. Pragmatic Paranoia
## Design by Contract
## Dead Programs Tell No Lies
## Assertive Programming
## When to Use Exceptions
## How to Balance Resources
## Objects and Exceptions
## Balancing and Exceptions
## When You Can't Balance Resources
## Checking the Balance
## Exercises

# 5. Bend or Break
## Decoupling and the Law of Demeter
## Metaprogramming
## Temporal Coupling
## It's Just a View
## Blackboards

# 6. While You Are Coding
## Programming by Coincidence
## Algorithm Speed
## Refactoring
## Code That's Easy to Test
## Evil Wizards

# 7. Before the Project
## The Requirements Pit
## Solving Impossible Puzzles
## Not Until You're Ready
## The Specification Trap
## Circles and Arrows

# 8. Pragmatic Projects
## Pragmatic Teams
## Ubiquitous Automation
## Ruthless Testing
## It's All Writing
## Great Expectations
## Pride and Prejudice
