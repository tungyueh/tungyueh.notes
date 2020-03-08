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

## Chapter 5: Configuring Microservices using Kubernetes ConfigMaps

## Chapter 6: Securing Microservices on Kubernetes

## Chapter 7: Talking to the World - APIs and Load Balancers

## Chapter 8: Working with Stateful Services

## Chapter 9: Running Serverless Tasks on Kubernetes

## Chapter 10: Testing Microservices

## Chapter 11: Deploying Microservices

## Chapter 12: Monitoring, Logging, and Metrics

## Chapter 13: Service Mesh - Working with Istio

## Chapter 14: The Future of Microservices and Kubernetes
