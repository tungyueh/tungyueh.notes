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
