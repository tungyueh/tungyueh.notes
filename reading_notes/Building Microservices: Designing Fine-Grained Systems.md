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
