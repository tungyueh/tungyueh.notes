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

### Access by Reference
* 當我們收到 service 給的 resource 我們所看到的是當前狀態，隨著時間過去，真正的狀態會與當前狀態漸行漸遠
* 使用存在記憶體裡的 resource 去改變需要包含 original resource 的 reference 能夠拿到最新狀態
* 與其命令 email service 去寄信倒不如送 Customer 跟 Order resource URI 讓 email server 去檢查是否需要寄信比較好，因為狀況隨時可能改變，一但寄信的 event 被放到 queue 就比較難改變了
* 當 resource 改變了則我們需要知道目前的狀態，所以需要有 reference 才能知道目前狀態
* 查看 reference 會造成 service 太過忙碌，如果 client 可以知道 resource 更新時間則可以使用 caching 減少負擔

### Versioning
* 通常都會覺 service interface 會改變所以需要 versioning
#### Defer It for as Long as Possible
* 最好降低改變得時候壞掉的機率是鼓勵 client 的好行為跟避免與 server 綁的太死
* Client 只讀需要的資料並且要預期 server 的資料結構都有可能改變，讓 code 對結構改變有一定容忍度
#### Catch Breaking Changes Early
* 如果 change 會造成 consumer 壞掉要確保儘早壞掉
* 如果無法避免 consumer 壞掉則避免所有的 consumer 一起壞掉或者與 consumer 溝通
#### Use Semantic Versioning
* Semantic versioning 讓 client 可以看到 service 的 version 判斷是否能夠整合
* MAJOR.MINOR.PATCH: MAJOR 數字遞增代表與之前不相容，MINOR 數字遞增代表可以與之前相容，PATCH 代表功能性的修正
* Semantic versioning 將很多資訊與預期放在簡單的三個欄位省去很多溝通成本
#### Coexist Different Endpoints
* 為了避免 client 需要一起在 lock-step 升級，可以使用 service 同時包含新舊版本的 endpoint，讓新的 interface 可以推出而且給 consumer 時間去從舊的換到新的，當所有 consumer 都沒有用到舊的就可以把舊的 code 移除
* 當有多個版本的時候要區分好 request，將所有 request 到 V1 endpoint 轉成 V2 request，然後送到 V3 endpoint，這樣才能清楚的刪掉不需要的 code
#### Use Multiple Concurrent Service Versions
* 同時有不同版本在線上服務 consumer，缺點是修 bug 需要在不同版本修正後部屬、需要將正確的 consumer 導流到正確的 endpoint、需要將不同版本的資料一起呈現出來，這些都增加了系統複雜度
* 短時間同時存在不同版本是合理，有可能需要做 blue/green deployment 或 canary release，不同版本共存時間如果過長應該要考慮用不同 endpoint 而不是同時存在不同版本

### User Interfaces
* User interface 需要以整合的方式來思考因為終究是 user interface 把 microservice 集合起來呈現有意義的東西給 customer
* Desktop client 通常都比較大而 Web client 通常比較小因為主要都在 server 上去處理，但隨著 JavaScript 興起有些 application 也會變得跟 desktop 差不多大小
#### Toward Digital
* 因為無法預期 customer 會如何使用服務所以採用 granular API，組合不同 service 的功能 expose 不同的方式來提供不同的體驗
* User interface 應該要用不同層的組合方式來思考
#### Constraints
* User 與系統互動的時候在不同裝置會有不同的限制
* Interface 提供的核心功能都是一樣的但要能找到方式讓 interface 能夠適應不同的限制，
#### API Composition
* Service 透過 HTTP 使用 XML 或 JSON 與彼此溝通才 user interface 很合理就直接使用這些 API，如果使用 binary protocol 則 web-based client 整合比較困難
* 這種方法無法為不同 device 打造適合的內容，client 只能自己說想要那些欄位來製造自己想要的回應但 service 需要支援這項功能
* Client 需要使用很多 API call 來與 service 溝通，雖然可以使用 API gate way 來 aggregate 但有缺點
#### UI Fragment Composition
* 與其讓 client 不斷呼叫 API 打造出 UI 倒不如由 service 把提供的功能組合成 client 方便做出 UI 的樣子
* 這種方式適用於使用 coarser-grained fragment 組合成 UI
* Fragment 最好與 team ownership 一致
* 需要 assembly later 把不同部分組合起來
* 優點是負責 service 的 team 也負責 UI 的這個部分
* 使用者體驗需要一致，不同部分的風格要一致
* Client 不是使用 Web 則無法使用此方法
* Web 需要在不同地方使用互動式的功能時則此方法無法提供功能只能回到原本的 API call
#### Backends for Frontends
* Server-side aggregation endpoint 或 API gateway 把多個 backend calls 組合起來提供給不同 device 來減少 backend service 收到過多的 API calls，缺點是 servder-side endpoint 可能會包含太多行為與邏輯在裡面然後由不同 team 維護，所以當改變功能的時候這邊也需要更改
* 一個太巨大的 layer 在所有 service 上面會導致所有東西都放進裡面開始失去獨立性，藉由限制 backend 只服務一個特定 user interface 可以改善問題
* Backends for Fromtends(BFFs) 讓 team 可以專注在特定 UI 上面處理 servder-side components，這些 backend 當成 UI 的一部分嵌入在 server 上
* BFFs 也有把邏輯放進不應該放的位置的問題，business logic 應該只待在 service 上面，BFFs 應該只有對特定 interface 的行為 
#### A Hybrid Approach
* 不一定只能採用其中一個方法，可以依據不同方法在不同 interface 上面，只要確保要維持 cohesion 的前提下提供 user interface

### Integrating with Third-Party Software
* 有時候需要整合很少控制權的軟體
* 如果軟體是很獨特才自己打造不然就買，這樣 CP 值比較高
#### Lack of Control
* 如果要擴充或整合 COTS 產品通常都由 vendor 決定如何擴充或整合
* 不管好不好整合都有部分控制權掌握在別人手上，重點在於如何把控制權找回來
#### Customization
* 購買現成品再要求高度客製化的成本會比從頭客製化來的高，所以與其要求高度客製化倒不如改變組織的運作方式
* 現成品的升級可能會導致客製化的東西失效
#### Integration Spaghetti
* 整合 tool 必須使用產品的溝通方式，所以需要整合不同的溝通方式造成困難
* Tool 如果可以直接使用 data store 可能會造成 coupling
#### On Your Own Terms
* 全部東西都從頭打造是不合理的，使用 COTS 或 SaaS 產品整合重要的是要把東西移回自己的 team 裡
* 客製化要坐在自己的平台上面然後限制 tool consumer 的數量
#### The Strangler Pattern
* 想要擺脫不受控制的 legacy platform 可以使用 Strangler Application Pattern
* 攔截送往舊系統的呼叫後，決定繼續送往舊系統還是新系統，這樣就可以不用大量改寫就可以慢慢換掉功能
* Microservice 可以使用一連串的 microservice 來達成攔截呼叫的功能

### Summary
* 介紹不同整合的方式，使用保持 microservices decouple 的方式來整合是最好的
    * 不要使用 database
    * 最好使用 REST 而不是 RPC
    * Choreography 比 orchestration 好
    * 避免需要同時改動 client 跟 server 的 change
    * Reader 要能適應 server 回應的改動
    * User interface 用 compositional layer
* 整合外部產品需要注意控制權在自己身上

## CHAPTER 5 Splitting the Monolith
* 目前已經知道好的 service 的模樣但現存的 codebases 沒有遵守好的 pattern，需要在不重頭改寫的情況下分解
* Monolith 會隨著時間成長大到大家都不敢改動，但使用正確的工具可以好好處置

### It’s All About Seams
* Monolith 通常無法達成 highly cohesion 跟 loosely coupled，雖然只改一行 code 但就需要重新佈署整個系統
* Seam 概念原本是一段 code 可以獨立運作而不影響剩下 codebase 的 code，但在這邊使用 service boundaries 來代表
* Bounded context 可以用來當作 seam，因為根據定義就是在組織裡有 cohesion 但夠 loosely coupled boundaries，所以第一步先要找出 code 之中的 boundaries
* 大部分的程式語言都有 namespace 的概念來幫助我們把相似的 code 放在一起

### Breaking Apart MusicCorp
* 假設我們有一個 monolithic service 包含所有 MusicCrorp's online system 功能，為了拆分首先需要釐清 high-level bounded context，主要可以分成四大類
    * Catalog: 任何銷售產品的 metadata
    * Finance: 帳戶、付款、退款的報告等等
    * Warehouse: 處理客戶訂單跟管理庫存等等
    * Recommendation: 嶄新的推薦系統裡面有由 PhDs 寫出的很多複雜的 code 
* 首先建立 namespace 代表 bounded context 然後把相關 code 放進去，需要測試確認搬移 code 是否導致壞掉，剩下不知道搬去哪邊的 code 很有可能是遺忘的 bounded context
* Code 代表組織而 package 代表 bounded context，所以 package 之間的互動要跟現實中一樣，如果發現不一致就可以找到問題並解決
* 不需要把所有 code 都放到相對應的 package 才能開始切分出 service，可以先把重要的切分出來，之後慢慢一步一步慢慢切分

### The Reasons to Split the Monolith
* 不用一次把 monolith 切成多個 service，慢慢的切分出來可以減少出錯的機率也可以學習到 microservice 的概念
* 從能得到最多好處的 service 開始慢慢切分出來，不要只是為了切分而切分
#### Pace of Change
* 切分出來的 service 可以更快的改動
#### Team Structure
* 切分出來的 service 可以讓 team 有完全的控制權
#### Security
* 切分出來的 service 可以增加更多安全性的機制
#### Technology
* 切分出來的 service 可以採用不同的技術
#### Tangled Dependencies
* 從最低 dependency 的 seam 切分出來幫助釐清複雜的 dependencies

### The Database
* 因為用 databases 來整合 service 會有很多困難點所以為了不要用 databases 要找出 databases seam 以方便切分

### Getting to Grips with the Problem
* 第一步找出 code 去存取 DB 的部分，通常使用 repository layer 跟 framework 來把 code bind 到 DB 方便對應 object 或 data structure 到 DB
* Database mapping code 跟使用的 code 放在一起方便理解 code 使用到 DB 的部分
* 知道 code 使用哪個 table 但無法知道 table 之間的關係，所以要利用工具來視覺化 table 之間的關係，然後切開這些 coupling 變成 service boundaries

### Example: Breaking Foreign Key Relationships
* 原本不同領域的 code 需要存取其他人的 table 得到想要的資訊在 mircorservice 世界中要變成使用 API 來獲取資訊才能切開 coupling
* 讀取 DB 速度會快於用 API 但如果使用 API 的速度是可接受的則用速度降低來交換 decoupling 是值得的
* Foreign key relationship 會因為使用 API 喪失掉，所以要轉由 service 來負責管理關係，根據使用者期望的行為來做出適當的管理

### Example: Shared Static Data
* Table 複製多份到不同的地方，缺點是不容易維持一致
* Property file deploy 到 service，雖然還是有需要維持一致性的缺點但比起改 DB 改檔案比較容易
* Push static data 到 service 

### Example: Shared Data
* 不同類別的 code 使用同一個 DB，這就是典型的 domain concept 沒有對應到 code，而是把 domain concpet 放進 DB 裡面
* Domain concept 的 DB 拉出來成一個 service 讓不同類別的 code 使用 API 來存取資訊

### Example: Shared Tables
* 不同類別的 code 使用同一個 table 的時候要把 table 分開變得更專屬於該類別的 table

### Refactoring Databases
#### Staging the Break
* 找出 databases seams 先 release monolithic service 但用不同 schema，之後才把 service 切開
* Schema 分開後需要更多對 DB 的操作，可能在 join 資料用到更多 memory，可能破壞掉 transactional integrity，所以先維持 monolithic service 可以 revert 到之前狀態

### Transactional Boundaries
* Transaction 確保所有動作都完成或都失敗，讓我們可以對多個 table 同時 update 但只要有一個失敗又會回到之前狀態，確保狀態可以正確的改變
* Monolithic schema 下所有 create 或 update 都在一個 transactional boundaries 之下，當我們切開成不同 schema 則失去一個 transaction 確保的狀態
#### Try Again Later
* 當失敗的時候可以之後再嘗試做一遍操作，這樣可以達到 eventual consistency
#### Abort the Entire Operation
* 回朔之前成功的操作回到先前的狀態，還要考慮到 service 是否有做好回朔的處理
* 回朔的操作也有可能失敗
* 如果要回朔多個操作會變得很困難
#### Distributed Transactions
* 透過 transaction manager 來編排不同的 transactions，確保狀態保持一致，橫跨不同系統透過網路溝通
* Two-phase commit 是一種演算法處理短期的 distributed transactions，第一階段 voting phase 詢問大家 local transaction 是否可以 commit，如果大家都說可以則會通知大家可以 commit，只要有一個說不行就通知大家 rollback
* Two-phase commit 缺點是大家都需要停下來等回應，所以很容易壞掉，只要 manager 或一個人在 voting phase 不回應就會 block 大家，另外回答了可以但實際失敗會讓其他人誤會操作已經 commit 成功了
* Distributed transactions 已經有在特定的技術上實作了而且邏輯很複雜所以不要自己做，找別人已經做好的
#### So What to Do?
* Distributed transactions 很難做對而且讓系統不易規模化，回朔操作的邏輯不容易理解而且需要額外的行為來修正不一致的狀態
* 遇到商業邏輯的操作出現在 single transaction，思考是否一定要這樣做，看看是否可以用 local transaction 或 eventually consistency 的概念
* 如果一定要保持一致性則盡量避免切分，但如果還是需要切出來則創造一個 concrete concept 代表的這個 transaction，如此一來讓我們能夠監控管理這些複雜的 transaction

### Reporting
* Service 切成多個部分會讓資料分散所以 report 變得不容易產生
* 改變架構不應該捨棄 reporting，要把使用 report 的人也是跟一般使用者一樣考量需求

### The Reporting Database
* Report 通常都是要從不同的地方收集資料後形成有意義的內容
* Monolithic service architecture 把資料都存在一個 DB，產生 report 的時候用 SQL queries 就可以拿到想要的資料，通常為了避免加重 DB 的負擔會使用另一個 DB 定期同步，從該 DB 拿取資料
* 資料都集中在同一個地方很容易查詢但也造成 monolithic service 跟 report system 跟 DB schema 綁死，改變 schema 需要特別注意所以沒有人會想改動
* 資料都集中在同一個地方讓可以優化的選項不多，running system 跟 reporting system 所使用的 data structure 一樣，所以改變 data structure 可能會造成其中一方變慢
* 現在 DB 的選擇有很多，running system 跟 reporting system 如果想要用不同的 DB 來讓自己更好就無法辦到

### Data Retrieval via Service Calls
* 使用 API call 對 source system 拿取需要的資料
* 如果要拿取大量的資料時候會讓操作變得緩慢，也不能先把資料存在 local 因為就算是歷史資料也可能會改變
* Reporting system 通常會用 thir-party tool 去拿取資料，所以提供 SQL interface 方便整合
* Microservice expose API 通常不是為 report 所設計，所以為了拿取需要的資訊會用很多 API call 也會增加 service 的負擔
* Cache header 可以加速資料拿取的速度但通常 reporting system 拿取的資料都是第一次所以會造成很多 cache miss
* Expose batch API 讓 reporting 更簡單
* 作者不太喜歡 batch 的方式，覺得應該要更間單的方式可以更有效率的規模化

### Data Pumps
* 與其讓 reporting system 不斷用 API call 去拿取資料不如直接把資料送給 reporting system，直接定期從 service 的 database 拿資料後送給 reporting system DB
* 直接使用 DB 整合會造成 coupling 但為了讓 reporting system 容易實作還是可以接受的
* Data pump 由該 service 的團隊所實作，減少跟 schema coupling 的問題
* Data pupmp 需要知道 service 跟 report 的 schema，負責對應 service 到 report
* 建議將 service 跟 data pump 的 version-control 放在一起，把 data pupmp 當成 service 的一部分
* Report schema coupling 還是存在著，但我們要把它當成 publish API 不會輕易改變
* 有些 DB 提供 materialized view 去建立 aggregate view 為每個 service 建立專屬的 schema，這種做法可以減少 data pump 與 report schema coupling
#### Alternative Destinations
* Data pump populate JSON file 到 Amazon S3 用 S3 當成 data mart
* Data pupmp 可以改成 populate cude 讓 Excel 或 Tableau 可以整合

### Event Data Pump
* State change event 發生的時候 event data pump 送資料到 reporting database
* 避免與 source service database 產生 coupling，改成跟 service 的 change event 產生 coupling，change event 本來就是 expose 給大家知道所以比起與 database 產生 coupling 與 change event 產生 coupling 是更好的選擇
* 可以更及時的傳送資料到 reporting database 比起定時送資料好
* 如果有把處理過的 event 存下來則只需要處理新 event 就可以了，不像 data pump 需要自己管理要送那些資料
* 不跟 service internal 綁再一起所以可以由別的 team 來負責，可以更獨立的發展
* 缺點是所有需要的資訊都要變成 event 廣播出來，不容易規模化
* 如果已經有 expose event 則可以考慮這種方式所帶來的 looser coupling 跟即時的資料

### Backup Data Pump
* Netflix 結合現有 backup solution 跟規模化的問題所使用的選擇
* Netflix 複製 Cassandra data 成 SSTables 存在 Amazon S3 object store
* Netflix 需要從這些資料產生 report 所以使用 Hadoop 用 SSTable 當作來源產生 report
* 使用備份資料作為 data pump 來源 

### Toward Real Time
* Report 大部分都是從不同地方收集資料產生，但不同的 report 對於資料的精準度與即時性有不同的要求，所以可能用不同技術來產生 report

### Cost of Change
* 了解每次改變的代價與目的，這樣才能讓我們能夠減少改變發生錯誤的機率
* 不同的改變會有不同的風險，盡量在風險小的時候嘗試錯誤
* 思考如何讓改變所產生的錯誤最小化，先提出設計再用 use case 看看情況如何
* class-responsibility-collaboration (CRC) cards 列出 service 的職責跟合作的 service，用 use case 慢慢知道該如何把所有東西串再一起

### Understanding Root Causes
* 要在 service 成長到很難分割之前就去分割，但雖然知道大的 service 不好但往往還是很難再適合的時機分割
* 讓分割 service 變得容易，像是使用 framwork 或有環境讓人測試之類

### Summary
* 一點一滴的找出系統的 seams 後切分出來，這期間還是可以讓系統正常的成長應付需求，也因為慢慢的改變所以不用害怕去改變它

## CHAPTER 6 Deployment
* Microservices 比起 monolithic application 部署更為困難，如果沒有採取好的方式會一團混亂
* 使用 CI 跟 CD 的觀念幫助我們做出如何部署的決定

### A Brief Introduction to Continuous Integration
* CI 目的是讓所有人保持同步，確保新進的 code 可以跟現有的 code 整合好
* 進行 CI 過程中會產生 artifact 用來做更進階的驗證，用來部署那個版本的 code，這樣可以確保部署的跟測試的是同一個
* CI 讓我們快速得到目前 code 的品質，自動產生 binary artifacts，build artifact 所需要的 code 都有版本控制，可以從 artifact 回推 code
#### Are You Really Doing It?
* 要經常檢查有沒有與其他人的 change 整合好，不然越晚整合會越難
* 如果 CI 沒有檢查行爲的正確性則無法知道整合的正確性
* 當發現 build 失敗後要馬上停止整合，先把當次整合失敗的問題修正後再繼續整合，不然堆積越多的 change 會導致修正時間快速增長

### Mapping Continuous Integration to Microservices
* Single source code repository: 當有任何 check-in 時則跑過所有驗證後把所有 artifact build 出來
    * 優點: 很直覺
    * 缺點: 需要接受 lock-step release，全部 service 要一起 deploy，不需要驗證的地方都重新驗證過讓時間拉長，無法知道哪些 artifact 是需要 deploy，如果 build 失敗則全部的人都無法做任何 change 直到修正完畢
* 另一種變形是 CI 只看部分 source tree，如果結構有設計好很容易對應 build 跟 code，缺點是容易同時 commit 多個 service 造成 coupling
* 一個 micorserve 用一個 source code repository 跟一個 CI build，可以快速驗證後 deploy
* microservice tests 要放在 source control 確保知道跑了哪些測試

### Build Pipelines and Continuous Delivery
* Build 過程中可能會有好幾個階段，例如測試可能有快慢之分，分階段可以更快得到回饋，不必等全部的測試都跑完才知道，build 中分為不同階段稱為 buld pipeline
* Build pipeline 幫助追蹤進度，當 build 出來的 artifact 通過越多階段則越有可能在 production 可以正常運作
* CD 讓我們可以持續得到每個 check-in 的回饋，可以把每個 check-in 都當成 release candidate
* CD 標準流程是 compile&fast tests -> slow tests -> UAT -> Performance testing -> Production
* 使用以 CD 為概念所設計的工具，不要使用其他工具來做 CD 的事情這樣會讓系統變的很複雜
* CD tool 要能夠讓人自己定義跟視覺化 pipelines，某些階段可以是手動的，像是做完手動測試後標註該階段通過後 CD 發現到會自動往下走流程
* 藉由 modeling 從 source code 到 production 可以視覺化軟體品質，減少 release 之間的時間
* Microservices 期望可以獨立 deploy 所以一個 service 會有一個 pipeline

#### And the Inevitable Exceptions
* 一個 microservice 一個 build 是目標但有時候可能會有例外，像是初期的 microservice 可能對於 service boundaries 還不清楚就先保持大的 microservice 後等徹底理解在慢慢往目標前進
* 釐清 service boundaries 的時期讓所有 service 都在一個 build 減少跨 services changes 的成本
* 把所有 service 綁在一起 build 等到穩定後開始讓 serivce 有自己的 build，如果又不穩定則可以在 merge 回去，讓自己有足夠的時間摸清楚 domain

### Platform-Specific Artifacts
* 很多語言都有 artifact 跟 tool 可以建立跟安裝，Ruby->gems, Java->JAR, Python->eggs
* Microservice 運用自動化的設定管理工具可以使用 artifact 直接提供服務
* 缺點是 artifact 綁定在特定技術，如果使用多種技術會變得很困難
* 自動化可以把不同 artifact deployment 的差異隱藏起來但還是有更簡單的 artifact 可以使用

### Operating System Artifacts
* 使用原生於系統上的 artifact 可以避免綁定特定技術的問題
* 優點在於我們不需要知道底下使用何種技術，只需要安裝在 OS 上面就可以使用
* 缺點是不容易產生 OS artifact，另外如果要 deploy 在不同 OS 則管理變得困難
* 使用 OS-based package management 可以簡化部署的流程

### Custom Images
* Automated configuration management systems 挑戰在於需要花很久的時間在機器上跑 script，隨著需要安裝的軟體則時間會越久
* 雖然有些工具會檢查已經安裝過就不用安裝但檢查還是需要花時間，如果機器會需要常常開開關關也會很耗時間
* 同樣的工具不斷的被重新安裝會讓 CI 流程變慢，downtime 時間變長
* 使用已經安裝好常用的工具的 virtual machine image 可以減少安裝時間
* 因為已經在 image 當裝好常用的工具所以大大節省時間，只需要安裝最新版本的 service，可以不斷重複利用這些 base image
* 缺點是 build image 需要很久，image 也有可能變很大造成用網路需要傳很久
#### Images as Artifacts
* 除了把常用的工具放到 image 也可以直接把 service 放進 image 讓部署可以更節省時間
* OS-specific package 讓我們不用在乎是用何種技術提供服務，我們需要的只是可以運作就好，接著我們就可以把注意力放在自動建立部署這些 image
#### Immutable Servers
* 藉由把 configuration 放進 source control 可以讓我們不斷自動產生出 service，但如果有人做了不再 source control 裡面得修改就會造成 configuration drift 讓 code 不跟 running host 一樣
* 為了避免對於 running host 做修改，所有修改都需要進入 build pipeline 後產生新 machine，為了確保不能被修改可以關掉 SSH
* 確保我們需要的資料都有存在別的地方讓我們更直覺部署跟理解環境

### Environments
* CD piepline stages 可以是不同的環境，這些不同的環境差異會有一些問題
* Service 在測試環境中部署在單一機器上沒有問題但部署在 production 環境上用兩台機器做備援就可能會發生 replication session state 失敗的情形
* Service 部署在不同環境有著不同的需求，個人電腦需要快速的部署，production 上需要有多個 service 互相備援
* 從 laptop 到 build server 到 UAT environment 到 production 需要越來越接近實際的環境才能儘早找到問題，但接近實際環境成本很高需要做取捨，另外使用接近 production 的環境會造成 feedback loop 變慢

### Service Configuration
* Service configuration 理想上要盡量少而且在不同環境都是一樣的，如果不同的環境都有不同的 configuration 則 service 的行為會不同會造成很多問題
* 雖然可以為每一個環境都做出包含 configuration artifact 但是就違反 continuous delivery 將每個 build 當成 release candidate 的原則，因爲不同環境用不同 build 所以無法確保在測試環境通過也會在 production 通過測試
* 需要更多時間去 build artifact，另外還需要處理 sesitive configuration 的問題
* 比較好的方式是一個 artifact 但不同的 configuration 跟 properties file 來管理不同環境

### Service-to-Host Mapping
* 用 host 來代表 unit of isolation，如果直接跑在 physical machine 則就是一台 host，使用 virtualization 則可以有多個 host 跑在 physical machine 上
* 多少個 microservice 跑在 host 上是合理的？ 以下是不同的因素可以考量要用何種 model 也要注意某些 model 會限制 deployment 選擇
#### Multiple Services Per Host
* 多個 service 跑在一個 host 上容易管理而且成本較低
* 部署多個 service 在一個 host 上跟開發的環境類似
* 監控變的困難，不知道是哪個 service 造成 workload
* Service 會互相影響
* 部署變困難，需要確認 deployment 不會影響到其他 service，雖然可以將所有 service 綁在一起部署但就失去 microservice 的好處
* 不同 team 部署 service 在同一個 host 需要花費協調的成本，讓 team 的自主性變低
* Scaling 變得困難，如果有特定 service 需要不同的環境則無法辦到
* 很多部署跟 host 管理都是為了好好利用稀少的 resource
#### Application Containers
* 多個 service 在一個 application container 之中便於管理
* 減少 language run times 的 overhead
* Application container 有很多缺點需要確定是必須使用才用
* 需要綁定特定技術，除了限制可能的選擇還會影響到自動化管理工具的使用
* 特定平台上也會對於管理不容易，無法監控個別 service
* 這種方式也是為了優化使用的 resource，建議使用 self-contained deployable microservice as artifacts
#### Single Service Per Host
* 一個 host 只有一個 service 避免 single points of failure 跟 host 斷掉就整個 service 失效
* 如果沒有可用的 PaaS 就可以用這種方式可以有效的減低複雜度
* 需要管理更多 server 跟需要更多成本，但還是推薦這種方式
#### Platform as a Service
* 多數 platform 依賴於特定技術的 artifact
* Heroku 是不錯的 PaaS solution
* PaaS solution 如果沒問題就可以用的很好，如果有問題就難以解決
* PaaS solution 通常都是設定成一般 application 使用，所以如果 service 不是一般就容易無法使用

### Automation
* 當機器數量不多得時候可以使用手動管理，但數量一多就需要自動化管理
* 手動管理的難度會隨著 host 數量增加而變能導致無法使用 single-service-per-host setup，如果用自動化管理則不用擔心 host 數量的增加
* 就算不增加 host 還是會有很多 service 所以還是需要自動化處理 multiple deployments 
* 自動化可以確保開發者保持生產力，理想上開發用的 tool chain 要跟 production 上的一樣才能儘早發現問題
* 使用自動化才能控制 microservice architecture 複雜度
#### Two Case Studies on the Power of Automation
* 一開始花時間去找尋正確的工具的成本可以在後期做回收

### From Physical to Virtual
* 透過找出把 physical machine 切分是讓我們能夠管理大量 hosts 的關鍵
#### Traditional Virtualization
* Virtualization 讓我們能夠把 physical server 分成好幾個 host 以節省成本
* 無法無限制的切分機器因為切分越多需要更多管理資源
#### Vagrant
* Vagrant 是好用的 deployment platform 通常用於開發跟測試，不再 production 使用
* Vagrant 提供 virtual cloud 在 laptop 上面，透過定義 VM 之間的網路在檔案就可以幫我們設定好
* 簡單模擬出 production 環境，還可以測試各種 failure mode
* 缺點是對系統效能負擔很重，如果太多 VM 可能會無法建立好環境
#### Linux Containers
* Linux container 建立一個不同的 process space 讓其他 process 活在裡面
* Lunux container 是整個 system process tree 的一個 subtree，有 physical resources 配置
* 不需要 hypervisor，每個 container 用自己的 OS 但 kernel 是共用的
* Linux container 速度比起完整的 virtual machines 還要快
* Container 天生輕量化的特性讓我們可以跑很多在同一個 host，也就可以一個 container 跑一個 service
* 外面需要 route 到 container 裡面，需要設定 IPTables 將 container expose 出來
* Container 不是完全獨立，設計上或 bug 都會讓 container 之間可以溝通
#### Docker
* Docker 是建立在 container 上的 platform 幫忙處理 container 的事情
* 在 Docker 上我們建立部署 apps，也就是 image
* Docker app 幫我們隱藏底下的技術還可以存在 Docker registry
* 可以在 Vagrant 起一個 VM 跑 Docker 後，用 Docker 快速的跑個別 services
* CoreOS 只提供最基本的功能讓 Docker 可以跑起來減少需要的資源
* Docker 就像是 PaaS 運作在 single machine 上，如果需要管理多個 Docker instance 就需要找另外的軟體來管理
* PaaS solution 都會把 abstraction level 搞錯但 Docker 做得很正確
* Docker 已經被使用在很多公司因為提供lightweit container 增加效率跟快速的部署


### A Deployment Interface
* 一致化的 interface to deploy 是很重要的，我們希望不管是 dev test prod 都是使用相同的方式
* 最合理去啟動 deployment 是一行有參數的指令
* 要知道我們 deploy 哪個 build 所以需要給名稱，也需要知道我們想要何種版本
* 要知道我們要 deploy 到哪種環境
* Python Fabric 很好的支援 commnad-line calls
#### Environment Definition
* 要能夠知道目前環境是如何跟 service 在環境裡看起來怎麼樣

### Summary
* 先是讓一個 service 可以穩定獨立的 release
* 一個 host 只放一個 service，自動化是管理的關鍵
* 確保 deployment 過程是簡單的，使用 tool 才能容易 deploy 到不同環境

## CHAPTER 7 Testing
* 了解測試的種類考量上架速度與軟體品質的平衡

### Types of Tests
* 技術面向的測試是開發人員對於系統第一線的測試，像是 unit tests 跟 performance tests，通常為自動化的測試
* 商業面向的測試提供非技術人員了解系統如何運作，end-to-end tests 或者手動測試
* 不同面向的測試都有其地位端看系統的特性，最近趨勢都是朝向自動化測試才能快速有效率測試軟體，如果還大量使用手動測試就應該先轉成自動測試再繼續往下走

### Test Scope
#### Unit Tests
* 通常只對於一個 function 或 method 做測試，是 TDD 過程中產生出來的，過程不會啟動 service、限制使用外部檔案、不使用網路，速度非常快通常不到一分鐘
* 幫助開發者快速發現 bugs，涵蓋 service 中小範圍的獨立測試
* 主要可以得到快速的反饋也可以無後顧之憂的 refactoring
#### Service Tests
* Service tests 主要用來測試 user interface 跟 service 本身
* 讓測試可以不受其他 service 影響方便找出跟修復問題
* 如果不使用真實 database 跟通過網路與其他 service 溝通則測試速度還是很快
* 比起 unit test 測試的範圍大，所以發現問題比較難修復
#### End-to-End Tests
* 測試整個系統
* 通過測試可以對於整個系統很有信心
#### Trade-Offs
* 測試的範圍越大我們會越有信心但反饋的速度很慢也難以找到問題點
* 大範圍測試失敗會先想寫出快速的 unit test 來抓到問題點，這樣可以不斷增加反饋的速度
#### How Many?
* 範圍越小的測試要越多，有不同種類的測試
* 當大範圍的測試太多可以用 test coverage 的方向換成小範圍的測試

### Implementing Service Tests
* Service test 測試整個 service 的功能為了要獨立測試需要 stub out 其他合作的 service
* CI build 第一步是 create binary artifact 才能 deploy
* Service test 需要 launch stub service 並且設定好 service 到 stub service 的連線，接著設定 stub service 的 response 模仿現實的 response
#### Mocking or Stubbing
* Stub service 只會回應固定 response，如果需要回傳變動的 response 則需要使用 mock
* 使用 mock 的時候要注意如果預期的呼叫沒有出現則 test 就會失敗，如果過度使用 mock 會讓測試變得容易壞掉
* 有時候 mock 再確認預期 side effect 的時候很有用，stub 跟 mock 的平衡要抓好，通常 stub 會用的比 mock 多
#### A Smarter Stub Service
* Mountebank 像是個小型 software appliance 可以控制需要的回應，只用一個就可以 stub 多個合作 service

### Those Tricky End-to-End Tests
* Microservice 系統的功能都是透過 user interface 由許多 service 合作提供的
* End-to-end tests 需要把所有 service 都跑起來才有辦法測試，測試的範圍大帶來的信心高但速度慢難以診斷問題點
* 假設其中一個 service 有新版要做 end-to-end test 需要把所有 service 跑起來但不知道其他 service 要用哪種版本，因為其他 service 可能也有新版本要上到 production
* 如果有一堆 service test 需要 deploy 其他很多 service 會造成重複的測試
* 用 multiple pipelines 就可以當有 service build 就會 trigger end-to-end tests

### Downsides to End-to-End Testing
* End-to-end testing 有很多缺點

### Flaky and Brittle Tests
* 當測試範圍變大則影響的因素就越多，有時候雖然功能沒有壞掉但因為其他因素讓測試失敗
* 越多影響因素越容易讓測試失敗，這種測試容易因為重跑就好所以對於該測試不太信任，這類測試稱為 flaky test，這類測試失敗也無法告知太多訊息，透過不斷重跑期待下次會好會慢慢累積 check-in code 最後發現很多功能都壞掉了
* 當我們發現 flaky test 就要試著移除，因為人類很容易習慣成自然就把測試失敗當成常態而不去理會
* 如果無法修正 flaky test 則先記錄下來後從目前測試移除，之後嘗試改寫移除不穩定因素或者換成小範圍測試，有時候直接改系統讓測試變得容易也是方法之一
#### Who Writes These Tests?
* 每個 team 為自己的 service 寫測試是很正常的，但 end-to-end test 測試包含很多 team 所以不知道該由誰來寫測試
* 所有 team 都可以來寫的話變成測試會太多，如果失敗大家都會認為是別人的錯所以最後就會忽略
* 有些組織會讓一個 team 專門負責寫測試，缺點是開發 code 的人需要等別人來寫測試，而且寫測試的人也不太知道該如何修正測試
* 不想 double effort 跟讓開發與測試的距離太遠，可以把 end-to-end test suite 當成 shared codebased 加上 ownership，大家都可以
#### How Long?
* End-to-end test 需要跑一段時間，但很少人會去減少 overlap 跟加速
* 速度慢加上 flaky test 是個嚴重的問題，測試時間長而且失敗後可能功能是正確的，就算功能失敗大家早就將注意力放到其他事情上，還需要再轉回來處理
* 可以藉由平行跑測試來解決但不是移除不需要的測試的替代方式
* 移除重複的測試是不容易的事情，很難說服大家帶來的好處比壞處多
#### The Great Pile-up
* 當修正整合測試後會有越多的 check-in 堆積起來讓 building 越來越困難
* 越大範圍的 deployment 則 release 越危險，所以盡量頻繁的 release 就會讓小的 change 準備好就 release
#### The Metaversion
* 既然 end-to-end tests 通過代表整個系統成功，為何不給所有 services 一個版本後一起 release?
* 雖然沒有 microservice 的好處但也是可以接受一起 release 的方式
* 一起 release 容易造成 coupling，這些 coupling 會造成比 single monolithic application 更嚴重的問題

### Test Journeys, Not Stories
* 雖然 end-to-end test 有很多缺點但在只有一到兩個 service 的情形下還是有辦法管理，但太多 service 管理困難度會陡升
* 每實作一個功能就增加一個 end-to-end test 就會使測試數量爆炸很難很快的得到 feedback
* 最好專注於小數量測試整個核心系統，任何沒在核心系統測試到的需要另外測試該 service ，主要測試 service 之間的互動

### Consumer-Driven Tests to the Rescue
* End-to-end test 主要想解決 deploy 到 production 的 service 不會讓 consumer 無法使用，所以可以用 consumer-driven contract (CDC) 來讓我們不需要真實的 consumer 幫忙測試
* CDC 定義 consumer 對於 service 期待的行為，CDC 成為 CI 的一部分確保改動沒有讓預期行為壞掉，重點是要個別 service 進行 CDC 測試才能快速的得到 feedback
* 最好與實際 consumer 的 team 一起寫測試
* 因為 CDC 是 customer service 該有的行為所以可以跟 customer service 一起測試就好，所有 downstream dependencies 都可以 stub out
* 如果 CDC 測試壞掉也代表 customer service 也會壞掉，這時候可以修好問題或與 customer 討論解決方法
* CDCs 幫助我們不需要 end-to-end tests 就可以找出 breaking change 
#### Pact
* Pact 是個 consumer-driven testing tool
* Consumer 先用 Ruby DSL 定義好 producer 的預期行為後跑 local mock server 產生 Pact specification file，這些檔案讓我們能夠跑 mock server 之後用來獨立測試 consumer
* Producer 用 Pact specification file 產生 API call 後驗證 response
* Pact specification file 由 consumer 所產生所以要把這些給 producer 使用，可以利用 CI/CD 或 Pact Broker 來把這些檔案放到不同的 repository 裡面
#### It’s About Conversations
* CDC 是 service API 討論的結果，如果壞掉可以觸發關於 API 要如何演化的討論
* CDC 只有在 consumer 與 producer service 互相信任下才有辦法運作良好，有時候需要自己扮演 consumer

### So Should You Use End-to-End Tests?
* 大家慢慢傾向使用 CDC 類型的 tool 或 monitoring 來取代 end-to-end tests
* 不斷增進各種技巧像是 CDC, monitoring, deployment 後 end-to-end test 變成只是用來用 cycle time 變成換取降低危險，如果不危險就可以把不需要的 end-to-end test 移除掉
* 就算在開發環境中把所有問題排除也不能保證上到 production 會沒有問題，所以 production 還是需要 monitoring 跟 remediation

### Testing After Production
* Systen 進入 production 之前都會根據期待的行為做好測試，但如果有未預期到的行為就會造成問題
* 改善方式為不斷定義更多的測試企圖在production system 減少錯誤，但到某個時間點會發現很難再減少錯誤
#### Separating Deployment from Release
* 透過在部屬環境跑測試可以抓到更多在尚未部屬之前測試的問題，smoke test suite 就是部屬之後跑測試確保部屬成功，所以部屬之後應該要自動跑 smoke test suite
* Blue/green deployment 讓我們部屬不同版本的 service，只有其中一個 version 接受實際的 requests
* 部屬新版 service 後跑 smoke test 成功就把 request 從舊版導到新版，舊版留一段時間讓我們發現新版本有問體可以快速 fallback 回來
* Blue/green deployment 需要做到可以把 production traffic 導到不同 hosts，另外需要配置足夠的 hosts 讓不同版本可以同時跑起來
* Blue/green deployment 除了可以在 production 測試還可以達成 zero-downtime 的 deployment
#### Canary Releasing
* 分流一些 production traffice 到新版本的 service 上面觀察是否運作正確，包含功能面、效能面、結果面，錯誤率是否和舊版差不多
* 如果新 release 壞掉可以趕緊把流量導到舊版，如果運作良好就可以逐漸增加流量，比起 blue/green deployment 版本共存的時間更久
* 使用 canary releasing 需要考量要分流 production request 還是複製到兩個 service，好處是可以比較 respsonse，壞處是更複雜而且對那些不是重複的 reqeust 結果會不同的 service 來說更加複雜
* Canary deployment 是很好用的技巧來驗證新 service 對於 production traffic 的反應，同時可以管理 release 壞掉 service 的風險，雖然比較複雜而切需要更多設備來應付不同版本長時間的共存 
#### Mean Time to Repair Over Mean Time Between Failures?
* Blue/green deployment 或 cananry releasing 都是讓測試更接近於 production 環境而且可以管理壞掉的情況
* 有時候把時間花在如何快速修復問題比起更多測試還要有效益，mean time between failures(MTBF)跟 mean time to repaire(MTTR) 要互相取捨
* 減少 recovery 的時間只要有好的監控機制就可以快速 rollback，也可以部屬新版本後跑測試來及早發現問題
* 大部分的公司都把時間花在建立 functional test suite 但很少把時間花在 monitoring 跟 recovering 上面，所以常常第一時間發現有問題但無法修復
* 有人實際使用的時候才去做測試，要先提出東西來證明概念可行

### Cross-Functional Testing
* 有一些非功能面的需求無法單純用 feature 來呈現，像是 acceptable latency, 支援 user 的上限，interface 對於 disabilites 的支援，資料安全性
* Cross-functional requirement(CFR) 更適合用來呈現以上的需求
* CFR 只能在 production 達成，被分類再 property testing 裡面
* 某些 CFR 我們可能想要追蹤每個 service level，其中會有 trade-offs
* CFR 測試也要 follow pyramid too，有些可能是 end-to-end test 有些則不是
* 提早檢視 CFR 並定期 review
#### Performance Tests
* Performance tests 可以幫我們確認符合某些 CFR，而且對於 microservices 更為重要因為會有更多跨 network boudary 的呼叫所以會更慢，而且在 synchronuous call 的情形下只要其中一個部份出問題就會讓效能顯著下降
* 從 core journey 開始做 performace test
* 為了產生有價值的結果需要漸漸增加模擬的 customer 這樣可以讓我們觀察不同數量造成的 latency，就算無法接近 production 可以幫助我們找出 bottleneck
* 定期跑 performance tests 才容易找出問題點
* 要去看結果，要把結果用通過或失敗來表現，不然單純看數據無法知道是否正確

### Summary
* 測試要能快速得到 feedback，不同種類的測試要分開
* 使用 consumer-driven contracts 取代 end-to-end tests
* Consumer-drive contracts 提供不同 team 該注意的重點
* 了解該如何取捨要花時間做測試還是花時間監控偵測問題

## CHAPTER 8 Monitoring
* 雖然把系統分成 microservice 帶來許多好處但也讓監控在 production 上的系統變得困難，本章會講解遇到的困難跟提供解法
* 發生問題時如果是 monolithic application 就容易調查，但 microservice 有太多可能的原因變得難以調查
* 監控 microservice 需要監控多個 server 所以會有很多 logfiles 另外有很多個地方像是網路會造成問題，可以監控細節後用 aggregation 看到 big picture

### Single Service, Single Server
* 監控 host 本身的 CPU 跟 memory，需要知道正常情形的數值，如果超過就可以警告
* 我們需要能從 server 上拿到 log 來釐清問題，因為只有一個 host 所以可以直接登入用 command-line tool scan log
* 監控 application 本身，至少可以記錄 response time 

### Single Service, Multiple Servers
* 當 CPU 很高的時候，要能知道所有 hosts 的整體情況也要能知道個別的情況
* 使用 tool 像是 ssh-multiplexers 讓我們能夠在每一台 hosts 能執行相同動作收集 log
* Response time 可以透過 load balancer 來得知，但有可能 load balancer 的資訊有問題就會誤導我們，這時候我們應該把心力放在正常的 service 應該是如何來讓 load balancer 可以移除不正常的 service

### Multiple Services, Multiple Servers
* 如何從分散在多個 hosts 上的成千上萬的 log 找出問題?
* 如何知道是 server 有問題還是系統的問題?
* 如何追蹤橫跨多個 hosts 的 call chain 來找到問題點?
* 答案是盡可能收集所有的 log 後集中 aggregation

### Logs, Logs, and Yet More Logs…
* 當越來越多的 hosts 則收集 log 會越來越困難，所以需要用特別的子系統收集 log 後集中放在一起
* Kibana 可以讓我們用 query syntax 來找 log

### Metric Tracking Across Multiple Services
* 收集夠多 log 後困難的是如何知道狀態是好是壞，需要等系統跑一陣子之後才會有 pattern 出現
* 複雜的系統上我們會常常配置新的 instance 給 service 因此需要能夠簡單收集新的 hosts log 也要能夠知道全面系統的 metric 或者 service 的所有 instance 或者特定 service 的特定 instance，之類的各種需求
* Graphite 能夠及時送 metric, 查詢 metric 產生圖表, 還可以降低舊 metric 的資料詳細度避免佔用太多空間
* 如果能夠了解 usage pattern 就能讓系統的 infrastrure 剛好符合需求，所以了解系統的趨勢是很重要的

### Service Metrics
* OS 可以藉由安裝工具來產生 metrics
* 建議 service 要有最基本的 metric 例如 web service 就要有 response time 跟 error rate
* 需要有 metric 的理由如下
    * 大約有 80% 功能從沒被用到所以有 metric 才能知道那些功能會被使用
    * 了解使用者的習慣來改善系統
    * 不知道有什麼資料是有用的所以先收集所有資料再來分析
* 有很多 libraries 讓我們 service 送 metric 到系統

### Synthetic Monitoring
* 設定 service 正常狀態的數值後當數值異常就發出警告
* 設定正常的數值並不能代表系統正常運作，還有許多因素會影響到系統
* 藉由 fake event 來檢視結果是否正常來測試系統是否運作正常
* Fake event 稱為 synthetic transaction
* 使用 synthetic transaction 來測試系統是否正常稱為 semantic monitoring
* Semantic monitoring 不像 low-level metric 常常有 noise 可以比較貼近系統正常運作，但還是需要 low-level metric 來幫助我們找出問題點
#### Implementing Semantic Monitoring
* 使用 end-to-end testing 的 subset 來做 semantic monitoring
* 測試資料需要貼近實際的資料
* 確保假資料不會走到正常流程

### Correlation IDs
* 一個 initial call 會觸發需多的 service call 當出現問題的時候就難以分析
* 要能夠從許多 service call 還原出 stack trace 來分析問題
* 使用 Corelation IDs 把 initial call 上一個 GUID 後傳下去每個之後的 call，把 log 結構設計好就可以用 log aggregation tool 來追蹤 event 
* 常見問題是當狀況發生時候才想要引入 corelation IDs 但要增加這個功能需要每個 service 的配合，所以建議盡量早點加入這個功能
* 為了讓每個 service 都可以簡單使用 corelation IDs 可以建立 shared client wrapper libraries，隨著規模擴大會越難保證 service 都有用正確的方式呼叫，如果決定要用 in-house client library 讓 service 不須要使用特定方式呼叫則要注意不用與特定 producer service couple

### The Cascade
* 當 service 網路不通但狀態正常會讓我們沒有發現問題，雖然用 synthetic monitoring 可以發現問題但還是需要 report 來知道哪個 service 不通
* Serice 也需要 expose 所用到的 database 跟 service 的狀態才能讓我們還原出 big-picture 找出問題點

### Standardization
* 對於 monitoring 來說有一個標準化的規定是很重要的，這樣才能讓我們看到系統的整體性
* 需要依照規定的方式寫 log 才能讓 metric 集中分析
* 可以準備好一台設定好的 VM 包過 logstash 跟 collectd 讓人可以快速上手

### Consider the Audience
* 資料的目標是提供給人來使用，所以了解人們如何使用資料是很重要的
* 馬上告知需要立即知道的事情，對於之後可能想要的資料提供簡單找到的方式，花時間了解想要那些資料來解讀

### The Future
* 很多組織都把 metric 分散在不同的系統，application-levle metric 使用特定分析系統導致能能分析部分，也無法提供即時的報告，system-level 就可以即時產生報告來讓人採取行動
* 以前因為很久在 release 一版所以 business metric 一到兩天才產生是沒關係的，但現在一天可以 release 很多版所以需要即時的產出 metric 來做評估
* System metric 跟 business metric 其實都是 event 所以可以使用同一種方式來處理就可以得到更簡單的架構

### Summary
* Service
    * 至少要能夠知道 response time，之後再來做 application-level metric
    * 追蹤使用的 service 的 response 正常與否
    * 標準化 metrics
    * 用一致的格式寫 log
    * Monitor OS 才能找出 rogue process 跟 capacity planning
* System
    * Aggregate host-level metric 跟 application-level metrics
    * 確保 metric storage tool 可以 aggregate 整個系統、單一 serice 到單一 host
    * 確保 metric storage tool 可以讓我們有足夠的 log 來分析系統的走向
    * 有一個查詢 aggregating 跟 storing log
    * 使用 Corelation IDs
    * 知道什麼時候要採取行動進而規劃 alerting 跟 dashboards
    * 研究整合不同 metrics 
* Monitoring 的目的是讓我們能夠看見系統的全貌

## CHAPTER 9 Security

### Authentication and Authorization
* Authentication 代表確認某人確實是某人的過程
* Authorization 代表此身份可以做的事情
* Single monolithic application 會有 framewok 來幫我們處理這些事情但到分散式系統就需要有更多進階的方案來處理
#### Common Single Sign-On Implementations
* Single sign-on (SSO) solution 通常用來處理 authentication 跟 authorization
* 需要存取 resource 會先向 identity provider 請求需可，identity provider 審核通過身分跟想要的動作後將資訊提供給 service provider 讓使用者能夠存取 resource
* Identity provder 可以是外部提供的服務或者組織內的服務
#### Single Sign-On Gateway
* Microservice 中如果每個 service 都可以與 identity provider 互動會有很多重複的部分，所以 shared library 可以幫助解決這個問題但要小心不要造成 coupling
* 使用 gateway 避免每個 service 都與 identity provider 溝通，集中化 redirecting 跟 handshaking
* Downstream serivce 可以在 HTTP header 傳遞 username 跟 role 之類的訊息
* 使用 gateway 要確保 service 能夠簡單的就跑在 gateway 之後不會使開發者需要做太多努力
* 使用 gateway 容易讓人把所有關於安全性的東西放在裡面，但雞蛋不能放在同一個籃子裡要分散風險
* Gateway 盡量不要用來做太多事情，不然會變得太大讓攻擊點變多
#### Fine-Grained Authorization
* Gateway 可以用來做更細緻的 authentication
* 太過細緻的劃分 role 會讓管理變得不容易
* 將 role model 到現實組織的人員，也是之前提到將 software model 成實際組織運作的樣子

### Service-to-Service Authentication and Authorization
#### Allow Everything Inside the Perimeter
* 信任所有內部 service 的呼叫
* 好處是當 service 溝通不需要多作任何事情，不過被攻擊者滲透就很難抵擋 man-in-the-middle attack
#### HTTP(S) Basic Authentication
* Client 將 username 跟 password 放在 HTTP header 讓 server 做驗證
    * 好處是很好理解而且 protocol 也很好的支援
    * 壞處是 username 跟 password 沒有在安全的情況下傳輸，所以通常都在 HTTPS 之上使用
* HTTPS 讓 server 更能確保 client 的身份也避免其他人的監聽
    * Server 需要管理 SSL certificates
    * Traffic 無法被 reverse proxies cache，所以需要做到 server 或 client 裡面
* Server 只知道 client 有 username 跟 password 並不知道是從哪邊送過來的
#### Use SAML or OpenID Connect
* 如果已經有在使用 SAML 或 OpenID Connect 可以直接用在 server 之前的溝通
* 好處是可以使用現存的架構並且集中化管理
* Client 都需要 account 所以 service 也需要，service account 不要塞額外的東西進去才不會讓 revoking/changing access 變得很複雜
* 壞處是需要自己保護存好 credentials
#### Client Certificates
* 使用 Transport Layer Security (TLS) 來驗證 client
* Certificate 的管理很繁重，除錯也很困難
#### HMAC Over HTTP
* 使用 HTTP 的驗證方式在 HTTPS 上面會無法做 cached 跟增加負擔
* Amazon's S3 API 提出用 hash-based messaging code (HMAC) 來 sign the request
* Client 把 body 用 private key hashed 後放在 request 裡面，server 收到後用 private key hashed body 檢查 body 是否一樣
* Client 跟 server 需要先說好 secret 又不能在網路上傳輸，通常 hardcoded 但無法 revoke
* 這種方法沒有標準所以有各種實作的方式
* 只能保證沒有其他人改過 request 但內容還是會被看到
#### API Keys
* API key 讓 service 可以知道是誰做出呼叫進而限制存取 resource 的能力，或者做到 rate-limiting 來保護 service 品質
* 可以把 API key 當成 HMAC 來使用或者用 public private key pair
* API key-based authentication 比較簡單直覺
* 有很多 commercial 跟 open source space 可以選擇
* 有些 API system 可以用 API key 接到現存 directory service 讓 service 可以用不同方式做認證
#### The Deputy Problem
* Service 需要額外的 call 才能完整整個操作，但 downloadstream service 不知道什麼是合法的操作
* 除了將需要的東西傳下去之外還包含誰傳的提供 downloadstream service 做確認但問題是不知道這些資訊要帶多遠

### Securing Data at Rest
* 雖然我們有做好所有防護工作但還是無法確保攻擊者無法入侵所以需要有不同深度的保護
* 關於保護機密資料可以有很多不同的方法
#### Go with the Well Known
* 使用有在定期 patched 的 encryption algorithm
* 通常使用 AES-128 或 AES-256
* 密碼要用 salted password hashing
* 錯誤的加密比沒有加密還糟，因為可以能會忽略
#### It’s All About the Keys
* 用 key 跟加密演算法加密資料後得到加密過後的資料，key 跟加密資料應該要分開存放
* 用不同的 security appliance 加密跟解密，或者使用 key vault 管理 key
* 有些 database 內建加密功能但還是需要注意是如何管理 key 
#### Pick Your Targets
* 雖然將所有東西加密就不用思考需不需要加密但對於需要釐清問題的 logfiles 跟會加重機器負擔，另外如果加密 DB 則之後要 refactoring schemas 也會變得麻煩
* 區分好系統不同的部分使用不同的資料保護機制
#### Decrypt on Demand
* 看到檔案就加密，只在有需要的時候解密
#### Encrypt Backups
* 重要的資料需要備份所以也需要加密

### Defense in Depth
* 為了分散風險需要有不同程度的防禦
#### Firewalls
* 使用多個防火牆是很合理的，有些可能很簡單只能阻止某些類型的無法通過特定 port，複雜的可以 throttle connection 或偵測攻擊
#### Logging
* Logging 幫助我們偵測跟修復
* 需要注意存在 log 的資料，不要把敏感性資料放進去
#### Intrusion Detection (and Prevention) System
* Intrusion detection systems (IDS) 偵測不正常行為並且回報
* Intrusion prevention systems 除了可以偵測回報外還可以停止壞事繼續發生
* 使用 passive IDS 來發出警告讓我們可以調整 rule
#### Network Segregation
* Monolithic system 很難提供額外的保護，而 microservices 放到不同的網路所以可以完全控制 service 之前的溝通
* AWS 提供 virtual private cloud (VPC) 讓 hosts 可以在不同的 subnetxt 上，藉由定好 VPC 來規定互相溝通的規則
* 根據 team ownership 或 risk level 來切開網路
#### Operating System
* 系統依靠很多不是我們寫的軟體來運作，為了避免這些軟體有漏洞讓我們的 application 被滲透所以要盡量給這些軟體最低所需要的權限就好
* 定期自動化 patch 系統

### A Worked Example
* Finer-grained system 對於 security 可以更好的支援，對於不同部分採取不同的保護方式
* Browser: nosecure content 才能被 cache，secure content 一定要用 HTTPS 傳送
* Third-party royalty payment system: third part 要使用 client certificate，所有 data 都要透過 secure, cryptographic channel 傳輸
* Catalog data: 想要給大家使用但不想被濫用，所以使用 API key
* Network perimeter: 使用 firewall 跟有 security appliance 的 hardware 或 software
* Customer 資料很重要所以都要加密，即使被拿走 database 還需要 key 來解密才能使用
### Be Frugal
* 資料會洩漏 user 的隱私是最重要的
* 只要不存下資料就沒有人可以偷走
* 只存下最低限度需要的資料來達成 business operation
### The Human Element
* 除了外來的攻擊者也需要注意組織內部的人
* 需要有 process 跟 policy 來處理，例如 revoke 離職人員的 access credentials
### The Golden Rule
* 不要自己寫加密方法也不要自己發明 security prococols，即便是專家也是可能會出錯
* 很多 tool 都已經經過市場上的考驗並且已經測試修補很多年了
### Baking Security In
* 把資訊安全意識植入在工程師之中才會在工作的時候能夠察覺資安問題，先從 OWASP 開始
* 很多 tool 可以探測系統的漏洞
### External Verification
* 尋求第三方的測試找出漏洞
* 思考每次 release 需要做多少資安測試，因為完整的 penetration test 無法快速的迭代
### Summary
* Microservice 讓我們有能力在敏感的資料多做些保護，其他資料用間單的保護
* 當知道系統不同部分的危險程度就知道該如何導入保護
* 要 patch OS，不要自己實作 cryptography

## CHAPTER 11 Microservices at Scale

### Failure Is Everywhere
* 任何東西都有可能壞掉，當規模越大則就有一定的機率壞掉
* 最好壞掉的準備比起毫無計畫好
* 比起阻止壞掉發生應該要做好處理壞掉的情形
* 不論用多好的設備都會壞掉，所以只要知道系統能夠處理壞掉的情況就可以使用便宜有效率的方式

### How Much Is Too Much?
* 不一定要使用 autoscaling 系統，根據系統需求使用適合的方式
* 藉由問問題來知道系統能夠承受多少 failure 或系統需要多快速度來使用適合的技術
* 雖然不同 sevice 有不同的需求但是有一個統一的需求之後處理 load 或 failure 比較容易
* Response time/latency: 不同 operation 需要多久回應，幫助了解不同數量的 user 對於系統的影響
* Availability: 系統是否允許短暫下線，能夠下線多久?
* Durability of data: 多少資料遺失是可以被接受，資料需要保存多久
* 當需求釐清就可以基於這些設計系統同時有 performance test 確保系統有符合需求

### Degrading Functionality
* 系統是由很多 microservice 組成則其中一些失效造成系統不穩定
* 要處理如果其中 microservice 失效要盡量讓系統不要全部壞掉，持續提供能夠提供的功能
* 不同於 single monolithic application 不是好就是壞，microservice 系統都需要思考過如果某個 microservice 壞掉該如何處理

### Architectural Safety Measures
* 系統用到外部系統，當其中一個外部系統回應過慢導致 connection pool 逐漸累積到上限導致整個系統崩饋
* 設好 connection timeout，對於不同外部系統使用不同 conntion pool，實作 circuit breaker 避免送到不健康的外部系統

### The Antifragile Organization
* Google 跟 Netflix 都會做失敗的演練讓 developer 可以從中學習改進系統
* Chaos Monkey 會隨機關掉某些 machine
* Latency Monkey 模擬很慢的網路連線
* 雖然不需要做到像大公司這麼頻繁的演練，但承認接受失敗的發生可以使系統更有價值
#### Timeouts
* timeout 很容易被忽略但處理得方式會影響整個系統的效能
* 等太久會拖慢系統速度，等太短但其實已經成功，完全等待回應會讓整個系統停止
* 每個 out-of-process call 都需要有 default timeout，若超過 timeout 則記下 log 以便後續分析
#### Circuit Breakers
* 為了不讓 downstream 壞掉導致拖慢系統速度，可以設計都特定次數的 request 都失敗就不再發送 request，特定時間後再來測試是否回復了
* 挑選合適的啟動 circuit breaker 的參數當作預設，為特定地方特別調整
* 啟動 circuit breaker 後如果是非同步的時候可以 queue 起來之後重試，同步的時候馬上回傳錯誤讓呼叫的人知道
* Circuit breaker 也可以幫助我們進行 microservice 的維護，當需要維護時把 circuit breaker 啟動就可以安心維護了
#### Bulkheads
* 採用類似船艙的概念，透過 downstream microservice 使用不同 connection pool 才不會當 connection pool 壞掉就讓整個系統壞掉
* 使用 concerns 分離也是可不錯的做法
* Circuit breaker 也可以當作 bulkhead，避免 consumer from downstream 遇到問題，另一方面也避免 downstream service 在壞掉的狀態收到更多呼叫。
#### Isolation
* Service 越多 depend 則彼此影響越多
* Service 彼此越獨立則需要合作的部分越少，team 可以有更多的自主性

### Idempotency
* Idempotency operation 在第一次被 apply 後就不會改變，這種有利於我們對於結果不確定的時候可以放心重試
* 非第一次後續呼叫不影響重點是在不影響商業邏輯而非當作沒收到呼叫而不去紀錄 log
* HTTP 的 GET PUT 暗示 caller 該 API 為 idempotency，如果是 non-idempotency 容易一團混亂

### Scaling
* 為了處理 failure 會 scaling
* 為了 performance 會 scaling
#### Go Bigger
* 直接加強 server 規格，用更強的 CPU 跟 I/O 增加 throughput 與降低 latency，也稱為 vertical scaling
* 缺點是價錢昂貴，software 無法妥善利用多出來的資源，沒有增加系統彈性
#### Splitting Workloads
* 把多個 microservice 跑在同一台 host 上移到不同 host 上增加 throughput，也避免 host down 影響多個 microservice
* 隨著規模擴大可以將現有 microservice 在切分成不同 microservice 分散 workload
#### Spreading Your Risk
* 雖然把 microservice 放在不同 host 可以避免 host down 造成多個 microservice down 但是如果這些 host 都是 virtual 跑在同一個實體機器上則該機器壞掉也會影響所有 microservice，所以可以設定要讓 host 跑在不同實體機器上
* Storage area network(SAN) 也是有可能壞掉導致 VM 都壞掉
* 可以將 service 放在不同 data center
* AWS 的 AZ 就是 data center，因為 AWS 只保證 99.95% uptime 在一個 AZ 所以要將 service 放到不同 AZ，更進階可以放在不同 region
* 要有備案才能在提供服務的廠商失效時能做災難還原
#### Load Balancing
* 通常會以 load balancer 後面接多個 microservice instance 來提供 microservice 的服務，呼叫的人會以為對一個 microservice 而已
* Load balancer 有很多種形式跟大小，都是根據演算法決定將不健康的 instance 移出，加入正常的 instance
* 有些 load balancer 提供 SSL termination 功能將收到的 HTTPS connection 換成 HTTP　以節省管理 SSL 的 overhead
* 使用 hardware load balancer 很難自動化，可以在後面加上 software load balancer 讓團隊能自由改變設定
* 雖然 load balancer 可以讓我們跑多個 microserice instance 避免 SPOF 但是 microservice 都用同一個 database 也是有可能遇到 database SPOF
#### Worker-Based Systems
* 不是只有 load balancer 可以有多個 instance，worker-based system 有一個 shared queue of work 讓 instance 拿出來處理，適合 batch work 或 asynchronous jobs
* 如果 work queue 是可靠的可以單純藉由增加 instance 來消化對應的 load
#### Starting Again
* 設計的系統不知道會不會成功，所以當成功的時候必然需要改寫以符合龐大的 workload
* 快速的建立系統讓市場驗證是否需要，不然花太多時間設計沒有人想使用的東西也是沒有用
* 需要系統改變不是失敗的象徵反而是成功的象徵

### Scaling Databases
* Scaling stateless microservice 很容易但是有依賴於 database 的 microservice 就不容易
#### Availability of Service Versus Durability of Data
* 可用的 service 跟 data 有沒有保存好是不同的事情
* 可以把 DB 資料存到可靠的系統，所以 DB 掛掉資料還是好的但是 service 卻不可用
#### Scaling for Reads
* 大部分的 service 都比較常 read，而且 read 比起 write 更好 scale
* RDBMS 可以將 data 從 primary node 複製到其他 node 讓 service 可以 read，中間可能會有不一致的情況，但只要系統有處理好暫時不一致的情形就可以很好 scale
* 雖然 read replicas 可以增加 read scale 但是建議做 caching 可以直接顯著提升 performance
#### Scaling for Writes
* Sharding 根據資料 key 的 hash 寫進不同的 DB 以增加 write scale
* 缺點是查詢需要橫跨多個 DB 就變得困難需要在記憶體 join 每個 DB 結果
* 若要加入 shard instance 之前可能需要暫時下線才有辦法，但現在可以直接加入
* Sharding 只能 scale 不會增加 resiliency，但是 Cassandra 有提供將每個 shard instance 複製到多個 ring 裡的 node 增加 resiliency
* 短期可以直接使用更大的 DB 但是長期就需要使用其他model 來解決
#### Shared Database Infrastructure
* RDBMS 把 database 跟 schema 分開，讓不同 schema 可以跑在相同 database，但是要注意一但 database 失效可能會連帶多個 microservice 失效
#### CQRS
* Command-Query Responsibility Segregation (CQRS) pattern 是另外一種儲存查詢的方式，一部分系統處理 command 另外一部分處理查詢
* 處理 command 與查詢分離可以各自 scale，使用不同 service 或 hardware，可以使用不同方式 scale

### Caching
* Caching 很常用來優化效能，藉由存好之前的結果直接回傳以節省計算資源
* Microservice architecture 每個 service 都有自己的資料跟行為所以有各種 caching 的方式，還有可以選擇在 client side 還是 server side
#### Client-Side, Proxy, and Server-Side Caching
* Client-side caching 讓 client cached 結果由自己決定什麼時候要打新 reqeust，通常 downstream service 會幫助 client 做判斷
* Proxy caching 在 client 跟 server 之間，例如 reverse proxy 或 CDN
* Server-side caching 由 server 負責 caching 像是 Redis 或 Memcache
* Client-side caching 可以幫助減少 network calls，是最快速減少 downstream service load 的方式，缺點是需要改動所有 consumer
* Proxy caching 對於 client 跟 server 都是不知情的，所以可以很簡單加入系統，雖然會多一個 hop 但是應該不會造成問題
* Server-side caching 對於 client 是不知情的，cache 在 service boundary 裡面很容易可以驗證資料或追蹤 cache miss
* 通常會混用各種 caching，另外也是有完全沒 caching 的系統
#### Caching in HTTP
* HTTP 不管在 client 或 server side 都有提供好用的 cache 機制
* HTTP cache-control 讓 client 知道可以 cache，另外有 Expires header 知道可以暫存多久
* HTTP Entity Tags 或 ETags 讓我們知道 resource 有沒有改變，用 conditional GET 可以帶 header 告訴 service 達到特定條件才回傳
* ETags, Expires, cache-control 有點重複，如果沒有小心使用可能會互相矛盾
#### Caching for Writes
* Write-behind cache 先將改動暫存起來之後才 flush 到 downstream source，對於資料多次改動會有效能提升
* Write-behind cache 暫存 writes 就算 downstream service 不可用還是可以之後等可用在送
#### Caching for Resilience
* Caching 可以幫助遇到 failure 的時候持續提供服務，有時候回傳 stale data 比起什麼都不回好
* 有一種爬蟲會定期把 live site 轉成 static version，當 website down 就可以提供舊版本
#### Hiding the Origin
* 如果 cache miss 則 request 就會導向源頭去取的資料，但當大範圍的 cache miss 會造成大量的 request 打向源頭
* 避免源頭被打爆的方式有不允許直接向源頭發 request，當 cache miss 則通知源頭，源頭之後再去 update cache，好處是可以背景重建 cache
* 雖然對於某些情況不合理但是可以保持系統上限，讓 request 快速失敗避免讓 resource 增加 latency
#### Keep It Simple
* 太多的 cache 會有更多的 stale data 也就更難偵測資料的即時與否
#### Cache Poisoning: A Cautionary Tale
* Caching 有可能不小心永遠使用到 state data
* 更新後有很多地方都需要清空 cache 但也有地方是無法清空的像是 user browser
* Caching 是很有用的工具但是需要徹底理解資料從 source 到 destination 是如何被 caching 才能注意到可能出錯的地方

### Autoscaling
* 如果有能夠自動化 provision virtual hosts 的功能可以根據 loading 來調整機器數量節省花費
* 遇到 load 增加或者 instance fail 可以增加 instance 來應付，但要注意 scale 速度要能跟上 loading 上漲的速度才會有效
* autoscaling 通常都用來處理 instances failure 情況
* 發生 failure 的時候建議先 autoscaling 再來收集 data
* Scaling down 不要太快，畢竟有剩餘的 computing power 比完全沒有好

### CAP Theorem
* 系統中最多只能兼顧 consistency, availability, partition tolerance 其中兩種
* Consistency 是指不管到哪個 node 都會給我相同的答案，Availability 是指每個 request 都收得到 response，Partition tolerance 是指系統能夠處理有時候無法溝通的情況
#### Sacrificing Consistency
* AP system: 資料不一致但是服務還是可用而且系統中部分溝通失效
* 我們最後還是需要在 partition 消失的時候重新同步資料，partition 越久則越難同步好
* Eventually consistent 是指我們預期在某個時間點所有 node 都有一致的資料，因為不是立刻發生所以要假設可能會看到舊資料
#### Sacrificing Availability
* 為了 consistency 當發生 partition 就只能犧牲 availability，CP 就是一但發生 partition 就停止提供服務直到 partition 消失
* 維持多個 nodes 的 consistency 是很困難的，需要有 transactional read 才能確保所有 nodes 保持一致但這個操作需要 lock 所以很慢，所有 consistent system 都需要某種程度上的 lock
* Lock 在分散式系統很難正確使用，因為可能要 release lock 的時候無法溝通
* 不要自己發明 multinode consistency 的方法，採用現存的 data store 或 lock service
#### Sacrificing Partition Tolerance?
* 無法犧牲 partition tolerance 因為是跑在 network 上面無法確保一定不會斷線
* CA system 只能存在於單一 process 所以無法在分散式系統上
#### AP or CP?
* AP systems 容易建造跟規模化，CP system 需要在分散式系統面臨更多挑戰
* 根據商業性質決定採取何種系統，若可以容忍短暫資料不一致則使用 AP system
#### It’s Not All or Nothing
* 一個系統不一定全部都是 CP 或 AP
* Service 也不一定是要 CP 或 AP，可以在 CAP 中尋找合適的平衡點，CP 或 AP 只是代表把各個面向推到極端而已
* 有些系統可以提供更細微的控制，例如 Cassandra 可以要求 individual calls 需要有幾台 replica 保證一致
* 有些人嘗試打破 CAP theorem 但是最終都只是 CP 或 AP，因為 CAP theorem 由數學證明出來了
#### And the Real World
* 我的系統是從現實世界對應出來的，就算系統保持一致但現實世界有可能有意外導致不一致
* 雖然系統保持多一致但是永遠無法預測所有的情形，所以 AP system 比起 CP system 容易應付

### Service Discovery
* 有很多 microservice 的時候就會需要知道該如何找到想要的 service，這些都可以在 service discovery 找到對應的功能
* 大部分解決方法是提供某個機制讓 instance 註冊，接下來提供某個方法讓人可以找到註冊過的 instance
#### DNS
* DNS 讓我們可以用 name 找到 IP，如果要將 account service 綁在某個 IP 上當我們 deploy 也需要更新 entries
* Service 有很多 instance 可以用 convention-based domain template，例如 /<servicename/>-/<environment/>.musiccorp.com
* 使用不同 DNS server 在不同的環境，所以可以使用相同 IP 但是根據查詢不同 DNS server 會有不同結果
* DNS 是很通用的標準所以很多技術都會支援，但在會快速變動的 host 環境下更新 DNS entries 是個問題
* DNS entries 有 time to live (TTL) 告訴 client 可以使用多久，當我們改變 host 去更新 entry 但需要假設 client 有舊 IP 直到過期，另外還會暫存在很多地方
* 使用 load balancer 避免 stale DNS entries 

### Dynamic Service Registries
* 由於 DNS 在高度變動的環境有有很多缺點所以有很多替代系統，其他系統通常有集中的註冊服務，而且系統通常有更多功能。
#### Zookeeper
* 擁有 configuration management, synchronizing data between services, leader election, message queues and naming service
* Zookeeper 仰賴 cluster 裡面多個 node 來履行承諾，所以至少要有三個 node 來保證 data 安全的複製到其他 node 而且當 node fail 仍然保持一致
* Zookeeper 提供 hierarchical namespace，在其中的 node 當改變可以收到通知所以可以把 service 放在裡面，當 service 有改變就可以收到通知，但建議盡量不要使用因為會使 service 行為更加複雜
* Zookeeper 很通用可以想成一個 replicated tree information，當有改變就會收到通知
* 雖然有點舊但有被廣泛使用測試過，Zookeeper 底下的邏輯也是很難被正確實作所以跟加密一樣盡量不要自己實作
#### Consul
* 跟 Zookeeper 一樣支援 configuration management 跟 service discovery 並且更進一步提供 serice discovery 的 HTTP interface 另外提供 out-of-box DNS server 讓目前有在使用 DNS server 可以無痛轉換
* 支援對 node 進行檢查取代 monitoring tool
* 都使用 RESTful HTTP interface 包含註冊 service、查詢 key/value store 或 health check，這讓整合變得容易
* Consul 把偵測失敗 node 跟警告機制與 service discovery 跟 configuration management 分開
* Consul 是新的東西而且使用複雜的 algorithm 但開發團隊都有過往成績
#### Eureka
* Netflix open source 專注於特定 use case
* 提供基本 load-balancing 用 round-robin 方式來檢查 service instance
* 提供 REST-based endpoint 所以可以自己寫 client 或用 library
* Client 需要處理 service discovery，所以如果 client 有不同則是個挑戰
#### Rolling Your Own
* 自己設計找到 service 的流程
* 用 AWS 上 tag，讓 AWS 幫我們找到 service
#### Don’t Forget the Humans!
* 人也需要能找到 service 在哪邊

### Documenting Services
* 我們希望用 API 來操作 microservices，但是需要有文件來知道該如何使用 API 並且持續更新
#### Swagger
* Swagger 讓我們能夠用 web UI 來描述 API 並且直接使用 API
* Service 需要 expose sidecar file 來符合 Swagger 格式
* Swagger 有很好的使用者體驗但是對於 incremental exploration concept 支援不足
#### HAL and the HAL Browser
* Hypertext Application Language 是一種標準用來描述 expose 的 hypermedia control
* hypermedia control 只讓 client 可以慢慢的探索出 API
* HAL 有很多支援不同語言的 libraries
* HAL browser 可以讓我們在 web browser 上用 API
* HAL 比起 swagger 在執行 API 沒有這麼順暢需要做更多的事情，但反面來說可以更有效率的 follow links 來探索 API
* API 如果已經使用 hypermedia control 則花很少力氣就可以 expose HAL browser，若沒有使用 hypermedia control 則無法使用 HAL
* 對於已經採用 hypermedia control 的人通常會使用 HAL

### The Self-Describing System
* Martin Fowler 建議使用 humane registry 這種方式，製造出一個地方讓人可以方便紀錄 service 的資訊
* 了解系統的行為是很重要的
    * 用 correlation ID 知道 call chains 追蹤所使用的 service 
    * 使用 service discovery systems 來找到 service 在哪邊跑
    * HAL 讓我們知道 endpoint 有哪些功能
    * Health-check pages 跟 monitoring system 讓我們知道系統跟 service 的狀態
* 用來了解系統的資訊讓我們可以打造 humane registry，呈現出所有系統的資訊
* 從簡單的頁面開始，從系統撈出的資訊越來越多讓頁面越來越豐富，讓資訊可以被讀懂才能管好日益複雜的系統

### Summary
* Microservice 尚在萌芽階段，未來會有很多 pattern 在 scale 的時候來幫助我們
