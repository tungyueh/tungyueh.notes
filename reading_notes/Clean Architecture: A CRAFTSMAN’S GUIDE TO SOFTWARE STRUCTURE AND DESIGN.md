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
## PART V Architecture
### Chapter 15 What Is Architecture?
* Software architect 是頂尖 programmer 專注於讓團隊生產力最大化的設計，同時持續進行 programming task 以便了解可能遇到的問題
* 軟體架構讓人對於系統的模樣有基本概念，理解有哪些 component 跟彼此如何合作
* 軟體架構不只是要讓系統可以動，而是要能持續開發跟維護
* 好的軟體架構要能讓系統可以容易理解，容易開發，容易維護跟容易部署，最終目標是讓生產力最大化
#### Development
* 五人以下團隊在初期開發系統容易沒有定好 components 跟 interface 因為不想有太多障礙，所以也造成有很多系統都沒有良好的架構
* 系統由多個團隊共同開發一定需要定好 components 釐清責任才能開始開發，通常 components 數量就是 team 的數量
#### Deployment
* 快速部署讓系統更有用
* 部署方式在開發時期常常被忽略，通常只會以容易快速開發為主
* 例如為了開發方便使用 micro-service 架構但部署時發現需要很多設定也可能部署後有很多錯誤，反而花很多時間在部署上
* 開發初期如果有考慮部署就可能會思考用較少的 services 或其他架構來減少管理的難度
#### Operation
* Operation 遇到的問題基本上可以使用投入更多硬體資源來解決而不需要動到軟體架構
* 好的軟體架構要能讓 operation 知道系統的需求
#### Maintenance
* 維護是永無止盡跟最花成本的事情
* 主要是在現有的架構找到好的地方新增 feature 或修復問題，另外評估改動可能造成的危機
* 好的架構藉由區分出 component 讓改動的影響降低
#### Keeping Options Open
* 軟體架構重要在於可以快速簡單的變更行為，這取決於系統的 componenect 如何彼此合作
* 讓多種可能的選項存在是讓軟體可以持續變動的要素
* 系統可被分為 policy 跟 detail 兩種要素
    * Policy 是指 business rule 跟 procedure 也是系統價值所在
    * Detail 是指讓其他系統跟 programmer 達成 policy 行為的東西
* Architect 要將 policy 視為重要的東西，讓 detail 盡量與 policy 無關，讓 detail 可以推遲到之後再決定
* 越晚決定 detail 可以有更多資訊做出正確的決定
* 有選擇的權利代表可以嘗試不同的選擇來分析比較優劣
* 如果已經被其他人決定好 detail 身為 architect 還是可以假裝這個決定還沒下，把系統還是設計的可以用不同 deatil 取代
* 好的 architect 可以最大化不需要馬上做的決定
#### Device Independence
* 程式綁定特定裝置的話只要換裝置就需要全部重寫，所以發明 device independence 讓 operate system 負責處理不同裝置的行為
* 也就是 Open-Closed Principle
#### Junk Mail
* Policy 是把 name 跟 address record format 好
* Detail 是要用哪種 device 印出來
* 推遲 detail 直到決定要用哪種 device
#### Physical Addressing
* 讓 high-level policy 跟實際存在硬碟格式分離出來才能讓 application 決定要用哪種 detail
#### Conclusion
* Architect 需要仔細的把 policy 跟 detail 分開，讓 detail 的決定都推遲到最後一秒
### Chapter 16 Independence
#### Use Cases
* 系統架構要能符合 use cases
* 系統架構要能清楚顯示出意圖跟行為
#### Operation
* 系統架構要能符合 operation 的規模
* 系統架構有分好 component 之後比較容易轉換成不同架構
#### Development
* 系統由多組團隊開發會需要分出獨立的 component 才能分配給 team 來開發
#### Deployment
* 好的系統架構要能在 build 完馬上部署，而不需要調整一堆設定跟把檔案放到正確的地方
#### Leaving Options Open
* 當不知道需求跟限制時還是有些原則可以依循來平衡不同的考量，留下可以選擇的空間
#### Decoupling Layers
* 雖然無法知道全部的 use case 但系統還是有主要的目的，根據這些依循 Single Responsibility Principle 跟 Common Closure Principle 區分有共同改變理由的部分使用 context 包裝起來
* User interface 改變跟 business rule 改變是不同原因，所以需要分開
* 驗證欄位跟改變計算利息公式雖然都是在 application 裡面但改變的速度跟原因不同也需要分開
* Database schema 也會改變而且不屬於 business rule 或 UI 也需要分開
#### Decoupling Use Cases
* Use cases 也會改變所以也可以用來當成切分系統的依據
* Use case 是垂直切分系統，每個 use case 都會用到一點 UI、一點 application-specific business rules、一點 application-independent business rules、一點 database functionality，所以也要考量如何垂直分割系統
* 每個平行的 layer 要根據 use case 來切分出來
* 之後增加 use case 就很容易在現有系統上支援而不干擾之前的 use case
#### Decoupling Mode
* Use case 有分開則特定 use case 可能需要較高的 throughput 的時候就可以支援
* UI 跟 database 有跟 business rules 分開就可以在不同 server 上跑
* 為了支援 operation 需要有不同的 mode，為了在不同 server 上跑，component 不能 depend address space，而是需要變成獨立的 services 可以透過 network 溝通
#### Independent Develop-ability
* Compoenets 有被清楚分開且定好 interface 才可以讓不同團隊有效率的合作
#### Independent Deployability
* Use case 跟 layer 切分好之後就有很高的部署彈性，就可以 hot-swap layer 跟 use case，增加新 use case 也很容易
#### Duplication
* 真正的 duplication 是指每次改變 instance 都需要改變 duplicate 的 instance，而兩個初期看起來是 duplicate 的 instance 以不同的速度跟原因改變就不是 duplication
* 不同 use case 有相似的 screen structure 但其實最後會有不同的結果，要注意在初期不要統一起來，不然之後很難切分
#### Decoupling Modes (Again)
* 可以在 source code level 或 binary code level 或 execution unit level 做 decoupled
* Source level: 切分 source code 成 module 但還是使用相同的 address space 跟使用 function call 溝通，是個 monolithic structure
* Deployment level: 使用 jar files, DLLs, shared libraries 讓在 source code module 的改變不會影響到其他人需要 rebuilt 跟 redeployed，但還是在相同的 address space 跟使用 function calls 溝通
* Service level: 使用 network packet 減少 data structure 跟 function calls 溝通的方式
* 很難知道系統要用哪種程度的 decoupling
* 預設從 service level 開始 decouple 但成本更高而且會鼓勵更大的 component 產生
* 建議先把 decoupling 做到 service 可以成形就好，先盡量保持在相同 address space 留條後路就好
* Component 由 source code level 所分開的，等到在開發或部署有問題在使用 deployment level 的 decoupling
* 當 development, deployment, operational 的問題增加就把 deployable unit 轉換成 service，讓系統慢慢朝向這個方向
* 當 operational 需求降低，則 service level 的 decoupling 變少而 deployment-level 跟 source-levle decoupling 變多
* 好的系統架構能夠從 monolith 變成很多 deployable unit 在變成 services，也能慢慢變回來 monolith
#### Conclusion
* Decoulping mode 會隨著時間變動，而好的 architect 能夠遇見未來適當的採用不同的 mode
### Chapter 17 Boundaries: Drawing Lines
* Software 早期的 boundaries 用來推遲下決定的時間，讓這些決定不會污染到 business logic
* 過早的決定容易造成 coupling
#### A Couple of Sad Stories
* 過早決定複雜的系統架構讓開發不容易而且也不一定用的上
#### FitNesse
* 先用自製 web server 推遲採用何種 web framework 的決定
* 先用 data access interface 存取資料推遲採用何種 database
* 先用 data access method interface 但先不要實作這些 method，之後再來決定開如何實作
* 只將實作細節做到目前需要的部分，最後發現足夠使用就停下來決定用該方式實作細節，避免使用太複雜但不需要的功能
#### Which Lines Do You Draw, and When Do You Draw Them?
* 彼此不相關的元件就該區分開來
* BusinessRules 只靠 DatabaseInterface 來存取資料，DatabaseAccess 實作 DatabaseInterface 去 Database 拿資料
* BusinessRules 跟 DatabaseAccess 都知道 DatabaseInterface 的存在但 DatabaseAccess 的存在沒人知道
* DatabaseInterface 算是廣義的 BusinessRules 的一部份，而 DatabaseAccess 算是廣義的 Database 的一部份
#### What About Input and Output?
* 使用者只看到 GUI 而不知道背後是如何運作的
* GUI 依賴於 Business Rules，所以 GUI 可以換成其他 interface
#### Plugin Architecture
* 系統開發通常都是用 plugin 建立 scalable 跟 maintinable 的系統架構
* User interface 是個 plugin，可以做出不同的 user interface
* Database 是個 plugin，可以使用不同種類的 database
#### The Plugin Argument
* Business rule 不該因為其他 component 的改變而壞掉
* Plugin 架構建立起防火牆避免讓改變擴散到所有 component
* Boundary 根據改變的速度建立，變動速度一樣的可以歸類在一起
* Single Responsibility Principle 也是在告訴我們 boundaries 在哪邊
#### Conclusion
* 把架構 boundary 建立起來，business rule 是一類，其他都是 plugin
* Dependency Inversion Principle 跟 Stable Abstractions Principle 幫助我們認知彼此依賴的關係
### Chapter 18 Boundary Anatomy
#### Boundary Crossing
* 在 runtime, function 建立起 boundary 用來管理 source code dependencies
* Source code module 之間的 boundary 用來管理跟建立防火牆
#### The Dreaded Monolith
* Monolith 有一個可以執行的檔案，內部使用 polymorphism 去管理內部的 dependencies
* Monolith 裡面 component 使用 function call 彼此溝通
* Monolith 裡面溝通很快所以通常溝通的很頻繁
#### Deployment Components
* Dynamically linked library 用成可以部署的單位，部署過程可以不用 compilation 直接把這些組合起來用
* 跟 monolith 一樣因為溝通成本很低所以很常溝通
#### Threads
* Thread 用來安排 schedule 跟執行的順序，不算是 boundaries 或 deployment unit，會在一個或多個 components 使用
#### Local Processes
* 可以跑在一個 processor 或一組 processors 裡面但不同 address space
* 使用 socket 或其他 OS 的溝通方式
* 溝通成本稍微多一點因為使用 OS calls 跟 data 需要經過 marshaling 跟 decoding 跟 process 有 context switches
#### Services
* 不依賴於 phyiscal location，彼此透過網路溝通
* 溝通很慢所以避免很頻繁的溝通，需要處理 latency 的問題
#### Conclusion
* 系統有很多種劃出 boundary 的方式
* 系統會有很頻繁溝通的 boundaries 也有需要注意 latency 的 boundaries
