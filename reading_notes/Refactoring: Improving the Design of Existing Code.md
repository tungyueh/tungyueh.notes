# Refactoring: Improving the Design of Existing Code
## Chapter 1. Refactoring, a First Example
* 舉例比起原則更人讓人了解該如何應用而不向原則把事情弄得太通用
### The Starting Point
* 複製貼上 code 的問題在於之後需要修改的時候
* 當需要加功能的時候發現不好加就要先改成好加的樣子再加功能
### The First Step in Refactoring
* 需要有測試避免犯錯
* 用一行指令快速跑完所有測試，refactoring 過程會常常重複這個動作
* 測試要能顯示正確或失敗才不用花時間看到底結果如何
* 因為 refactoring 依賴於測試所以花時間去建立一個好的測試是很重要的
### Decomposing and Redistributing the Statement Method
* 從過長的部分拆解成小部分以便後續流程
* 目標是把新增功能的 duplicate code 減少
* 每次改一點點發現錯誤可以很快找到錯誤
* 任何人都可以寫出電腦懂得 code 但是只有好的 programmer 才有辦法寫出讓人懂得 code
#### Extract Method
* 找出有複雜邏輯的地方使用 Extract Method 
* 錯誤的 extract 會造成 bug 所以需要注意拿些地方可能會出錯
* 檢視準備修改的片段在 method 的 local variable
    * 沒被修改的可以當成 parameter 傳進去
    * 會被修改的如果只有一個可以回傳回去
* Extract 出來的 method 可以改變裡面 variable 賦予更有意義的名字讓 code 更清晰
#### Move Method
* 發現 method 沒使用到該 class 的資料代表放錯 class，使用 Move Method 移到適合的 class
* 移到合適的 class 可以少掉 parameter，可以重新命名 method
* 原本的 class 的 method 的 body 換成從新 class 拿到資料
* 找出所有舊的 method 替換成從新 method
* 如果舊的 method 是 public 可以留著保持該 class interface 不變
#### Replace Temp with Query
* Temp variable 在 refactoring 之後不會變動就可以移除
* 每次要用到值就去問
* Temp 通常需要傳來傳去，容易讓人迷失在該數值如何變化，在 long method 造成的影響更大
* 雖然改成去問的方式會需要每次重新計算但因為變成更小的部分之後優化會更容易
### Replacing the Conditional Logic on Price Code with Polymorphism
* switch statement 不要用其他人的東西當做 key，用自己的東西
* 不同的種類對於回答問題有不同的方式就可以用 subclasses
* 可以用 polymorphism 替換掉 switch statement
### Final Thoughts
* Extract Method, Move Method, Replace Conditional with Polymorphism 都是讓責任分散到更適合的地方所以更容易維護
* Refactoring 的節奏就是測試跟改一點的不斷循環
## Chapter 2. Principles in Refactoring
### Defining Refactoring
* 以名詞來解釋為更改內部架構以獲得更好理解或更容易修改但不影響外在行為
* 以動詞來解釋為用一連串的改變在不影響外在行為下重新架構軟體
* Reforing 雖然是清理 code 的行為但以更有效率、產生更少 bug、可控制的情況下清理 code
* Refactoring 是為了讓程式更容易理解跟修改，與其相反的是 performance optimization 是為了速度而讓程式變複雜
* Refactoring 不改變外在行為，軟體照常提供相同功能
#### The Two Hats
* 加新功能不改變現有的 code，藉由增加測試後讓測試通過當作進度
* Refactor 不增加新功能，只有 restructure code，不增加任何測試
* 開發軟體需要時常在上面兩種活動切換
* 加新功能的時候發現如果改變架構比較好加就該換帽子，改變架構完再換帽子開始加新功能，加完新功能發現不好懂就就繼續換帽子，開發過程中要隨時注意是戴哪頂帽子
### Why Should You Refactor?
* Refactoring 只是個工具而不是萬靈藥，是為了某些目的所使用的工具
#### Refactoring Improves the Design of Software
* 程式不 refactor 只會隨著時間變差，當改變 code 就會讓 code 失去原有的結構也就違背當初設計的目的
* 設計不好的 code 用了更多 code 做相同的事情，因為在不同地方都用不同 code 相同的事情，越多的 code 越難改對、需要越多時間讀懂，所以要讓設計更好就是要去除 duplicate code，讓未來的修改更容易，確保程式只在一個地方做一件事情
#### Refactoring Makes Software Easier to Understand
* 寫 code 就只是告訴電腦該做什麼事，用這種方式代表只用 code 跟電腦溝通，但除此之外還會有人看懂 code 後去修改，我們在乎能快速的修改好而不在乎電腦多跑一點 cycle
* Refactoring 能夠運作的 code 但沒有好的架構可以讓 code 更容易理解，用這種方式代表用 code 表達意圖
* 通常需要修改自己的 code 就是自己，能夠把需要知道的東西放在 code 裡面就不需要記太多東西
* Refacotring 幫助理解不熟悉的 code，讓 code 能更符合自己的理解，然後使用測試檢查所理解的東西是否正確
* Refactoring code 幫助我們排除細節更容易理解更高階的設計
### Refactoring Helps You Find Bugs
* 對於 code 了解程度越高則越容易找到 bugs，藉由 refacotring 慢慢對 code 認識越深，驗證之前的假設，bug 就會浮現出來
* 有著良好寫 code 習慣的 programmer 才是好的 programmer
#### Refactoring Helps You Program Faster
* Refactoring 顯而易見的可以增加軟體品質，但好像會讓開發速度變慢?
* 好的設計是快速開發的條件，如果沒有好設計一開始雖然會很快但之後常常需要修 bugs 而不是開發新功能，花更多時間理解系統找出 duplicate code 做修改，做新功能需要寫更多 code 來不斷 patch
* Refactoring 讓系統的設計不會毀壞，保持好的系統設計才能保持開發速度
### When Should You Refactor?
* Refactoring 不應該獨立時間出來而是存在於開發過程中，當決定要 refactor 代表要做些別的事情
#### The Rule of Three
* 第一次就直接做，第二次做相似的事情雖然知道會產生 duplicat code 但就這樣，第三次又遇到就該 refactor
#### Refactor When You Add Function
* 最常 refactor 的時候是要加入新 function 的時候，如果需要了解 code 在做什麼就 refactor 成更好理解的狀態，下次遇到就更容易理解
* 當發現目前設計不好加入新功能就思考該如何設計才比較容易加入新功能，藉由 refactoring 修正設計讓之後擴充更容易，這也是最快速的方法
#### Refactor When You Need to Fix a Bug
* 修正 bug 的 refactoring 是為了讓 code 更好理解
* 當發現 bug 代表 code 不夠清晰以至於沒發現 bug
#### Refactor As You Do a Code Review
* Code review 讓知識得以傳遞到整個 team，讓有經驗的人傳授知識給比較沒有經驗的人，讓人檢視寫的 code 對於 team 是否 clean，讓大家幫忙提供更好的想法
* Review 別人的 code 時給意見需要知道是否可以簡單做出來，可以自己試著先 refactor 讓 code 處在於給建議很容易理解的狀態，建議不再需要想像才能知道而是直接看見，再來可能又有第二層的想法跳出來。
* Refactoring 幫助 code review 有更實際的成果，而不只是建議而已
* 找一個 reviewer 跟作者一起 coding 由 reviewer 提供意見一起決定是否好 refactor
* 大一點的 design 應該要找多個人提供意見，使用 UML diagram 然後用 CRC cards 跑過不同情況
#### Why Refactoring Works
* 只做好今天的事情而不考慮未來則無法做未來的事情
* 發現之前錯誤的決定就該改變決定
* 為何寫程式這們難?
    * 越難讀懂得程式越難修改
    * 有 duplicated login 的程式很難修改
    * 程式加新功能需要動到之前的 code 會很難修改
    * 程式有複雜邏輯會很難修改
* 程式簡單易懂，所有邏輯都集中在一個地方，不允許修改現有 code 去影響行為，讓複雜邏輯簡單表達是我們所想要的
* Refactoring 是讓程式增加價值的過程，雖然沒有改變行為但是確保的品質可以維持開發速度
### What Do I Tell My Manager?
* 可以由品質或 review 的方式說服使用 refactoring
* 或者不用說因為怎麼做事是 programmer 的方式，refactoring 可以增加開發軟體或修正 bug 速度也剛好符合 manager 目標
### Problems with Refactoring
* 當新學到的工具可以大幅增加效率就很難知道什麼時候不該使用，因為尚未遇到不適合使用的情境所以無法得知
* 使用 refactoring 關注進度來判斷 refactoring 有沒有問題，然後找出解法或者知道該問題不適合使用 refactoring
#### Databases
* 很多商業邏輯都會跟 DB 綁再一起，所以很難 refactoring，另外一點是改變 schema 需要做 migration
* Nonobject databases 可以在 object model 跟 database model 加一層隔離，當改變其中一個 model 就不需要動到另外一個，雖然增加複雜度但是也增加彈性
* Object databases 改變 class 的 data structure 需要特別注意，可以移動行為但是移動 field 要注意
#### Changing Interfaces
* Object 好處在於在 interface 之內的實作都可以改變而不影響行為但是改變 interface 則什麼都可能發生
* Rename method 是一種簡單的改變 interface 的例子
* 改變 interface name 只要找出所有用到的地方一起改名字就可以但是有些無法找到跟改變的 code 就只能用更複雜的方式，這類 interface 稱為 published interface
* Refactoring published interface 需要讓新舊 interface 並存，讓舊 interface 的實作改由呼叫新 interface
* 盡量不要 published interface，團隊合作可以用 code base 來避免 publish interface
#### Design Changes That Are Difficult to Refactor
* 設計本身很難靠 refactor 來解決
* 先思考換設計是否很簡單，如果很簡單即是尚未滿足潛在的需求還是可以做 refactor，如果很困難則專注於設計
#### When Shouldn't You Refactor?
* Mess code 重寫可能比 refactor 好但目前沒有好的判斷方式
* Code 本身就有錯誤則就可以直接重寫，Refactor 前提是 code 是正確無誤的
* 先用 refactor 把系統分成許多部份，之後看要重寫還是 refactor 這些部分
* Deadline 之前不要 refactor 因為得到的效益只會在之後才顯現，適當的累積技術債也是維持功能正常的方式，需要好好管理債務避免被壓垮
* 時間常常不夠用是個需要 refactoring 的訊號
### Refactoring and Design
* Refactoring 可以補充設計的不足，先設計好再寫 code 可以減少 rework 的時間，設計可能存在很多缺漏
* Refactoring 可以是 upfront design 的替代品，先寫出來在 refactor 成形，這種方式也能夠寫出設計的很好的 code，但不是最有效率的方式
* 先設計好合理的版本後再去 refactor，可以減少設計一次到位的壓力
* Refactor 提供不斷嘗試解法的方法，每當想出一個解法會對於問題更加了解
* 為了能夠應付未來的變動會設計的很有彈性，但也變得難維護，也不一定用的上
* Refactoring 改變對於 change 評估，一樣可以假想可能的變動跟彈性的設計，但要思考是否可以從簡單的設計 refactor 成複雜且有彈性的設計，如果很容易就先做簡單的就好了
* Refactoring 可以讓我們使用簡單設計但不犧牲彈性，讓設計過程更簡單更沒有壓力，當需要改變時再 refactor 成彈性的設計就好
### Refactoring and Performance
* Refactoring 可能會造成速度變慢但是 refactoring 之後才比較好調整效能問題
* 在 hard real-time system 把使用時間用預算制分配給不同部分
* 每個 programmer 都要努力把速度提高，但通常運作不起來因為為了速度會讓系統複雜度提高之後開發就會變慢，可以增加速度的地方到處散佈所以改善某些地方只會改善整體一點點
* 通常最花時間的 code 只會集中在一小部分，在其他部分改善都是浪費時間而且讓系統更難懂
* 先不管速度只讓系統 well-factored 等到需要優化的時候再來調整
* 使用 profiler 找出花最多時間的部分，修改後重新用 profiler 量測，如果沒有改善就 revert，直到有改善為止
* Well-factored 系統讓我們更快速加入調整的 function，可以更精準找出花時間的地方因為 profiler 會幫助找到小的區塊
* 雖然一開始花比較多時間 refactoring 但之後比較好優化最終還是更快完成優化
### Where Did Refactoring Come From?
* 好的 programmer 會時常去清理 code 因為 clean code 比起 messy code 更容易修改
