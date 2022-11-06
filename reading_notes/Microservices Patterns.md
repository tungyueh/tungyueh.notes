# Microservices Patterns
## Chapter 2 Decomposition strategies
### 2.1 What is the microservice architecture exactly?
### 2.1.1 What is software architecture and why does it matter?
* 之前 architecture 注重 scalibility, reliability, security 現在還多注重 maintainability, testability, deployability
* Decomposition 可以讓多個 team 發揮各自的長處有效率的合作
* 系統可以從 logic, implementaion, process, deployment 等面向來研究，另外 scenario 是從不同情境看上述的面向會如何合作
### 2.1.2 Overview of architectural styles
* Layered architecture 把系統分成 presentation, business logic, persistence layeer
* Hexagonal architecture 的 business logic 處於中心，inbound adapter 處理外部的需求，outbound adapter 傳達 business logic 的產出
### 2.1.3 The microservice architecture is an architectural style
* Application 由一堆 loose coupling service 組成，serice 之間使用 API 來溝通
* Service 大小不重要重點是能快速開發減少與其他 team 溝通的時間成本
## 2.2 Defining an application’s microservice architecture
* 第一步先分析需求，第二步切分出 service，第三步訂好 service API
* 會有 network latency, service 同步問題跟 data consistency 問題
### 2.2.1 Identifying the system operations
* 從需求推出 domain model 再來根據 domain model 訂出 system operation
* 透過分析需求的情境跟專家討論後產出 domain model，就可以定義好系統所需要使用的詞彙
* 參考情境所使用的動詞對應出 system command
### 2.2.2 Defining services by applying the Decompose by business capability pattern
* 藉由分析 business 功能來打造 microservice 架構
* Business 功能比較穩定而如何提供功能則常常變動
* 藉由分析組織的目的、架構跟處理業務的流程找出 business 功能
* 功能底下可以再切分成很多小功能
* 相似的東西可以對應到 service，類型不同就對應不同 service，但有相關的也可以對應成一個 service，但之後都可以隨著系統的需要做改變
### 2.2.3 Defining services by applying the Decompose by sub-domain pattern
* 從要解決的問題產生 sub-domain 對應成 service
* Domain model 的 scope 稱為 bounded context 包含 code artifact 來實作 model，對應到一個或多個 services
### 2.2.4 Decomposition guidelines
* 應用 SRP 概念劃分 microservice 架構會得到很多小但有單一責任的 services，可以增加穩定性
* 應用 CCP 可以讓改變所需要的 service 變少
### 2.2.5 Obstacles to decomposing an application into services
* 把 application 切分成 service 需要面對新的問題
* Service 透過網路溝通就需要面對網路延遲的問題，可以藉由實作 batch API 或者把 service 合併
* Synchronous 與 service 溝通會讓可用性降低，使用 asynchronous messaging 可以減少 tight coupling 跟增加可用性
* 需要維持 service 之間的資料一致性，傳統有 two-phase, commit-based, distributed transaction management mechanism，新一點的有 Saga pattern 比起 ACID 更複雜一點但也能應付更多情境，限制是 eventually consistent，如果需要 atomically 更新則需要再 service 裡面
* 無法在多個 database 的到一致的 data
* God class 無法切分成 service，使用 DDD 把 subdomain 當成一個 service，每個 service 對於 God class 有不同的看法讓 God Class 形成自己的版本進而變成 service
### 2.2.6 Defining service APIs
* Service API 的 operation 跟 event 需要被訂好，跟系統有關的 operation 會被外部的人或內部 service 呼叫，只跟 service 之間溝通的 oepration 只會被內部 service 呼叫
* Service publish events 主要是為了與其他 service 合作
* 先把 system operation 對應到 service 再來決定如何實作
* 決定要把哪個 service 負責處理一開始的 request，可以選擇需要 opearation 資訊的 service 來負責處理，或者有足夠知識應付 operation 的 service 來負責處理
* Service 之間為了達成 system operation 需要用 API 互相溝通，分析每個 system operation 所需要合作的 service 來定好 API
### Summary
* Architecture 定義好應用程式的能力包含可維護性、可測試性、可部屬性這些都對於開發速度很重要
* Microservice architecture 讓開發速度變很快
* Service 跟 business concerns 有關跟 technical concerns 無關
* 用 Business capability 切分 service 或者用 subdomain
* 使用 DDD 來切分 god classes

## Chapter 3 Interprocess communication in a microservice architecture
### 3.1 Overview of interprocess communication in a microservice architecture
* 有分為以 request 跟 response 為主的同步溝通機制跟以 message 為主的非同步溝通機制，溝通用的媒介有分為人類看的懂的格式跟有效率的 binary 格式
* client 跟 service 溝通方式要與技術無關、以 API 為優先的設計方式、API 如何演化、message 格式如何適應 API 演化
#### 3.1.1 Interaction styles
* clien 的 request 由多個或一個 service 來處理跟訊息處理世同步還是非同步可以切出基本風格
* 一對一的有
    * Request/Response: client 送 request 給 service 並且等待回應，client 預期會很即時的回應，缺點是 coupling
    * Asynchronous request/respsonse: client 送 request 之後並不等待因為 service 可能會花很久的時間才回應
    * One-way notification: clieny 送 request 之後並不會收到回應
* 一對多的有
    * Publish/subscribe: client 送出通知讓有興趣的 service 去處理
    * Publish/async responses: client 送出 request 然後等待有興趣的 service 給予回應
#### 3.1.2 Defining APIs in a microservice architecture
* Module 的 interface 可以提供完整的功能並且隱藏實作讓實作改變不影響到 client
* Monolithic application 的 interface 通常是使用特定語言的 interface
* API 跟 interface 是 client 跟 service 之前溝通的橋樑，API 定義提供的功能讓 client 使用
* Service API 跟語言無關所以到 runtime 才會發現錯誤
* 先定義好 interface 後讓 client review 可以訂出更符合 client 需求的 API
#### 3.1.3 Evolving APIs
* Microservice-based application 改動 API 都要讓新舊 client 都可以使用，因為無法強迫 client 做出改動
* 將 API 加上版本，使用 MAJOR.MINOR.PACH 的方式，MAJOR 代表作出不相容的 API、MINOR 代表有向後相容、PATCH 修正向後相容的問題
* 盡力使用向後相容的方式: 增加 optional attribute、response 增加 attribute、增加新的 operation，service 提供預設的值而 client 要忽略多餘的回應
#### 3.1.4 Message formats
* 使用跨語言的格式
* 文字格式例如 JSON 跟 XML，優點是人類看的懂而且具備自我描述能力，能讓 consumer 只用自己想要的值，所以很容易向後相容，缺點是太過詳細有很多不必要的東西，parsing 沒有效率
* Binary 格式例如 Protocol Buffers 跟 Avro，compiler 會 serializes 跟 deserialize 訊息，如果使用靜態語言還可以檢查是否正確
### 3.2 Communicating using the synchronous Remote procedure invocation pattern
* Client 的 business logic 呼叫 proxy interface 而 RPI proxy 送 request 給 RPI server 轉送給 service business logic 處理
* Proxy interface 隱藏了使用何種 protocol 溝通
#### 3.2.1 Using REST
* REST 是一種 IPC 機制通常用 HTTP 實作
* Resource 代表一個 business object，GET 可以取得 resource、POST 可以建立 resource、PUT 可以修改 resource
* 使用 Interface Define Language 定義好 REST API，最有名的是 Open API Specification 從 Swagger open source project 演化而來
* 使用 query parameter 定義需要多回傳的 resource 達成一個 API 回傳多個 resource 的需求
* 多定義 subresource 或在 query parameter 定義不同修改的方式
* 好處是簡單且熟悉、容易測試、直接支援 request/respsonse 機制、firewall friendly、不需要中間的 broker
* 壞處是只支援 request/respsonse 機制、減少可用性、client 需要知道 URL、很難一個 request 獲得多個 resource、不容易對應 HTTP verb 到實際的操作
#### 3.2.2 Using gRPC
* binary message-based protocol
* 支援 streaming RPC
* 使用 Protocol Buffers 當成 messge format，收到的人可以只解出自己需要的東西因此可以支援向後相容
* 好處是可以很直覺的設計 API 因為有很多 operation、很有效率的解開 message
* 壞處是 JavaScript client 要花更多時間處理、舊的 firewall 可能不支援 HTTP/2
#### 3.2.3 Handling partial failure using the Circuit breaker pattern
* 分散式系統需要考慮當 server 無法回應的情況，client 如果無限等待回應會造成整個系統失效
* 設計 RPI proxy 要考慮當 service 沒有回應不能因為 thread 用完而無法繼續處理 request，還要思考如何從失效的情況回復
* 不要永遠的等待回應需要設定 timeout
* 限制 client 可以發出的 request，當超過上限就馬上回應失敗
* 觀察失敗的比率太多就啟動 circuit breaker 讓後續的 request 馬上失敗讓 client 知道 server 失效繼續送 request 也沒用
* Server 失敗就直接回傳失敗給 client 或者回傳預設的值或暫存的值
#### 3.2.4 Using service discovery
* Service 的位置會常常變動所以 client 需要 service discovery 來找到 service 的位置
* Service registry 使用 database 紀錄 service 的位置，當 service 啟動或關掉就來更新，當 client 使用 service 就找出位置轉送 request
* 應用程式層級的 service discovery 會讓 service 會去註冊位置而 client 查詢位置後送 reqeust 給 service，好處是 service 可以部署在不同平台，壞處是每個語言都需要 service discovery library
* 平台提供的 service discovery 會讓每個 service 有 DNS name 跟虛擬 IP，client 對 DNS name 或虛擬 IP 發送 request 就可以自動送到可用的 service，好處是平台都處理好還有不跟語言綁定，壞處是只能用該平台部屬 service

## Chapter 4 Managing transactions with sagas
* 如何在不同 service 之間維持 transaction 是很重要的考量之一
* Saga 可以維持資料一致性但缺少 Isolation 的功能所以 service 需要考量這點做設計
### 4.1 Transaction management in a microservice architecture
* 利用 framework 或 library 可以簡單的管理 transacation
* 在多個 database 情況底下要管理 transaction 是很有挑戰性的事情
#### 4.1.1 The need for distributed transactions in a microservice architecture
* 每個 service 都有自己的 database 因此需要一個方法來維持跨 database 的資料一致性
#### 4.1.2 The trouble with distributed transactions
* 基本用 distributed transaction 來維持資料一致性但不能用 NoSQL database 而且流行的 message broker 都不支援
* Distributed transaction 會降低可用性因為使用 synchronous IPC，現代傾向於維持可用性而非一致性
#### 4.1.3 Using the Saga pattern to maintain data consistency
* Saga pattern 使用 local transaction 跟 asynchronous messaging 來讓不同 service 之前資料保持一致
* local transaction 是導致失去 isolation 的原因
* Asynchronous messaging 確保當有些參與者失效的時候仍然能夠持續運作
* 使用 compensating transaction 來 roll back
### 4.2 Coordinating sagas
#### 4.2.1 Choreography-based sagas
* 沒有 coordinator 由參與者自行決定要做什麼，自己要去 subscribe 其他人的 event 跟回應
* 參與者需要更新資訊到自己的 DB 然後 publish 訊息通知大家自己更新 DB 的資訊
* 參與者要能更把 event 對應到自己的 data
* 優點
    * 只要把自己更新的資訊 publish 出去就好
    * 使用 event 來同步而不需要知道其他參與者可以達到 loose coupling
* 缺點
    * 比較難理解整體怎麼運作
    * 造成 cyclic dependencies
    * 需要 subscribe 所有對於自己有影響的 event 造成 tight coupling 
#### 4.2.2 Orchestration-based sagas
* 由一個 coordinator 指揮參與者該做什麼指令將所有責任集中化
* 可以想成 state machine，由 event 驅動轉換成不同 state
* 優點
    * 簡單的依賴關係，只有 corordinator 依賴於參與者
    * 參與者不需要了解 saga pattern
* 缺點
    * 集中太多 business logic
### 4.3 Handling the lack of isolation
* Isolation 的特性可以簡單寫出同時執行的 business logic
* 缺少 isolation 造成 anomalies 也就是當 transcation 讀寫資料順序不同導致結果不同
* 之前 RMDB 有提供為了 high performance 而捨棄 isolation 的功能所以還是有些對於缺少 isolation 的解法
#### Overview of anomalies
* Lost update: 複寫其他 sagas 做的 change
* Dirty reads: 讀取更新到一半的資料
* Fuzzy/nonreapeatable reads: 不同步驟讀取資料會有不同的結果
#### Countermeasures for handling the lack of isolation
* Saga transaction type
    * Compensatable transaction: 可以被 rollback
    * Pivot transaction: 只有這個 commit 後才會跑完，可以是 compensatable 或 retriable transaction
    * Retriable transaction: pivot transaction 之後保證會做完

## Chapter 6 Developing business logic with event sourcing
### 6.1 Developing business logic using event sourcing
* Event sourcing 用 events 來保存狀態，每個 event 代表狀態的變更，藉由這些 event 來知道現在的狀態
* 好處是可以有歷史紀錄方便做稽核或其他用途，缺點是學習曲線較高而且查詢 event store 需要用到 CQRS pattern
#### The trouble with traditional persistence
* 圖形化的 domain model 很難對應到表格式的資料庫
* 缺少歷史紀錄
* 實作稽核紀錄容易產生錯誤
* Event publishing 跟 business logic coupling
#### Overview of event sourcing
* 用存在 event store 的 event 保存 aggregate 狀態
* Event 需要含有變動狀態需要的資訊
* Command 先檢查輸入資訊是否有誤，然後決定要改變成哪種狀態但還不會改變狀態，產生會改變狀態的 event，之後另外 method 接收 event 並且改變狀態
#### Handling concurrent updates using optimistic locking
* 多個 requests 需要更新同樣的 aggregate 通常使用 version 來判斷在讀取之後有沒有被改變
* 只有當 version 沒有變 update 才會成功
#### Event sourcing and publishing events
* 需要將所有 events 推給有興趣的 consumer
* Event publisher 拿到新的 event 然後 publish 給 message broker，困難點在於要知道哪些是新的 events
    * 如果 event id 是不斷遞增的話可以記住處理到哪個 event id 之後搜尋就找記住的 event id 之後當成新的 events 但 transaction 發生的順序不一定跟 commit 的順序一樣就有可能漏掉 events
    * 多一個 published flag 代表已經被處理過了來避免漏掉 events
#### Using snapshots to improve performance
* 有大量的 event 會使處理 events 變得沒有效率
* 定期儲存狀態來避免需要處理所有 events 只需要處理之後的 events 就好
* 使用快照來儲存狀態則重建過程會從快照的狀態開始而非從頭開始
#### Idempotent message processing
* 處理 domain event 需要確保就算處理多次結果也是一樣，因為有可能收到好幾次
* RMDB-based event store 把 message id 放進 table 紀錄處理過的 message
* NoSQL-based event store 把產生的 events 的 message id 記錄下來
#### Evolving domain events
* 把 event 存下來需要面對 event 格式會不斷變動需要做到 backward compatible
* Schema 改變可以用 migration 來處理
#### Benefits of event sourcing
* 不管 aggregate 的狀態是什麼 event 都可以被 publish
* 保存 aggregate 的歷史紀錄
* Event 結構簡單，可以藉由 event 重建 aggregate 狀態
* 提供開發者 time machine 來將新的功能套用到舊的狀態
#### Drawbacks of event sourcing
* 不容易學習
* 需要偵測 event 是否處理過
* Event 版本會不斷變動
* Delete 只是一個標記而已並不是真正 delete，但有時候資料需要真正被刪除，可以用加密的方式來解決，只要把加密金鑰刪除代表沒人可以存取資料也就等於刪除了
* 不容易查詢資料

## Chapter 7 Implementing queries in a microservice architecture
### 7.1 Querying using the API composition pattern
#### The findOrder() query operation
#### Overview of the API composition pattern
* API 負責去個別 service 獲取需要得資料組合好再回傳
#### Implementing the findOrder() query operation using the API composition pattern
#### API composition design issues
* 可以由 client 自行去問 service 得到想要的結果
* 可以用 API gateway 來讓 client 查詢結果
* 多一個 service 來讓 client 查詢結果
* 要能同時對多個 service 查詢以降低 respsonse time
#### The benefits and drawbacks of the API composition pattern
* 用了更多 computing resource 跟 network resource 所以會增加成本
* 越多的 service 會降低可用性因為需要同時所有 service 都可用才能組出結果
    * 使用暫存機制，如果 service 不可用就回傳上次的結果
    * 允許缺少部分資料，將可用的資料回傳
* 資料可能會不一致
### 7.2 Using the CQRS pattern
#### Motivations for using CQRS
* 當 service 沒有儲存需要 filter 的屬性就需要透過 API 來做 in-memory 的 join 這不只能效率而且可能會佔用龐大的記憶體
* 有時候擁有資料的 service 不適合實作查詢功能，或者無法有效率的查詢
* service 也要實作查詢功能會讓它的責任太多
#### Overview of CQRS
* 把 persistent data model 分成 command side 跟 query side，command side 負責 create, update, delete 操作而 query side 負責 queries
* Command side 當資料有變動就 publish domain events
* Query side subscribe domain events 後同步到自己的 database，不需要實作 business rules 所以相對簡單也不受 database 限制可以查詢的方式
* 可以用 CQRS 的概念在 service 上面，Query service 就是只提供查詢功能的 service 可以提供需要的 view，由於不屬於任何 service 所以可以是 standalone service
#### The benefits of CQRS
* 可以更有效率實作需要從多個 service 拿資料的查詢功能
* 可以支援各種類型的查詢而不受到 database 先天上的限制
* 在 event sourcing 的 application 可以支援查詢功能
* 讓 data model 可以專注於維持資料而不需支援查詢功能，可以在合適的 service 做查詢功能即時需要的資料在別的 service
#### The drawbacks of CQRS
* 更複雜的架構
* 需要處理 command-side 跟 query-side view 不一致的情況
    * 查詢結果加入 version 讓 client 辨別是否是過期的資訊
    * 直接使用 command side 回傳的結果
### 7.3 Designing CQRS views
* CQRS view model 從 DB 搜尋結果，訂閱不同 service 的 event
* Data access module 實作存取 DB 的邏輯，Event handler 跟 Query API module 使用 data access module 更新跟查詢 DB
#### Choosing a view datastore
* 選擇何種 DB 跟 schema 的設計會直接影響到後續實作查詢功能的困難與否
* NoSQL DB 適合 CQRS view 因為可以受益於 NoSQL 多樣化的 schema 跟效能又不會被NoSQL受限的查詢功能影響到
* Event handler 會使用 primary key 更新或刪除 view DB
#### Data access module design
* Data Access Object (DAO) 複雜把 high-level code 對應到 DB API
* DAO 有時候需要處理同時更新的問題但如果 event 都是按照順序就不會需要同時處理
* 要讓 client 知道現在有不一致的情況，可以利用 client 更新後回傳的 token 包含 event id ，client 查詢帶著 token 讓 server 發現有不一致就會傳錯誤
#### Adding and updating CQRS views
* 增加或修改 view 可以先開發好 query module 然後設置好 data store 最後 deploy service，讓 event handler 處理完所有 event 就可以讓 view 到最終一致的狀態
* CQRS 不會有所有的 event 所以必須要 archive event 才能重建 view
* 隨著 event 越來越多會讓建立 view 的時間越來越長，可以分成兩階段，第一階段先定期處理 snapshot，第二階段使用 snapshot 加上後續的 events
### 7.4 Implementing a CQRS view with AWS DynamoDB
* AWS DynamoDB 是 scalable NoSQL DB
#### The OrderHistoryEventHandlers module
#### Data modeling and query design with DynamoDB
* 要定義好 table 的 primary key 才能用來找到需要的資料
* 使用 composite primary key 來找另一個 table 的資料，第一個是 partition key 是 Z-axis scaling 第二個是 sort key
* Put 用來建立或替換掉整個 item 而 update 用來更新現有 item
* 不處理重複的 event 會造成暫時性的資料不即時會把資料設回原本舊狀態
* 紀錄上次處理到哪邊就可以偵測重複的 event
#### The OrderHistoryDaoDynamoDb class

## Chapter8 External API patterns
* 設計 application external API 困難點在於有各種不同的 client，每種 client 有不同的環境需求跟資料需求
* 無法有一種 API 適合所有 client 使用
### 8.1 External API design issues
* Web application 直接在 firewall 裡面所以有 low-latency 跟 high-bandwith 的環境
* Browser 裡的 JavaScript application、Mobile application、third-party application 在 firewall 之外，所以存取 service 會有lower-bandwidth, higher-latency
* Client 直接使用 service API 但很少這樣因為有以下缺點
    * Client 需要打很多次 API 才能拿到需要的資料
    * API 沒有被封裝好很容易讓 client 跟 service 太過黏導致以後無法改變架構
    * Service 可能是使用 IPC 就無法讓 client 簡單使用
#### API design issues for the FTGO mobile client
* 原本 monolithic 版本只需要打一個 API 就可以收集到所有資料但改成 microservice 需要打好幾個 API 才能收集完所有資料
* Mobile client 等於是 API composer 但有以下缺點
    * 使用者體驗變差，因為當網路環境不穩定的時候太常與 service 溝通會看起來沒有回應的樣子
    * Mobile application 把 service 的知識放進去會導致之後 service 架構無法變更
    * Service 可能使用對於 client 不友善的 IPC mechanisms
#### API design issues for other kinds of clients
* Server-side web application 通常跟 service 同一個 LAN 而且與開發 service 人員合作密切所以適合直接呼叫 service
* Browser-based JavaScript applications 雖然很容易跟著 service API 做變更但會跟 mobile application 一樣遇到網路問題
* Third-party applications 需要穩定的 API 所以需要注意 API 的版本問題
### 8.2 The API gateway pattern
* API gateway 是一個 service 當作外部的 entry point 負責 request routing, API composition 跟其他一些功能例如 authentication
#### Overview of the API gateway pattern
* 跟 object-oriented design 的 Facade pattern 很像
* 把 request 導向對應的 service
* 對不同 service 收集 client 需要的所有資料組合好
* Service 可能有不同 protocol 需要轉換成對應的 protocol
* 提供不同 client 有不同的 API，針對 client 去做 API 設計
* 實作 edge function 確認 client 有權限跟資格使用 API、限制 API 呼叫速率、暫存結果避免過多呼叫、收集 API 數據跟 log，可以實作在 backend service、edge service或在 API gateway
* API layer 有各種 API 的實作而 common layer 實作 edge function
* API layer 由負責的 client team 實作，API gateway team 只負責 common layer
* 每個 client 需要的 API 都有獨立的 API gateway 避免分工不明確
#### Benefits and drawbacks of an API gateway
* 封裝 application 內部架構，減少 round-trip 次數
* 需要有高可用性，可能成為開發瓶頸
