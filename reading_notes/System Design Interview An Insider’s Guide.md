# System Design Interview An Insider’s Guide
## CHAPTER 1: SCALE FROM ZERO TO MILLIONS OF USERS
### Single server setup
* User 拿網址詢問 DNS 得到 IP 後送 request 給 web server 等待回應 HTML pages 或 JSON response
* Web server 會收到兩種 request 一種是 Web application 另一種是 mobile application
### Database
* User 增加後需要多台 server，一個用來處理 web/mobile request 另一個用來處理 database 資料
* 分開 server 才能夠分別 scale
### Which databases to use?
* 正常使用 relational database 但有特殊需求才考慮 non-relational database
### Vertical scaling vs horizontal scaling
* Vertical scaling 是指將 server 增加更多運算資源
* Horizontal scaling 是指使用更多台 server
* 流量小的時候用 vertical scaling 因為比較簡單但無法無限制的增加資源，而且 server 壞掉整個服務就中斷
* Hirizontal scaling 是為了突破 vertical scaling 的限制才採用
### Load balancer
* Load balancer 將 request 平均分配到 server
* User 使用 public IP 連到 load balancer 後，load balancer 轉換成 private IP 送給對應的 server 處理
* 當有 server 離線後自動將所有 request 送到其他 server
* 當 request 突然暴增，加入 server 讓 load balancer 自動將 request 平均分配
### Database replication
* 主要的 database 通常只負責 write operation 其他的 database 負責從主要的 database 複製後提供 read operation
* 通常 read operation 的需求會大於 write operation 所以只要增加 database 就可以有效提升 read operation 的速度
* 負責 read operation 的 database 壞掉其中一個可以讓其他 read operation 的 database 支援
* 負責 write operation 的 database 壞掉可以找其他的 database 負責 write operation，需要有機制重建回改變中遺失的資料
### Cache
* Cache 是暫時存放需要快速讀取資料的地方tier
### Cache tier
* 比起 database 更快速，可以增加 performance 跟減少 database workload
### Considerations for using cache
* 資料被頻繁存取且很少修改適合放在 cache，需要永久保存的資料不適合放到 cache
* Expirartion policy 對於 cache 很重要，設太短需要常常去 database 拿資料，設太長造成資料過時
* 確保 database 的資料與 cache 裡的一致
* 避免 cache 成為 single point of failure (SPOF)
* 當 cache 滿的時候 Least Recently Used 是最常用的方法，其他有 Least Frequently Used 或 First In First Out
### Content delivery network (CDN)
* CDN 將靜態的資料散佈到全球
* CDN 發現沒有所需要的資料會去跟 Server 拿到後根據 HTTP header 中的 Time-to-Live (TTL) 決定要暫存多久
### Considerations of using a CDN
* CDN 由第三方營運所以需要成本，暫存不常用的資料不會有特別的好處
* 設置適當的過期機制
* 思考 CDN 失敗的情況
* 要能夠讓暫存的檔案失效
### Stateless web tier
* 為了讓 web 可以 horizontal scale 需要將 state 移出，可以將 session data 存在 persistent storage 讓 web server 從 databases 拿資料
### Stateful architecture
* Stateful server 會將 client data 存下來以便記住相同 client 的 request
* 這樣只能讓相同的 server 服務同樣的 user，不容易增加減少 server 也難處理 server 壞掉的情況
### Stateless architecture
* Web server 都從 shared data store 拿資料
* Stateless system 比較簡單、穩固跟容易 scale
### Data centers
* 使用多個 data center 避免整個 data center 失效
* 需要將 traffic 導向正確的地方
* 需要同步好資料
* 測試部署在不同地方有相同的 service
### Message queue
* 支援 asynchronous communication
* 使用 buffer 來 distribute asynchronuous requests
* 將 publisher 跟 subscribe 分開讓 architecture 可以 scalable
### Logging, metrics, automation
* 紀錄 error log 可以幫助找出問題，使用 tool aggregate 起來在同個地方分析
* 收集 metrics 可以知道重要的地方跟了解系統的健康狀態
* 自動化增加開發部署的效率
### Database scaling
* Vertical scaling: 增加 RAM, CPU，增加 SPOF 風險
* Horizontal scaling: sharding 把 database 分成多個 shard 每個都是相同的 schema

## CHAPTER 4: DESIGN A RATE LIMITER
* 限制 client 或 service 需求的速度
* 避免 Denial of Service 造成 resource starvation
* 減少花費，限制過度 request 代表有機會處理優先度較高的 API
* 避免 server overloaded
### Step 1 - Understand the problem and establish design scope
* 有分為 client side 跟 server 的 rate limiter
* 根據 IP, user ID 或其他來限制
* 是否會運行在分散式系統
* 是否需要通知 user 正在被 throttled
* 需求: 準確的限制過度的 requests, low latency, memory 使用量越低越好，能在多個 server 或 process 使用，user 可以清楚知道被 throttled，rate limiter 壞掉時系統能夠持續運作
### Step 2 - Propose high-level design and get buy-in
#### Where to put the rate limiter?
* 在 client-side 做 rate limit 容易被假造而解除限制
* 在 server-side 做 rate limit
* 在 client 與 server 之前做 rate limit
* Cloud microservice 有 API gateway 有 rate limiting 功能
#### Algorithms for rate limiting
* Token bucket: 一定的速度把 token 放入 bucket，當 request 通過就拿一個 token 沒有 token 就丟棄
* Leaking bucket: request 進入 queue 當 queue 已經滿了就丟棄，然後以一定的速度拿 request 出來
* Fixed window counter: 每個 window 有固定時間大小，request 會增加 counter 超過 counter 就丟棄
* Sliding window log: 紀錄 request 的時間當 log size 超過就丟棄，等到時間經過把之前的 outdated 才能繼續收 request
* Sliding window counter
#### High-level architecture
* 使用 in-memory cache 比較推薦因為可以比較快又可以支援 time-based expiration strategy
### Step 3 - Design deep dive
#### Exceeding the rate limit
* Request 被限制時可以回傳 429 根據不同情況先存下來之後再處理
* 使用 rate limiter headers 讓 client 知道被 throttled
#### Detailed design
* Rules 存在 disk
* client 送 request 會先給 rate limiter middleware
* Rate limiter 從 cache 拿 rule 判斷是否要轉送給 API server
#### Rate limiter in a distributed environment
* 需要處理 race condition 跟 synchronization issue
* 同時讀取 counter 後存進 cache 實際上增加大於一
* lock 可以解決 race condition 但檢查效能，通城用 Lua script 跟 sorted sets data structure in Redis
* 多台 rate limiter 沒有紀錄上次 client 連到其他 rate limiter 的紀錄，可以用 readis 讓所有 rate limiter 都把狀態存進去
### Monitoring
* 需要數據知道 rate limit 是否運作的有效率
### Step 4 - Wrap up
* 有 token bucket, leaking bucket, fixed window, sliding window log, sliding window counter 可以選擇
* 可以允不允許短暫超過 threshold
* 針對 application level 或 IP address 來限速
* 使用 client cache 避免過多的 API 呼叫，避免很短時間內送出大量 request
## CHAPTER 5: DESIGN CONSISTENT HASHING
* 根據 server 數量把 hash 除上數量對應到該放哪個 server，但如果 server 有減少不只是減少的會重新分配而是全部都重新分配造成大量 cache miss
* Consistent hashing 是指當 hash table 改變大小後只會有 k/n 的 key 需要重新分配，k 是指 key 的數量、n 是指 slot 數量
## CHAPTER 6: DESIGN A KEY-VALUE STORE
* 沒有最好得設計，每種設計都是為解決特定問題而對於 read, write, memory usage 做出取捨，還有 consistency 跟 availability 的取捨
### Single server key-value store
* 使用 hash table 將所有東西存在 memory，雖然 memory 存取很快但會受到記憶體空間的限制
* 使用壓縮減少資料大小
* 將頻繁存取的資料放在記憶體其他放在硬碟
### Distributed key-value store
#### Data partition
* 既然資料無法完整放在一個 server 就需要分散到多個 server 上面，但會面臨到需要將資料平均分散到多個 server 還有當 server 加入活移除需要減少資料移動的數量
* 使用 Consistent hashing 可以解決問題，還可以自動增減 server，使用 virtual nodes 平均分散資料
#### Data replication
* 為了提升可靠性需要將資料複製到其他 server，通常資料中心的 server 會一起壞掉所以需要把資料放在不同的資料中心
#### Consistency
* 資料複製的數量跟寫入需要等待的 ACK 個數還有讀取需要等待的 ACK 個數主要是對於 latency 跟 consistency 的 tradeoff
##### Consistency models
* Strong consistency: read 不會拿到過期的資料
* Weak consistency: read 可能不會拿到最新的資料
* Eventual consistency: 屬於 weak consistency 但一定時間後所有更新都會 propagate 讓所有 replicas 的資料都是一致的
#### Inconsistency resolution: versioning
* Versioning 將修改的資料當成一個不可改動的新版本
* Vector clock 紀錄資料修改的 server 跟版本判斷先後順序
### Handling failures
#### Failure detection
* 在分散式系統中只靠一個 server 說另外一個 server 已經失效是不可靠的，但如果要求大家都說會沒有效率
* Gossip protocol: 使用 heart beat 跟 member list 紀錄各個 node 的狀態，如果太久沒有 heart beat 就判斷為失效
#### Handling temporary failures
* Sloppy quorum 選擇一些 server 負責 write 跟 read 並忽略 offline server
#### Handling permanent failures
## CHAPTER 7: DESIGN A UNIQUE ID GENERATOR IN DISTRIBUTED SYSTEMS
* Multi-master replication: ID 每次增加跟總共的 database server 數量一致，不同 server 的時間會不照順序，server 增加減少無法 scale
* UUID: server 之間不需要溝通就能產生 ID，但不合規定沒有只包含數字也跟時間無關
* Ticket Server: 將 auto_increment feature 放在一台 database server，會遇到 SPOF
* Twitter snowflake approach: 將 ID 分成不同部分，分為 timestamp, datacenter ID, machind ID, sequence number

## CHAPTER 8: DESIGN A URL SHORTENER
### API Endpoints
* POST api/v1/data/shorten: request 給 long URL return short URL
* GET api/v1/shortUrl return long URL for HTTP redrection
### URL redirecting
* 301 redirect 代表網址永遠移走了，下次相同 URL 會直接到 long URL 而不會先去問 service 減少負擔
* 302 redirect 代表網址暫時移走，每次都會問 service 可以做數據統計
### URL shortening
* 使用 hash table 紀錄 short URL 跟 long URL 的對應
* Hash function 可以把 long URL hash 出一個 value，每個 hash value 可以推回原本的 long URL
* Hash + collision resolution: 使用常見的 hash function 只取前面幾位數，當重複的時候加上不同字串重新 hash
* Base 62 conversion: 用 unique ID generator 產生數字後用 base 64 轉換
### Wrap up
* Rate limiter: 根據 IP address 做 filter
* Web server scaling: 因為 web tier 是 statelss 所以可以動態增加減少 web server
* Database scaling: 使用 replication 跟 sharding
* Analytic: 整合統計分析的功能進去
* Availability, consistency, and reliability

## CHAPTER 10: DESIGN A NOTIFICATION SYSTEM
### Different types of notifications
* iOS 用 Apple Push Notification Service
* Android 用 Firebase Cloud Messaging
* SMS message
* Email 用 email servers
### Contact info gathering flow
* User 有一組 Email address 跟 phone number 但有多個 device
### Notification sending/receiving flow
* Service 1 to N: 使用 event 觸發通知
* Notification system: 提供 API 給 service 然後請 third-party service 送 notification 給 user
* Third-party services: 需要有良好的擴充性
* 只有一台 notification server 會有 SPOF, 難以增加 databases, caches 跟變成效能瓶頸的問題
### High-level design (improved)
* 把 database 跟 cache 移出 notification server 後增加 notification servers 數量在使用 message queus 去解開送 notification 的 couple
* Notification servers 驗證資料、查詢 database 跟把 notification data 送到 message queues
* Message queue 可以處理不同 notification type 讓彼此不互相干擾
* Workers 負責從 message queue 拿出資料送給 third-party services
### Design deep dive
#### Reliability
* Notification 只能允許延遲或亂序但不能沒送，所以將 notification data 存在 persistent 的地方然後 retry 確保都有送 notification
* Distributed 系統天生無法避免重複送 notification 所以需要好好處理，例如看 event ID 判斷是否送過
#### Additional components and considerations
* Notification template: 提供基本格式讓人只填可能變動的部分可以維持一致性、減少格式問題跟節省時間
* Notification template: 收到通知後要根據 setting 判斷 user 想不想收到這種類型的通知
* Rate limiting: user 可能會因為太過頻繁的通知而關閉所以需要限制一定的數量
* Retry mechanism: 在 message queue 增加 retry 機制
* Security in push notifications 讓受過驗證的人才能發送 notification
* Monitor queued notifications 藉由監測 queued notification 的數量來判斷 worker 是否足夠來處理通知
* Events tracking: 收集通知是否有被打開跟點擊的數據讓通知的人有數據可以改進
