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
### Chapter 19 Policy and Level
* 架構裡面的 component 會形成 directed acyclic graph，node 就是 component 而 edge 代表 component 之間的相依性
* 好的架構裡面的 components 的相依性都是同個 level，low-level components 只會相依於 high-level components
#### Level
* Level 是指 input 跟 output 的距離， policy 跟 input output 的距離越長的距離代表越高的 level，管理 input 跟 ouput 的 policies 是最低 level 的 policies
* 我們想要 source code dependencies 不相依於 data flow 然後相依於 level
* Level 越高的越不容易變動，low level 通常變動頻繁、比較緊急、理由比較不重要
* 讓 source code dependencies 相依於 higher-level policies 讓 low level 的改動不會影響到 high level
* Low-levle components 應該要是 higher-level components 的 plugin
#### Conclusion
* Policies 包括 Single Responsibility Principle, Open-Closed Principle, Common Closure Principle, Dependency Inversion Principle, Stable Dependencies Principle, and Stable Abstractions Principle
### Chapter 20 Business Rules
* 想要將架構分成 business rule 跟 plugin 則需要知道真正的 business rule 是什麼
* 能夠賺錢或省錢的 rule 就是 business rule
* Business rule 只要 business 還存在就一定會有不論是手工去執行或者自動化執行
* Business rule 需要資料而這些資料稱為 Busines Data
* Rule 跟 Data 緊緊聯繫在一起所以適合變成 object，將這種 object 稱為 Entity
#### Entities
* Entity 包含重要的 business rule 跟 business data，interface 包含 function 去實作 business rule
* 只把重要的 business 放在裡面不管用哪種 database, user interfae, thir-party framework，所以可以在任何系統使用
#### Use Cases
* 有些 business rule 用規範 automated system operate 來賺錢，所以在手動的環境沒有意義
* Use case 只有定義 application-specific rule 而沒有說明 user interface 或在那種平台上跑
* Entities 沒有 use case 的知識，所以 Entities 是 high-level 而 use case 是 low-level
* Use case 只針對特定 application 而且距離 input 跟 output 所以比較 lower level
* Entities 對於多個 applications 做 generalization 所以跟 input output 的距離比較遠，是 high-level
#### Request and Response Models
* Use case 的 input output data 要是個簡單的 data structure 不要依賴於任何 framework interfaces
* Data structure 可能會有很多 reference 到 Entity object 但這是因為本來就需要 share 很多資料，不過因為變動的速度跟原因不同所以不能放在一起，不然會有很多 tramp data 跟 conditional
#### Conclusion
* Business rule 是系統存在的原因，是核心的功能，讓 code 可以賺錢跟省錢
* Business rule 要保持跟其他 concern 分離，其他 concern 要用 plugin 的方式讓 business rule 可以獨立跟 reuse
### Chapter 21 Screaming Architecture
* 從 top-level directory structure 跟 source files 應該要能看出這是什麼類型的系統
#### The Theme of an Architecture
* 軟體架構是為了讓系統可以符合 use cases
* Framework 只是一項工具而軟體架構不該建立在這上面而是應該建立在 use case 上面
#### The Purpose of an Architecture
* 好的軟體架構要專注在 use case 而不是 framework, tools 跟環境
* 好的軟體架構要能提供細節實作的各種選擇讓決定可以推遲到最後一刻
#### But What About the Web?
* Web 只是一種最後呈現機制不應該主導系統架構，系統架構要能讓人能夠選擇最後的呈現方式
#### Frameworks Are Tools, Not Ways of Life
* Framework 代表作者背後相信的東西，根據所相信的東西所打造讓人可以將所有事情都在 framework 完成
* 使用 framework 之前要仔細考量如何使用，如何不被 framework 綁架，如何專注在系統的 use case 上面
#### Testable Architectures
* 因為系統架構針對 use case 則需要單獨能夠對 use case 做 unit-test
* Entity object 要是單純的 object 沒有任何 dependencies，才能夠單純測試 use case
#### Conclusion
* 系統架構要能從 source repository 看出是那種系統
* 讓人知道有哪些 use case 而不需要知道細節如何實作
### Chapter 22 The Clean Architecture
* 之前提出過的系統架構基本上都用分層的概念切分系統，通常有 business rules layer, user interface layer, and system interface layer
* 不依賴 framework，把 framework 當成工具使用避免讓系統被 framework 限制
* 系統要可以被測試，讓 business rule 可以單獨抽出來測試
* 與 UI 獨立因為 UI 很容易變動，business rule 不應該隨著 UI 變動
* 與 database 獨立因為可以換 DB，business rule 不應該因為換 DB 而變動
* 與 external agency 獨立，business rule 不該知道外部的 interface
#### The Dependency Rule
* 走進越深的地方就是愈高的 level，外圍是 mechanisms 內部是 policies
* Source code 應該要往內依賴，依賴於 higher-level policies
* 內部不應該對於外部有所認識，外部的東西不該被內部使用，因為內部的東西不該被外部影響
##### ENTITIES
* Entities 封裝了重要的 Business Rule，單純只是 object 或 data structure 跟 function
* 如果只是寫一個 application 則 entities 就是 business object 包含 high-level rules，不受外部影響最少改變的 object
##### USE CASES
* Use cases layer 包含 application-specific business rules，封裝跟實作 use case，處理資料流進跟流出 entities，使用 Critical Bussiness Rules 來達成 use case 的目標
* Use cases layer 不會影響 entities 也不受到外部的影響
##### INTERFACE ADAPTERS
* 把外部資料轉成讓 use case 跟 entities 容易使用的格式
* 把內部資料轉成 framework 方便使用的格式
##### FRAMEWORKS AND DRIVERS
* 基本上都是 glue code，細節都在這邊實作
##### ONLY FOUR CURCLES?
* 有時候需要更多層的 layer，不過只能依賴於內部的原則仍是一樣
##### CROSSING BOUNDARIES
* 資料從外部流到內部在流到外部但是 layer 都還是依賴於內部
* 使用 Dependency Inversion Principle 讓依賴的方向性不同於資料的流向
##### WHICH DATA CROSSES THE BOUNDARIES
* 跨 boundary 的資料應該要是基本的資料型態才能保持獨立性
* 傳到內部的資料都是要以內部方便使用為優先
#### Conclusion
* 使用 layer 並遵照 dependency rule 可以讓系統被測試跟之前提到的所有好處
### Chapter 23 Presenters and Humble Objects
#### The Humble Object Pattern
* 把容易測試跟不容易測試的行為分開，Humble module 包含不容易測試的行為，其他則包含容易測試的行為
* GUI 很難被測試但使用 Humble Object pattern 可以把行為分成 Presenter 跟 View
#### Presenters and Views
* View 是 humble object 包含難以測試的行為，裡面的 code 要盡量簡單只是把資料放到 GUI 而不進行處理
* Presenter 是可以被測試的 object，把從 application 接受的資料轉換成讓 view 可以簡單移到 screen
* Screen 上出現的東西都是在 view model 以 string, boolean, enum 來呈現，View 只是把 data 放到 screen 上面
#### Testing and Architecture
* 可被測試跟不可被測試也是一種 architectural boundary
#### Database Gateways
* Use case 跟 database 之前的就是 database gateway，有著 polymorphic interface 包含 method 可以對 database 做操作
* Use case layer 不能直接使用 SQL 所以要用 gateway interface，這些實作在 database layer，這些是 humble object，單純只用 SQL，而 use case interactor encapsulate application-specific business rules 所以可以被測試
#### Data Mappers
* Object relational mapper 不存在因為 object 不是 data structure，user 使用看到 public method 而不會看到 data
* Data structure 是一堆 public data 不包含行為所以 ORMs 最好被稱為 data mappers，因為他們只是把資料從 relational database tables 放到 data structure
* Data mapper 在 database layer 也是 Humble Object
#### Service Listeners
* Application 需要跟其他 service 溝通則會有 Humble Object pattern 建立 boundary
* Application 會放資料到 data structure 跨越 bounday 送到 service，而 service listener 會從 service interface 接收資料然後 format 成 application 容易使用的格式
#### Conclusion
* Architectural bounday 常常會看到 Humble Object pattern，跨越 bounday 都會看到簡單的 data strcutre 的移動，容易測試跟不容易測試也會漸漸有 boundary，使用 Humble Object pattern 讓整個系統更容易測試
### Chapter 24 Partial Boundaries
* 完整的 boundaaries 所要付出的代價很昂貴，需要有 Boundary interface 跟 Input Output data structure，管理兩邊能夠獨立相容跟部署需要付出很多心力
* Architect 需要考量 boundary 的成本跟預留空間以便真的需要 boundary
* 不能因為過度設計就沒有考量到 boundary 的需求，折衷方案是使用 partial boundary
#### Skip the Last Step
* Partial boundary 的其中一種做法是先把 boundary 需要的東西都準備好只是放在同一個 component
* 所需要的 code 還是一樣只是不用分成多個 componet，減少 version number tracking 跟 release managment 的工作
#### One-Dimensional Boundaries
* 維護雙向的獨立性讓初始化設定跟後續維護比較困難
* 使用類似 Strategy pattern 保留日後變成完整的 boundary 彈性
#### Facades
* 更簡單的 boundary 是使用 Facade pattern，沒有 dependency inversion 但是透過 Facade class 設立 boundary 把 service 變成 method
* 缺點是 client 慧根 service class 有 dependency，改動 Service classes 需要 Client 重新 compile
#### Conclusion
* 提供三種方法實作 boundary
* 每種方法都有其代價跟好處都需要去取捨，但都可以變成完整的 boundary 或把 boundary 移除
* Architect 需要去決定到底需不需要 boundary 要做完整的還是部份的
### Chapter 25 Layers and Boundaries
* 簡單的系統而也通常由 UI, business rules 跟 database 所組成
#### Hunt the Wumpus
* 不同語言的 UI 要跟 business rules 切乾淨才能 reuse business rules，使用跟語言無關的 API 跟不同語言的 UI 溝通讓 UI 翻譯
* 狀態可以選擇不同的實作所以 business rule 不應該知道狀態儲存如何實作，使用 API 與狀態儲存溝通
#### Clean Architecture?
* 需要找到所有可以畫出邊界的元素，例如 UI 不只有語言會造成改變還有互動的機制
* 考慮未來可能造成邊界的情況設計好架構
* Business rule 定義好會如何使用 API 讓不同語言各自去實作，再來語言也定義好會如何使用 API 讓互動機制實作，所以 API 是由使用者所定義的而不是實作的人
* Business rule 裡面有 Boundary interface 被 Business Rule 使用跟 UI 實作的 code 使用
* Business rule 會處在最上層而 UI 傳遞使用者的輸入給 business rule 後由 business rule 存到 data storage
#### Crossing the Streams
* 如果支援多人就需要多一個網路的組件讓整體變得更複雜了
#### Splitting the Streams
* Business rule 裡面也是還有分 high level 跟 low level
* 分出 high level 跟 low level 就需要 API 來溝通
* high level 跟 low level 形成一個 architecture boundary
#### Conclusion
* Architectural boundaries 到處都存在需要仔細的找出來，需要認知到 boundaries 的存在做好準備避免之後需要花很多時間來增加 boundaries
* 有智慧的思考哪些 boundary 需要被完整實作哪些需要部份實作哪些應該先忽略
* 不是一開始就要決定根據系統演化仔細觀察
* 常常衡量實作 boundary 的成本跟忽略的代價
### Chapter 26 The Main Component
* 每個系統至少都有一個 component 負責建立跟協調其他 component，這個 component 稱為 main
#### The Ultimate Detail
* Main component 是系統的入口負責建立 Factories, Strategies 跟其他 global facilities 之後接給系統的 high-level 的部分
* Main ccomponent 的 dependencies 需要被 Dependency Injection
* Main component 是 dirty low-level module 位於最外圈，把東西載入之後接給 high level
#### Conclusion
* Main 是 application 的 plugin 把所有基本設定好轉交給 high-level policy，如果有多個 plugin 就會有多個 Main components
* 把 Main 想成 plugin component 則 configuration 的問題會比較簡單一點
### Chapter 27 Services: Great and Small
* Service-oriented 或 mirco-service architectures 很流行因常常被認為具有 strongly decoupled 跟 development 跟 deploymeny 的inenpendence 但其實只有部分是對的 
#### Service Architecture?
* Services 只是用更昂貴一點的 function call 並沒有把 high-level policy 跟 low-level detail 依照 Dependency Rule 分開
* Service 並不是全部都是為了架構而切分成 service 
* 只要 function call 有跨越不同 boundaries 並且遵守 Dependency Rule 則就有架構的概念
* 具有形成架構的 service 是需要關注的
#### Service Benefits?
* 到底 service 帶來什麼好處
##### THE DECOUPLING FALLACY
* Service 雖然可以在變數上 decoupled 但還是可能在 shared resources 上 couple
* 例如 data record 增加欄位後就必須改變所有有用到的 service，這些 service 就間接的 couple 在一起
##### THE FALLACY OF INDEPENDENT DEVELOPMENT AND DEPLOYMENT
* Service 並不是唯一可以建造出 scalable system 的選項
* 只要 service 之前對於 data 或行為有 couple 就無法分別獨立開發跟部屬
#### The Kitty Problem
* 只要加入新功能就需要全部的 service 做改動代表這些都無法獨立開發部屬跟維護了
* 使用功能面的切法對於新功能的加入是很脆落的，必需要改變所有的行為
#### Objects to the Rescue
* SOLID 設計原則可以幫助我們建立可以多樣擴展性的 class
* 增加 feature 的時候只需要改 UI 就代表 decoupled 其他 service
#### Component-Based Services
* Service 也可以根據 SOLID 原則來設計，component structure 有設計好就可以新增 component 而不影響現有的 component
* 讓新功能在 service 的內部架構下可以新增就可以不影響到其他 service
#### Cross-Cutting Concerns
* 為了不被 cross-cutting concerns 所影響需要再 service 裡面設計好 component archecture 遵循 Dependency Rule
* Service 沒有定義 boundaries 但裡面的 compenent 有
#### Conclusion
* Service 讓系統具備 scalability 跟 develop-ability 但不一定是切架構的元素，能夠依循 Dependency Rule 定義 boundaries 才能形成架構
* Service 可以是被 boundary 包在裡面也可以是多個 service 裡面切好 component 形成 boundaries
### Chapter 28 The Test Boundary
* 測試也是系統的一部分
#### Tests as System Components
* 以架構的觀點所有測試都是一樣的
* 測試也遵循 Dependency Rule，非常 detail 可以想成最外圈的部分，沒有任何東西 depend on 測試
* 測試通常只被部屬在測試環境而非正式環境
* 測試不是系統運作的必要組成，負責支援開發而非運作
#### Design for Testability
* 測試如果沒有在系統設計考量進去則系統容易變的脆弱不容易改動
* 測試跟系統 couple 後只要系統改動就需要改很多測試
* 如果只是簡單的改動卻讓成千上百的測試壞掉則稱為 Fragile Tests Problem
* 只要測試都需要特定功能初始化改動這些功能就會讓許多測試壞掉
* Fragile tests 會讓系統變得僵化因為開發者知道改動會造成很多測試壞掉就拒絕改動
* 測試不該依賴於容易變化的東西，測試需要設計才能夠直接測到想要的部分而不需要依賴其他東西
#### The Testing API
* Testing API 要能用來直接測試所有 business rules 繞過安全限制，database 讓系統可以直接被測試
* Testing API 目的是要讓測試跟 application decouple
##### STRUCTURAL COUPLING
* Test suite 的 test class 測了每個 production class，test method 也對應測了 production 的每個 method，則 test suite 就跟 application 的 structure deeply coupled
* 當 production class 或 method 改變則需要改變很多 test 則這些測試就很 fragile 讓 production code 變的僵化
* Testing API 隱藏 application 的結構讓 production 跟 test 可以各自以自己的方式做進化而不影響彼此
* 隨著時間測試會變的更具體而 production 會變的更抽象，structural coupling 讓兩者無法各自進化下去
##### SECURITY
* Testing API 如果部署在 production 系統上會變成危險的部分，應該要把測試分開成可以獨立部署的元件
#### Conclusion
* 測試是系統的一部分要能好好的設計提供穩定性跟 regression，而不是反而造成系統僵化的原因
### Chapter 29 Clean Embedded Architecture
* 軟體不像硬體會耗損所以需要不斷修改
* 軟體雖然不會耗損但會因為依賴於硬體的複雜度而被摧毀
* Firmware 是依賴於硬體需要跟著硬體一起演化
