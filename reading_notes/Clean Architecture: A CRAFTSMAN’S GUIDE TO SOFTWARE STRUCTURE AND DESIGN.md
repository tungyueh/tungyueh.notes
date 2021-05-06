# Clean Architecture: A CRAFTSMAN’S GUIDE TO SOFTWARE STRUCTURE AND DESIGN
## PART I Introduction
* Architecture 用在 high level，Design 用在 low level 對於 structure 的選擇
* High-level structure 跟 low-level detail 是相輔相成的，需要兩者來形成整個系統的樣子
### Chapter 1 What Is Design and Architecture?
#### The Goal?
* 使用最少的人力來建造維護系統
* 設計好壞在於需要多少 effort 來達成需求
#### Case Study
* 工程師的數量增加並沒有與開發效率成等比
* 隨著 release 次數增加每行 code 的成本驚人的增加，最終會侵蝕掉本來的盈利
##### THE SIGNATURE OF A MESS
* 雖然都很努力但生產力逐漸降低趨近於零
* 常常常常在處理 mess 沒有餘力開發新功能
##### THE EXECUTIVE VIEW
* 每次 release 付出的人力成本不斷增加但功能卻沒有增加
* 任何 CFO 看到這種現象都知道該立刻阻止情況惡化下去
##### WHAT WENT WRONG?
* Developer 只想先推出產品之後在清理技術債，但市場壓力總是在，所以就不會去清理技術債
* Developer 過度相信自己的生產力可以持續維持，但 mess code 會讓生產力幾個月之後就趨近於零
* Mess code 不論在短期或長期都會降低生產力
* 維持生產力的方式在於認真處理 mess code 
#### Conclusion
* 避免過度自信，認真對待軟體品質
* 知道什麼是好的軟體架構，建造能夠花最少力氣達到越多生產力的架構
### Chapter 2 A Tale of Two Values
* 軟體系統提供行為跟架構，但通常只會專注一個或都不管
#### Behavior
* Programmer 根據需求寫 code 讓系統能夠做出需求的行為
* 當行為錯誤就需要找出問題來修正
* 只覺得工作就是寫 code 跟修 bug 是錯誤的
#### Architecture
* 軟體取名的目的就是形容機器的行為很容易被改變
* 軟體應該能夠隨著需求改變而簡單的變更，scope 的改變比較容易，shape 的改變比較困難
* 當改變的形狀無法輕鬆拼進系統的形狀則改變就變的很困難
#### The Greater Value
* 一般來說大家都會覺得系統能夠運作比起系統能改改變更重要
* 假設程式運作很好但無法改變只要有改變需求則此程式就沒有用了
* 假設程式無法運作但很好改變就可以改變成可以運作的，之後也可以依據需求改變，所以程式才有用
#### Eisenhower’s Matrix
* 軟體行為是緊急但不重要
* 軟體架構是不緊急但很重要
* 通常都會把緊急但不重要的東西排在較高的順位導致軟體架構被忽略而專注在不重要的行為上
* Manager 無法衡量架構的重要性所以要靠 developer
#### Fight for the Architecture
* 需要與不同 team 取得平衡保持架構的完整
* 架構師需要關注於建立容易開發修改擴充功能的架構
* 最後才考慮架構會讓成本更高讓變動更不容易，當這種情形發生代表沒有在捍衛架構的完整度
