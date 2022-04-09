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
