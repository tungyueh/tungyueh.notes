# The Staff Engineer's Path
# Foreword
# Introduction
## Two Paths
* 通常會有兩種選擇，一個是 manager 另一個是 technical leader，雖然看起來是截然不同的道路但越往後會發現有很多相通的地方
* Manager 的道路比較清晰因為有很多如何當 manager 的資訊，通常善於溝通、處理危機跟幫助同事的人通常會成為 manager
* Staff engineer 比較少被定義，各自的期待也不盡相同，就算被定義好後也沒不知道如何達到
## The Pillars of Staff Engineering
* Big-picture thinking: 對於專案有更長遠的思考而非只注意過程的細節
* Execution: 負責的專案會更模糊不清也有更多人參加，需要能引導專案成功
* Leveling up: 能夠為其他人帶來正向的成長，
* 都基於對於技術的知識跟經驗才能做出正確的判斷，執行時候才能解決問題，透過 review 幫助 code 跟 design 品質更好
# PART I | The Big Picture
## 1 | What Would You Say You Do Here?
### What Even Is a Staff Engineer?
* 成為 senior engineer 靠自己的學習到達但要往上需要增加對其他人的影響力跟負起更多的責任
* 不同情況都有不同的最佳選擇，有經驗的人會分析好壞處同時也需要了解細節
* 團隊都想要完成自己的目標但對於團隊的最佳解並非是整個組織的最佳解，所以需要有能更跨團隊的視野判斷哪個決定對組織最好
* 有經驗的工程師可以幫助管理階層不需要做出每個決定使產出提高
* 專案有很多團隊負責，雖然各自做出最佳解但有可能會造成專案停滯，如果有一個整體負責人可以在專案開始之際負責規劃 high level system design，維持標準跟 mentoring，專案之外對其他人說明該專案的進度跟願景
* Technical Project Manager 主要負責規畫時程而非 review design 跟確保軟體品質，staff engineer 負責產出系統的穩定性管理技術債
### Enough Philosophy. What’s My Job?
* 軟體是為了解決問題而存在所以需要有良好的品質，但再有 deadline 情況之下要兼顧品質就需要 senior engineer 帶頭做起起到示範作用讓大家自然的跟著做才能有好的軟體品質
* 為了達成更多的事情除了技術之外還需要有更多的能力，要能說服別人走向正確的道路，這樣做就自然會成為 leader
* Leadership 可能來自於可以避免常見錯誤的設計、review code 跟 design 增加其他人的能力、指出 design 與大方向不同的地方，最終其他人願意採用自己的方案只因為累積的信用
* 透過深厚的技術背景才能了解做出的技術決定的 trade-offs 跟幫助其他人理解，在必要的時候能夠深入了解細節，問出正確的問題跟理解回答
* 有效率的處理問題才能最好的利用時間，將問題整理到一段落就可以放手讓其他經驗較少的人處理提供成長的機會
* 確認組織有好的技術方向，architecture, storage system, tool 跟 framework 都能解決設定的問題讓大家可以完成工作並且做的好
* 越資深越仰賴溝通技巧，因為需要常常跟大家交換腦中的想法
### Understanding Your Role
* 根據公司的需求而有不同的責任，所以不同公司的 staff engineer 很難互相比較
* 跟更高 level 的人報告會有更廣的視野，可以得到更高等級的資訊也需要處理更高難度的問題，但被 manager 分配的時間會更少，也無法給予太多幫助
* 責任範圍之外要試著去補足能力，而非只停留在自己的賽道上面，危機來臨能夠補上組織缺少的地方
* 守備範圍太廣會沒有影響力而只負責解決其他人所有的問題而非重要的問題
* 所有事情都需要自己做覺得未造成 bottleneck 而陷入不斷開會的漩渦之中
* 選擇一個領域建立影響力做出成果，把時間貢獻給解決問題上
* 在一個 team 負責範圍太小容易沒把時間放在需要被解決的問題上也會剝奪其他工程師學習的機會
* 根據自己的個性選擇要深度還是廣度
* 基礎能力、產品管理、時程管理、人員管理是需要具備的能力
* 如果想要維持 coding 能力不要做太重要的事情只要做一點簡單工作就好
* coding 的成果可以很快在每個 compile 或測試成功看到但組織的改變是需要好幾個月的，藉由定期詢問信任的 manager 知道成果或者負責時程較短的project
* Tech lead manager 同時有技術跟管理的工作，所以也比較難同時兼顧兩種技能的發展，有些人會接管理職位幾年在改去技術就可以一段時間內專精所需技能
* Staff engineer有一些型態
    * Tech Leads: 跟 manager 合作負責執行面
    * Architects: 負責跨領域的方向跟品質
    * Solvers: 解決單一困難的問題
    * Right hands: 協調複雜組織間的問題
* 在 staff engineer level 做事的機會成本很高要找重要但沒人發現的事情
* 不要選擇出意見的人比實際做事的人多的案子，選擇需要自己才能解決的問題
### Aligning on Scope, Shape, and Primary Focus
* 寫下自己對於該職位的目標、執行方式跟預期成果跟 manager 對齊目標才能符合其他人的期待

## 2 | Three Maps
### Uh, Did Anyone Bring a Map?
* 有地圖才能指引我們往目標前進，太多資訊的地圖容易看不清因此分成三種地圖代表不同作用
* Locator Map 幫助團隊理解產出的內容如何幫助組織
* Topographical Map 幫助與其他團隊溝通
* Treasure Map 幫助大家確定目標在哪邊
* 加入新公司像是一團迷霧隨著探索而撥開迷霧持續更新地圖
* 對於事物保持好奇心才能注意到細節讓地圖更豐富
### The Locator Map: Getting Perspective
* 過於專注會有風險
    * 搞錯優先順序，只追求 local maximum 而非 global maximum
    * 只用特定視角看世界
    * 忽視小問題漸漸演變成危機像是溫水煮青蛙而不自覺
    * 忘記目的是什麼
* 使用局外人的視角比較容易發現問題而不陷入溫水煮青蛙的情境
    * 當人們都給出同樣的意見可以嘗試詢問不同團隊的意見
    * 知道現階段重要的事，少了核心功能要盡快推出避免流失客戶而非償還技術債
    * 了解客戶在意的事情而非只看著 service 可靠性的數據
    * 很多問題都是重複發生，解決之前先找找看組織內部是否已經有解決方案
