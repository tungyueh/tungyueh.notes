# Hands-On Microservices with Kubernetes
> Build, deploy, and manage scalable microservices
on Kubernetes

## Chapter 1: Introduction to Kubernetes for Developers
### Kubernetes in a nutshell
#### Kubernetes - the container orchestration platform
* K8s 主要功能是部屬跟管理在一群電腦上大量 container-based 的 workload
* K8s 支援多種 schedule 有效率的打包 container 到 clusted node
* K8s 支援監控 container，可以在 container 壞掉的時候重啟
* K8s 支援重新分配 workload，把有問題的 node 的 workload 分配的其他正常 node
* K8s 是個很有彈性的平台，只跟所配置的運算、記憶體、儲存、網路資源有關

#### The history of Kubernetes
* 從 Docker 開始流行並且改變人們 package、distribute、deploy 軟體的方式
* Docker 單靠自己並不容易規模化的部屬在大的分散式系統
* 漸漸出現可以規模化部屬 Docker 的解決方案
* Google 推出集結數十年經驗打造而成的 Kubernetes 作為規模化部屬 Docker 的解決方案
* Kubernetes 為希臘文舵手的意思所以相關專案常常有關於航海的術語

#### The state of Kubernetes
* K8s 目前已經是家喻戶曉的名字
* DevOps 已經將管理 container 與 K8s 畫上等號
* 知名的 Cloud provider 都支援 K8s
* K8s 不論是在大小型公司都會使用，在各種公司都已經得到驗證
* K8s 背後也有需多大公司像是 Google, Microsoft, Amazon, IBM, VMware 在背後支持
* K8s 每三個月推出一個版本

### Understanding the Kubernetes architecture
* K8s 的設計架構對於 K8s 的成功占了很大一部分的部分
* 每個 Cluster 都有 control plane 跟 data plane
    * Control plane: 在 production 環境中會分散在不同機器上確保高可用性
        * API server
        * Metadata store: 儲存 cluster 的狀態
        * Multiple controller: 管理 data plane 的 node 跟提供 user 的存取功能
    * Data plane
        * Multiple nodes
        * Multiple workers
* Control plane 部屬跟執行 pods (groups of container) 在 nodes 上面並且監控改變跟回應

#### The control plane
##### The API server
* **kube-api-server** 是 REST server expose K8s API
* 可以在 control plane 使用多個 API server 來達到高可用性
* API server 把 cluster 狀態存在 etcd

##### The etcd store
* **etcd store** 是 key-value store 有 consistent, reliable, distributed 的特性
* 如果遺失掉 etcd store 裡面的資料則整個 cluster 也會跟著遺失，所以通常會使用 3-5 個 etcd

##### The scheduler
* **kube-scheduler** 負責將 pods 排程到 worker node
* 根據 node 的資源多寡、user 的 constraint、可用的 node 類型、資源限制等等因素來決定如何排程

##### The controller manager
* **kube-conroller manager** 是個單一個程式包含多個 controller
* 這些 controller 監控 cluster 的 event 跟 change
* Node controller: node 失效時負責通知跟回應
* Replication controller: 確保每個 replica set 有正確數量的 pods
* Endpoints controller: 給每個 service endpoint object，列出 service pods
* Service account and token controllers: 使用預設的 service accounts 跟相關的 API access tokens 初始化新的 namesapce 

#### Data plane
* Data plane 是 cluster 裡的一群 node 負責執行 containernized workloads
* Data plane 跟 control plane 可以共用機器，當 cluster 只有一個 node 會共用但 proudction 環境上 data plane 會有自己的 node

##### The kubelet
* **kubelet** 是 k8s 的 agent
* Talk to API server
    * 從 API server 下載 pod secret
* 執行管理 pods
    * Mounting volumes
    * 透過 Container Runtime Interface(CRI) 執行 pod container
    * 回報 node 跟 pod 狀態
    * 探測 container 是否還活著

##### The kube proxy
* 負責 node 的網路部分
* Forward TCP and UDP packets
* 透過 DNS 或環境變數找出 service 的 IP address

##### The container runtime
* K8s 支援不同的 container runtime
* 原本只支援 Docker 後來支援基於 gRPC 的 CRI
* 每個 container runtime 實作 CRI 讓 **kubelet** 可以使用

##### Kubectl
* **kubectl** 是 k8s 的 CLI
    * Cluster 管理
    * 部屬
    * 偵錯
    * 資源管理
    * Configuration and metadata

#### Kubernetes and microservices - a perfect match
* K8s 與 microservice 跟 software development life cycle(SDLC) 有很多可以互相對應的概念

##### Packaging and deploying microservices
* 當採用了以 microserverce 為主的架構就會有很多 microservice
    * Microservive 彼此獨立的開發與部屬
    * 開發的每個 microservice 都會有 Dockerfile ，所產生的 image 代表開發的單位
    * K8s 在 pod 上面執行 microservice image
* **ReplicaSets** 是有一定數量複製的 sets of pods
    * 使用 ReplicaSet 得時候，K8s 會保證有正確數量的 pod 在 cluster 運作
``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.4
        ports:
        - containerPort: 80
  resources:
    requests:
      cpu: 500m
```
* K8s deployment manifest
* YAML file
* `apiVersion`: K8s resources version
    * Resource version 分為兩部分
        * API group: apps
        * version number: v1
* `kind` 決定處理何種 resource 或 API object
* `metadata` 包含 resource name 跟一些 key-value 的 label
    * resource name 用來參考特定 resource
    * Lable 用來讓有相同 lable 的 resource 操作
* `spec` 是 ReplicaSet 的 spec
    * `replicas` 規定複製幾份
    * `selector` 有一些 `matchLabels`
    * `template` 是 pod 的 template
    * ReplicaSet 管理 label 符合 `matchLabels` 的 pods
    * Pod template 為兩部分
        * `metadata` 使用 label 的地方
        * `spec` 規定在 pod 裡的 container
            * `name`
            * `image`: package microservice 的地方，通常是 Docker image
            * `ports`

##### Exposing and discovering microservices
* 我們需要 expose 部屬好的 microservice 才能讓 cluster 裡的其他 service 可以使用或者讓外面的人使用
* K8s 提供 `service` 的 resource 來 expose service
* Service 用 DNS 或者環境變數來在 cluster 裡面找到其他 service
* 如果想要讓 servicer 讓外面的世界使用則需要用設只好 ingress object 或 load balancer

##### Securing microservices
* 運行龐大的系統時，security 是一個重要考量的問題
* Microservice 比起單一的系統要處理 security 問題更困難，因為有很多 internal communication
* Micorservice 也強調隨時變化的軟體開發特性所以做好 security 更加困難
* K8s 有提供須多關於 security 的 feature 但本身還是要依照 best practice 來開發

###### Namspaces
* Namespce 讓我們可以將 cluster 裡面切割成不同獨立部分
* Namespace 可以規定 resource 的 scope 跟 operation
* Pod 只能只能直接存取 namespace，其他 namespace 需要透過 public API

###### Service accounts
* Service account 提供識別 microservice
* 每個 service account 都有自己的權限
* 可以將 pod 或 microservice 關連到 service account 來享有相同的權限
* Service account 有 secret 來做 authentication

###### Secrets
* K8s 提供所有 microservice 管理 secret 的功能
* Secret 被加密存在 etcd 跟傳輸時都會加密
* Secret 被個別 namespace 所管理
* Secret 在 pod 以 file 或環境變數的形式所使用
* Secret 有兩個 map `data` 跟 `stringData`

###### Secure communication
* K8s 使用 client-side certificate 做雙向驗證
* Cluster 外部來的都需要使用 HTTP
* Cluster 內部之間的溝通需要使用 HTTPS
* 需要先 enable 才能使用此功能，預設不開啟
* API server, node, pod, service 之前的溝通預設使用 HTTP 跟不驗證

###### Network policies
* Container, pod, node 之間的透過網路的溝通控制是很重要的
* K8s 支援 network policy 讓我們可以彈性定義網路流量與 cluster 之間的溝通

##### Authenticating and authorizing microservices
* Authentication 與 authorization 跟 security 也相關，限制 user 的存取權限與 K8s 功能
* K8s 支援多種 authentaction，X.509 certificate, HTTP basic authentication, external authentication server via webhook
* Authentication 確認符合身分
* Authorization 決定可以做哪些事情

###### Role-based access control (RBAC)
* K8s 並不一定需要 RBAC 可以用其他方式但 RBAC 是 best practice
    * Role: 使用 rule 來規定 resource 的 permission
        * `Role`: 只在一個 namespace 使用
        * `ClusterRole`: cluster 所有的 namespace 都使用
    * Binding: 關於一系列的 subject 像是 users, user groups, service accounts
        * `RoleBinding`
        * `ClusterRoleBinding`: 可以指使用在特定 namespace，為了方便定義在多個 namspace 下使用的 role

##### Upgrading microservices
* 部屬後隨著系統演化則會需要升級 microservice
* 升級的目標是無縫接軌跟有問題發生可以安全 rollback 回之前狀態
* 升級 service 通常會升級 image、改變支援的 resource、改變存取的 volumes, roles, quotas, limits 等等

###### Scaling microservices
* 使用多個 pods 備份 microservices
* 增加 Cluster 的 capacity
* 透過增加 replica 的數量可以簡單增加 microservice 的 scale，但對於 request 不固定的 microservice 需要花心力來維持，太多浪費太少不足以應付
* K8s 支援 horizontal pods autoscaling，根據 CPU, memory 等等其他定義的 metric 來自動增減規模

###### Monitoring microservices
* 雖然 k8s 可以自動重開機跟延展服務，但是還是需要監控問題與效能以優化與減少開支
* Third-party logs, Application logs, Application errors, Kubernetes events, Metric 是需要監測的幾個方向
* 充分的 logs 對於系統是很重要的，集中化紀錄 log 可以方便照我們的意願分析
    * 紀錄 error 在 log 裡包含 stack trace 
    * Metric 分析效能跟系統健康狀態

###### Logging
* K8s 上要集中 log 有以下幾種方式
    * 在每個 node 上跑 logging agent
    * Inject logging sidecar container 在每個 application pod 中
    * 讓 Application 送 log 到 central logging service

###### Metrics
* K8s 使用 cAdvisor 作為紀錄 metric 的工具
* K8s 通常提供名為 **heapster** 的 metric server 但需要額外的 backend 與 UI

### Creating a local cluster
* K8s 在開發上可以容易的在 local 產生近似於實際環境的 cluster
* Local cluster 方便讓人在 local 測試開發的 microservice 與 cluster 裡的 microservice 互動


## Chapter 2: Getting Started with Microservices
### Programming in the small - less is more
* 簡單的程式由於腦中記得所有事情所以很好除錯
* 如果要將單一程式加入大系統中需要考量很多事情
    * Rate limit call
    * Logging
    * Metrics collection
    * Versioning
    * Health check
    * Configuration
* 大系統會有很多準則需要遵守，microservice 與其他 microservice 彼此合作達到系統需求
* 開發 microservice 需要花功夫達到某種程度上的獨立才能方便測試跟除錯

### Making your microservice autonomous
* 對抗複雜的好方法之一就是讓 microservice 獨立不需要依賴於其他 service，獨立的 microservice 只需要管理好自己的狀態並最大限度的不管系統其他部分
* 獨立的 microservice 好處在於不管系統其他的 service 如何演化或自己被多常使用到，自己的複雜度都不會改變

### Employing interfaces and contracts
* 使用 interface 的好處在於只要 expose 後就可以隨意改變實作的方式
* Interface 存在於 process 內被其他人所使用的構造
* Interface 對於測試 components 之間的互動很好用
* Interface 只定義 method 跟輸入輸出，並沒有定義實際的行為，Contract 則是定義實際的行為，contract 不容易被定義清楚而且 Go 也不支援

### Exposing your service via APIs
* Microservices 透過網路與其他 service 或外面世界溝通，expose 自己的 API 來讓其他人使用自己所提供的功能
* API 就算是程式語言的 interface 一樣提供 high levle representation
* Microservice 雖然可以使用其他網路 protocal 來提供功能，但除非為了要支援 legacy 外盡量都使用 API
* Internal microservices 是指允許被相通網路或 cluster 存取，這些可以 expose 特製的 API 因為 service 與 client 端都被我們所控制
* External microservice 被外界所用，通常會被 web browser 或不同的程式語言存取
* 使用標準網路 API 而不是用特定語言的協定好處在於可以支援多種語言

### Using client library
* 使用 network API 需要考量網路的各種問題，善用 library 有效率的處理這些問題

### Managing dependencies
* Library/packages linked 到 running service process，透過語言的 package management system 管理 library 或 package，Go 沒有 official package management system，目前 Go modules 是 official solution
* Remote services 透過 network 存取，透過 endpoint 與 API version 來管理

### Coordintating microservices
* Microservice 為主的系統相較於一般系統而言各方面都更複雜，理解、修改、偵錯，透過網路會有更多未預期的問題出現
* 為了得到 microservice 的好處需要有紀律的採用 best practice 來實作

#### The uniformity versuse flexibility trade-off
* 假設有數百的 microservice 但都非常小也非常相似，各方面都是一樣的，只有小部分特別處理 use case，這類型的系統雖然有很多 microservice 但不難理解，隨著 use case 的增加也不會讓複雜度提高太多
* Uniform microservice 相反會失去一些好處，像是無法適用最適當的語言來開發，需要遵守 log 與回報錯誤的方式
* 需要了解不同 microservices 的好壞處，根據系統找出最適當的方式

### Taking advantage of ownership
* 因為 microservice 很小所以一個人就可以負責整個 microservice 並且完全瞭解
* 一個人負責整個 microservice 可以獨立開發增加產能而且對於系統的危險也較低

### Understanding Conway's law
* 系統的結構會反應組織的結構
* 不需要指定 team 負責單一 microservice
* 多群 high-level microservice 互相合作來實作系統功能
* Vertical, Horizontal, Matrix 為思考 high-level structure 的選擇
* 系統需要轉變的時候通常依照 Vertical, Horizontal, Matrix
* Microservice-based 系統比起一般系統容易轉變

#### Vertical
* 將系統功能切分由 team 負責相關的 microservice 從設計、實作、部署、維護一手包辦
* 容易擴展的特性讓大型組織常常採用，但不同的 team 容易有 duplication 的部分
* 需要找的一個折衷點把共同的功能包裝起來讓不同 team 可以使用

#### Horizontal
* 將系統分層而 team 也依照分層，可能有前端、後端、維運的 team
* 功能的實現仰賴於跨 team 的合作
* 較適合小型的組織
* 好處在於可以在 layer 中共享專業的知識，通常會從 horizontal 開始，隨著組織成長再將切分成 vertical

#### Matrix
* 組織察覺到不同 vertical 的 duplication 跟 variation 因而調動人員到不同的 vertical
* 有 cross-cutting group 與 vertical team 合作為整體提供一致性

### Troubleshooting across multiple services
* 系統中的功能由許多 microservice 互動而產生，所以為了可以追蹤 request 在不同 microservice 與 datastore 的流向可以上個 tag 讓我們有辦法從頭到尾找出軌跡
* 偵錯分散式系統需要有很多不同方面的知識，所以造成的阻礙就很多，需要建立一個透過整合 microservice 內部資訊而形成系統觀點的資訊來幫助分析

### Utilizing shared service libraries
* 採取 uniform microservice 的時候使用 shared library 是非常好用的，所有 microservice 都可以使用 cross-cutting concerns 像是 Configuration, Secret management, Service discovery, API wrapping, Logging, Distributed tracing
* Library 可能負責大部分的 workflow 而 microservice 只負責使用 library 跟實作屬於自己的功能
* 維護 library 的成本會隨著越多 microservice 採用的提高，尤其在於不同 microservice 使用不同版本後版本升級所造成的問題可能更多

### Choosing a source control strategy
#### Monorepo
* Code base 都在一個 control repsitory
* 好處:
    * 簡單操作，因為所做的改動都會立即反應
* 壞處
    * 當要逐步改動系統就需要 workaround，像是使用複製加上新 changes
    * 雖然 source code 與 code base 保持同步但不保證部署的 code 也是最新的
    * 需要客製化工具來管理 multi repo

#### Multiple repos
* 每個 project 有自己的 source control repsitory
* Project 把其他人都當成 third-party libraries
* 好處:
    * Project 與 service 有清楚的界線
    * Source control repositories 與 service 或 project 一對一的對應
    * service 的開發過程可以從 source control repositories 對應
    * 對於 dependencies 一視同仁
* 壞處:
    * 需要在多個 repositories 做改動
    * 需要維護 repository 的多個版本
    * 不容易使用 cross-cutting changes

#### Hybrid
* 使用少量的 repositories，每個包含多個 service 跟 project
* 適合使用在組織與地理區隔明確的時候

### Creating a data strategy
* 管理系統裡的資料是很重要的，大部分的資料應該能在系統失效的時候可以保存下來或者重建
* Microservice 只對自己的資料負責

#### One data store per microservice
* 一個 microservice 有一個 data store 是 microservice architecture 的準則，因為如果共用 data store 會有 coupling 的問題
* 如果想要同時使用 in-memory data store 跟 persistent data store 就需要寫入到兩個 data store，查詢只從 in-memory
* 由於設定可能不同所以最好每個 microservice 的 database 由許多 logical databse 組成

#### Running distributed queries
* Microservice 有自己的 data store 代表著整個系統的 state 分散著
* 查詢都需要從多個 data store 才能拼湊出答案
* Consumer 為了最佳化查詢步驟需要知道
    * Consumer 需要知道資料是怎麼被系統管理的
    * Consumer 需要拿到 microservice 的權限
    * 改動 architecture 可能需要改動 consumers
* CQRS 跟 API composition 可以解決這些問題
* 這兩個解決方案都可以使用相同的 API

##### Employing Command Query Responsibility Segregation
* Command Query Responsibility Segregation (CQRS)
* 從不同 microservice 收集來的 data 聚集在一個 read-only data store 來回到特定的問題
* CQRS 把更新資料的責任與讀取資料的責任分開
* Pros:
    * 查詢不會影響更新資料過程
    * API 專為特定問題設計
    * 可以輕易改變管理 data 的方式而不影響查訊的人
    * 反應時間很快
* Cons:
    * 增加系統的複雜度
    * Data duplication

##### Employing API composition
* 像是 CQRS expose 回答特定問題的 API 但沒有自己的 data store
* Request 來的時候才開始去收集分析資料
* 系統沒有提供 event notification 的時候很有用
* Pros:
    * 輕量解決方案
    * API 專為特定問題設計
    * 結果都是即時的
    * 沒有 architectural 需求
* Cons:
    * 任何一個 microservice 失敗都會導致失敗，需要有 retries 跟 timeout 機制
    * 頻繁查詢會造成主要 data store 的負擔

#### Using sagas to manage transactions across multiple serivces
* 保持 distributed data integrity是複雜的問題
* 如果把資料都存在一個 relational database 就可以讓 database engine 負責 data integrity
* 分散式的 data store 需要靠自己的 code 來維護，而 saga pattern 可以解決這樣的顧慮

##### Understanding ACID
* Atomic: 全部的操作要馬一起成功要馬一起失敗
* Consisten: data 操作前後的狀態都要符合限制
* Isolated: 同步的 transactions 要像是 serialized
* Durable: 當 transaction 成功完成則結果會持續保存
    * Persistence to disk: disk 沒 fail 的情況下重啟還是可以保持
    * Redundant memory on multiple nodes: 所有 node 都沒有暫時失效時重啟 node 而且 disk fail 還可以保持
    * Redundant disks: Disk fail 還可以保持
    * Geo-distributed replicas: Data center 失效還可以保持
    * Backups: 便宜的儲存很多資訊但要回覆的時候會緩慢的回覆通常無法即時回覆

##### Understanding the CAP theorem
* Consistency
* Availability
* Partition resiliency
* 實際上通常會打造 CP system, AP system
    * Consistent and partition resilient (CP system)
        * 永遠保持一致
        * Components 彼此網路分離時無法查詢跟改動
    * Acailable and partition resilient (AP system)
        * 永遠可用
        * 系統分離時個部分還可以運作
        * 系統可能會不一致
        * 最終會保持一致，當個部分的連結回覆後會開始同步

##### Applying the saga pattern to microservices
* Saga pattern 集中管理所有 microservice 的 operation
* Atomic: 如果有 transaction 失敗則 compensating operaiont 會被執行，transaction 途中所造成的改動會立刻反映在 microservice 上面但最終還是會達成一致，但寫 code 需要知道資料有可能有時後不一致
* Saga 是 operation 的集合都有 compensating operation 對應，當 operation 失敗則本身與之前的 compensating operation 都會被執行，系統就會被 roll back 到之前的狀態
* Saga 並不容易的實作因為 compensating operation 也有可能失敗，好的實作方式是在其他地方跑一個程式定期去清理失敗的 sagas

### Summary
* 本章介紹 microservice 的基本原則跟如何使用很多小的 microservice 來組成容易擴張的系統
* 討論了開發 microservice architecture 的挑戰，提供一些概念、選擇、好的實作方式跟程式上的建議 

## Chapter 3: Delinkcious - the sample application
* Delinkcious 效仿 Delicious 網站，主要功能是管理網路書籤
* Delinkcious 讓使用者可以在網頁上儲存 URL、為 URL 上 tag 跟支援各種查詢
* Delinkcious 會 demo 很多的 microservices 跟 K8s 的觀念
* 本章節會讓讀者知道為何選擇 Go 跟介紹Go microservice toolkit - Go kit，最後藉由 socail graph service 從不同面向剖析 Delinkcious
* 本章涵蓋下列主題
    * The Delinkcious microservices
    * The Delinkcious data storage
    * The Delinkcious API
    * The Delinkcious client libraries
* The code
    * https://github.com/the-gigi/delinkcious
    * https://github.com/PacktPublishing/Hands-On-Microservices-with-Kubernetes/

### Choosing Go for Delinkcious
* Go 對於 microservice 是個優秀的語言
    * Go compile 成一個 binary 沒有 dependencies
    * Go 很容易讀也很容易學
    * Go 對於網路與 concurrency 支援度很高
    * Go 實作很多 cloud-native data store, queues and framework

### Getting to know Go kit
* 可以非常簡單的用任何一個語言寫出 microservice 用 API 與其他人互動，但現實中還需要有很多 concern
    * Configuration
    * Secret management
    * Central logging
    * Metrics
    * Authentication
    * Authorization
    * Security
    * Distributed tracing
    * Service discovery
* Go kit 採用模組化的方式來面對 microservice，把 concern 很高程度的分開

#### Structuring microservices with Go kit
* Business logic 可以很單純的用 Go libraries 寫好，剩下關於 microservice 的複雜邏輯像是 APIs, serialization, routing, networking 移到另一層
* 把 Business logic 與 microservice logic 分開後可以在簡單的環境中測試開發，讓整個開發流程變得很好
* Delinkcious 的 social graph service interface，只單純的 Go 沒有包含 API, microservice, Go kit 的 notation
``` go
type SocialGraphManager interface {
	Follow(followed string, follower string) error
	Unfollow(followed string, follower string) error

	GetFollowing(username string) (map[string]bool, error)
	GetFollowers(username string) (map[string]bool, error)
}
```
* 實作的地方也都跟 Go kit 與 microservice 無關
``` go
package social_graph_manager

import (
	"errors"
	om "github.com/the-gigi/delinkcious/pkg/object_model"
)

type SocialGraphManager struct {
	store om.SocialGraphManager
}

func NewSocialGraphManager(store om.SocialGraphManager) (om.SocialGraphManager, error) {
	if store == nil {
		return nil, errors.New("store can't be nil")
	}
	return &SocialGraphManager{store: store}, nil
}

func (m *SocialGraphManager) Follow(followed string, follower string) (err error) {
	if followed == "" || follower == "" {
		err = errors.New("followed and follower can't be empty")
		return
	}

	return m.store.Follow(followed, follower)
}

func (m *SocialGraphManager) Unfollow(followed string, follower string) (err error) {
	if followed == "" || follower == "" {
		err = errors.New("followed and follower can't be empty")
		return
	}

	return m.store.Unfollow(followed, follower)
}

func (m *SocialGraphManager) GetFollowing(username string) (map[string]bool, error) {
	return m.store.GetFollowing(username)
}

func (m *SocialGraphManager) GetFollowers(username string) (map[string]bool, error) {
	return m.store.GetFollowers(username)
}
```
* Go kit service 就像個洋蔥，核心部分為 business logic，之後的每一層都是一種 concern 像是 routing, rate limiting, logging, metrics, 最後 expose 給其他人用
* Go kit 主要使用 request-response model 支援 RPC-style communication 

#### Understanding transports
* Microservice 重要考量在於要透過網路其他人互動所以 Go kit 透過 trnasport 的概念支援 microservice 需要的網路部分
* Go kit 支援 HTTP, gRPC, Thrift, net/rpc
* Go kit transport 可以在不改變 code 之下用不同的 transport 方式 export service

#### Understanding endpoints
* Go kit microservces 其實就是由一堆 endpoints 所組成，每個 endpoint 對應到 service interface 的一個 method
* Endpoint 一定用一個 transport 跟一個 handler
* Go kit endpoint 支援 RPC-style communication 跟有 request reqsponse struct
* Factory function for the `Follow()` method endpoint
    ``` go
    func makeFollowEndpoint(svc om.SocialGraphManager) endpoint.Endpoint {
        return func(_ context.Context, request interface{}) (interface{}, error) {
            req := request.(followRequest)
            err := svc.Follow(req.Followed, req.Follower)
            res := followResponse{}
            if err != nil {
                res.Err = err.Error()
            }
            return res, nil
        }
    }
    ```

#### Understanding services
* Code 插入到系統的地方，當 endpoint 被呼叫後則 service 相關的 method 做事
* Request 跟 respsone 的 encoding 跟 decoding 都被 endpoint wrapper 做完了，我們可以只專注在如何達到最好的 abstraction
* Implementation of `SocailGraphManager` function `Follow()` method
    ``` go
    func (m *SocialGraphManager) Follow(followed string, follower string) (err error) {
        if followed == "" || follower == "" {
            err = errors.New("followed and follower can't be empty")
            return
        }

        return m.store.Follow(followed, follower)
    }
    ```
    
#### Understanding middleware
* Go kit 是組合式的，除了基本的 transport, endpoint, service，也用 decorator pattern 可以透過 wrap service 跟 endpoint 增加 cross-cutting concerns 像是 Resiliency, Authentication and Authorization, Logging, Metric collection, Distributed tracing, Service discovery, Rate limiting
* Go kit 的 decorator pattern 除了基本需求外可以用同樣的方式增加自己的想要的 concern，這種方式讓人很容易理解與使用

#### Understanding clients
* Chapter 2 有提到 microservice 最好有 client library 透過 expose interface 與其他 microservice 互動，Go kit 支援寫出 client libraries
* 透過 interface 就可以使用 microservice 除了方便測試也可以容易將太大的 microservice refactoring 成小的 microservice
* Go kit 的 client endpoint 跟 service endpoint 很像但是只是反過來而已
    * Service endpoint
        1. Decode request
        2. Delegate work to service
        3. Encode respsones
    * Client endpint
        1. Encode request
        2. Invoke remote service over the network
        3. Decode response
* `Follow()` method of the client
    ``` go
    func (s EndpointSet) Follow(followed string, follower string) (err error) {
        resp, err := s.FollowEndpoint(context.Background(), FollowRequest{Followed: followed, Follower: follower})
        if err != nil {
            return err
        }
        response := resp.(SimpleResponse)

        if response.Err != "" {
            err = errors.New(response.Err)
        }
        return
    }
    ```

#### Generatin the boilerplate
* 清楚的分離 concern 與 architecture 也要付出代價，就是需要處理一堆翻譯不同的 struct 與 method signature 的 request, response 的 boilerplate
* 可以了解 Go kit 如何使用 strong-typed interface 通用的支援
* 對於大型 project 最好使用從 Go interface 與 data type 自動產生 boilerplate，目前 Go kit 有 kitgen 的 project 來做這件事

### Introducing the Delinkcious directory structure
* 初期階段有三個 service
    * Link service
    * User service
    * Social graph service
* High-level directory structure 包含下列的 sub directories
    * `cmd`
    * `pkg`
    * `svc`
* `root` directory 包含一般檔案像是 `README.md` 跟支援 Go module 的檔案像是 `go.mod`, `go.sum`
* 使用 monorepo 所以整個 Delinkcious 系統都在這個 directory structure 裡面，可以想像成有很多 package 的一個 Go module

#### The cmd subdirectory
* 支援開發的 tool 跟 command 
* operation 
* End-to-end tests 包含 actors, services, external dependencies 像是使用 client library 測試 microservice

#### The pkg subdirectory
* 所有 `package` 存在的地方
    * Microservices
    * Client libraries
    * Abstract object model
    * Other support packages
    * Unit tests
* 這些 code 以 Go package 的方式存在可以在與 microservice 綁再一起之前方便做開發跟測試

#### The svc subdirectory
* Microservices 存在的地方
* 每個 microservice 在 main package 都有自己的 binary

### Introducing the Delinkcious microservices
* 從裡面的 service layer 慢慢做到 endpoint 最後到 transport
* 有三種 service
    * Link service
    * User service
    * Social graph service

#### The object model
* 所有 service implement 的 interface 跟相關的 data types
* `interfaces.go`: Delinkcious 三個 service 的 interface
    ``` go
    package object_model

    type LinkManager interface {
        GetLinks(request GetLinksRequest) (GetLinksResult, error)
        AddLink(request AddLinkRequest) error
        UpdateLink(request UpdateLinkRequest) error
        DeleteLink(username string, url string) error
    }

    type UserManager interface {
        Register(user User) error
        Login(username string, authToken string) (session string, err error)
        Logout(username string, session string) error
    }

    type SocialGraphManager interface {
        Follow(followed string, follower string) error
        Unfollow(followed string, follower string) error

        GetFollowing(username string) (map[string]bool, error)
        GetFollowers(username string) (map[string]bool, error)
    }

    type LinkManagerEvents interface {
        OnLinkAdded(username string, link *Link)
        OnLinkUpdated(username string, link *Link)
        OnLinkDeleted(username string, url string)
    }
    ```
* `types.go`: interface 的 method 用到的 struct
    ``` go
    package object_model

    import "time"

    type Link struct {
        Url         string
        Title       string
        Description string
        Status      LinkStatus
        Tags        map[string]bool
        CreatedAt   time.Time
        UpdatedAt   time.Time
    }

    type GetLinksRequest struct {
        UrlRegex         string
        TitleRegex       string
        DescriptionRegex string
        Username         string
        Tag              string
        StartToken       string
    }

    type GetLinksResult struct {
        Links         []Link
        NextPageToken string
    }

    type AddLinkRequest struct {
        Url         string
        Title       string
        Description string
        Username    string
        Tags        map[string]bool
    }

    type UpdateLinkRequest struct {
        Url         string
        Title       string
        Description string
        Username    string
        AddTags     map[string]bool
        RemoveTags  map[string]bool
    }

    type User struct {
        Email string
        Name  string
    }
    ```
* `object_model` package 使用到基本的 Go types 跟 standard library types 跟自訂定義的 Delinkcious types，因此沒有與 networking, API, microservice, Go kit 的 dependency

#### The service implementation
* 下一層開始實作 service interface，每個 service 有自己的 package
    * github.com/the-gigi/delinkcious/tree/master/pkg/link_manager
    * github.com/the-gigi/delinkcious/tree/master/pkg/user_manager
    * github.com/the-gigi/delinkcious/tree/master/pkg/social_graph_manager
``` go
package social_graph_manager

import (
    "errors"
    om "github.com/the-gigi/delinkcious/pkg/object_model"
)

type SocialGraphManager struct {
    store om.SocialGraphManager
}
```
* import `object_model` 因為需要實作 `om.SocialGraphManager` interface
* Define `SocialGraphManager` 的 `struct` 有一個 field 名為 `store` 型態為 `om.SocialGraphManager`
* `store` field 的 interface 與 manager interface 一致
* 多一個 `store` field 是為了之後可以方便做 validation logic 跟 delegate the heavy lifting to store
``` go
func NewSocialGraphManager(store om.SocialGraphManager) (om.SocialGraphManager, error) {
    if store == nil {
        return nil, errors.New("store can't be nil")
    }
    return &SocialGraphManager{store: store}, nil
}
```
* `store` field 是個 interface 方便我們用相同 interface 不同 store
* `NewSocialGraphManager` 回傳一個使用提供的 store 的 SocialGraphManager
* `SocialGraphManager` 很簡單只有做些驗證後就給 `store` 去做
``` go
func (m *SocialGraphManager) Follow(followed string, follower string) (err error) {
    if followed == "" || follower == "" {
        err = errors.New("followed and follower can't be empty")
        return
    }

    return m.store.Follow(followed, follower)
}

func (m *SocialGraphManager) Unfollow(followed string, follower string) (err error) {
    if followed == "" || follower == "" {
        err = errors.New("followed and follower can't be empty")
        return
    }

    return m.store.Unfollow(followed, follower)
}

func (m *SocialGraphManager) GetFollowing(username string) (map[string]bool, error) {
    return m.store.GetFollowing(username)
}

func (m *SocialGraphManager) GetFollowers(username string) (result map[string]bool, err error) {
    result, err = m.store.GetFollowers(username)
    if err != nil {
        log.Printf("Error in GetFollowers(), %v\n", err)
    }
    return
}
```
* Social grapth manager 是個很簡單的 library 所以繼續研究 service 
* `social_graph_service.go` 在 `service` pacakge 裡面
    ``` go
    package service

    import (
        httptransport "github.com/go-kit/kit/transport/http"
        "github.com/gorilla/mux"
        "github.com/the-gigi/delinkcious/pkg/db_util"
        sgm "github.com/the-gigi/delinkcious/pkg/social_graph_manager"
        "log"
        "net/http"
    )
    ```
    * Go kit `http` transport 對於使用 HTTP transport 是必要的
    * `gorilla/mux` package 提供 routing 功能
    * `social_graph_manager` 實作 service 
    * `log` for logging
    * `net/http` serving HTTP
* 只有一個 function `Run()`
    ``` go
    func Run() {
        store, err := sgm.NewDbSocialGraphStore("localhost", 5432, "postgres", "postgres")
        if err != nil {
            log.Fatal(err)
        }
        svc, err := sgm.NewSocialGraphManager(store)
        if err != nil {
            log.Fatal(err)
        }
    ```
    * 建立一個 dat store 給 socail graph manager 
    * `socail_graph_manager` 在該 package 實作功能但由 `service` 做出正確的決定後把 config 好的 data store 傳給它
* Construct 好每個 endpoint 的 handler
    ``` go
    followHandler := httptransport.NewServer(
            makeFollowEndpoint(svc),
            decodeFollowRequest,
            encodeResponse,
        )

        unfollowHandler := httptransport.NewServer(
            makeUnfollowEndpoint(svc),
            decodeUnfollowRequest,
            encodeResponse,
        )
        
        getFollowingHandler := httptransport.NewServer(
            makeGetFollowingEndpoint(svc),
            decodeGetFollowingRequest,
            encodeResponse,
        )

        getFollowersHandler := httptransport.NewServer(
            makeGetFollowersEndpoint(svc),
            decodeGetFollowersRequest,
            encodeResponse,
        )
    ```
    * `NewServer()` function 建立出每個 HTTP transport 的 endpoint
        * `Endpoint` factory function
        * request decoder function
        * `response` encoder function
    * HTTP service 通常都會使用讓 request 或 respsone encode 成 JSON
* 基本上有初始化好的 `SocialGraphManager` 跟 endpoint handler，準備用 `gorilla` router 將 service expose
    ``` go
    r := mux.NewRouter()
    r.Methods("POST").Path("/follow").Handler(followHandler)
    r.Methods("POST").Path("/unfollow").Handler(unfollowHandler)
    r.Methods("GET").Path("/following/{username}").Handler(getFollowingHandler)
    r.Methods("GET").Path("/followers/{username}").Handler(getFollowersHandler)
    ```
* 把 router 傳給 HTTP package 的 method `ListenAndServe()`
    ``` go
    log.Printf("Listening on port %s...\n", port)
	log.Fatal(http.ListenAndServe(":"+port, r))
    ```

#### Implementing the support functions
* 在 `pkg/social_graph_manager` package 完全與 transport 無關所以需要有人幫忙處理 translation, encoding, decoding，這些都在 `transport.go`
* Endpoint 都有三個 function 分別傳入 Go kit 的 HTTP transport's `NewServer()` function
    * The `Endpoint` factory function
    * The `request` decoder
    * The `response` encoder
* The `Endpoint` factory function
    * 以 `GetFollowing()` operation 為例子
    * `makeGetFollowingEndpoint()` 接收 `SocialGraphManager` interface 回傳 `endpoint.Endpoint` function
    * `endpoint.Endpoint` function 接收 `Context` 跟 `request` 回傳 `response` 跟 `error`
    ``` go
    type Endpoint func(ctx context.Context, request interface{}) (response interface{}, err error)
    ```
    * `makeGetFollowingEndpoint()` 任務是回傳一個可以接收尚未 type assert 的 generic request 的 function 
    ``` go
    req := request.(getByUsernameRequest)
    ```
    * 我們使用可能從任何 stronglg typed struct 變來的通用的 object 所以需要使用 type assertion 確保都有我們需要的 field
    ``` go
    type getByUsernameRequest struct {
        Username string `json:"username"`
    }
    ```
    * 只含有一個 field 叫 `Username`
    * 有 JSON struct tag，JSON package 會自動處理 field name 不同的問題
    * 確保 request 有 Username 就可以使用在 `GetFollowing()` method
    ``` go
    followingMap, err := svc.GetFollowing(req.Username)
    ```
    * 得到一個有 follow 該 user 的 user map 跟 standard error
    ``` go
    type getFollowingResponse struct {
        Following map[string]bool `json:"following"`
        Err       string          `json:"err"`
    }
    ```
    * 因為這是 HTTP endpoint 所以需要轉成 JSON
    * 將 map package 到 `getFollowingResponse` struct
    ``` go
    res := getFollowingResponse{Following: followingMap}
	if err != nil {
		res.Err = err.Error()
	}
    ```
    * 完整 function
    ``` go
    func makeGetFollowingEndpoint(svc om.SocialGraphManager) endpoint.Endpoint {
        return func(_ context.Context, request interface{}) (interface{}, error) {
            req := request.(getByUsernameRequest)
            followingMap, err := svc.GetFollowing(req.Username)
            res := getFollowingResponse{Following: followingMap}
            if err != nil {
                res.Err = err.Error()
            }
            return res, nil
        }
    }
    ```
    ``` go
    func decodeGetFollowingRequest(_ context.Context, r *http.Request) (interface{}, error) {
        parts := strings.Split(r.URL.Path, "/")
        username := parts[len(parts)-1]
        if username == "" || username == "following" {
            return nil, errors.New("user name must not be empty")
        }
        request := getByUsernameRequest{Username: username}
        return request, nil
    }
    ```
    * 接收 standard `http.Request`
    * 需要 extract username 然後回傳 `getByUsernameRequest` struct 讓後續 endpoint 使用
    ``` go
    func encodeResponse(_ context.Context, w http.ResponseWriter, response interface{}) error {
        return json.NewEncoder(w).Encode(response)
    }
    ```
    * 照理來說每個 endpoint 都有自己的 `response` encoding function
    * 這邊就使用通用的 encoding function
    * response struct 都要能 JSON serializable

#### Invoking the API via a client library
* REST API 讓 caller 需要了解 URL schema 跟 decode 跟 encode JSON payload
* 提供 client library 可以讓 caller 直接使用，通常在 internal microservice 都是使用相同語言
* Go kit 提供 client-side endpoint 跟 server-side endpoint 非常相似
* social graph 在 `pkg/social_graph_client` package 提供 client library
* `client.go` 與 `social_graph_service.go` 很像
* `NewClient()` function 建立好 endpoints 然後回傳 `SocialGraphManager` interface
    * 使用 Go kit 的 HTTP transport 建好 endpoints
    * 每個 endpoint 都有 URL, a method(GET or POST), a `request` encoder, `response` decoder，就像是 service 的相反
    * 將所有 endpoint assing 到 `EndpointSet` struct 讓 endpoit 可以 expose
    ``` go
    func NewClient(baseURL string) (om.SocialGraphManager, error) {
        // Quickly sanitize the instance string.
        if !strings.HasPrefix(baseURL, "http") {
            baseURL = "http://" + baseURL
        }
        u, err := url.Parse(baseURL)
        if err != nil {
            return nil, err
        }

        followEndpoint := httptransport.NewClient(
            "POST",
            copyURL(u, "/follow"),
            encodeHTTPGenericRequest,
            decodeSimpleResponse).Endpoint()

        unfollowEndpoint := httptransport.NewClient(
            "POST",
            copyURL(u, "/unfollow"),
            encodeHTTPGenericRequest,
            decodeSimpleResponse).Endpoint()

        getFollowingEndpoint := httptransport.NewClient(
            "GET",
            copyURL(u, "/following"),
            encodeGetByUsernameRequest,
            decodeGetFollowingResponse).Endpoint()

        getFollowersEndpoint := httptransport.NewClient(
            "GET",
            copyURL(u, "/followers"),
            encodeGetByUsernameRequest,
            decodeGetFollowersResponse).Endpoint()

        // Returning the EndpointSet as an interface relies on the
        // EndpointSet implementing the Service methods. That's just a simple bit
        // of glue code.
        return EndpointSet{
            FollowEndpoint:       followEndpoint,
            UnfollowEndpoint:     unfollowEndpoint,
            GetFollowingEndpoint: getFollowingEndpoint,
            GetFollowersEndpoint: getFollowersEndpoint,
        }, nil
    }
    ```
* `EndpointSet` 定義在 `endpoints.go` 包含 endpoints，這些 endpoits 都是 function 實作 `SocialGraphManager` method
    ``` go
    type EndpointSet struct {
        FollowEndpoint       endpoint.Endpoint
        UnfollowEndpoint     endpoint.Endpoint
        GetFollowingEndpoint endpoint.Endpoint
        GetFollowersEndpoint endpoint.Endpoint
    }
    ```
* `EndpointSet` 的 method `GetFollowing()`
    ``` go
    func (s EndpointSet) GetFollowing(username string) (following map[string]bool, err error) {
        resp, err := s.GetFollowingEndpoint(context.Background(), getByUserNameRequest{Username: username})
        if err != nil {
            return
        }

        response := resp.(getFollowingResponse)
        if response.Err != "" {
            err = errors.New(response.Err)
        }
        following = response.Following
        return
    }
    ```
    * Accept username as a string
    * 呼叫 endpoint 的 `getByUserNameRequest` 拿到有 username 的 request
    * 因為直接呼叫 endpoint function 發生錯誤不會顯示所以需要 type assert 確保 response 正確，如果有錯則變成一般的 Go error

### Storing data
* 我們已經知道從 HTTP request 的 JSON payload 變成 Go struct 後開始 service implementation 最後 encode response 成 JSON 回傳給 caller，現在開始更深入的看 persistent storage
* SocialGraphManager 只負責處理 followed/follower 的關係所以我們對於如何儲存資料有很多選擇，像是 relational database, key-value stores, graph databases，目前選擇 relational database，如果之後要換 data store 或增加 caching 機制是很容易的因為對於 SocialGraphmanger 來說 data store 是被隱藏起來的
``` go
package social_graph_manager

import (
	"errors"
	om "github.com/the-gigi/delinkcious/pkg/object_model"
)

type Followers map[string]bool
type Following map[string]bool

type SocialUser struct {
	Username  string
	Followers Followers
	Following Following
}

func NewSocialUser(username string) (user *SocialUser, err error) {
	if username == "" {
		err = errors.New("user name can't be empty")
		return
	}

	user = &SocialUser{Username: username, Followers: Followers{}, Following: Following{}}
	return
}
```
* `SocialUser` struct 有 username 跟 follower 跟 followed username
* Data store 名為 `InMemorySocialGraphStore` 包含一個 map，username 對應到 `SocialUser
    ``` go
    type SocialGraph map[string]*SocialUser

    type InMemorySocialGraphStore struct {
        socialGraph SocialGraph
    }

    func NewInMemorySocialGraphStore() om.SocialGraphManager {
        return &InMemorySocialGraphStore{
            socialGraph: SocialGraph{},
        }
    }
    ```
* `InMemorySocialGraphStore` 實作了 `SocialGraphManager` method，像是 `Follow()`
    ``` go
    func (m *InMemorySocialGraphStore) Follow(followed string, follower string) (err error) {
        followedUser := m.socialGraph[followed]
        if followedUser == nil {
            followedUser, _ = NewSocialUser(followed)
            m.socialGraph[followed] = followedUser
        }

        if followedUser.Followers[follower] {
            return errors.New("already following")
        }

        followedUser.Followers[follower] = true

        followerUser := m.socialGraph[follower]
        if followerUser == nil {
            followerUser, _ = NewSocialUser(follower)
            m.socialGraph[follower] = followerUser
        }

        followerUser.Following[followed] = true

        return
    }
    ```
* 藉由 interface as abstraction 可以很好的切分出 concern，只專注在系統的特定部分
* 如果想要換 data store 則 interface 可以省下很多時間

### Summary
* 本章認識到 Go kit toolkit 跟整個 Delinkcious 系統跟 microservices
* Go kit 提供清楚的 abstraction 像是 service endpoint, transport
* 將自己的 code 加入系統中的同時還保持 loosely-coupled
* 走過從 client 到 service 在回來經過的所有 layer

## Chapter 4: Setting Up the CI/CD Pipeline
* 本章節描述 CI/CD pipeline 想要解決的問題跟 K8s 有哪些選擇最後幫 Delinkcious 建好 CI/CD pipeline

### Understanding CI/CD pipeline
* 開發者 commit change 到 source control system 後，這些 change 被 continuous integration(CI) system 偵測到後開始跑測試
* 在把 code 從 feature branch 或 development branch merge 到 master branch 後 CI system 負責把 Docker image build 出來並且放到 image registry
* Continuous Delivery (CD) system 負責把 image 部屬到目標機器上面，CD 確保系統有在期望的狀態上面
* CI/CD pipeline 從偵測 code change 到部屬到 production 的一連串過程，主要由 DepOps 負責 build 跟 maintain pipeline
* CI/CD pipline 是個由事件觸發的工作流程
    * Developer commit change 到 GitHub
    * CI server 跑測試、build Docker image、push image 到 Docker Hub
    * Argo CD server 偵測到新的 image 後部屬到 K8s cluster

### Options for the Delinkcious CI/CD pipeline
* 選擇 CI/CD pipline 是對於系統很重要的，但是通常沒有明顯的選擇
* 一開始想要選一套系統能夠處理整個 CI/CD pipline 但後來決定使用 CircleCI 跟 Argo CD

#### Jenkins X
* Jenkins X 是隱藏 Jenkins 複雜部分提供給 K8s 的 streamlined workflow
* 缺點是 trouble shooting 很複雜、自以為是的設計、不支援 monorepo

#### Spinnaker
* Netflix 的 open source CI/CD solution
* 缺點是複雜的系統、很難學、不是專為 K8s 打造的

#### Travis CI and Circle CI
* 通常會將 CI 跟 CD 分開來因為 CI 是負責產生 image 放到 registry 所以不需要與 K8s 有任何關連，但是 CD 就需要所以適合在 cluster 裡面跑
* Circle CI 功能比較完整而且 UI 比較好
* CI 在 pipeline 中應該與 K8s 無關因為產生的 image 不一定要用在 K8s cluster

#### Tekton
* K8s 原生的 project
* 缺點是因為比較新所以沒有這麼穩定而且功能也沒有像其他人這麼完整

#### Argo CD
* CD solution 需要與 K8s 合作
* 優點是有為 K8s 特製、在 general-purpose workflow engine 上面實作、可以跑在 K8s cluster 上面、用 Go 寫的
* 缺點是不是 CD foundation 或 CNCF 的成員、背後支持的公司不是強大的公司

#### Rolling your own
* 自己做簡單的 CI/CD pipeline 並不是會太困難但考量到讀者就還是使用現有的工具
* 認識了眾多 CI/CD solution 最後決定使用 CircleCI 跟 Argo CD 來當作 Delinkcious 的 CI/CD solution

### GitOps
* Code, configuration, 需要的 resource 都要存在 source control repository
* 當 push code change 到 repository 後 CI/CD solution 就會開始採取相對應的動作
* Rollback 可以藉由 revert 到前一個版本來完成
* Circle CI 與 Argo CD 都支援 GitOps model
    * 當 git push code change， Circle CI 會開始 build 正確的 image
    * 當 git push K8s manifest，Argo CD 會開始部屬 change 到 K8s cluster 上面

### Building your images with Circle CI
#### Reviewing the source tree
* CI 主要是 building 與 testing，所以第一步是先了解 Deklincious 有什麼是需要 build 跟 test
* `pkg` 資料夾含有 service 與 command 用到的 package，需要對這些 package 進行測試
* `svc` 資料夾含有 microservice 所以需要 build service 跟 package，產生 Docker image 然後 push 到 Docker Hub
* `cmd` 資料夾含有 end-to-end test

#### Configuring the CI pipeline
* Circle CI 使用 YAML 檔案來設定
``` yaml
version: 2
jobs:
  build:
    docker:
    - image: circleci/golang:1.11
    - image: circleci/postgres:9.6-alpine
      environment: # environment variables for primary container
        POSTGRES_USER: postgres
    working_directory: /go/src/github.com/the-gigi/delinkcious
    steps:
    - checkout
    - run:
        name: Get all dependencies
        command: |
          go get -v ./...
          go get -u github.com/onsi/ginkgo/ginkgo
          go get -u github.com/onsi/gomega/...
    - run:
        name: Test everything
        command: ginkgo -r -race -failFast -progress
    - setup_remote_docker:
        docker_layer_caching: true
    - run:
        name: build and push Docker images
        shell: /bin/bash
        command: |
          chmod +x ./build.sh
          ./build.sh
```
* 首先定義 build job 跟需要的 Docker images 與環境，接著需要 working directory 讓 `build` command 可以執行
    ``` yaml

    version: 2
    jobs:
      build:
        docker:
        - image: circleci/golang:1.11
        - image: circleci/postgres:9.6-alpine
          environment: # environment variables for primary container
            POSTGRES_USER: postgres
        working_directory: /go/src/github.com/the-gigi/delinkcious
    ```
* 接下來是 build 的步驟，首先是 checkout，在 Circle UI 先連結好 Delinkcious GitHub repository 所以就知道要怎麼 checkout，接著 `run` command 把所有 Delinkcious 的 Go dependencies 抓好
    ``` yaml
        steps:
        - checkout
        - run:
            name: Get all dependencies
            command: |
              go get -v ./...
              go get -u github.com/onsi/ginkgo/ginkgo
              go get -u github.com/onsi/gomega/...
    ```
* 抓好了所有 dependencies 就可以開始跑測試了，使用 `ginkgo` 測試 framework
    ``` yaml
        - run:
            name: Test everything
            command: ginkgo -r -race -failFast -progress
    ```
* 接下來是 build 跟 push 到 Docker images，`setup_remote_docker` 才可以使用 docker daemon，`docker_layer_caching` option 可以藉由 resue 之前的 layer 來加快速度，`build.sh` 實際 build 跟 push，`chmod +x` 確保可以執行
    ``` yaml
        - setup_remote_docker:
            docker_layer_caching: true
        - run:
            name: build and push Docker images
            shell: /bin/bash
            command: |
              chmod +x ./build.sh
              ./build.sh
    ```

#### Understanding the build.sh script
* 開頭加上 shebang 跟會執行 script 的 binary 的路徑
* 如果 script 需要跨平台執行則需要依賴於路徑或使用其他技巧
* `set -eo pipefail` 會在任何錯誤發生的時候馬上停下來
    ``` bash
    #!/bin/bash

    set -eo pipefail
    ```
* 下列幾行只是設好一歇變數跟 Docker image 的 tag
    * `STABLE_TAB`: major 跟 minor version，每次 build 不會變動
    * `TAG`: 包含 CircleCI 提供的 `CIRCLE_BUILD_NUM`，每次 build 都會增加代表 `TAG` 永遠會是 unique
    ``` bash
    IMAGE_PREFIX='g1g1'
    STABLE_TAG='0.6'

    TAG="${STABLE_TAG}.${CIRCLE_BUILD_NUM}"
    ROOT_DIR="$(pwd)"
    SVC_DIR="${ROOT_DIR}/svc"
    ```
* 進到 `svc` 資料夾有所有的 service，接著使用在 CircleCI 設好的環境變數登入 Docker Hub
    ``` bash
    cd $SVC_DIR
    docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD
    ```
* script 會跑過所有 `svc` 內的子資料夾看看有沒有 `Dockerfile`，如果有就開始 build image、上 tags、push 到 registry
    ``` bash
    for svc in *; do
        cd "${SVC_DIR}/$svc"
        if [[ ! -f Dockerfile ]]; then
            continue
        fi
        UNTAGGED_IMAGE=$(echo "${IMAGE_PREFIX}/delinkcious-${svc}" | sed -e 's/_/-/g' -e 's/-service//g')
        STABLE_IMAGE="${UNTAGGED_IMAGE}:${STABLE_TAG}"
        IMAGE="${UNTAGGED_IMAGE}:${TAG}"
        echo "image: $IMAGE"
        echo "stable image: ${STABLE_IMAGE}"
        docker build -t "$IMAGE" .
        docker tag "${IMAGE}" "${STABLE_IMAGE}"
        docker push "${IMAGE}"
        docker push "${STABLE_IMAGE}"
    done
    cd $ROOT_DIR
    ```

#### Dockerizing a Go service with a multi-stage Dockerfile
* Docker image 在 microservice system 很重要，會 build 很多 image 而且每一個都會 build 很多次
* 這些 image 會常常傳輸所以容易變成攻擊的目標，所以需要以 lightweight 跟 present minimal attack surface 的原則來 build
* 使用 base image 可以達成目標，因為最基本的 image 沒有什麼漏洞可以攻擊的
* 使用標準的 Golang image 來 build，最後一行讓 build 變成 static 跟 self-contained Golang binary
    ``` dockerfile
    FROM golang:1.11 AS builder
    ADD ./main.go main.go
    ADD ./service service
    # Fetch dependencies
    RUN go get -d -v

    # Build image as a truly static Go binary
    RUN CGO_ENABLED=0 GOOS=linux go build -o /link_service -a -tags netgo -ldflags '-s -w' .
    ```
* 複製 binary 到 base image 然後建立最小最安全的 image，expose `8080` port
    ``` dockerfile
    FROM scratch
    MAINTAINER Gigi Sayfan <the.gigi@gmail.com>
    COPY --from=builder /link_service /app/link_service
    EXPOSE 8080
    ENTRYPOINT ["/app/link_service"]
    ```

#### Exploring the Circle UI
* 可以設各種 project setting
* Explore build
* 看特定 build 的情況
* 確認 step 的 consol output

#### Considering the future improvements
* Dockerfiles 很多部分都是重複的，有些假設都可以參數化
* 只測試跟 build 有變動的 service

### Setting up continuous delivery for Delinkcious
#### Deploying a Delinkcious microservice
* 每個 Delinkcious microservice 都有一些定義在 YAML manifests 的 resource 在 `k8s` 裡面
* `link_manager.yaml` 包含兩個 resource
    * K8s deployment
    ``` yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: link-manager
      labels:
        svc: link
        app: manager
    spec:
      replicas: 1
      selector:
        matchLabels:
          svc: link
          app: manager
      template:
        metadata:
          labels:
            svc: link
            app: manager
        spec:
          serviceAccount: link-manager
          containers:
          - name: link-manager
            image: g1g1/delinkcious-link:0.6
            imagePullPolicy: Always
            ports:
            - containerPort: 8080
    ```
    * K8s service
    ``` yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: link-manager
    spec:
      ports:
      - port:  8080
      selector:
        svc: link
        app: manager
    ```
* `db.yaml` 描述 link service 如何在 database 存取狀態，用 `kubectl` 來部署: `kubectl apply -f k8s`

#### Understanding Argo CD
* Argi CD 是為 K8s 打造的 continuous delivery solution
* Intuit 創造而 Google, NVIDIA, Datadog 跟 Adobe 都採用

##### Argo CD is built on Argo
* Argo CD 專為 CD pipeline 特製，建立在 Argo workflow engine

##### Argo CD utilizes GitOps
* Argo CD 將 state 存在 Git
* Argo CD 藉由檢查 Git diffs 跟用 Git primitives 去 roll back 跟調整狀態

#### Getting started with Argo CD
* Argo CD 應該要裝在 K8s cluster 特定的 namespace
* Argo CD 安裝了四種 object: pods, services, deployments, replica sets
* Argo CD 安裝了兩個 custom resource dfinitions(CRDs)
* CRDs 讓 project 可以擴充 K8s API 然後增加自己的 domain object、controller跟其他 K8s resources

#### Configuring Argo CD
* 支援 Delinkcious monorepo out of the box
* 會詢問檢查 change 的 Git repository，K8s cluster，之後偵測 repository 的 manifests
* Argo CD 支援多種 manifest 格式跟樣板像是 Helm, ksonnect, kustomize

##### Using sync policy
* Argo CD 預設只會偵測 application manifest 不一致的情況而不會自動同步，但這是好的預設因為有時候可能需要再特定環境下跑測試或者人為需要介入
* 大部分情形都可以自動部署到 cluster 而不需要人為介入，而且 Argo CD 有按照 GitOps 的觀念可以簡單的回到之前的版本
* 自動同步有幾項限制
    * Application 在錯誤狀態不會嘗試自動同步
    * Argo CD 只會嘗試特定 commit SHA 跟 parameters 的自動同步
    * 自動同步失敗後就不會再嘗試了
    * 自動同步後無法 roll back
* Argo CD 提供自動 pruning 的功能，本來預設把不存在 Git 裡面的 resource 不會刪掉，但開啟後就會去刪掉，只要知道自己在做什麼就可以使用

#### Exploring Argo CD
* Argo CD 把 grouping application 稱為 project
* Namespace 就是 K8s namespace 也是 application 安裝的地方
* Cluster 就是 K8s 的 cluster，`https://kubernetes.default.svc` 是 Argo CD 安裝的 cluster
* Status 分辨是否有與 Git repository 的 YAML manifests 同步
* Health 顯示 application 狀態
* Repository 就是 Git repositroy
* Path 是在 repostiroy 裡相對於 `k8s` YAML manifest 存在的地方
* UI 可以顯示所有 `k8s` resource 與 application 的關係，包含 application 本身、service、deployments、pods 跟有多少 pods 再跑

### Summary
* 本章節探討 CI/CD pipeline 對於 microservices-based distributed system 的重要性
* 介紹各種 best practice
    * multi-stage builds 來 building Docker image
    * `k8s` YAML manifest for Postgres DB
    * Deplyment and service `k8s` resources

## Chapter 5: Configuring Microservices using Kubernetes ConfigMaps
* Configuration 在複雜的分散式系統是很重要的，configuration 涉及系統每個方面所以寫程式必須考慮到 configuration
* 本章節結束後會知道 configuration 的重要性、知道如何 static 或 dynamic 的 configure 系統、也會知道 Kubernetes 如何彈性管理 configuration

### What is configuration all about?
* Configuration 是指為了 computing 的 operational data
* Code 根據 configuration 來處理輸入資料來達成系統所需的功能而不是根據 configuration 指定不同演算法，但也是有特例先不討論
* 設計 configuration 思考會被誰所更新是很重要的

#### Configuration and secrets
* 需要存取 database 或 service 的時候需要 secret 來認證，secret 雖然算是 configuration 但由於特殊性質所以會與普通的 configuration 分開管理，會加密存在不同地方來管理
* 本章節先討論普通的 configuration

### Managing configuration the old-fasioned way
#### Convention over configuration
* 有時候根本不需要 configuration，程式只要自己做好決定，好處是結果只要去看程式就好容易預測，壞處是沒有彈性
* 透過 convention 來減少 configuration 的數量

#### Command-line flags
* 程式透過 command-line flag 或 argument 來 configure 自己
* 好處是有彈性、每種語言都適用、已經有好的做法可以遵循
* 壞處是有空格時後需要 quote、multiline argument 不好支援、arguments 數量有限制、每個 argument 大小有限制

#### Environment variables
* 當程式跑在設好的環境中從環境拿取需要的資訊方便使用者不需要重複輸入相同的 arugment

#### Configuration files
* 再有很多 configuration data 的時候適合使用
* 程式通常會搜尋路徑來找尋 configuration 所以提供很大的彈性去共用 configuration
* Configuration files 有很多種格式需要思考何種最適合

##### INI format
* INI files 出現在 Windows 上面，INI 代表 initialization
* 由 key-value pairs 跟 comment 所組成
* Windows API 提供讀寫 INI files 的功能所以很多 Windows application 都使用 INI files 來當作 configuration files

##### XML format
* XML 是 W3C standard
* **eXtensible Markup Language**
* 使用在各種地方像是 data, document, APIs(SOAP), configuration files
* 有自我描述跟自帶 metadata 的特性

##### JSON format
* **JavaScript Object Notation**
* 跟著 dynamic web application 跟 REST APIs 常被使用
* 可以一對一轉成 JavaScript objects
* 不支援註解
* 對於格式有嚴格限制，像是不能再 array 後面加 comma, 需要 serialize dates 跟 times 
* 看起來很累贅，充斥著 quotes, parentheses 跟 需要 escape 很多 characters

##### YAML format
* YAML 是 JSON 的 superset 但是提供更多精準的 syntax 更容易讓人讀懂像是 reference、autodetection of types、支援 multiline

##### TOML format
* **Tom's Obvious Minimal Language**
* 介於 JSON 跟 YAML 之間
* 支援 autodetection types 跟註解

##### Proprietary formats
* 有些程式直接使用自己特製的格式
* 不建議使用自製的格式，應該要在 JSON, YAML, TOML 之間找到適合的格式，因為這樣才有辦法使用 library 來 parse 的 compose

#### Hybrid configuration and defaults
* 很多程式支援多種 configuration 方式
* 通常都會有 configuration resolution mechanism 決定 configuration file 的名字跟位置但還是可以透過設定環境變數或 command-line argument 方式改變 configuration file
* Kubectl 預設去 `$HOME/.kube` 尋找 configuration file 但也可以透過設定 `KUBECONFIG` 來改變 configuration file 的位置，也可以使用 command-line argument `--config` 指定 configuration file 的位置
* Kubectl 支援多個 cluster/context 在同一個 configuration file 裡面，使用 `kube use-context` 切換

#### Twelve factor app configuration
* Web service 跟 appliction 應該都要把 configuration 存在環境變數中，但是都 configuration 改變就需要重新啟動，也受到環境變數本來就有的限制

### Managing configuration dynamically
* Static configuration 改變的時候都需要重新啟動
* 好處是可以不用擔心 configuration 是否會改變 in-memory state 跟正在處理的 reqeust
* 壞處是會失去所有收到的 request 跟 warm-up caches 跟初始化的時間
* 使用 rolling update 或 blue-green deployments 減輕壞處

#### Understanding dynamic configuration
* Service 使用相同的 code 跟 in-memory state 持續運作的時候可以偵測 configuration 改變進而調整行為，對於 operator 來說只要去改變 configuration 而不需要重啟或重新佈署 code 沒改變的 service
* Service 可以有些 configuration 需要重新啟動，有些 configuration 不需要重新啟動
* Dynamic configuration 會改變行為所以需要記錄下來 configuration 做了那些改變

##### When is dynamic configuration useful?
* 如果 single instance service 重新啟動會造成暫時中斷
* 如果有 feature flag 常常來回切換
* 如果 service 對於 initialization 或丟掉收到的 request 處理很複雜得時候
* 如果 service 不支援進階的部屬像是 rolling update 或 blue-green 或 canary deployments
* 如果重新佈署可能會加入不相關的 code

##### When should you avoid dynamic configuration?
* Dynamic configuration 不是萬能的，如果想要完全安全的改變 configuration，則重新啟動會比較好分析處理
* Microservice 通常簡單到可以讓我們找到所有 configuration 改變的地方
* 如果有下列情形最好不要使用 dynamic configuration
    * Service 改變 configuration 需要經過審查
    * 重要的 service 使用 static configuration 比起 dynamic configuration 有較低的風險
    * Dynamic configuration 機制不存在而且開發此機制帶來的好處沒有很多
    * 現存的 service 轉換成 dynamic configuration 的成本沒有少於帶來的好處
    * 進階部屬方式讓使用 static configuration 需要重新啟動的好處跟 dynamic configuration 差不多
    * 追蹤 configuration 改動增加的複雜度太高

#### Remote configuration store
* 全部的 instance 會不斷的查詢 configuration store 看看有沒有改變
* 常見的選擇為 Relational databases(Postgres, MySQL), Key-value stores(Etcd, Redis), Shared filesystems(NFS, EFS)
* 不要把 configuration 存在 service-persistent store，會造成 configuration 散佈在各種 data store 讓管理困難

#### Remote configuration service
* 建立一個 service 提供所有關於 configuration 的需求
* 優點是方便為每個 service 實作管理機制
* 缺點是會有 single point of failure(SPOF)

### Configuration microservices with Kubernetes
* K8s 幫我們跑 container 所以無法為特定的 run 設定不同的環境變數或 command-line arguments
* 只能透過在 Docker image 包含 configuration files 或改變 run command，這也代表需要為每個 configuration 都作一個 image 才有辦法達成，雖然有辦法做到但會很辛苦
* 要使用 dynamic configuration 可以使用 Remote configuration store 跟 Remote configuration service

#### Working with Kubernetes ConfigMaps
* ConfigMap 是 K8s 每個 namespace 所管理的 resource
* 下列是 `link-manager` 的 ConfigMap
    ``` yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: link-service-config
      namespace: default
    data:
      MAX_LINKS_PER_USER: "10"
      PORT: "8080"
    ```
* `link-manager` deployment resource 使用 `envForm` import 這個到 pod
    ``` yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: link-manager
      labels:
        svc: link
        app: manager
    spec:
      replicas: 1
      selector:
        matchLabels:
          svc: link
          app: manager
      template:
        metadata:
          labels:
            svc: link
            app: manager
        spec:
          containers:
          - name: link-manager
            image: g1g1/delinkcious-link:0.2
            ports:
            - containerPort: 8080
          envFrom:
          - configMapRef:
              name: link-manager-config
    ```
* ConfigMap 的 `data` section 設好環境變數在 `link-manager` 跑的時候
* ConfigMap 用來設定環境變數所以是 static configuration，如果要改變需要重啟 service，在 K8s 有下列幾項重啟的方式
    * Kill the pods 會讓 replica set 重新建立 pods
    * 刪除跟重新部屬跟上面一樣效果但不需要刪除 pod
    * 作些其他改變後重新佈署
``` go
	port := os.Getenv("PORT")
	if port == "" {
		port = "8080"
	}

	maxLinksPerUserStr := os.Getenv("MAX_LINKS_PER_USER")
	if maxLinksPerUserStr == "" {
		maxLinksPerUserStr = "10"
	}
```
* `os.Getenv` 從環境變數 `PORT` 跟 `MAX_LINKS_PER_USER` 拿取值，讓我們能在非 K8s 環境中測試
``` go
func runLinkService(ctx context.Context) {
	// Set environment
	err := os.Setenv("PORT", "8080")
	Check(err)

	err = os.Setenv("MAX_LINKS_PER_USER", "10")
	Check(err)

	RunService(ctx, ".", "link_service")
}

func runSocialGraphService(ctx context.Context) {
	err := os.Setenv("PORT", "9090")
	Check(err)

	RunService(ctx, "../social_graph_service", "social_graph_service")
}
```
* 在非 K8s 也能透過設定好環境變數就可以測試

##### Creating and managing ConfigMaps
* K8s 透過 command-line values、一個或多個檔案、整個目錄、直接建立 ConfigMap YAML 檔來建立 ConfigMap
* command-line: `kubectl create configmap test --dry-run --from-literal=a=1 --from-literal=b=2 -o yaml`
    * `--dry-run`: 不實際產生檔案而是先看看實際產生內容會是什麼
    * `--from-literal`: 定義實際的 config item
* 透過檔案建立 ConfigMap 跟 GitOps 觀念很符合，可以追蹤 source configuration 的歷史
* 直接建立 ConfigMap 也不難因為只是一堆 key-value pair
* Pod 以環境變數的方式使用 ConfigMap，改變 ConfigMap 內容需要重啟
* Pod 以檔案的形式使用 ConfigMap，改變 ConfigMap 內容不需要重啟

##### Applying advanced configuration
* 當系統有很多 service 跟很多 configuration 的時候會想要有一個 service 來處理很多的 ConfigMap
* 不同環境有不同的 configuration

#### Kubernetes custom resources
* K8s 可以增加自己的 resource 同時享有 K8s API 所享有的好處
* 首先定義好 custom resource (CRD)，包含 endpoints, API, version, scope, kind, name
* Custom resource 在所有 namespace 都可以使用
* Custom resource 有 CRUD API 跟 CLI support 跟 persistent storage 所以可以發明自己的 object model 來達成很多事情
* 對於 configuraiont 因為 custom resource 在不同的 cluster 都可以使用所以可以拿來當成不同 namespace 的 shared configuration
* CRDs 可以當成集中式的 remote configuration service
* 建立 controller 來監控 CRDs 然後自動複製到相關 namespace 的 ConfigMap

#### Service discovery
* K8s 內建支援 service discovery
* K8s 持續更新每個 service 的 endpoint resource 的所有 backing pod 的 address
* 正常來說不需要直接處理 endpoint resource，service 會自動 expose 給其他 service
* 自己需要處理再 K8s cluster 之外的 service discovery，好的作法是加入 ConfigMap 後，當改變的時候更新

### Summary
* 本章節討論了除了 secret 之外的所有 configuration 的東西
* 從 classic configuration 到 dynamic configuration 到 remote configuration stores 跟 remote configuration services
* 研究 K8s 如何使用 ConfigMap 來處理 configuration，知道 pod 可以用環境變數或檔案的方式來處理 ConfigMap，最後介紹 custom resource

## Chapter 6: Securing Microservices on Kubernetes
* 對於安全需要很嚴格的看待因為對手總是會嘗試尋找漏洞來獲取敏感資訊、跑 botnet、偷取資料、毀損資料、破壞資料、讓系統不穩定
* 安全性需要再一開始就考慮進去

### Applying sound security principles
* 介紹重要的原則背後是如何避免或讓攻擊更困難達到最小化傷害與從傷害中回復
* Defense in depth: 透過多重的 security 防護讓攻擊者很難知道系統的構成，攻擊者只要不知道其中一層就無法攻破，讓系統更安全、攻擊者需要花更多成本、對於非惡意的錯誤保護更完善
* Principle of least privilege: 如果不知情就無法洩漏，所以只給最基本的權限可以降低受到傷害的程度
* Minimize the attack surface: 可以攻擊的點越少則越容易保護，不要 expose 不需要的 API、不要保存不需要的資料、不要提供不同的方式來執行相通任務
* Minimize the blast radius: component 之間除了合作的之外不要讓其他人可以存取確保不會散布到整個系統
* Trust no one: 任何人都不該相信因為任何人都有可能犯錯，保持著這個假設會讓受到的傷害降低
* Be conservative: 不要升到最新版、偏好穩定性、偏好簡單的東西
* Be vigilant: security 需要持續維護，規律的 patch 系統、變換 secret、使用短期的 keys, tokens, certificate、追蹤 CVEs、紀錄所有事情、測試安全性
* Be ready: 發現漏洞時要確保設定處理的協議、遵循協議、修補漏洞、回復系統安全、研究漏洞、評估並且學習、更新 process, tools, security 增加安全性
* Do not write your own crypto: 自製的加密可能會影響效能，加密門檻很高的技術所以就讓專家來做

### Differentiating between user accounts and service accounts
* 每個經過 K8s API server 的 request 都需要根據 account 來做 authenticate、authorize、admit

#### User accounts
* User account 是給人類的像是 cluster 的 admin 或 developer，這些人從 K8s 外面使用 kubectl 操作 K8s
* End user 不需要因為他們只需要 application-level 的 user accounts
* User credential 存在 `~/.kube/config` 檔案，如果有多個 cluster 則會有多個 cluster, user, context 存在裡面

#### Service accounts
* 每個 pod 都有連結到一個 service account，pod 裡的 workload 都使用 service account 來當作 identity
* Service account 的 scope 是 namespace
* 如果 create pod 沒有給 service 就用 namespace 預設的 service account
* 每個 service account 都有 secret 用來與 API server 溝通

### Managing secrets with Kubenetes
* K8s 把 secret 存在 etcd

#### Understanding the three types of Kubernetes secret
* Service account API token: 用來與 API server 溝通的 credential
* Registry secret: 從 private registries pull image 的 credential
* Opaque secret: 自己的 secret

#### Creating your own secrets
* 最簡單也是最好用的建立 secret 的方式是從有 key-value pair 的 `env` 檔案建立

#### Passing secrets to containers
* 將 secret 放在 image: 任何人都可以從 image 知道 secret
* 將 secret 放在環境變數: 可以使用各種指令看到 secret，通常環境變數都會被 log 下來
* 將 secret 當成檔案 mount: 最常見的方式，沒有設好 permission 則任何人都可以進去看所有檔案

#### Building a secure pod
* 不需要與 API server 溝通
* 提供從 private registries pull image 的功能
* Generic secret mount 成檔案

### Managing permissions with RBAC
* RBAC 在 requst 進到 API server 會按照下列步驟處理
    1. Authenticate request 的 user credential 或 service account credential
    2. 檢查 RBAC policy authorize requester 是否可以對於 target resrouce 採用 operation
    3. 跑過 admission controller 看看是否有需要因為任何理由拒絕 request
* RBAC model 包含 identities,  resource, verbs, roles, role binding
* Role 有一堆 rule，每條 rule 決定哪些 verb 可以到 API group 跟 resource
* RBAC role 只在被創造出來的 namespace  有效
* `ClusterRole` 的 permission 在每個 cluster 都有效
* Role 都是只有列出 permission，真正使用上會綁定 account

### Controlling access with authentication, authorization, and admission
#### Authentication microservices
* Service account 跟 RBAC 雖然是個好的 solution 去管理 K8s object identity 跟 access 但是在 cluster 裡面比較不會受到攻擊所以可以使用簡單一點的方式
* 使用 private key infastructure(PKI) 跟 certificate authority(CA) 來處理 issue, revoke, update certificate，但有點複雜
* 使用 K8s secret 並使用 shared secret 給允許溝通的 microservice，當 microservice 收到 request 就去檢查 secret 是否正確

#### Authorization microservices
* Authorize 可以很簡單也可以很複雜，最簡單就是 authenticate 過的就可以執行任何動作

#### Admitting microservices
* Admission 是在 authorize 完根據當下情況再決定是否可以允許 request，通常都是 server side 有 rate limit 或其他關係導致暫時不允許 request

### Hardening your Kubernetes cluster using security best practices
#### Securing your images
##### Always pull images
* `ImagePullPolicy` 決定什麼時候要 pull image，預設是不存在才去 pull
    * 如果使用 tag 才永遠不會去 pull 更新的 image
    * 可能會在相同 node 上與其他 tenant 發生衝突
    * 相同 node 上面其他 tenant 可以跑我們的 image
* `AlwaysPullImages` 會每次都去 pull image

##### Scan for vulnerabilities
* 去 vulnerability database 學習新的漏洞與如何管理
* 使用 tool 掃描 service

##### Udate your dependencies
* 修好漏洞時候要去更新 dependency
* 在安全性與保守之間要取得平衡

##### Pinning the versions of your base images
* 固定好 base image 才能確保每次 build 都是我們所想要的

##### Using minimal base images
* 最小化可以被攻擊的地方會使我們盡量使用 minimal base image
* 除了安全性之外也可以享受快速 pull 跟 push image 的好處

#### Dividing and conquering your network
* 透過 namespace 跟 network policy 限制 service 可以溝通的範圍
* Cluster 裡的 pod 都可以互相溝通，不論是在哪個 node
* 每個 pod 都會有自己的 IP
* Network policy 是一堆規則限制跨 cluster 的溝通
* Netwokr policy 是以 pod 為單位來限制的，所以要把 pod 分好不同的 group 後上好 label 之後才能拿來使用

#### Safeguarding your image registry
* 使用 private registry，不要公開含有自己 code 的 container 因為會被逆向工程找出漏洞
    * 可以使用 third part 管理的 private registry
    * 用自己的 private registry

#### Granting access to Kubernetes resources as needed
* 只提供需要的權限的原則讓我們知道要讓 service 只有權限讀取需要的 K8s resource
* RBAC 先把所有全線都關掉後再增加特定的需求

#### Using quotas to minimize the blast radius
* Limits 跟 quotas 是 K8s 用來控制有限資源的方式
* 如果 workload 有預算限制則可以使用 limit 跟 quotas 更好預測
* 從安全性來看就算讓 attacker 獲得 workload 也可以藉由 limit 來限制可以使用的資源
* Network policy 藉由限制 pod 之間的溝通來減少受影響的 workload，而 resource quota 藉由限制 resrouce 來減少跑在 node 上的 pod 受到的影響
* 有幾種 quota 的型態
    * Compute quota (CPU and memory)
    * Storage quota (disks and external storage)
    * Objects (K8s object)
    * Extended resrouce (non-K8s resrouce like GPU)
* Resource quota 都差不多，重點是 unit 跟 scope 還有 request 跟 limit
    * Request: container 要滿足條件才能啟動，workload 確保有訂好的 memory 與 CPU 可以使用
    * Limits: workload 可以使用資源的上限，超過就有可能被 kill 或導致整個 pod 被 node 趕出去，K8s 會重新啟動 container 
* 通常都會把 request 跟 limit 的限制設成一樣，workload 可以確保有足夠的資源可以使用也不用擔心超過會怎麼樣
* Resource 都是限定於每個 container，如果 pod 有很多 container 才需要考慮整個 pod 所需要的 resrouce
##### Units for requests and limits
* Suffix of memory request and limit: E, P, T, G, M and K
* CPU unit: 1 AWS vCPU, 1 GCP Core, 1 Azure vCore, 1 IBM vCPU, 1 hyperthread on a bare-mental Intel processor with hyperthreading

#### Implementing security contexts
* 有時候需要讓 pod 跟 container 可以有權限存取 node，雖然這情況很少見但是 K8s 還是有 securoty context 的觀念來實作 Linux security
* Security context 方便集中化管理 pod 跟 container 的 security，但在大的 cluster 中使用 third-party packages 很難確保每個 pod 跟 container 都使用正確的 security context，所以需要 pod security policy

#### Hardening your pods with security policies
* Pod security policy 讓我們可以設定 global policy 在新建立的 pod 上
* Pod security policy 可以為 pod 加上 secuirty context 或只允許符合 security context 的 pod 建立

#### Hardening your toolchain
* Argo CD 提供關於 security 的功能
##### Authentication of admin user via JWT tokens
* Argo CD 有 build-in admin user，其他 user 需要 Single-sign on (SSO)
* 對 server 做 authentication 使用 JSON Web Token (JWT)
##### Authorization with RBAC
* Argo CD authorize request 使用對應使用者的 JWT group 到 RBAC role
##### Secure communication over HTTPS
* 所有 communication 都在 HTTPS/TLS 之上
##### Secret and credential management
* Argo CD 需要管理很多敏感性資料
    * K8s secrets
    * Git credentials
    * OAuth client credentials
    * Credentials to external cluster
* Argo CD 決不會在 response 或 log 把敏感性資料洩漏出去
##### Audits
* 藉由查看 git commit log 可以檢查所有活動因為 Argo CD 都是由 git commit 來驅動的
* Argo CD 另外也送出多種 event 讓 cluster 裡面的活動能被看見
##### Cluster RBAC
* Argo CD 使用 cluster-wide admin role

### Summary
* Microservice-based architecture 對於大型企業分散式系統適合的架構，系統需要能夠支援處理敏感性資訊，開發過程中需要減少讓攻擊者可以攻擊的目標
* 我們需要採取嚴格跟最好的方式來保護使用者與資料，介紹了 security 的基本原則跟如何用 K8s 實現
* 介紹 authentication, authorization, admission, secure communication inside and outside cluster, strong secret managment
* Security 永遠無法做好但是遵循最佳實踐方法可以找到安全性與需求上的平衡

## Chapter 7: Talking to the World - APIs and Load Balancers
* 需要將 Delinkcious 給外面的世界使用，因為 Delinkcious user 無法使用跑在 cluster 裡的 service
* 增加 python-based 的 API gateway service 增加 Delinkcious 的功能並 expose 出去
* 增加 gRPC-based news service 讓 user 可以知道 follow user 的 news
* 增加 message queue 來讓 service 之間的溝通不會 coupling

### Getting familiar with Kubernetes services
* Pods 是 K8s 裡的工作基本單位
* K8s service 是將 pod expose 給相關的 sevice 或者外部的世界
* K8s service 有固定的 identity 跟一對一的對應到 application service
* K8s 可以使用環境變數或 DNS-based 的方式找到 service
    * K8s 提供使用 DNS-based 的 discovery，可以使用 `<service name>.<namespace>.svc.cluster.local` 來存取在 cluster 裡面的 service
    * 使用環境變數好處是可以把 service 跑在 K8s 之外的時候測試
* K8s 使用 labled selector 把 service 跟備用的 pod 連結起來後使用 `endpoint` resource 管理符合 label selector 的所有 pod 的 IP address
#### Service types in Kubernetes
* ClusterIP(default): 只有 cluster 裡的 service 能讀取
* NodePort: 將 service 透過特定 port expose 到外面
* LoadBalancer: K8s cluster 跑在 cloud platform 上的 service 提供 load balancer 的功能
* ExternalName: 處理 request 送往在 cluster 外的有提供 DNS name 的 service

### East-west versus north-south communication
* East-west communication 是指 cluster 裡 service 的溝通
* North-south communication 是指 expose service 與外部的溝通

### Understanding ingress and load balancing
* K8s 裡的 ingress 概念是指控制外部對於 service 的 request
* Ingress resource 定義 routing rule，分配所有 request 到目標的 service
* Ingress controller 是 cluster-wide 的 load-balancer 跟 router

### Providing and consuming a public REST API
* 創建一個用 python 的 API gateway servie
* 增加使用 OAuth2 的 user authentication
* Expose API gateway service 到外面
#### Building a Python-based API gateway service
* API gateway service 接收所有外面來的 request 後 route 到正確的 service
##### Implementing social login
* 使用 GitHub 來實作
    * `login()` method 去到 GitHub 要求驗證 user 後給 Delinkcious 權限
    * `logout()` method 移除 access token
    * `authorized()` method 被 GitHub 驗證完後呼叫，提供 access token
##### Routing traffic to internal microservices
* get link request 後 route 到 link microservice 適當的 metdho
##### Utilizing base Docker images to reduce build time
* Go microservice image 大約只需要 10MB 但 API gateway 需要 500MB
* API gateway 需要安裝 C/C++ toolchain 後 build library 需要 15 分鐘以上，但 Docker 可以重複使用 layer 跟 base image，所以將 heavyweight 的事情放到 base image，之後再使用這個 base image 來加上 code 來 build image，好處是 build 速度變快跟只有 API gateway code 的變動
#### Adding ingress
* Minikube 要 enable ingress
* 寫好 ingress manifest 給 API gateway service 就可以讓整個 cluster 都用單一 ingress 讓 request 都導到 API gateway service
#### Verifying that the API gateway is available outside the cluster
##### Finding the Delinkcious URL
* Production cluster 會有 DNS name 但 minikube 要自己拿到 URL
* 把 URL 設在環境變數以便後續使用
##### Getting an access token
1. 去 login endpoint 去登入 GitHub
2. GitHub 詢問是否要授權給 Delinkcious 拿到 email 跟 name
3. 同意後被導向有個人資訊的頁面後把 access token 存在環境變數
##### Hitting the Delinkcious API gateway from outside the cluster
* 使用 HTTPie 呼叫 endpoint 為了驗證需要再 header 加入 access token

### Providinng and consuming an internal gRPC API
* 實作 new service，功能是追蹤 link event，像是 link 的新增跟更新還有回傳所有新的 event 給 user
#### Defining the NewsManager interface
* `GetNews()` method 來得到所追蹤 user 的 link event
#### Implementing the news manager package
* `NewsManager` 裡面的 `InMemoryStore` 實作 `GetNews()`
#### Exposing NewsManager as a gRPC service
* gRPC 集合 wire protocol, payload format, conceptual framework, code genereation facilities 給跨 service 跟 application 使用
* 因為使用 Go-kit framewok 也支援 gRPC
##### Defining the gRPC service contract
* gRPC 需要定義好 service 的 contract
##### Generating service stubs and client libraries with gRP
* `protoc` 產生 servie stub 跟 client library
* 需要產生 Go code 跟 new service 跟 python code 給 API gateway
##### Using Go-kit to build the NewsManager service
* import library 包含產生的 gRPC code 跟 gRPC library
* 建立 TCP listner
* 初始化 news manager，建立 gRPC server，建立 news manager object，把 news manager 註冊到 gRPC server
* 最後 gRCP server 開始接收從 TCP listner 拿到的東西
##### Implementing the gRPC transport
* gRPC transport 與 HTTP trsnport 很像但細節有點不一樣

### Sending and receiving events via a message queue
* news service 儲存 link event 但是 link service 才知道 link 被新增、更新或刪除
    * news service 增加 API 給 link service 去通知，但會讓這兩個 service 綁的很死
    * Link service 送 event 給通用的 message queue service，news service 訂閱後從 message queue 得到 message，這樣就不會綁太死了 
#### What is NATS?
* Open source message queue service
* 支援 Publish-subscribe, Reqeust-reply, Queueing 的 model
#### Deploying NATS in the cluster
* 安裝 NATS operator
* NATS operator 提供 NasCluster Custom Resource Definition(CRD)
#### Sedning link events with NATS
* 當 Link 被新增更新或刪除時 LinkManager 會呼叫相對應的 method
* 把 event sender 在初始化 LinkManager 的時候放進去，之後當 method 被呼叫就會送出 event 給 NATS
#### Subscribing to link events with NATS
* news service 使用 `Listen()` method 來訂閱 link events
#### Handling link events
* news maanger 訂閱 link event 後，結果當 `OnLinkAdded()` 或 `OnLinkUpdated()` 被呼叫時把 event 加進 store
* 之後 user 呼叫 `GetNews()` 就會把 store 存的 event 回傳回去 
* NATS service 就是 command query responsibility segregation(CQRS) pattern

### Uderstanding service meshes
* Service mesh 是另一種層面來管理 cluster
* Service mesh 對於 ingress controller 也可以發揮作用
* Built-in ingress resource 有以下缺點
    * 沒有好的方法驗證 rule
    * Ingress resource 互相 conflict
    * 使用特定 ingress controller 滿複雜而且需要客製化的註解

### Summary
* 實作 API gateway 跟 CQRS 兩種 microservice design pattern
* Cluster 中增加 Python 寫的 service, gRPC service, message queue system
* 目前為止 Delinkcious 已經很接近 production 的階段

## Chapter 8: Working with Stateful Services
* 對於 stateless service 可以跑在任何地方但如果分散式系統需要管理資料則存 data 的 host 失效的時候不能直接啟動新的 instance 因為 data 會不見
* Redundancy 可以避免 data lost，Kubernetes 提供 storage model concept 與相關的 resrouce 來增加 redundancy

### Abstracting storage
* K8s 是個管理 containerized workload 的 engine，從最一開始只支援 Docker 到現在提出 Container Runtime Interface (CRI) 讓符合 CRI 的 runtime 都可以支援
* 網路用 Container Network Interface (CNI) 讓不同 networking solution 使用 CNI plugin 來支援
* 之後介紹 K8s storage model 了解 in-tree out-of-tree plugin 的差異跟 Container Storage Interface (CSI)
#### The Kubernetes storage model
##### Storage classes
* 描述可以支援的 storage type，沒有指定就使用預設值
* 不同的 storage class 有不同的參數綁定到實體的 storage
##### Volumes, persistent volumes, and provisioning
* Volumn 的 lifetime 都跟 pod 一樣，當 pod 消失則 storage 也跟著消失
* 之前看過的 ConfigMap 與 secret volumn 也是種 volumn type，其他還有用來 reading 跟 writing
* K8s 支援 persistent volumn 用來存需要 persistent 的資料，需要由 administrator 提供並不受 K8s 管理
* Dynamic provsioning 是一種在動態 create volumes 的過程
##### Persistent volume claims
* Workload 可以 create persistent volumn claim 來使用 storage
* Persistent volumes 可以被用來在不同 instance 分享資訊，因為 persistent volumes 會被使用相同 persistent storage claim 的 container mount 起來
#### In-tree and out-of-tree storage plugins
* Storage plugin 分成兩種 in-tree 跟 out-of-tree
* In-tree 代表 storage plugin 是 K8s 的一部份，當 provider 需要新增或修改 plugin 都需要改動 K8s
* Out-of-tree 是 K8s 定義了一套 interface 跟一套提供 plugin 的方式，支援 FlexVolume 跟 CSI 但 FlexVolume 已經不建議使用了
#### Understanding CSI
* CSI 是為了解決 in-tree plguing 跟 FlexVolume 的問題
* CSI 不只是 K8s 的標準同時也是工業界的標準，所以寫一個 driver 就可以適用很多地方
* K8s team 提供 Driver registrar, external provisioner, external attacher 三個 component 來支援 CSI
* 這三個 component 的任務是 interface 成 API server，storage provider 會 package sidecar container 到 storage driver，之後可以用 K8s DaemonSet 部屬到 node 上面
##### Standardizing on CSI
* CSI 比起 in-tree plugin 好，但是目前都是用 hybrid 的方式，未來 K8s 會慢慢將 in-tree plugin migrate 到 CSI

### Storing data outside your Kubernetes cluster
* K8s 不是 closed system，跑在 cluster 裡面的 workload 可以存取外面的 storage，通常使用在要 migrate 現有的 application 的時候，先將 workload 從外面移到裡面後讓 container 可以去存取原本在無外面的 storage 最後思考值不值得把外部的 storage 帶到裡面
* 其他適合使用外部 storage 的情況
    * Storage cluster 使用 exotic hardware 或者 networking 沒有合適的 in-tree 或 CSI plugin
    * 在 cloud provider 跑 K8s 但 migrate 的成本跟風險都太高
    * 其他 application 都使用相同 storage cluster，migrate 進 K8s 不實際
    * 因為需求所以需要自己管理 data
* 以下是自己管理外部 storage 的缺點
    * Security
    * 自己需要實作 scaling, availability, monitoring, configuration of storage cluster
    * 改動 storage cluster 需要跟著改動 K8s
    * Performance 下降，因為多出外部網路跟需要 authentication, authorization, encryption 的時間

### Storing data inside your cluster with StatefulSets
* 最好是將資料存在 K8s 裡面因為有提供一站式的管理，另外可以整合 storage 到自己的 streamline monitoring
* 如果將資料存在 node 上面但是 data store pod 在不同的 node 上啟動則資料就會消失，可以自己使用 pod-node affinity 或其他方法但最好是使用 StatefulSet
#### Understanding a StatefulSet
* StatefulSet 是個 controller 負責管理一群 pod 的額外屬性像是 ordering 跟 uniqueness
* StatefulSet 允許部屬 pod 的時候保留特殊屬性
##### StatefulSet components
* StatefulSet metadata and definition: 
* A pod template
* Volumn claim templates
##### Pod identity
* StatefulSet pod 有 stable identity 包含 stable network identity, ordinal index, stable storage，名稱為 `<statefuleset name>-<ordinal>`
* StatefulSet 提供的 stable network identity 的 DNS name: `<service name>.<namespace>.svc.cluster.local`
* 對於每個 pod 的 DNS name 就是 `<statefuleset name>-<ordinal>.<service name>.<namespace>.svc.cluster.local`
##### Orderliness
* StatefulSet 確保 pod initialized, scaled up, scaled down 都會依照順序執行
* K8s 1.7 放寬限制讓 pod 可以同時操作，在 `podPolicy` field 做設定，預設是依照順序操作
#### When should you use a StatefulSet?
* 在 cloud 上 管理 data store 或需要對 data store 有良好管控就適合使用 StatefulSet
##### Comparing deployment and StatefulSets
* Deployment 設計來管理一群 pod 但也可管理一群 data store
* StatefulSet 特別設計來支援分散式 data store
* Deployment 不需要連結到 storage 但 StatefulSet 需要
* Deployment 不需要連結到 servicae 但 StatefulSet 需要
* Deployment pod 沒有 DNS name 但 StatefulSet pod 有
* Deployment 可以以任何順序啟動跟中止 pod 但 StatefulSet 需要依照規定好的順序
* 建議先用 deployment 直到分散式的 data store 需要 StatefulSet 的 special properties
#### Reviewing a large StatefulSet example
* Cassandra 是分散式 data store，很強大但需要很多背景知識
##### A quick introduction to Cassandra
* Cassandra 是 Apache open source project，是 columnar data store 所以適合管理 time series data
* Cassandra node 使用 distributed hash table (DHT) 分享資料，許多複製的資料都分散在不同 node
* 當有 node 失效則其他 node 會負責處理 query，當有 node 新加入則會重新分配資料給它
##### Deploying Cassandra on Kubernetes using StatefulSets
* Cassandra 需要讓 node 彼此不斷溝通，所以需要 seed provider 設定好 IP 給 seed nodes，這些 seed nodes 會通知其他的 nodes
* Seed provider 需要一個 custom class 因為 seed node 也有可能被移除掉
* `KubernetesSeedProvider` class 與 API server 溝通，可以每次都回傳  seed nodes 的 IP address

### Achieving high performance with local storage
* Data 在 processor 附近則可以快速拿來處理，如果需要放在網路上就會變慢
* 主要是將 data 存在記憶體或 local drive 來達成 local storage
#### Storing your data in memory
* Performance 最好的方式
* 缺點是: node 有更多相較於硬碟的限制、memory 很貴、memory 是 ephemeral
* 如果需要將整個 dataset 放在 memory，通常是 dataset 很小或可以分散在不同機器上，可以利用保存 persistent copy 或 Redundancy 來應付 memory ephemeral 的特性
#### Storing your data on a local SSD
* Local SSD 雖然沒有像 memory 這麼快但也足夠了
* 當需要速度但 memory 又不夠大就適合使用

### Using relational database in Kubernetes
* 目前為止使用的 relational database 都不是真正的 persistence
* 先是看資料怎麼儲存的，再來讓它可以 durable，最後 migrate 到 StatefulSet
#### Understanding where the data is stored
* 對於 PostgreSQL 來說，都會有個 `data` 資料夾，根據 mounted 的方式來決定資料是否 persistent
#### Using a deployment and service
* 當 database pod 被 kill 的時候可能會在不同 node 被重新啟動所以之前的資料都會不見
* Delinkcious service 需要將資料存在 PostgreDB container 來保存好資料
#### Using a StatefuleSet
* Data 資料夾是被 mount 在 container 但 storage 是在外部，所以只要外部的 storage 沒問題則我們的資料就沒問題，不管 container, pod, node 發生什麼事情
* 之前介紹過使用 headless service 來定義 StatefulSt 給 user storage，但 headless service 接到 StatefulSet 卻沒有 cluster IP
#### Helping the user service locate StatefuleSet pods
* 雖然 headless 沒有 IP 但卻有 endpoint，endpoint 就是 cluster 裡的 IP 給其他 pod 知道
* 需要找到 headless service endpoint 的人都需要查詢 K8s API，雖然這樣會增加 service 跟 K8s 的 coupling 讓我們難以直接測試，但可以利用 config map 來騙 user service 跟 populate `USER_DB_SERVICE_HOST` 跟 `USER_DB_SERVICE_PORT`
#### Managing schema changes
* 使用 relational database 常常遇到的問題是改變 schema 的時候需要處理 migrate 與否的問題
* 如果可以短暫關機則流程比較簡單
    1. 關掉相關所有受到影響的 service 後執行 DB migration
    2. 部屬新的 code 知道蓋如何處理新 schema
    3. 一切都恢復正常
* 如果需要保持系統持續運作則需要更多步驟，藉由把 schema change 拆解成很多向後相容的 change
    1. 保持原本的 table
    2. 新增兩個 table
    3. 部屬可以讀懂舊跟新 table 的 code
    4. Migrate 所有 data 從舊 table 到新 table
    5. 部屬只讀得懂新 table 的 code
    6. 刪除舊 table

### Using non-relational data stores in Kubernetes
* K8s 跟 StatefulSet 不限於 relational data store，non-relational data store 也很好用
* Redis 是出名的 in-memory data store
#### An introduction to Redis
* Redis 通常被形容成 data structure server，因為它將所有 data store 存在 memory 中而且可以有效率的進行很多進階的操作
* Redis 可以被用來當成快速分散式的 cache 給 hot data 使用
* Redis 支援 clusters 分散資料在不同 node 上面
##### Persisting events in the news service
* `NewRedisNewStore` 建立一個 Redis client 然後 ping 確認 Redis 有啟動
* `RedisNewsStore` 儲存 event 在 Redis list，`GetNews` method 照順序去擷取 event

### Summary
* 本章節談到 storage 與 data persistence 的概念
* 學習到 K8s storage model, common storage interface 與 StatefulSets
* 如何在 K8s 管理 relational 與 non-relation data，然後 migrate Delinkcious service 使用 persistence storage
* 最後使用 Redis 為 news service 實作 non-ephemeral data store

## Chapter 9: Running Serverless Tasks on Kubernetes

### Serverless in the cloud
* Serverless in the cloud 通常有兩個定義
    * 不需要自己管理 cluster 的 nodes
    * Code 不用 deploy 成為 long-running service 但是 package 成 function 在被 invoke 或 trigger
#### Microservices and serverless functions
* 相同的 code 可以以 service 的方式或 serverless function 的方式跑來，主要差別是 operational 方面
* 下列情況比較適合使用 microservice
    * Workload 無法中斷
    * 每個 request 需要較久的時間，無法用 serverless function 支援
    * Workload 使用 local cache
#### Modeling serverless functions in Kubernetes
* K8s 是跑 container 所以 serverless function 會被包在 container 裡面
##### Functions as code
* 單純提供 function
* 好處是開發者可以不管 building image, tagging, pushing to registry 跟 deploy 到 cluster 所有關於 K8s 的事情
##### Functions as containers
* 使用 serverless framework build 成一般的 container
* 雖然跟之前 build microservice 一樣但比較 lightweight，可以不用做 HTTP 或 gRPC server 或 listen events
#### Building, configuring, and deploying serverless functions
* 做好 serverless function 需要 deploy 到 cluster 的時候需要做設定
* 設定包含 scale limit, 怎麼 invoke 跟 trigger
#### Invoking serverless functions
* Serverless function 被 deploy 到 cluster 後就會休眠
* Controller 會監聽 request 後去 trigger function

### Link checking with Delinkcious
* Delinkcious 是個 lin management system，確保 link 的好壞是重要的事情
#### Designing link checks
* Link 有可能暫時或永久壞掉、檢查 Link 可能會是 heavyweight operation、Link staus 可能會隨時改變
* Delinkcious 每個 user 都各自存 link，因此會檢查到重複的 link，也可能有不同 user 有不同的 link status
* 為了把重點擺在 serverless function 所以先接受這些限制，未來在改善
* 設計 link check 有以下幾種選項
    * 當新增 link 的時候，只跑 link service 裡的 link checking code -> 不容易定期檢查 link、檢查 link 是不同的責任應該要分開
    * 當新增 link 的時候，呼叫不同的 link checking service -> 檢查 link 不需要一直跑所以不需要用到 service
    * 當新增 link 的時候，invoke link checking serverless function
    * 當新增 link 的時候，先 pending，定期去檢查最近新增的 link
* 檢查 link 流程
    1. 新增 link 的時候，link manager 會 invoke serverless function
    2. 新增的 link 一開始是 pending state
    3. Serverless function 只會檢查 reachable
    4. Serverless function 送 events 到 NATS system，NATS system 送給有訂閱的 link manager
    5. Link maanger 收到 event 後 update link status 成 valid 或 invalid
#### Implementing link checks
* 增加 `Status` 的 field 到 `Link` object，有 `pending`, `invalid`, `valid` 的值
* 定義 `CheckLinkRequest` object 有 user name 跟 url field
* 定義 interface 給 `LinkManager` 實作當收到 link 已經被檢查後的 event
* 建立一個新 package 用來獨立出來，只有一個 `CheckLink()` function
* HEAD method 只會回傳少量資訊，可以足夠判斷 reachable
* 建立新的 package `link_service_events` 來與 NATS system 整合，負責送出與訂閱 link checking events

### Serverless link checking with Nuclio
#### A quick introduction to Nuclio
* Nuclio 是個 open source platform 提供 high performance serverless functions
#### Creating a link checker serverless function
* Serverless function 有兩個部分: function code 跟 function configuration
* 把 serverless function 放在 `fun` 因為都不屬於任何現存分類
* `link_checker` function 負責被 trigger 的時候檢查 link 後 publish event 到 NATS
* 實作 Nuclio serverless function 就是要實作 certain signarture 的 handler function
#### Deploying the link checker function with nuctl
* Nuclio deploy function 實際上是 build Doecker image 後 push 到 registry
#### Deploying a function using the Nuclio dashboard
* Nuclio 有 web UI dashboard 可以 view, deploy, test serverless function
#### Invoking the link-checker function directly
* 使用 `nuctl` invoke function 只要提供 function name, namespace, cluster IP address 跟 body
#### Triggering link checking in LinkManager
* 在 production 環境中需要使用 HTTP endpoint 當成 trigger

### Other Kubernetes serverless frameworks
* AWS Lambda function 讓 serverless function 變得很流行
#### Kubernetes Jobs and CronJobs
* K8s Job 跑 pods 直到其中一個 pod 成功為止
* K8s Cron Job 類似 Job 但是會被定期啟動
#### KNative
* 設計得非常 pluggable 所以可以使用自己的 builder 或其他 event source
#### Fission
* 支援多種語，還有準備好的 container
* Fission workflows 可以 compose 跟 chain function
#### Kubeless
* K8s 原生 framework
* 無法 scale to zero
#### OpenFaas
* 跨平台
* 有完整的 GitOps-based CI/CD pipeline 管理 serverless function

### Summary
* 介紹 serverless 的概念跟什麼時候適合使用
* 實作 link checking serverless function
* 介紹不同 serverless function framework

## Chapter 10: Testing Microservices
* Software 很複雜而且 programmer 寫的 code 常常有錯誤，分散式系統有很多 而且與很多 component 互動，隨著時間過去還有變動的需求
* 適當的測試確保系統如預期運作跟快速驗證問題是否正確解決，microservices-based architecture 讓測試更困難因為有不同 microservice 組成 workflow

### Unit testing
* Unit testing 是測試中最簡單的方式
* Ginkgo test framework 是 Go 的 unit testing
#### Unit testing with Go
* Go 鼓勵每個檔案都有相對應的 test.go
* Go command 使用 test 來跑測試
* 單純使用 testing package 難以管理複雜的測試
#### Unit testing with Ginkgo and Gomega
* Ginkgo 是 behavior-drive development (BDD) testing framework
* Gomega 是 assertions library
#### Delinkcious unit testing
* 以 `LinkManager` 作為示範，因為有管理 data store、呼叫其他 microservice、trigger serverless function、回應 link check events
* 從測試出發來設計會容易達成 high level 的測試
##### Designing for testability
* 測試的時候對於所有相依的東西提供另一種實作讓我們可以完全控制環境跟結果
#### The art of mocking
* 理想上所有 object create 的時候應該都要有 dependencies injected
##### Bootstrapping your test suite
* Ginkgo build 在 Go testing package 上面所以可以使用 `go test` 跑 Ginkgo 的測試
* `ginkgo bootstrap` 會產生 `<package>_suite_test.go`，這個檔案會把 Ginkgo test 包成標準 Go testing，另外 import `ginkgo` 跟 `gomega` packages
##### Implementing the LinkManager unit tests
* Ginkgo `Describe` 描述所有測試的檔案跟定義變數
* `BeforeEach()` 會在每個測試前被呼叫
#### Should you test everything?
* 不需要測試所有的東西，因為測試的邊際效應會隨著測試增加而減少，測試也需要成本所以要做到好的取捨
* Unit test 很好但還不夠，對於 microservice-based 架構來說還需要測試各部分組合後的行為

### Integration testing
* Integration testing 是將多個部分組合後一起測試，通常使用少量的 mock 與實際環境盡量一樣
* 以 `link_manager_e2e` 為例
    1. 以 local process 的方式啟動 socail graph service 跟 link service
    2. 啟動 Postgres DB 在 Docker container 裡面
    3. 跑測試
    4. 檢查結果
#### Initializing a test database
* `initDB()` 呼叫 `RunLocalDB()` 來製造新的 DB
#### Running services
* 設置好環境變數後呼叫 `RunService()`
#### Running the actual test
* Integration 不是自動的而是設計成互動式的，因為這方便讓開發者用來 debug 不同 service
#### Implementing database test helpers
* 製造一個 local empty database 後跑在 Docker container
#### Implementing service test helpers
* 建立 `test_util` package 單純使用 Go 標準 package
##### Checking errors
* Go 需總是要做 explicit error checking 導致有點煩
* 使用 `Check()` 當有 erro 就 panic 讓 code 看起來稍微精簡一點
##### Running a local service
* Microservice 通常 depend 其他 microservice 所以測試一個 service 的時候通常需要跑相關的 service
##### Stopping a local service
* 呼叫 context 的`Done()` 就可以停止 service

### Local testing with Kubernetes
* K8s 可以將同一個 cluster 在任何地方跑起來
#### Writing a smoke test
* 最好將 smoke test 做成自動化
* `smoke.go` 使用 exposed REST API 來使用 Delinkcious
* 測試會預期 Minikube cluser 會有 Delinkcious 安裝在上面並且跑起來
* Smoke test 跑過 Delinkcious 的功能 
##### Running the test
#### Telepresence
* Telepresence 讓 service 雖然是跑在 local 但會像是在 K8s cluster 裡面
* 發現失敗的時候希望加很多 log 來找到 root cause 後修正後驗證是否正確，但 deploy 到 K8s 步驟很繁瑣
* Telepresence 讓我們能直接在 loca 改 code 後由 Telepresence 確保 local service 是完整的 service
* Telepresence 安裝 proxy 在 cluster 裡面讓 cluster 裡的 service 可以與 local service 溝通
##### Installing Telepresence
##### Running a local link service via Telepresence
* 雖然 K8s 以為 local srvice 跑在 cluster 裡面但 output 不會出現在 K8s log
##### Attaching to the local link service with GoLand for live debugging
* 將 GoLand interactive debugger 接到 local service 可以讓 debuggin 更簡單

### Isolating tests
* 測試與環境要能彼此獨立，測試之間也要能獨立，才能讓測試不受到干擾
#### Test clusters
* 測試用的 cluster 獨立出來，困難點事如何保持與實際環境一致，但可以用好的 CI/CD system 來解決
##### Cluster per developer
* 每個 deveploper 都有自己的 cluster 好處是不用怕影響其他人的環境，壞處是提供每個人一個 cluster 成本太高、與實際環境可能不同、需要另外的整合環境來測試整合的 change
* K8s 使用 Minikube 當成每個人的 cluser 避免壞處
##### Dedicated clusters for system tests
* 建立測試專用的 cluster 可以整合 changes
* 測試 cluster 可以跑更多的仔細的測試
* 測試 cluster 可以 depend external resource 跟 third-party service
* 測試 cluster 成本高昂需要好好管理
#### Test namespaces
* Test namesapce 是輕量化的獨立方式，在實際環境中切出 test namespace，還可以使用實際環境的 resrouce
* 缺點是 test namespace 多了一些相依性，預設 service 在不同 namespace 依然可以彼此溝通，但 test namespace 需要仔細確保與實際環境獨立
##### Writing multi-tenant systems
* Multi-tenant system 完全區分共用的 resource
* K8s namespaces 提供不同的機智支援 multi-tenant，可以自己定義 network policy 來避免 namespace 之間連線，定義 resrouce quota 跟 limit
#### Cross namespace/cluster
* 如果系統是會部署在多個 namesapce 或多個 cluster 則需要更仔細設計測試來模擬實際的架構

### End-to-end testing
* End-to-end test 對於複雜的分散式系統是很重要的
* 週期性的跑在特定的環境，因為耗時較久
* 測試很複雜也需要較高成本，最困難的是管理測試資料
#### Acceptance testing
* 測試系統行為符合預期，由負責人決定怎樣算是符合預期行為，最理想是由 business stakeholder 能夠撰寫 acceptance test
* 很多系統都有 web application 所以可以在網頁上跑 acceptance test 
#### Regression testing
* 用來確定新系統與舊系統的行為一致
* 如果 acceptance test 很完整就只要確認新系統有通過所有測試就好
* 當 acceptance test 不完全可以使用隨機 input 來驗證新舊系統的 output 一致來增加信心
#### Performance testing
* 測試系統的效能而不是正確性，錯誤會顯著影響效能，如果會重試錯誤
* Microservices architecture 通常使用 asynchronous processing, queue 跟其他機制來讓測試效能變得困難
* 效能並不只是 response time 還包含 CPU, memory utilization, extrenal API calls, access to network storage 等等

### Managing test data
* K8s 可以間單的部署軟體，但資料通常比較少變動
* 有不同的方法可以產生跟管理測試資料
#### Synthetic data
* 使用程式產生測試資料
* 好處: 容易控制與更新、容易產生壞資料、容易產生大量資料
* 壞處: 需要寫 code、與實際資料不同
#### manual test data
* 手動產生資料
* 好處: 完整的控制、可以根據範例資料作微調、容易開始、不需要 filter
* 壞處: 容易出錯、難以產生大量資料、難以產生多個 microservice 的相關資料、需要手動更新資料
#### Production snapshot
* 記錄實際的資料然後使用到測試系統
* 好處: 跟實際資料相關、重新收集確保測試資料與實際資料相符
* 壞處: 需要過濾 sensitive data、資料可能不支援所有的測試情境、可能難以收集所有相關的資料

### Summary
* 介紹不同的測試方式
* 測試需要成本而越多的測試並不會讓系統更好的品質，需要取捨
* 測試需要以系統一同演化，如果越多人使用在重要的時候則需要更嚴謹的測試

## Chapter 11: Deploying Microservices
* Deployment 分為 production 與 development 兩種
* Production deployment 要求系統保持穩定、有可以預測的 build 跟 deployment、可以確認 bad deployment 後 rollback
* Development deployment 要求每個 developer 的環境獨立、快速部屬、有暫時版本的 CI/CD 系統

### Kubernetes deployments
* Deployment 是 K8s resource 藉由 ReplicaSet 管理 pods
* ReplicaSet 是有相同 lable 的 pods
* ReplicaSet 使用 pod's metadat `ownerReferences` 欄位與 pods 連結
* `Deployment` object own ReploicaSet
* `Deployment` object encapsulate deployment 的概念，包含 deployment strategy 跟 rollout history

### Deploying to multiple environments
* 建立 staging namespace 給 Delinkcious 在 staging environment
* `staging` namespace 是完全複製 default namespace 用來當成 production environment

### Understanding deployment strategies
* Deployment service 新版代表要把 service 的 N 個 backing pods 換成某些跑新版的 pods
* 只有在 deployment spec's pod template 改變時才會 rollout new pods，通常發生在改變 image version
* Scaling 不適用 deployment strateg 因為都是同樣版本
#### Recreating deployment
* 停止所有跑 version X 的 pods 後建立一個新環境跑 version X+1 pods
* 缺點: service 會暫時中斷直到新 pods 上線、新版本出現問題需要等到 process reversed
* 可以允許暫停中斷服務、不希望有兩個版本共存、改變 API 成無法向後相容的時候
#### Rolling updates
* 慢慢用新的 pod 替換掉舊得 pod
* 新舊 pods 總數最多 replica count 加上 max surge
* `maxUnavailable` 允許 pods 數量在 deployment 的時候低於 replica count 的數量
* 新版本與舊版本相容時使用
#### Blue-green deployment
* 建立一個有新版本的全新環境，測試好後切換舊版本的 traffic 到新版本，當有問題可以立即切換回去，如果確定沒有問題就把舊版本移除
* 好處可以避免 update 多個 service 但其中幾個 service 沒有 update 成功後需要全部還原回去
* Load balancer 將 traffic 從 blue 導到 green
##### Adding deployment - the blue label
* 加入 `deployment: blue` label
* 因為改變 lable 所以會 trigger redeployment
##### Updateing the link-manager srvice to match blue pods only
* 確保所有 pods 都有 `deployment blue` label
##### Prefixing the description of each link with \[green\]
* 加上 prefix [green] 後 commit 讓我們有新 version
##### Bumping the version number
* 在 `build.sh` 改變 TAG version
##### Letting Circle CI build the new image
* Publish change 到 GitHub 後 CircleCI 會開始 build new code 後 create new image 後 push 到 Docker Hub registry
##### Deploying the new (green) version
* 把 Docker Hub 新版本的 image deploy 到 cluster
##### Updating the link-manager service to use green deployment
* 先確定 service 使用 blue deployment
* Patch green yaml
##### Verifying that the service now uses the green pods to serve requests
* 確認 service selector 是 green
* Get link 看看有沒有 green prefix
* 刪除 blue deployment
#### Canary deployments
* Production 太過複雜而且難以複製的時候可以使用
* 讓小部分的 production traffic 流到改變的地方測試是否正常
* 需要能新舊版本可以同時存在
##### Employing a basic canary deployment for Delinkcious
* 跟 blue-green deployment 很像，使用 deployment yellow
* 目標是 10% requet 到新版本所以 scale 目前版本到九個 replica 後加入一個新版本
* Drop service selector deployment label 才能 select 兩種 pod
##### Using canary deployments for A/B testing
* 可以用來支援 A/B testing
* 可以多版本，每個版本有自己獨特的 log 來用分析

### Rolling back deployments
* Deploy 在 production 失敗的時候最好的方法是 roll back change 回到原本的版本
#### Rolling back standard Kubernetes deployments
* K8s 會保存 deployment history
#### Rolling back blue-green deployments
* K8s 不支援，但只要把 `Service` selector 的 green 換成 blue
#### Rolling back canary deployments
* 直接刪除新版本的 deployment
#### Dealing with a rollback after a schema, API, or payload change
* 需要根據 change 的內容決定該如何 deploy，這樣才容易處理 rollback 導致無法相容的問題

### Managing versions and dependencies
#### Managing public APIs
* Public API 是 cluster 外使用者使用的 network API
* 通常都用 V1, V2 來當成 versioning scheme
#### Managing cross-service dependencies
* 通常使用 internal API 來定義跟文件化
#### Managing third-party dependencies
* Libraries 跟 packages 用來 build software
* 使用 API 來 access third-party service
* 使用的 service 跟跑 system
* 使用 third-party dependency 需要考量當 unavailable 的情況
* 升級 third-party library 或 API version 不應該改變 service API 或 public interface
* 升級 third-party libraries 需要確認所有用到的 service 一併更新
#### Managing your infrastructure and toolchain
* CI/CD pipeline 有很多自動的 script 所以需要做好版本控制

### Local developments deployments
* Developer 希望能快速的 iteration 來找到問題，但在 K8s 中需要打包 iamge 在部屬到 cluster 會讓 iteration 變慢
#### Ko
* Go-specific tool 目標是隱藏 build image 的過程
#### Ksync
* 直接同步檔案到 cluster 裡面的 container 的 remote directory
#### Draft
* 很快的 build image 但不用 Dockerfile
* Depend on Helm
#### Skaffold
* 有完整的解決方案
* 支援 local development 與 CI/CD integration
#### Tilt
* 以 Tiltfile 為中心是用 Starlark 語言撰寫，是 python sub set
* 提供 live development environment，highlight evetns 跟 errors

### Summary
* 介紹不同的 deployment 跟 roll back 的方式
* 介紹讓 local development 快速 iteration 的 tool

## Chapter 12: Monitoring, Logging, and Metrics
* K8s 提供 self-healing, auto scaling, resource managment
* Developer 需要平衡 high availability, robustness, performance, security, cost
* Monitoring 可以提供對於系統的了解進而採取相對應的平衡
    * Logging: 特別將 application code 裡的資訊記錄下來
    * Metrics: 收集系統資訊例如 CPU, memory, disk usage, disk I/O, network
    * Tracing: attach ID 追蹤 reqeust 在不同的 microservices

### Self-healing with Kuvernetes
* Self-healing 對於大型分散式系統很重要，確保整個系統不會 fail 即便裡面的 components fail
* Redundancy 代表沒有 single point of failures (SPOFs)，可以跑多個 components 的 replica 或者 deploy 系統到不同的 cloud platforms 但成本很貴
* Observability 是發現到錯誤的能力，是 recovery 的第一步
* Auto-recovery 並不一定需要因為可以使用人力來監控修復，但是人類反應緩慢而且容易出錯，雖然一開始可以用人力但之後為了減少成本會需要自動修復
#### Container failures
* K8s 在 pods 裡面跑 container 當 container 失敗則會立即重新啟動
* `restartPolicy` 檔案控制 K8s 對於 container 失敗的時候的行為分為 `Always`, `OnFailure`, `Never`
* Pods 裡的所有 container 都適用相同的 restart policy
* Container 持續 fail 後重啟一段時間會進入 `CrashOff` 狀態，K8s 會開始使用 exponential backoff delay 的方式重啟
#### Node failure
* Node 失敗則上面所有的 pods 會無法使用，K8s 會 schedule 這些 pods 到其他 node 
#### Systemic failures
* Total networking failure
* Data center outage
* Availability zone outage
* Region outage
* Cloud provider outage
* 遇到以上情形會讓系統失效，但重要的是不要讓資料毀損能夠回復到先前狀態
* 可以將 K8s cluster 放在不同的 data center 或 availability zone 或不同 region 或不同 cloud provider

### Autoscalling a Kubernetes cluster
* Autoscaling 是為了讓系統適應目前的需求
    * 系統配置不足會讓 request 等待太久最後 timeout
    * 系統配置過剩會浪費資源
* 與 self-healing 一樣需要先偵測到系統需要 scale 才去做 scale
#### Horizontal pod autoscaling
* Horizontal pod autoscaler 是個 controller 控制 pod 的數量
* 根據 metric 來控制 pod 的數量
* autoscaler 在 standard K8s deployment 之上所以 pod 不會察覺自己被 scale
##### Using the horizontal pod autoscaler
* Autoscaler 需要 heapster 跟 metrics
#### Cluster autoscaling
* 當 scale up 超過 cluster capacity 則會 fail
* `auto-scaler` cluster project 可以讓 cluster auto scale
    * 當 K8s 因為 insufficient resource 無法 schedule pods 就會 trigger 改變 cluster size
    * cluster autoscaler 不管為什麼無法 schedule pods 就會調整 cluster size
#### Vertical pod autoscaling
* 微調 pod 要求的 CPU 或 memory
* Pod 要求太多資源會排擠到其他 pod 導致無法 schedule
* 需要監控 CPU 與 memory 所以要安裝 metrics server

### Provisioning resources with Kubernetes
* 配置資源的責任應該是 operator 但 developer 常常負責資源配置
#### What resources should you provision?
* 資源有兩種，一種是 K8s resources 另一種是 infrastructure resource
* Automatic provisioning 定義好合理的 quotas 跟 resource 限制很重要
#### Defining container limits
* K8s 可以定義 container 的 CPU 與 memeory
    * 避免同個的 pod 上的 container 互相競爭
    * 讓 K8s scedule pods 更有效率因為知道 pod 最多會用多少資源
* 設定 container limmit 無法解決 runaway allocation resources
#### Specifying resrouce quotas
* K8s 可以定義 namesapce 的 quota
* 有很多 option 定義 resource quota 像是可以根據情況來定義不同的 quota
#### Manual provisioning
* 手動調整配置雖然聽起來不太好但是許多情況可以使用
    * 管理內部的 cluster 的時候
    * 開發 automated provisioning 實驗何種比較好的時候
#### Utilizing autoscaling
* 雲端上建議使用 autoscaling
#### Rolling your own automated provisioning
* 如果有更複雜的需求可以使用自己的自動配置

### Getting performance right
* 需要在正確的時間才去調整效能，先確定可以動、行為正確才去調整效能
* 調整效能需要 profiling 跟 benchmarking，不然無法確定是否有往正確的方向邁進
* 當影響使用者體驗就需要調整效能但往往要付出更多成本，但隨著系統演化跟使用者人數增減很難找到平衡點
#### Performance and user experience
* User experience 由觀察到的現象來感覺效能
* 雖然透過升級硬體、平行處理、改善演算法可以改善但其實可以使用不同的機制來改善像是增加 cache 在不改變原本架構的情形下達到差不多的效能又可以節省成本
#### Performance and high availability
* Timeout 會影響效能所以在 highly available 系統下可以減少 timeout 次數來達成較好的效能
* 另外一方面 highly available 系統需要處理同步問題，要確認使用最新的回復
#### Performance and cost
* 效能與成本息息相關
* 需要先找到效能的瓶頸再去改善不然可能白費力氣
#### Performance and security
* 效能與安全性常常站在對立面，但有時候安全性會藉由最小化 system surface 達到較好的效能或避免不需要的 feature

### Logging
* Logging 在操作系統的時候紀錄 log 下來
* Log 通常有結構跟時間戳記，對於分析問題很有幫助
#### What should you log?
* 需要徹底了解系統需要記錄下那些資訊來
* 可以從記錄 incoming request, microservice 之間的 ineratcion 下手
#### Logging versus error reporting
* 程式可以處理的錯誤可以不用 log 像是 retry 
* 不需要立即處理的錯誤可以先記錄下來之後處理或者或者使用 error report service 記錄特定的 error
* Log error 要包含 stack trace 
#### The quest for the perfect Go logging interface
* Go 的 logger 是個 struct，不支援 log levels 跟 structed logging
#### Logging with Go-kit
* Go-kit 有簡單的 interface 提供單一 `Log()` 可以傳進一連串的 key and value
* Go-kit 不管 log message 全部由使用者用 key value 來決定
##### Setting up a logger with Go-kit
* Logger 需要 JSON formatter 跟 sync writer
    * JSON formatter 把 key 跟 value format 成 JSON
    * sync write 允許讓多個 Go routine 同時使用
* 加入預設的欄位像是 service name, current timestamp
* 增加 `Fatal()` method 導到 `log.Fatal()`
##### Using a logging middleware
* 需要考慮 logger 會在哪邊 instantiate 跟使用來 log message
* 確保所有需要 log message 都能使用 logger
* Go-kit 提供 middleware 觀念，讓我們能夠 chain 多個 component
* Middleware 可以預先處理 input 後才傳到下個 component 也可以對 output 處理完在傳給下個 component，也可以當成 short curcuit 直接回傳
#### Centralized logging with Kubernetes
* K8s container 寫到 standard output 跟 error stream 的東西可以使用 `kubectl logs` 看的到但是如果 pod 被 reschedule 則裡面的 container 的 log 就會消失
* Centrilized logging 是利用 log agent 及時收集所有的 logs 到一個地方
* 最簡單跟穩定的方式適用 demon set，在每個 node 上都有 agent，不需要改任何東西，只要 code 寫到 standard output 就會被收集起來

### Collecting metrics on Kubernetes
* Metrics 是 self-healing, autoscaling 或 alerting 的關鍵
* K8s 提供 metrics API
* 之前版本的 K8s 透過 cAdvisor 跟 Heapster 支援 metrics，目前版本使用內建的 metrics API 跟 metrics server
#### Introducing the Kubernetes metrics API
* K8s metrics API 支援 node 跟 pod metrics
* Metric 有 usage, timestamp, window field
#### Understanding the Kubernetes metrics server
* K8s metric server 實作 metrics API 跟提供 nodes 跟 pods metrics
* metrics-server 是標準的 K8s metrics solution 但如果想要更客製化的 metrics 可以使用 Prometheus
#### Using Prometheus
* 實際上大家都使用的 metrics collection solution
##### Deploying Prometheus into the cluster
* Prometheus 有很多功能所以不容易 deploy 跟 manage
* Prometheus operator 可以幫助 configure Promethes
##### Recording custom metrics from Delinkcious
* 需要提供 metric 的 service 需要 expose /metrics endpoint，使用 Go-kit 來加入 metrics middleware

### Alerting
* 無法打造不會 fail 的系統所以設計系統的時候需要認知到 fail 可能會發生
* 當 failure 發生的時候需要能夠警告對的人來分析處理
* 設計系統的時候能夠好好處理 failure 的方式可以讓系統繼續執行而不是直接壞掉，如果 failure 嚴重到會讓效能降低就應該去找出原因來解決 
#### Embracing component failure
* 需要認知到所有 components 隨時都有可能 fail
#### Grudgingly accepting system failure
* System failure 有不同的程度，outage 時間的長短之類的
* Redundancy, backup, compartentalization 是通常避免 system failure 的手段，但成本高昂
#### Taking human factors into account
* 主要都是有人來處理意外的發生
* 有些 critical system 會有人全天候監視以便及時處理問題，但這類系統還是需要有 alert 來讓人可以快速了解目前的狀態
##### Warnings versus alerts
* 使用 warning 監控狀態避免等到危機降臨才去處理
##### Considering severity levels
* Alert 分不同的 level 來對應到不同的處理方式
##### Determining alert channels
* Alert channels 與 severity level 息息相關
* 通常一個 incident 會被 broadcast 到多個 channels
##### Fine-tuning noisy alerts
* 太多的 low-priority alert 會干擾到重要的 alert
    * 讓所有人分心
    * 讓人會忽略 alerts
* Fine-tuning alert 避免太多 noisy alerts 讓人忽略到重要的 alert
#### Utilizing the Prometheus alert manager
* Alert 通常根據 metric 來判斷是否要產生 alert
* Alert manager
    * Grouping: 把相關的 signal 聚集成同一個通知
    * Integration: notification targets
    * Inhibition: 忽略已經收到的 alert
    * Silences: 暫時 mute 某些 alert
##### Configuring alerts in Prometheus
* 可以根據某些條件來決定是否要發 alert

### Distributed tracing
* Alert 可能過於模糊不容易 troubleshooting
* 有幾種可以減少懷疑範圍的方式
    * 檢查最近關於 deployments 跟 configuration 的變動
    * 檢查 third-party dependencies 是否有 outage
    * 想想是否有尚未修正 root cause 類似的 issue 
#### Installing Jaeger
* 使用 Jaeger-operator 是最好安裝 Jaeger 的方式
#### Integrating tracing into your services
* 紀錄 request 橫跨多個 microservices 的相關 log

### Summary
* 介紹了關於 self-healing, autoscaling, logging, metrics, distributed tracing
* 管理 monitor service 跟 service 支援 logging 跟 tracing 增加了另外一層複雜度
* Monitoring 所有系統的資料後如何得到需要的東西又是另一種挑戰
## Chapter 13: Service Mesh - Working with Istio
* Service mesh 把 service 中複雜的工作放到了獨立的 proxy 對於不同語言開發的環境中非常有幫助
### What is a service mesh?
* 從 service mesh 如何解決 microservice 相較於一般 service 有的問題來了解
#### Comparing monoliths to microservices
* 如果使用一般 service 打造 Delinkcious 比較簡單但隨著功能越多會讓缺點越複雜，但使用 microservice 需要安裝許多額外的工具跟寫很多額外的 code
* Security 需要再不同 service 都做好互相的加密解密功能
* Distributed tracing 在一般 service 用 stack trace 可以追蹤呼叫的過程，但是 microservice 需要另外安裝 service 來達成
* Cetralized logging 在一般 service 很 trivail
* Microservice 雖然有很多好處但也不容易管理
#### Using a shared library to manage the cross-cutting concerns of microservices
* 使用 shared library 處理 cross-cutting concerns，所有 service 使用 library 來處理 cross-cutting aspects
* 缺點是 microservice 好處就是在於可以適用不同的語言但如果使用 shared library 就只能使用一種語言或多種語言的 library 或者接受不同語言的 library 有不同行為
* 沒有現成的 library 可以提供需要的功能，所以需要 update shared library 而其他使用到 library 的 service 也需要 update。就算使用 rolling update 也會有服務中斷的問題，因為管理 secret authentication 的 library 可能無法向後相容
#### Using a service mesh to manage the cross-cutting concerns of microservices
* Service mesh 是一群 intelligent proxies 跟 control infrastructure component 所組合起來的
* Proxies 被安裝在 cluster 裡每個 node 上面，proxies 會攔截所有 service 之前的溝通然後根據期望的行為來做事
#### Understanding the relationship between Kubernetes and a service mesh
* K8s 與 service mesh 很像都是有 proxy 在 node 上面而另外有 control plane
* Service mesh 像是 K8s 的一個 component，service mesh 可以提供更 fine-grained 的方式接管 service-to-service communication
* K8s 與 service mesh 互相獨立，不一定要搭配使用

### What does Istio bring to the table?
#### Getting to know the Istio architecture
* Istio 分為 control plane 跟 data plane
    * Control plane 包含負責設定 proxies 跟收集監控的資料
    * Data plane 包含在每個 pod 都有的 proxies
##### Envoy
* Envoy 是個 high-performance proxy
* Envoy proxy 在 Istio 的 data plane 使用但也可以獨立使用
* Istio inject Envoy side container 到每個 pod 上
* Envoy proxy 控制所有出入 pod 的 communication
##### Pilot
* 負責 platform-agnostic service discovery, dynamic load balancing, routing
* 把 high-level routing rules 轉換成 Envoy configuration
* 使用 Envoy data plane API 把 Envoy data plane configuration 設定到每個 Envoy proxy
* Pilot 設定都以 custom resrouces definitions 存在 etcd，所以是 stateless
##### Mixer
* 負責 metric collection 跟 policies
* 原本是由 service 讀取特定 backend 的方式，但透過 Mixer 可以讓 service developers 減少負擔跟把控制權還給 operator，更可以替換 backend 而不用改 code
* 處理 request 之前 Envoy proxy 先送到 Mixer 做檢查
* 處理 request 之後 Envoy proxy 對 Mixer 報告 metric
##### Citadel
* 負責管理 certificate 跟 key
* Citadel 產生 certificates 跟 key pair 給現存的 service accounts
* Citadel 監控 K8s API server 如果有新的 service accounts 就給 certificates 跟 key pair
* Citadel 把 certificates 跟 key pair 當成 K8s secrets
* K8s mount secret 到每個新 pod 到相關的 service account
* Citadel 當 certificates 過期時自動 rotate K8s secrets
* Pilot 產生 service account 相關的 secure naming information 然後傳給 Envoy proxy
##### Galley
* 負責不同平台使用者的 configuration
#### Managing traffic with Istio
* Istio 以 network level 的方式操作 service 的 traffic
##### Routing requests
* Istio 使用 CRD 產生自己的 virtual service
* Virtual service 有 version 觀念，相同 image 可以 deploy 成不同 version 的 service
* Pilot 送 ingress 跟 egress rules 到 proxies 來決定 request 是否要被處理
##### Load balancing
* Istio 透過 adapter 去 service discovery 底下的 platform service
* Istio 透過 platform 所管理的 service 來註冊跟移除掉壞掉的 instances 來更新 load balancing pools
* Istio 支援 Round robin, Random, Weighted least request
* Istio 週期性的檢查確保 pool 的 instance 是可用的，會移除掉不合規定的 instances
##### Handling failures
* Istio 提供 Timeouts, Retries, Rate limiting, Health checks, Circuit breaker 等錯誤處理的機制
##### Injecting faults for testing
* 處理錯誤的機制並無法真正修正錯誤，自動重試可能可以修正某些錯誤，但有些錯誤需要由 application 或 human operator 來修正
* 可以藉由主動插入錯誤到系統來測試系統在錯誤發生時候的行為
##### Doing canary deployments
* 先前做 canary deployments 需要根據比例來部署適當的 pods 但是 Istio 因為在 network level 上操作所以比例可以與 pod 數量脫鉤
#### Securing your cluster with Istio
##### Understanding Istio identity
* Istio 管理自己的 identity model
* Istio 使用 K8s service account 當作 identity
* Istio 使用 PKI 來為每個 pod 做 identity 加密
* Istio 為每個 service account 建立 X.509 certificate 跟 key pair 當成 secret 放到 pod 上
* 當 client 呼叫 service 會檢查 service identity
##### Authenticating users with Istio
* Istio authentication 是基於 policy，分別是 namespace policies 跟 mesh policies
    * Name policy 只適用於一個 namespace
    * Mesh policy 適用於整個 cluster
* 可以設定 peer authentication 跟 origin authentication
##### Authorizing requests with Istio
* Service 通常會 expose 多個 endpoints，可能只給某些 serivce 特定 endpoints
* Istio 藉由擴充 role-based access control (RBAC) 來支援此功能
#### Enforcing policies with Istio
* Mixer 有一群 adapter 負責在 request 處理之前跟之後做處理
* 可以建立 Mixer adapter 來使用自己的 policies，目前內建有 Check, Quota, Report
#### Collecting metrics with Istio
* Istio 在每個 request 之後收集 metrics
* Metrics 會被送到 Mixer
* Envoy 負責產出 metrics
#### When should you avoid Istio?
* 使用 Istio 需要注意是否或的效益比起成本高
* 使用 Istio 的缺點
    * 學習曲線高，有很多額外的概念跟管理系統建立在 K8s 上面
    * Troubleshooting configuration 很困難
    * 難以整合其他 project
    * Proxies 增加 latency 跟消耗 CPU 跟 memory 資源

### Delinkcious on Istio
* 把 application domain code 與 operational code 分開比較容易 troubleshoot 也不需要常常因為改了 operational code 而要重新 build
#### Removing mutual authentication between services
* 把 `link-manager` service 跟 `social-graph-manger` service 的互相驗證拿掉
* 互相驗證流程先去 encode secret 跟 mount 到 container 上面，然後 link manager 拿到 secret 後 inject 到 header，最後 socail graph manager 檢查 header 裡的 secret
* 互相驗證的流程與 service 幾乎沒有關係，而且如果很多不同的 service 則這種方法會很難做
* Istio 可以使用 role 跟 role binding 把這件事獨立出來
    * 先設定一個 role 可以 GET /following endpoint
    * 為了讓 link service 可以呼叫所以把 role bind 到 link service account 當成 subject user 
#### Utilizing better canary deployments
* 使用 pod 數量跟版本來控制 canary deployments 的比例雖然可以運作但也把 canary deployment 與 scaling deployments 綁在一起，成本也很高
* Istio 可以直接使用 virtual service 將 traffic 分成期待的比例所以不需要額外的 pods
#### Automatic logging and error reporting
* 當把 Delinkcious 跑在 GKE 並且使用 Istio add-on 則自動會有與 Stackdriver 的整合
* Stackdriver 是個 one-stop shop 來監控集中 logging, error reporting 跟 distributed tracing
#### Accommodating NATS
* 使用 Istio 會無法使用 NATS 因為 proxy 把 communication 給攔截掉了
* 解法是避免 Istio 攔截 NATS 的 communication
#### Examining the Istio footprint
* Istio control plane 是獨立在 `istio-system` namespace，但 CRD 是 cluster-wide
* 除了 CRD Istio 也有安裝自己的 component 在自己的 namespace 裡
* Istio 在每個 pod 都安裝了 sidecar proxies，所以 pod 在 default namespace 都有兩個 container

### Alternatives to Istio
#### Linkerd 2.0
* Lightweight design 跟 tighter implementation 所以比 Istio 消耗較少的資源
#### Envoy
* Istio 的 data plane
* 覺得 Istio control plane 太複雜想使用自己的 control plane
#### HashiCorp Consul
* 雖然 Consul 沒有 service mesh 所有的功能但是有提供 service discovery, service identity 跟 mTLS authorization
#### AWS App Mesh
* 如果把 infrastructure 跑在 AWS 上面，可以使用 AWS App Mesh，跟 AWS 整合較好
#### Others
* Aspen Mesh, Kong Mesh, AVI Networks Universal Service Mesh
#### The no mesh option
* Go kit, Hystrix, Finagle
* 如果 srvice 都使用同一種語言則可以使用 library 的方式

### Summary
* 本章介紹了 service mesh 跟 Istio
* Istio 在 K8s 上藉由安裝在 pod 上 proxies 建立 cluster shadow
