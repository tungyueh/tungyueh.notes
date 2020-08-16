# Building Microservices: Designing Fine-Grained Systems
## CHAPTER 1 Microservices
* Domain-driven design 幫助我們了解如何用 code 來表達現實世界的東西
* Continuous delivery 讓我們更有效率的推出產品
* On-demand virtualization 讓我們可以依照需求改變環境的規模
* Microservices 的出現是來自於融合現代軟體需求所產生
* 大多數公司發現使用 microservices architecture 可以更快速推出產品更使用新技術，更容易應付不同的需求

### What Are Microservices?
* Microservices 是一群小的、自主的 servicess 一起工作
#### Small, and Focused on Doing One Thing Well
* 當不斷加入新功能會使 codebase 變大以至於相似功能散落於各地讓修復 bugs 跟新增功能變得困難，所以在 monolithic system 中，我們盡量讓 code 相似的部分放在一起
* Microservices 使用相同方式讓 services 獨立，注重 business boundary 讓在裡面的 code 很明顯是為此功能所寫
#### Autonomous
* Microservice 是獨立的實體，雖然會有多餘的 overhead 但可以讓我們在分散式系統中更容易理解
* Microservice 彼此都是使用網路來溝通，強迫分離與減少 coupling
* Microservice 要仔細考量那些需要公開那些需要隱藏讓 consumer 與本身的 coupling 減少
* Microservice expose API 與 consumer 合作

### Key Benefits
* 雖然以下好處在分散式系統也是擁有，但 microservices 從根本考量這些好處而設計出來帶來更高維度的好處
#### Technology Heterogeneity
* 如果系統是由多個 service 所合作而成則可以挑選最好的方法來實作個別 service
* 使用不同的技術可以更容易增加系統效能
* 採用新技術可以降低風險
#### Resilience
* 系統一部分壞掉的時候如果可以隔離問題讓其他部分可以正常運作就可以讓系統更加穩定
* Monolithic service 中只要一部分壞掉則全部就會失效，但在 microservices 可以使用 service boundary 當成防火牆避免系統全部失效
* Microservices 系統會有額外的問題需要處理，例如網路失效
#### Scaling
* 對於 monolithic service 需要將全部一起 scale，但 microservices 可以只 scale 需要的部分來節省成本
#### Ease of Deployment
* 些微改動對於 monolithic service 需要全部重新 build 後 release 造成 release 緩慢，進而讓不同版本差異更大、風險更高
* Microservices 獨立 deploy 有改動的 service，當問題發生更容易知道是哪邊出錯馬上 rollback，也可以更快速推出新功能
#### Organizational Alignment
* 小的團隊負責小的 codebase 會更有效率，microservice 讓我們將組織與 service 更加貼近讓團隊人數與需要處理的 codebase 相符
#### Composability
* Microservice 可以讓 service 的功能以不同的方式被使用來適應現代有需多不同的需求
#### Optimizing for Replaceability
* 當 service 都很小的時候可以很容易刪除會重寫

### What About Service-Oriented Architecture?
* Service-oriented architecture (SOA) 是一種設計使用多個 service 一起合作提供系統的功能，這些 service 彼此以網路溝通而不是 process 裡的 method call
* SOA 推廣 service 的重複利用跟容易維護與修改
* SOA 是很好的點子但不知道該如何達成

### Other Decompositional Techniques
#### Shared Libraries
* 提供 library 讓不同 team 可以使用
* 缺點是 library 需要用同一種語言、部屬時候需要跟著 library 一起
#### Modules
* 有些語言提供 modular decomposition 可以 deploy 在 running process

### No Silver Bullet
* Microservice 並不是萬靈丹，使用它同時需要處理分散式系統有的問題，需要更了解部屬、測試、監控等方式

### Summary
* 了解到 microservices 的好處與跟其他 decompositional techniques 的差別

## CHAPTER 2 The Evolutionary Architect
* Microservice 讓我們有很多不同的選擇但也因此需要作出不同的選擇
### Inaccurate Comparisons
* Architect 負責確保技術可以整合來使系統可以給客戶使用，可能是與一個或多個團隊合作，會影響到軟體品質
* 我們嘗試以其他行業的方式來解釋我們正在做的事情，但又無法完全交代清楚，常常處在模糊地帶
* 其他行業的 architect 有嚴格的規定跟紀律，但軟體行業的 architect 沒有對於合格的定義
* 為了讓其他人了解所以挪用了其他專業的名稱，但也造成誤解
* Architect 會讓沉迷在圖表文件之中而遺忘了思考不斷變動的軟體本質，導致設計出來的系統無法彈性的符合需求
* 如果我們將 engineer 或 architect 的概念直接從其他地方挪用則會讓所有人造成傷害，所以需要重新定義符合我們情境的用詞

### An Evolutionary Vision for the Architect
* 軟體需要更快速的推出而且成品需要可以隨時更動符合需求，所以應該要專注於設計好的 framework 讓軟體可以進化成長而非製造出完美不可變動的成品
* IT architect 應該要像城市規劃者專注於考量目前與未來的情況規劃好區域規劃而不是實際去建造建築物
* 軟體就像城市需要迎合使用者的需求而變化而不是設想好所有可能的情形，architect 需要考量開發者跟維護者是否容易根據需求作改動
* Architect 需要知道有沒有按照規劃進行，如果越少規定則需要越多的參與來知道計畫是否有被正確執行，所以需要訂好大致的計畫然後只參與特定的實作
* Architect 需要讓使用者與開發者同樣開心

### Zoning
* Architect 不需要注意 service 裡面在做什麼而需要注意 service 之間怎麼溝通跟如何監控這些溝通來知道系統的狀態
* 雖然 team 可以對自己負責的 service 採用不同技術，但會讓人員難以在不同 team 流動跟難以招募人，如果不同 team 都採用不同的技術則可能會對於如何 scale 這些技術缺乏經驗
* 不同 service 都用不同 expose 技術會讓 consuming services 非常頭痛需要支援多種 interchange，所以要好好注意 service 之間的溝通
* Architect 需要參與 team 的運作來了解開發者的需求才有辦法打造開發者適合的架構

### A Principled Approach
* 設計系統就是再做決定中做取捨，而 microservice architecture 讓我們可以做出很多不同的取捨
* 透過 principles 跟 practuces 可以幫助我們做出好的決定
#### Strategic Goals
* Architect 不太需要定好 strategic goal 通常只需要確認技術跟的上目標就好
* 如果需要訂公司的 technical vision 則需要多花點時間考慮非技術面的部分
#### Principles
* Principle 是一堆 rule 幫助達成目標，有時也會改變
* Principle 數量最好少於 10 個才容易被記住也不容易重複或矛盾
* Heroku's 12 Factors 是一堆設計 principles 幫助 application 可以在 Heroku platform 運作良好。有些 principle 像是無法改變的限制。
#### Practices
* Practices 是如何實踐 principle 的方式包含詳細的實作方式，不同技術都有不同方式，開發者可以直接依照方法來使用
#### Combining Principles and Practices
* 只要可以讓系統進行好的演化則 principle 與 practice 的分別就不重要
* 在大組織中需要把 principle 與 practice 區分，因為不同的技術要有不同的 practice
#### A Real-World Example
* Practice 會規律的變動而 principle 通常保持固定

### The Required Standard
* 設計系統取捨時需要考量需要多少可變動性，考慮 service 之間哪些需要 constant 哪些不需要，在設計 microservice 時候不能失去 big picture，定義好每個 service 的 attributes
#### Monitoring
* 需要有整個系統的狀態，建議所有 service 都會發出狀態跟有統一的監控 metrics
* 不管是用 push 或 poll 的方式集中個 service 的狀態，重點是要統一就好
#### Interfaces
* 使用較少的 interface 來 integrate 是較好的作法
#### Architectural Safety
* 不要讓壞掉 service 毀掉整個系統，每個 service 都要做好 failure handling 避免系統太脆弱
* 根據規則做事才能快速又正確的處理錯誤

### Governance Through Code
* 花時間去確保大家都遵循 guildline 不是很容易做到
#### Exemplars
* 將標準或 best practice 寫成範例提供給開發者可以讓人探索 code 進而遵循
#### Tailored Service Template
* 如果讓開發者很容易依照提供的 guideline 開發就容易遵循 guideline
* 依照需求使用 library 可以減少需要的開發時間，藉由這些 library 讓 team 的開發速度變快而且不會讓 service 太糟
* 使用 library 就會造成選用語言的限制，要考量不用 library 可能造成的後果跟限定技術的好壞
* Service template 要謹慎使用，盡量為 optional，如果強制會讓開發負擔加重
* 過度的 shared code 後可能會造成 service 的 coupling 讓變動變緩慢

### Technical Debt
* 為了短期的利益而造成長期的成本稱為技術債，可能是為了快速推出 feature 所造成的
* 技術債不只是因為想要走捷徑而產生，也有可能是因為技術觀念改變導致系統不合也會
* Archietect 需要注意整個藍圖而了解其中的平衡，讓技術債可以被追蹤與償還

### Exception Handling
* 系統沒有依照所設計的運行就會產生例外，透過將這些例外記錄下來可以重新檢視是否與認知有所不同進而決定是否改變 principle 
* 有規模的公司會有許多限制來避免例外發生，而 microservice 給予 team 充分的自由來親自處理例外

### Governance and Leading from the Center
* Architect 的 technical goverance 是要確保實際的東西符合 technical vision
* Architect 要確保 principle 可以指導開發並且符合組織策略
* Governance 是團體活動需要大家一起不斷討論及改變
* Architect 職責在於讓 group 可以運作，而 group 整個要負責 governance
* 有時候 group 做出 architect 不同意的決定，但 architect 應該要依照 group 的決定因為 group 通常比個人睿智
* Group 是真正開發的人所以要放手讓他們自己決定， architect 要做的事情只是確保事情不會往最糟的方向去

### Building a Team
* 作為負責 technical vision 的人不只是要做出決定而是要幫助成員成長一起構築 vision
* 幫助周圍的人有很多不同的形式，在 microservice 中可以有自己的 service 所以比較容易成長
* 好的軟體通常由好的人所打造，要同時注重技術與人

### Summary
* Architect 要有 technical vision、了解所做的決定的影響、能夠與大家合作構築 vision、vision 能夠隨需求改變、找到標準與自治的平衡、管理團隊有依照 vision 實作
* Architect 能夠理解 feature 都是一連串的取捨之下才產生的，要能夠好好平衡這些取捨

## CHAPTER 3 How to Model Services
* 思考怎麼樣的 microservice boundary 可以使 microservice 帶來的好處多過於壞處

### What Makes a Good Service?
#### Loose Coupling
* Loosely coupled 的 services 代表改動其中一個 service 的時候不需要同時改變其他的 service
* 一個 loosely coupled service 對於合作的 service 了解很少，盡量減少 service 對外溝通的管道，太多的管道容易造成 tight coupling
#### High Cohesion
* 我們希望將相關的行為放在同一個地方，因為當我們需要改變行為需要同時改變很多地方會需要 release 多個 service 造成 deploy 的困難
* 找到 problem domain 的 boundary 來幫助我們將行為放在附近，也讓不同 boundary 的溝通 loose coupling

### The Bounded Context
* Domain 由許多 bouded context 組成，bounded context 裡面的東西不與外界溝通，bounded context 有特定的 interface 來決定與外界如何溝通
* Bounded context 也可以定義為在特定 boundary 中具有特定責任的單位
#### Shared and Hidden Models
* Shared model 不一定要 expose 所有的東西，可以只 expose 該 bounded context 所需要的東西，所以 shared model 會有內部與外部的 representation
* Shared model 有可能相同名字在不同 context 行為不同，context 要各自處理好 shared model 的處理行為
#### Modules and Services
* 藉由思考避免其中一個會造成 tight coupling 的方式來釐清 model shared 的範圍，也可以用 business domain 類似的行為聚在一起來釐清 boundary 達到 high cohesion
* Bounded context 在 codebase 中以 module 來呈現
* 通常 Modular boundary 就是切出 microservice 好的方法，雖然熟練的人會忽略把 bounded context model 成 modular 的過程，但是搞錯 service boundary 的代價是很高的
* Serice boundaries 與 bounded context 一致就算是好的開始，代表 microservice 都代表 bounded context，彼此 looosely coupling 與 strongly cohesive
#### Premature Decomposition
* 對於該 domain 熟悉後才去切成 microservice，不然切錯 microservice 的代價滿高的
* 從現有的 codebase 切成 microservice 比起從頭開始打造 microservice 簡單許多

### Business Capabilities
* 不只要想 shared data 還需要思考 context 需要提供什麼資訊給 domain
* 從 context 需要做些什麼再來想 data 需要提供些什麼，如從 data 思考起會容易讓 service 單純變成 CRUD-based service 而已

### Turtles All the Way Down
* Microservice boundary 先大一點，之後有好處在慢慢變小
* 有可能 bounded context 底下有 hidden bounded context，只是外面沒有察覺，如果把覺得把 higher-level bounded context 的 boundary 取消比較合理就把直接把 hidden bounded context 變成 service 讓外面可以溝通，可以由組織架構來決定要採取何種方式
*  Nested approach 方便測試

### Communication in Terms of Business Concepts
* 系統需要改變通常都是因應商業需求，所以依照 domain 切分的 bounded context 需要改變就只需要改動其中一個 bounded context 而不需要改動很多地方
* Microservice 之間的溝通應該也需要用商業概念來思考，不應只是將商業改變套用在 bounded context 上，

### The Technical Boundary
* 依照組織或地區切分 microservice 是合理的選擇，但依照技術切分而不是依照商業邏輯切分可能會造成 service 無法達到 loosely coupling
* 依照技術區分不一定是錯的，像是為了效能而做出的區分

### Summary
* Bounded context 是一種工具幫助我們區分好問題後根據 boundary 切好 microservice

## CHAPTER 4 Integration
* 正確的整合 microservice 可以保持其自主性，讓我們可以獨立改變跟部屬而不影響其餘部分

### Looking for the Ideal Integration Technology
* Microservice 之間的溝通有很多方式可以選擇，但首先需要先思考我們想要達成什麼目的
#### Avoid Breaking Changes
* 盡量避免本身與 consumer 同時需要一起改動
#### Keep Your APIs Technology-Agnostic
* 讓 microservice 溝通的 API 與技術無關才能不受限制容易採用新技術適應快速變動的環境
#### Make Your Service Simple for Consumers
* 讓 consumer 可以自由選擇想要採用的技術或者提供 client library，但 client library 可能會造成 coupling 的成本
#### Hide Internal Implementation Detail
* 讓 consumer 知道內部實作容易增加 coupling 與改動的成本

### Interfacing with Customers
* Customer 依靠直覺容易做出 CRUD operation 而已，但實際上複雜的多
* 建立一個 customer 需要有帳單跟歡迎信，改變或刪除 customer 也會牽涉很多商業邏輯

### The Shared Database
* Database 是最常用來整合的工具，service 都把資訊存在 DB 讓其他 service 可以存取更改
    * 所有 service 都可以看到 internal implementation 所以改變 schema 容易讓 consumer 壞掉，也導致需要大量的 regression test
    * Consumer 被迫使用特定的技術讓 service 無法輕易改用其他技術來儲存資訊，也容易讓 consumer 對於內部實作依賴太深無法維持 loose coupling
    * Constomer 改變的時候相關的邏輯會散布在不同地方，因為 consumer 可以直接操作 DB 也就知道相關邏輯所以有多個 consumer 就會散布在各處，無法維持 cohension
* Database integration 讓 services 容易 shared data 但沒有 shared behavior，內部實作暴露在外讓人很難避免改壞掉

### Synchronous Versus Asynchronous
* Service 使用同步或非同步的溝通方式會影響到以後的實作而且無法反悔
* 同步的溝通方式可以知道結果是否成功，而非同步溝通則是用在處理時間過長或者需要反應速度快的時候
* 通常使用 request/response model 來實作同步溝通方式，而 event-based 用來實作非同步溝通方式
* Event-based 本質上就是非同步的，發送 event 給訂閱者讓他們自己處理 event，可以很好的 decoupling business logic

### Orchestration Versus Choreography
* Orchestration 代表有一個中央的指揮處理過程，清楚知道每個步驟的成功與否，缺點是邏輯會集中在特定 service 身上
* Choreography 代表告知系統不同的部分的工作讓他們自己負責實作細節，缺點是 business process 不明顯，需要額外的監控追縱確定是否有照想像中執行
* 系統通常比較傾向於 choreography approach，比較容易修改、保持彈性跟更加 loossely coupled
* 同步比較接近我們的思考方式所以比較簡單，而非同步讓我們有辦法採用 choreography approach

### Remote Procedure Calls
* Remote procedure call 是一種在 local 可以呼叫執行程式在 remote service 上就像是在 local 上面
* RPC 讓人可以快速上手使用，因為只要正常的呼叫 method 而理論上可以忽略背後如何跨越網路的 boundary 的細節
#### Technology Coupling
* 有些 RPC implementation 與平台綁的很死進而限制可以使用的技術
* 技術的 coupling 造成 expose implementation detail
#### Local Calls Are Not Like Remote Calls
* RPC 的核心思想是隱藏 remote call 的複雜度但太多的 implementation 藏的太深，local call 與 remote call 本質上就很不同，使用者需要對兩者有不同的認知，直接把 remote call 當成 local call 會造成許多問題
* 對於網路需要假設為不可信任而且會失敗，要能分辨 bad call, server error 不同的問題進而處理，remote server 回應緩慢的時候該如何處理
#### Brittleness
* 新增 method 需要重新產生 client stub 因為 client 要能讀懂新的 method 但不需要讀懂新的 metho 的 client 也需要更新，RPC endpoint 最後都會有大量的 method 用來跟 object 互動
* 更新 object 的 field 也需要同時更新所有的 server 跟 client stub
* Object 需要 serialization 可以想成只能擴充，這樣會造成無法移除不需要的 field 使 field 越來越混亂
#### Is RPC Terrible?
* 雖然 RCP 有很多缺點但也不用馬上斷定 RCP 很糟糕，有很多情境很適合 RPC-based modle 而且現在的 mechanism 有減少過去的缺點
* 挑選 RCP 後要注意缺點，不要把 remote call 關於網路的部分完全隱藏起來，確保 server interface 改變的時候不需要升級 client
* 相較於 database integration，RPC 是比較好的方式但還有不同的選擇

### REST
* Resource 是 service 所認識的東西
* Server 定義 resource 在不同 request 的 representation
* Resource 外部與內部的是完全無關的
* Client 知道 resource 的 representation 後就可以使用 request 去改變
* REST 不管下面的 protocols 但通常都是 HTTP
#### REST and HTTP
* HTTP 定義的東西與 REST 很契合，像是 HTTP verbs 定義與 resource 如何互動跟 REST 規定 method 對於所有的 resource 的動作需要一樣的想法一致
* HTTP 帶來許多支援的工具與技術，像是 caching proxies, monitor tools
#### Hypermedia As the Engine of Application State
* Hypermedia As the Engine of Application State 的概念讓 client 與 server 避免 coupling
* Hypermedia 是一段包含連結到其他內容的內容
* Client 自己藉由 link 來找到自己需要的東西，不需要透過固定的 interface 來與 server 溝通
* 缺點是 client 需要不斷與 server 反覆溝通來找到自己需要的東西
#### JSON, XML, or Something Else?
* 使用標準的 textual format 讓 client 可以有很多彈性來處理 resource
* JSON 比起 XML 更流行但對於 link control 的支援不足，所以 Hypertext Appllication Language 補足 JSON 這部分的不足
#### Beware Too Much Convenience
* Framework 可以快速打造 RESTFul web service 但會為了快速上手讓 internal representation expose 到外部造成 coupling 讓長期的開發變得困難
* 如何儲存資料跟 expose 給 comsumer 很容易主宰思考，可以嘗試直到 interface 穩定下來才去實作內部的細節，確保由 consumer 的需求來驅動設計與實作方式
#### Downsides to REST Over HTTP
* 無法像 RPC 一樣產生 client stub 來簡化 consumption，如果想要實作 hypermedia control 應該要自己做而不是跑在 HTTP 之上
* Web server framwork 不一定支援所有 HTTP verbs
* 效能比不上 binary protocol 因為每個 HTTP request 都有 overhead
* HTTP 不適合 low-latency communication
* Server 之間溝通要求 low-latency 或 message 都很小則 HTTP 不適合
* 處理 payload 需要額外的 work 而 RPC 用 serialization 的方式解決

### Implementing Asynchronous Event-Based Collaboration
#### Technology Choices
* Microservice 要能 emit event 跟 consumer 要能知道 event 發生
* Message broker 可以處理兩種需求，producer 用 API publish event 給 broker，broker 處理 subscription 把 event 通知 consumer
* 不要把太多邏輯放到 middleware 上面，盡量保持 middleware 簡單而把邏輯放在 endpoint 上面
* 用 HTTP 來 propagate event，但 consumer 需要自己管理收到的 event 跟自己去 polling
#### Complexities of Asynchronous Architectures
* Event-driven architecture 讓系統更 decoupled 跟 scalable 但複雜度也跟著提高，除了需要處理 publish/subscribe message 還要思考 long-lived async 管理方式
* 採用 async 技術要很小心，需要有監控工具來追蹤問題

### Services as State Machines
* 不管是用 REST 或者 RCP，核心概念為 service 是個 state machine，customer microservice 有所有的邏輯相關的行為在 context 裡面
* Constomer service 完整控制 customer 本身的 lifecycle 不受外面控制保有 cohesion

### Reactive Extensions
* Rx 是一種把多個呼叫收集再一起後執行 operation 的機制
* Rx 反轉了一般先拿資料後再來操作的方式，而是發現到操作過後的 oepration 後才去反應變化
* Rx implementation 適合在 distributed system，觀察 downstream service 的結果後去反應的方式讓人可以不用管是用 blocking 還是 non-blocking call
* 如果使用多個 call 來執行單一 operation 則可以考慮 Rx 技術

### DRY and the Perils of Code Reuse in a Microservice World
* DRY 更精確來說是不要重複行為跟知識，當行為改變的時候不容易找出所有應該修改的地方
* DRY 讓我們建立 shared code 來避免 duplicated code 但是在 microservice 的世界是滿危險的行為
* Shared code 加深了 microservice 與 consumer 的 coupling
* 不要再 microservice 裡面違反 DRY 但 microservice 之間可以允許違反 DRY，因為 coupling 比起 DRY 更危險
#### Client Libraries
* Client library 雖然可以避免 code duplication 跟讓人容易使用 service 但容易讓 server 的邏輯滲透到 client 形成 coupling
* 可以使用不同的人來寫 client library 避免邏輯從 server 滲透到 client 的問題
