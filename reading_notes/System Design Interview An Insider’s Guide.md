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
