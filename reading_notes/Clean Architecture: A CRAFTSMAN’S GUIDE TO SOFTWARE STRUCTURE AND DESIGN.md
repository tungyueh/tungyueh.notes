# Clean Architecture: A CRAFTSMAN’S GUIDE TO SOFTWARE STRUCTURE AND DESIGN
## PART I Introduction
* Architecture 用在 high level，Design 用在 low level 對於 structure 的選擇
* High-level structure 跟 low-level detail 是相輔相成的，需要兩者來形成整個系統的樣子
### Chapter 1 What Is Design and Architecture?
#### The Goal?
* 使用最少的人力來建造維護系統
* 設計好壞在於需要多少 effort 來達成需求
#### Case Study
* 工程師的數量增加並沒有與開發效率成等比
* 隨著 release 次數增加每行 code 的成本驚人的增加，最終會侵蝕掉本來的盈利
##### THE SIGNATURE OF A MESS
* 雖然都很努力但生產力逐漸降低趨近於零
* 常常常常在處理 mess 沒有餘力開發新功能
##### THE EXECUTIVE VIEW
* 每次 release 付出的人力成本不斷增加但功能卻沒有增加
* 任何 CFO 看到這種現象都知道該立刻阻止情況惡化下去
##### WHAT WENT WRONG?
* Developer 只想先推出產品之後在清理技術債，但市場壓力總是在，所以就不會去清理技術債
* Developer 過度相信自己的生產力可以持續維持，但 mess code 會讓生產力幾個月之後就趨近於零
* Mess code 不論在短期或長期都會降低生產力
* 維持生產力的方式在於認真處理 mess code 
#### Conclusion
* 避免過度自信，認真對待軟體品質
* 知道什麼是好的軟體架構，建造能夠花最少力氣達到越多生產力的架構
### Chapter 2 A Tale of Two Values
* 軟體系統提供行為跟架構，但通常只會專注一個或都不管
#### Behavior
* Programmer 根據需求寫 code 讓系統能夠做出需求的行為
* 當行為錯誤就需要找出問題來修正
* 只覺得工作就是寫 code 跟修 bug 是錯誤的
#### Architecture
* 軟體取名的目的就是形容機器的行為很容易被改變
* 軟體應該能夠隨著需求改變而簡單的變更，scope 的改變比較容易，shape 的改變比較困難
* 當改變的形狀無法輕鬆拼進系統的形狀則改變就變的很困難
#### The Greater Value
* 一般來說大家都會覺得系統能夠運作比起系統能改改變更重要
* 假設程式運作很好但無法改變只要有改變需求則此程式就沒有用了
* 假設程式無法運作但很好改變就可以改變成可以運作的，之後也可以依據需求改變，所以程式才有用
#### Eisenhower’s Matrix
* 軟體行為是緊急但不重要
* 軟體架構是不緊急但很重要
* 通常都會把緊急但不重要的東西排在較高的順位導致軟體架構被忽略而專注在不重要的行為上
* Manager 無法衡量架構的重要性所以要靠 developer
#### Fight for the Architecture
* 需要與不同 team 取得平衡保持架構的完整
* 架構師需要關注於建立容易開發修改擴充功能的架構
* 最後才考慮架構會讓成本更高讓變動更不容易，當這種情形發生代表沒有在捍衛架構的完整度
## PART II Starting with the Bricks: Programming Paradigms
### Chapter 3 Paradigm Overview
#### Structured Programming
* goto 無限制的跳躍會破壞程式架構，用 if/then/else 跟 do/while/until 取代
#### Object-Oriented Programming
* 把變數放到 heap 讓存在的時間變長，function 也漸漸變成 constructor，變數變成 instance variable，nested function 變成 method
#### Functional Programming
* 沒有 assignment statement 讓值保持不變
#### Food for Thought
* 以上不同的程式方式都是限制某些行為達成目的
* 三種方式都捨棄 goto statement, function pointers 跟 assignment
#### Conclusion
* Polymorphism 機制幫助跨 architectural boundaries
* Functional programming 幫助資料管理
* Structured programming 分類好 modules
### Chapter 4 Structured Programming
#### Proof
* Programming 有太多的細節讓人無法注意到每個地方，只要忽略小地方就會造成程式不穩定
* Dijkstra 模仿數學家的方式，把程式用階層化的方式，只要使用已證明過的 structure 就可以確保正確性
* 某些 goto statement 讓 module 無法分解成更小的單位，所以無法使用 divide-and-conquer approach 來證明正確性
* 某些 goto statement 就只是簡單的決定跟 iteration control 就像是 if/then/else 跟 do/while，所以 module 只使用這些 control structure 就可以遞迴分解成可以被證明的單位
* 程式有被證明都是由 sequence, selection 跟 iteration 所組成的
* Module 是 control structure 最小集合，所有程式都可以用 module 建立起來
* Sequential 藉由比較 input 跟 output 來證明
* Selection 藉由窮舉來證明
* Iteration 藉由 induction 證明
#### A Harmful Proclamation
* 一開始提出 goto 不好受到很多人的質疑但之後漸漸證明是正確的因為現代語言漸漸不鼓勵使用 goto 甚至禁止
#### Functional Decomposition
* Structured programming 讓 module 可以不斷的被分解，所以從大的問題分解成許多 high-level functions，之後可以再分成 low-level functions
* Struce analysis 跟 design 幫助把大的系統分解成 module 跟 components
#### No Formal Proofs
* 沒有人會花時間證明每個 function 是正確的因為需要花很多時間，不過人們依然相信證明是個評量軟體品質的適當方法
#### Science to the Rescue
* 科學無法像數學一樣被嚴謹證明出來
* 科學主要是很難證明是錯的所以傾向相信是正去的
#### Tests
* 程式只能被檢測出有錯誤而無法證明完全沒錯誤，所以只能盡量讓我們程式找不出錯誤來提高正確的機率
* Structured programming 讓我們能夠把程式分解成小的 function 再去測試有沒有錯以此提高整體的正確性
#### Conclusion
* Structured programming 的價值在於可以提供小的單位來測試有沒有錯誤
* Software architect 定義容易被測試的 module, components 跟 service
### Chapter 5 Object-Oriented Programming
#### Encapsulation?
* OO 提供 data 與 function 的 encapsulation，外部的人看不到 data 只看的到部分 function
#### Inheritance?
* 在 enclosing scope 下重新定義一群變數跟 function
* OO 尚未出現時就有類似 Inheritance 的概念，只是更容易做到而已
#### Polymorphism?
* 雖然以前就用類似 polymorphism 的概念但 OO 讓這個概念能夠更安全更快速
##### THE POWER OF POLYMORPHISM
* Polymorphism 讓主程式不動，只要新增 plugin 就可以提供新功能
* OO 讓 plugin architecture 可以被用在任何地方
##### DEPENDENCY INVERSION
* 在沒有 polymorphism 之前呼叫就像顆樹一樣彼此有很高的相依性
* 軟架架構的選擇不多因為 control flow 限制了系統的行為
* 有了 polymorphism 讓相依性的關係反轉過來，讓 method 之間相依性變少了
* OO language 有著能夠掌握 control flow 的能力，也因此軟體架構的設計就更多元了
* Component 因為相依性降低就可以獨立部署增加開發的效率
#### Conclusion
* OO 是能夠自由自在控制相依性，因此能夠設計出 plugin archetitect，high-level policies 是獨立的 module 包含 low-level 的細節，low-level 細節使用 plugin 來實作還可以獨立開發
### Chapter 6 Functional Programming
* Functional programming 概念比起 programming 概念更早，起於 l-calculus
#### Squares of Integers
* Java 使用會一直改變的變數在每次的執行去改變程式的狀態，而 Clojure program 沒有可改變的變數，都是使用初始化出來就不會改變的變數
* Functional language 的變數都不會改變
#### Immutability and Architecture
* 會改變的變數會導致 race condition, deadlock condition 跟 concurrent update problem，如果變數都不會改變就不會有這些問題
* Cocurrent apllication 需要使用 multiple thread 跟 multiple processors 所以無法避免使用會變的變數
* 設計 concurrency 的時候在某些情況可以考慮是否可以使用 immutability 來讓設計更強固
#### Segregation of Mutability
* 把系統分成 mutable 跟 immutable 的部分，immutable 的部分只用做單純的功能而不使用任何可變變數，mutable 的部份則會隨著 state 改變
* Transactional memory 在 concurrency 的情況下保護狀態改變
#### Event Sourcing
* 資源越多可以有更快的速度對於 mutable state 的依賴就可以減少
* 例如銀行帳戶如果有無限運算資源可以直接把過去所有的交易紀錄算出帳戶餘額，就不需要維持帳戶餘額這種可變的東西了，只需要靠不變的交易紀錄來算出需要的東西
* Event sourcing 就是把 transaction 紀錄起來而不是 state，當需要 state 的時候才去藉由 transaction 算出來
#### Conclusion
* Strucutred programming 禁止 direct transfer of control
* Object-oriented programming 禁止 indirect transfer of control
* Functional programming 禁止 variable assignment
* 花了半世紀知道什麼事情不該做
* 軟體的本質從一開始就沒有改變，改變的只有硬體跟工具
* 軟體只是由 sequence, selection, iteration 跟 indirection 所組成
