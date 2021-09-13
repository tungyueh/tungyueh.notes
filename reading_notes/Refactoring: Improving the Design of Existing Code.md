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
## Chapter 3. Bad Smells in Code
* 需要知道開始跟結束 refactoring 的時機跟知道如何 refactoring　一樣重要
* 沒有很明確的規則主要靠一些不好的 code structure 來判斷
### Duplicated Code
* 找一個一致的方法來處理重複出現的 code structure
* 同一個 class 有兩個 method 是同樣的 expression 可以使用 Extract Method 讓兩個 method 從同一個地方呼叫
* 同一個 expression 存在不同的 subclass 可以先使用 Extract Method 後再使用 Pull Up Field
    * Expression 有相似的地方先使用 Extract Method 把同樣跟不同的地方方開在使用 Form Template Method
    * 做同樣事情但使用的 algorithm 不同可以使用 Substitute Algorithm
* Duplicate 發生在不同 class 使用 Extract Class 抽出來後兩個 class 使用這個 class
### Long Method
* 舊的語言呼叫 subroutine 會有 overhead 但現代 OO 語言沒有這個問題，剩下是讀 code 的人需要跳來跳去但 IDE 可以幫忙消除這個問題，但重點是只要名字取的夠好就不需要去看內容
* 積極的分解 method 當想要註解的時候用寫一個新 method 取代，把需要註解的 code 放在 method 而名字用意圖而不是實際行為，目的是消除語意與行為的落差
* Method 使用很多參數跟暫存變數，在 extract method 過程中就不需要這麼多參數與暫存變數，利用 Replace Temp with Query 消除暫存變數，利用 Introducce Parameter Object 跟 Preserve whole object 消除多參數
* 還是有很多參數跟暫存變數的話可以使用 Replace Method with Method Object
* 藉由註解找出可以 extract 的地方因為註解代表想消除 code 跟目的的落差
* Conditional 跟 loop 可以用 Decompose Conditional
### Large Class
* Class 做太多事情會表現在 instance variable 過多，也會導致 duplicate code 的發生
* 使用 Extract Class 把相關聯的 instance variable 綁在一起，如果有相同的 prefix 或 suffix 可以使用 Extract Subclass
* 決定 client 如何使用 class 可以使用 Extract Interface
* GUI Class 需要保持 duplicate data 在不同地方的時候可以使用 Duplicate Observed Data 
### Long Parameter List
* 之前傾向需要的東西都用傳進去，這樣做是因為如果不傳進去就只能靠 global data 而 global data 更不好，但現在有了 object 所以 method 可以自己去拿需要的東西
* 長參數很容易隨著需要更多的東西而改變而且也難以理解，但只要改成 object 就可以不用這麼常改變
* Replace Method with Method 就可以從 object 得到需要的東西，Preserve Whole Object 讓呼叫的人可以從 object 拿到需要的資料，Introduce Parameter Object 把沒有邏輯的相關資料放在一起形成 object
* 如果不想增加 dependency 則不該做 refactoring，要思考 dependency structure
### Divergent Change
* 我們希望讓軟體容易修改，快速找到需要修改的地方然後修改
* class 修改太過頻繁需要使用 Extract Class 把跟修改原因有關的東西拉出來
### Shotgun Surgery
* 需要改動很多個不同的 class 這樣容易找不到需要修改的地方
* Move Method 跟 Move Field 把改變的地方集中到 class ，使用 Inline Classes 把相關行為聚在一起
### Feature Envy
* 當 method 需要從其他 class 取得值代表 method 應該使用 Move Method 移到那個 class，或者只有一部分的 method 就可以用 Extract Method 再來用 Move Method
* 當從多個 class 取得資料則把移到使用最多 data 的 class，可以先用 Extract Method 把 method 變小比較好移
* 有些設計會打破規則像是 Strategy 或 Visitor，可以依據把東西放在一起後再改的原則，
### Data Clumps
* data 通常都會成群使用像是 class 的某些 field，parameter list 常常都一樣，可以先用 Extract Class 把 data 放在一起，再把 method signatue 用 Introduce Parameter Object 或 Preserve Whole Object 減少 parameter 個數
* 思考如果將其中一個 data 移出 clump 則剩下的 data 是否還有意義
* 減少 field list 跟 parameter list 可以移除 bad smells，利用 feature envy 找出可以移到新 class 的 data
### Primitive Obsession
* Record types 讓我們能把資料組成有意義的 group，Primitive types 是 building blocks
* Object 可以讓 primitive 跟 larger class 的界線連結模糊
* 新手使用 object 會抗拒小的 object，但這些小的 object 可以集中管理相關的資料，
    * 使用 Replace Data Value with Object 把有自己行為跟相關聯的 data 放到另外一個 class 增加可讀性跟容易維護
    * 使用 Replace Type Code with Class 把沒有行為的相關聯 data 集中在另一個 class 讓 type hint 能幫忙早點檢查到錯誤
    * 使用 Replace Type Code with Subclasses 或 Replace Type Code with State/Strategy 當 type code 有用在 conditional 上
* 當 array 存了不同種類的東西要使用 Replace Array with Object 減少有人放錯種類到 array 導致失敗，減少找出問題的時間
### Switch Statements
* Object-oriented code 特色是很少 switch statement，switch statement 問題是散落在各處，需要修改時要找出所有的地方，而 object-oriented 的 polymorphism 可以幫助解決這個問題
* 當看到 switch statement 就應該考慮 polymorphism
    * Switch statement 沒有用 type code 時候則用 Extract Method 把 switch statement 拆開，用 Move Method 移到需要的 class
    * Switch statement 有用 type code 的時候使用 Replace Type Code with Subclasses or Replace Type Code with State/Strategy
    * 最後使用 Replace Conditional with Polymorphism
* 如果 switch statement 不常改動可以用 Replace Parameter with Explicit Methods
* 如果條件判斷需要 null 則用 Introduce Null Object
### Parallel Inheritance Hierarchies
* 當新增 subclass 的時候都需要在另外一個繼承關係也新增 subclass，可以藉由 class 的 prefix name 跟其他繼承的一樣找到
* 先讓其中一個繼承關係的 instance 參考另一個，再用 Move Method 跟 Move Field 就可以讓 subclass 消失
### Lazy Class
* 新增的 Class 都需要花時間去維護，所以如果 class 的能力不值得以一個 class 存在則應該被消除
* 有可能是因為 refactoring 過度或者不再預期內的更改
* Subclasses 做得事情不到 class 的程度則用 Collapse Hierarchy 移除
* 使用 Inline Class 把不需要的 class 的東西移出來
### Speculative Generality
* 為了之後可能的需求設計 hook 或 special cases 但可能之後都不會用到只是白白增加複雜度
* 使用 Collapse Hierarchy 把 abstract class 移除，使用 Inline Class 把 delegation 移除，使用 Remove Parameter 把沒用到的 parameter 移除，使用 Rename Method 把名字從通用改成更實際
* 症狀是 method 只在測試被使用，把這些 method 刪除跟相關測試移除
### Temporary Field
* 有時候 object 的 instance variale 在特定情形才有，這樣很不容易理解因為我們預期 object 應該要使用到所有變數
* 使用 Extract Class 把獨立的變數跟相關的 method 放到新的 class，可以用 Introduce Null Object 把 conditional code 處理好
* 通常是因為不想把變數傳來傳去所以放在 field 但這個 field 只在某段計算過程有意義，其他時間只是徒增困惑，這時候可以用 Extract Class 來把這些變數跟 method 包裝起來，只在需要的時候使用，這種稱為 method object 
### Message Chains
* Client 會問 object 來找到另外一個 object 如此反覆，代表 client 跟找尋的架構綑綁再一起了，只要當中斷掉就會影響 client
* Hide Delegate 讓 class 新增一個 method 直接呼叫另外的 class，可以對每個 chain 的 class 的這樣做但很有可能變成 Middle Man
* 觀察 object 被用來做什麼，使用 Extract Method 把用到的 code 用 Move Method 移到該 method
### Middle Man
* Encapsulation 把細節隱藏，通常透過 delegation 來做到
* 有時候太過頭會發現 class 有一半以上的 method 都是 delegating 其他 class，這時候要使用 Remove Middle Man 讓人直接與 object 溝通，如果 class method 做不多事情就用 Inline Method，如果有額外的行為使用 Replace Delegation with Inheritance 把 middle man 變成 subclass
### Inappropriate Intimacy
* Classes 有時候太過親近導致需要花很多時間釐清 private 的部分
* Move Method 跟 Move Field 可以減低 classes 之前的關係
* 看是否可以使用 Change Bidirectional Association to Unidirectional
* Classes 都想要某些東西把這些東西用 Extract Class 集中
* 使用 Hide Delegate 讓 class 當中間人
* Replace Delegation with Inheritance 當 classes 有很多簡單的 method 都在 delegate 其他 class 
### Alternative Classes with Different Interfaces
* Rename Method 把不同的 class 但做同樣事情的 method 換成一樣的名字
* 不斷使用 Move Method 把行為移到 classes 直到都相同
* Extract Superclass 把相同的部分變成 superclass 原本的變 subclass
### Incomplete Library Class
* Reuse 通常是 object 的目的但不應該過度相信也不否認，因為我們還是很依賴於 library
* Library 的作者通常不知道使用者需要什麼但使用者也無法改 library
* Introduce Foreign Method 在我們只需要一些 method 可以使用
* Introduce Local Extension. 在想要更多行為可以使用
### Data Class
* Classes 只有 field 跟 getting 跟 setting method 很容易被拿來做太多關於內部細節的動作
    * 有 public field 應該要用 Encapsulate Field 變成 private 再用 access method 讀取避免其他人直接修改 field
    * 有 collection field 使用 Encapsulate Collection 讓 getter method 回傳的是 read-only 然後不能用 setter method對 collection field 直接改變，改用 add 跟 remove
    * Field 不可改變則使用 Remove Setting Method
* 觀察其他 class 怎麼使用 data class 把相關行為用 Move Method 搬進 data class，無法直接移動用 Extract Method 新增一個在移動，最後開始 Hide Method 把 getter 跟 setter 隱藏起來
* Data class 一開始沒有行為沒關係但隨著行為變多應該要慢慢給予責任
### Refused Bequest
* Subclasses 不想繼承所有 parent 的東西
* 使用 Push Down Method 跟 Push Down Field 把只有該 subclass 用的東西從 parent 移進來，讓 parent 只剩下 common 的東西
* 本來都會用繼承後用一部分的行為而已所以看到這種不一定需要 refactoring，只有在造成困惑跟問題才需要修改
* 如果 subclass 不支援 superclass 的 interface 就需要使用 Replace Inheritance with Delegation 來修正
### Comments
* Comment 讓我們離 bad code 更接近，refactoring 完就會發現 comment 是多餘的
* 如果需要 comment code block 在做什麼則使用 Extract Method 給這段 code 一個 method，如果還是需要 comment 則使用 Rename Method
* 需要解釋規則則使用 Introduce Assertion
* Comment 適合用在不知道該怎麼做的時候，或者說明為什麼要這樣做
## Chapter 4. Building Tests
* Refactor 之前要確保有健全的測試
* 好的測試可以幫助寫 code 就算不是在 refactoring 也是一樣，雖然有點不直覺但接下來會解釋
### The Value of Self-testing Code
* 大部分真正寫程式的時間只佔開發的一小部分，主要都需要先搞清楚要做什麼然後設計，大部分的時間都在找 bug 在哪邊，找到 bug 就可以很快修好
* Class 應該都要有測試來測試自己
* 測試要能直接顯示結果正確與否減少確認的時間
* 測試要能輕鬆的跑就可以頻繁的跑，當開發過程出現 bug 就可以知道是因為剛剛寫的 code 所導致的，因為記憶猶新的關係所以很容易找到 bug，所以測試變成一個很好的 bug detector
* 說服其他人寫測試很困難因為需要多寫 code，除非可以明顯改善開發效率
* 開發功能要先寫好測試，測試可以幫助釐清目的、專注於 interface、知道目開發是否完成
### The JUnit Testing Framework
* Assert 是自動檢測結果的方法
* 時常跑測試
* 測試速度要快不然會不想測試
* 讓測試錯誤訊息淺顯易懂
* 先故意讓測試失敗藉此知道有測試到想要的部分
#### Unit and Functional Tests
* Unit test 只是幫助維持生產力，只測試自己的 class 的 interface
* Functional tests 確保軟體的行為正確，提供品質保證不管生產力，應該要由別的 team 撰寫
* Functional test 把整個系統當黑盒子，只使用公開出來的方式測試，只觀察 ouput data 的變化
* 當找的 bug 應該要有 unit test 讓 bug 顯現出來再去修正確保該 bug 不會再出現
### Adding More Tests
* 測試是為了找出問題所以要對風險較高的部分增加多一點的測試，簡單的部分不容易有問題所以不用有測試
* 要求寫完整的測試會讓人不想寫測試，寫測試只是為了確保擔心的部分會不會出錯，所以只要寫能有效率幫助的測試就好
* 測試要注重 boundary condition 因為這是最容易出錯的地方
* 特殊的情況也要特別測試
* Exceptions 也需要被測試
* 測試需要持續更新，測試幫助釐清 error condition 跟 boundary condition
* 找出風險在哪邊，看看複雜的部分然後思考可能出現的問題，雖然測試無法找出所有 bug 但 refactor 後理解會增加可以找到更深層的 bug
* Object 的 inheritance 跟 polymorphism 的組合多樣性造成測試的難度，先測試獨立的找出大部分的問題
* 測試 code 可以複製，之後再來找到 common 的部分 refactor，可以快速產生測試就好
* 使用測試建立 bug detector 然後頻繁的跑可以為開發帶來很大的幫助
## Chapter 5. Toward a Catalog of Refactorings
### Format of the Refactorings
* Refactoring 會有五個部分
    * 從 refactoring 方式的名字開始，這樣才有能夠建立起 refactoring 的詞彙
    * 介紹造成需要 refactoring 的情況跟如何 refactoring
    * Refactoring 的動機，介紹該 refactoring 跟不要的情況
    * 如何一步一步做 refactoring
    * Example 介紹 refactoring 如何作用
* Summary 包含對於問題的介紹跟提供大概如何 refactoring
* Mechanics 介紹如何 refactoring，提供簡潔的步驟，有需要再去看 example 的解釋
* 盡可能使用越小幅度的修改，但實際使用可以稍微大一點的幅度遇到問題才縮小幅度
### Finding References
* 使用電腦搜尋出所有要 refactoring 的 method 的 references
* 不要無腦搜尋取代，要思考是否正確的取代掉
* Strong typed language 可以故意把 feature 移除讓 compile 找出 dangling reference，但這個方法有壞處
* Compiler 可能會找的很慢，可以先用文字搜尋再用 compiler 重複確認
* Compiler 找不到 reflection API
### How Mature Are These Refactorings?
* Refactoring 的基本概念是一次修改一點然後不斷測試
* 知道自己正在 refactoring 並根據情況採用合適的技巧
* Refactoring 的方法只適用於 single-process，其他系統例如 concurrent 跟 distributed programming 有不同的方法
* Desing Patterns 是 refactoring 的目標，透過 refactoring 從其他地方有條理的變成期望的 pattern
## Chapter 6. Composing Methods
* 大部分 refactoring 是重組 method 讓 code 放在適當的地方，大部分的問題出在 method 太長所以隱藏太多資訊在複雜的邏輯之下
    * Extract Method 把一部分的 code 獨立成自己的 method
    * Inline Method 把 method 換成 method body，當 Extract Method 太過頭使 method 失去身為 method 的責任的時候可以使用
* Extract Method 最大問題是 local variable 跟 temps
    * Replace Temp with Query 可以移除掉大部分的 temps
    * 如果 temp 用在很多地方則可以使用 Split Temporary Variable
* 有時候 temporary variables 太過糾纏則可以使用 Replace Method with Method Object 來處理，藉由多一個 class 的代價讓我們可以處理複雜的 method
* Parameter 如果有 assign 則使用 Remove Assignments to Parameter
* 當 method 足夠簡單就可以使用更好的 algorithm 去改善，使用 Substitute Algorithm
### Extract Method
* 把 fragment 變成 method 然後用名字表達意圖
#### Motivation
* 當 method 太長或有 comment 解釋意圖就會用 Extract Method
* 命名良好的短 method 可以讓其他人更容易使用，形成 high level 的 method 看起來就像一連串的註解，overriding 也很容易
* Method name 要能夠縮短於 method body 的距離讓人更清楚該 method 的意圖
#### Mechanics
* 建立一個新 method 使用意圖來命名而不用實際做的事情
* 從 source method 複製 extracted code 到 target method
* 掃過一遍 extracted code 在 source method 的 local scope 有使用到的變數，這些要變成 target method 的 local variable 跟 parameter
* 查看是否有任何 temporary variable 在要 extract 的 code 中，把它宣告在 target method
* 查看 extracted code 是否有改變 local scope 的變數，如果有則查詢 extracted code 在 assign 回變數，如果有很多變數先使用 Split Temporary Variable 後再試一遍，再來使用 Replace Temp with Query 刪除 temporary variables
* 把 local-scope variable 當成 parameter 傳進 target method
* 把在 source method 的 extracted code 用 target method 取代
* 測試
### Inline Method
* Method body 已經足夠清楚表達 method name 就可以直接把 method body 放進 caller 然後移除該 method
#### Motivation
* 使用 method 比較不直接當 method body 已經足夠表達用意就可以移除，不然只是很煩人而已
* Inline Method 使用在發現有一堆 method 被分散，先將所有 method inline 後再去 extract，通常在 Replace Method with Method Object 之前很適合使用
* 當有人用太多 indirection 而 method 都只是 delegation 其他 method 讓人容易迷失，這時候就會用 inline method 把有用的留下沒用的刪除
#### Mechanics
* 檢查 method 不是 polymorphic 因為無法 inline subclasses overrided method
* 找出所有呼叫的地方
* 把呼叫的地方換成 method body
* 測試
* 移除 method
### Inline Temp
* 如果 temp 只有被簡單的 expression assigned 一次則用 expression 取代 temp
#### Motivation
* Inline Temp 通常是 Replace Temp with Query 的一部分，如果 temp 只有被 method call assigned 值可以先不用管，但如果需要 extract method 則要先 inline
#### Mechanics
* 宣告 temp 是 final 確保只被 assigned 一次
* 找出所有 temp 使用 right-hand side assignment 取代
* 測試
* 移除 temp 宣告跟 temp 的 assignment
* 測試
### Replace Temp with Query
* 如果使用 temp 存 expression 的結果，把 expression 變成 method，把所有 temp 都換成 expression，之後新的 method 就可以被其他 method 用了
#### Motivation
* Temps 的問題是暫時的跟 local，因為只有該 method 裡面才看的到所以容易造成 long method，換成 query method 就可以讓其他 method 使用把 code 變得整齊
* Replace Temp with Query 是 Extract Method 之前重要的一步，因為 local variable 會很難 extract 所以盡可能把 variable 換成 queries
* 如果 temp 只有被 assigned 一次而且產生 expression 結果沒有 side effect 就很簡單，其他比較複雜的需要先做 Split Temporary Variable 或 Separate Query from Modifier 讓 refactoring 變簡單一點，如果是透過 loop 收集結果需要把複製一些邏輯進去 query method
#### Mechanics
* 找出只被 assigned 一次的 temps，如果有被多次 assigned 先使用 Split Temporary Variable
* 宣告 temp 為 final
* Compile
* 把 assignment 的 right-hand side 變成 method
    * 先把 method 宣告成 private
    * 確認 method 沒有 side effects，如果有需要使用 Separate Query from Modifier
* Compile and test
* 使用 Replace Temp with Query
* Temp 通常用來儲存 loop 的結果，loop 可以 extract 成 method
* 效能問題等到實際發生再說，如果把 code factored 好也容易精準的優化
### Introduce Explaining Variable
* 把複雜的 expression 結果放進一個 temps 然後名字解釋目的
#### Motivation
* 太複雜的 expression 會難以解讀，temps 有幫助於管理這些 expression
* Introduce Explaining Variable 藉由良好命名的 temps 解釋 conditional logic 的目的
* 如果可以用 Extract Method 就用，因為 temp 只在單一 method 有用，method 可以讓整個 object 或其他 object 都使用
#### Mechanics
* 宣告 final temporary variable 然後把 expression 一部分結果存下來
* 用 temp 把 expression 的部分邏輯取代掉，如果有多個一次換一個
* 編譯跟測試
* 繼續 expression 的其他部分
### Split Temporary Variable
* Temporary variable 被 assign 不只一次，但不是 loop variable 或 collecting temporary variable
#### Motivation
* Temporary variable 用來做很多事，有些天生就是會被 assign 很多次例如 loop variable 或用來收集資訊
* 大多數 temporaries 是用來存下結果讓很多 code 可以使用，應該只能被 assign 一次，因為被 assign 多次就違反 SRP，使用 temp 代表不同事情很讓人混淆
#### Mechanics
* 把 temp 的名字換掉
* 把新名字的 temp 宣告成 final
* 把第二個 assignment 之前所有 reference 到的 temp 換成新名字
* 宣告第二個 assignment 的 temp
* 編譯跟測試
* 重複執行
### Remove Assignments to Parameters
* 使用 temporary variable 取代 assigns 給 parameter
#### Motivation
* 對傳進的 object 使用本來的 method 是可以的但不要換成另外的 object
* 對於 pass by value 或 pass by reference 會容易混淆
* 讓 paramter 保持不變比較清楚
#### Mechanics
* 為 parameter 建立 temporary variable
* 把 parameter 的 reference 都換成 temporary variable
* 把 assignment 從 paramter 換到 temporary variable
* 編譯跟測試
### Replace Method with Method Object
* Long method 有用到許多 local variable 所以無法用 Extract Method
* 把 method 變成 object 讓 local variable 都變成 object 的 field，之後可以慢慢分離 method 
#### Motivation
* 小的 method 讓我們能夠把事情表達的更清楚
* Method 有用到很多 local variable 讓分解變難可以先使用 Replace Temp with Query，如果不行最後才使用 Replace Method with Method Object
* 使用 Replace Method with Method Object 把 local variable 都變成 object 的 field 之後就容易使用 Extract Method
#### Mechanics
* 建立新的 class 名字取的跟 method 一樣
* 新的 class 建立 final field 給在 source object 的 field，建立 field 給 temporary variable 跟 method 的 parameter
* 新的 class 的 constructor 使用 source object 跟每個 parameter
* 新的 class 建立 compute method
* 複製原本的 method body 到 compute method，使用 source object field 
* 編譯
* 把舊的 method 換成 new object 然後呼叫 compute
### Substitute Algorithm
* 換一個更簡潔的演算法
#### Motivation
* 當有不同種方式可以達成目的就應該使用最簡單的一種，當 refactoring 把複雜東西變成簡單的片段都會有極限，到這個時候只能整個將 algorithm 取代換成更簡單的
* 想要用不同 algorithm 做點不同的事情要先從簡單的開始取代
* 先把 method 分解成簡單的再換掉 algorithm，因為要換掉複雜的 algorithm 很不容易
#### Mechanics
* 準備另外的 algorithm 並編譯成功
* 用新的 algorithm 跑過所有測試
* 若結果不同則比較與舊的 algorithm 有何不同
## Chapter 7. Moving Features Between Objects
* Object design 的重點在於該把責任放到哪邊，很難一開始放對地方但之後可以用 refactoring 改變
* 先使用 Move Field 再用 Move Method 把行為轉移
* Extract Class 把責任太多的 class 分解成只有單一責任的 class，Inline Class 把責任太少的 class 移除
* Hide Delegate 把 class 隱藏起來，Remove Middle Man 把因為 hide delegate 而太常變動的 interface 直接讓 class 使用
* 當想要把責任移到無法改變的 class 的時候，如果只有一到兩個 method 則使用 Introduce Foreign Method，如果有多個 method 則使用 Introduce Local Extension
### Move Method
* 需要被 move 的 method 通常被其他 class 常常使用到反而自己的 class 比較少用到
* 建立一個與最常用到別人的相似的 method，把原本 method 移除或 delegate
#### Motivation
* Class 有太多行為或跟其他人合作太密切就會使用 move method 把 class 變簡單一點也讓責任更精確
* 移動 fields 的時候可以看一下有沒有 method 可以移動到更適合的地方
* 如果很難決定該不該移動 method 可以先移動其他 method 讓整體更清晰，如果還是很難就先放著
#### Mechanics
* 檢視 source class 中有用到 source method 的人看看需不需要一起移動，通常一次移動多個 method 比較容易 
* 檢查 subclasses 跟 superclasses 有沒有其他的宣告
* Target class 宣告 method，可以選擇更合適的名字
* 把 soure method 的 code 複製到 target 並調整一下，需要讓 target method 能 reference 到 source object
* 編譯 target class
* 決定該如何讓 source reference 到 target object
* 把 source method 變成 delegating method
* 編譯跟測試
* 決定是否要移除 source method 或保留成 delegating method
* 如果決定要移除 source method 要把所有 reference 的地方換成 target method
* 編譯跟測試
### Move Field
* Class 的 field 更常被其他 class 使用就在 target class 新增一個 field 然後把用的人都做修改
#### Motivation
* 在不同的 class 移動 state 跟行為對於 refactoring 是很重要的，設計的抉擇可能不斷的變動，當發現設計不符合現在的情況就該改動
* 發現 field 更常被其他 class 的 method 使用，如果用 getting 或 setting method 可能會移動 method 因為要保持 interface，如果 method 是合理的就移動 field
* Extract Class 的時候需要先把 field 移動再來 method
#### Mechanics
* 如果 field 是 public 就用 Encapsulate Field
* 編譯跟測試
* Target class 建立 field 包含 getting 跟 setting method
* 編譯跟測試 target class
* 決定如何讓 target object 可以 reference 到 source
* 移除 source class 的 field
* 替換掉所有使用到 source field 的 references
* 編譯跟測試
### Extract Class
* 有一個 class 做了應該由兩個 class 所做的事情
* 建立新的 class 然後把相關的 fields 跟 method 舊的移到新的
#### Motivation
* Classes 會不斷成長，只要加入一些 operation 或 data 都會賦予更多的責任，通常都不會覺得應該要獨立出來一個 class 最後導致 class 太複雜
* Class 有太多 method 跟 data 會讓人不容易理解，需要開始思考該從哪邊切割，看看有沒有 subset data 跟 method 通常都在一起或者那些常常一起改動，另外可以從如果拿掉一些 data 或 method 則其他的還會不會有意義來判斷
* Subtype class 常常只影響部分 feature 或者有些 feature 需要不同的 subtyped 方式
#### Mechanics
* 決定如何分割 class 的責任
* 建立一個 class 來負責切出來的責任
* 建立舊的跟新的 class 連結
* Move Field 移動需要的 field
* 每一個移動都需要編譯跟測試
* Move Method 把 method 從舊的移到新的，先從 low-level method 開始移動
* 每一個移動都需要編譯跟測試
* 減少每個 class 的 interface
* 決定新 class 該 expose 哪些東西，決定要用 reference object 或者 immutable value object
### Inline Class
* Class 做的事情不夠多就就把他的 feature 放到另外的 class 後刪除
#### Motivation
* Inline Class 是 Extract Class 的相反，當 Extract class 可能讓 class 存在意義消失就找一個 class 放進去
#### Mechanics
* 宣告 source class 的 public protocol 到要移進的 class，把要移進的 class 的所有 method 都 delegate 到 source method
* 把全部 reference 到 source classs 都替換成到要移進的 class
* 編譯跟測試
* 使用 Move Method 跟 Move Field 把 source class 的東西移進 class
### Hide Delegate
* Client 使用 object 的 delegate class
* 建立一個 method 隱藏 delegate
#### Motivation
* Encapsulation 讓 object 對於系統其他部分了解更少，當需要改變的時候就可以改比較少東西
* Delegate 如果改變則 client 也需要改變，如果 server 隱藏 delegate 則只需要 server 做改變
#### Mechanics
* 為每個 delegate 的 method 在 server 加上 delegating method
* 讓 client 呼叫 server
* 每次調整 method 都要編譯跟測試
* 如果 client 不需要讀取 delegate 就把他移除
* 編譯跟測試
### Remove Middle Man
* Class 做太多 簡單的 delegation
* 讓 client 直接呼叫 delegate
#### Motivation
* Hide Delegate 讓 client 可以直接使用 delegated object 但只要 client 需要 delegate 的新功能則 server class 就需要再加一個，最後有可能 server 充滿著簡單的 delegation
* 很難找出隱藏多少是正確的不過 Hide Delegate 跟 Remove Middle Man 並不是太重要，可以隨著系統的演化做調整
#### Mechanics
* 建立 delegat 的 accessor
* 把每個用到 client delegate method 的換成直接去呼叫 delegate 的 method 並移除掉 server delegate method
* 每次改變 method 都編譯跟測試
### Introduce Foreign Method
* Server class 需要用到多的 method 但無法修改 class
* 在 client classs 建立一個 method 然後第一個參數是 server class
#### Motivation
* 用的 class 提供很多好用的服務但有可能少了我們需要的，這時候又不能改 class 新增 method 所以只能在 client code around
* 如果 method 只用在一個 client class 就比較簡單，如果很多 class 都需要會造成 duplication，需要 refactor 成 foreign method
* 發現 server class 有太多 foreign method 就需要用 Introduce Local Extension
* Foreign method 只是 work-around 要試著讓該 method 回到正確的地方
#### Mechanics
* Client class 建立一個 method
* 讓 server class 的 instance 當成第一個 parameter
* 註解是 foreign method 應該要存在於 server，以便容易找出 foreign method
### Introduce Local Extension
* 需要在無法改動的 server class 增加許多 method
* 建立一個包含需要 method 新的 class，讓 extension class 為 subclass 或原本 class 的 wrapper
#### Motivation
* 把需要的 method 跟 server class 利用建立 subclass 或 wrapper 的方式放在一起
* Local extension 能夠提供所有 class 的功能加上額外的功能
* 如果只是把 code 放到別的 class 而不是放到 extension 最終無法 reuse 這些 methods
* 通常使用 subclass 的方式因為比較簡單，但如果原本的 data 會變動則使用原本的 class 所做的改變就無法反應到 subclass 上面，這時候就要用 wrapper
#### Mechanics
* 建立 extension class
* 在 extension 上增加 converting constructor
* 在 extension 上面增加新功能
* 替換掉原本的 class
* 把 foreign method 放到 extension
## Chapter 8. Organizing Data
* 對於能不能直接存取 object 的 data 有許多爭議但通常允許直接存取 data 因為這樣 refactoring 比較簡單，有時候真的需要就使用 Self Encapsulate Field
* Object langauge 讓我們定義新的形 type，有時候發現用 object 更適合就使用 Replace Data Value with Object，如果 object 有很多 instance 在不同地方被用到則使用 Change Value to Reference 讓他們變成 reference object
* 發現使用 array 的方式像是 data structure 就用 Replace Array with Object，之後可以用 Move Method 慢慢加入新行為
* Replace Magic Number with Symbolic Constant 把有特殊意義的數字成有意義的東西
* Object 之間關係可能是單向或雙向，有時候需要使用 Change Unidirectional Association to Bidirectional 支援新的功能，Change Bidirectional Association to Unidirectional 當發現不需要雙向關係時可以移除多餘的複雜度
* Business logic 存在不該存在的地方需要把相關行為移到 domain class，使用 Duplicate Observed Data 支援原本的 class
* 使用 Encapsulate Field 把 public data 封裝起來，使用 Encapsulate Collection 把 collection 封裝起來，使用 Replace Record with Data Class 把 record 封裝起來
* 當使用 type code 指向特定的 instance，如果不影響行為則使用 Replace Type Code with Class，如果會影響行為則用 Replace Type Code with Subclasses，最後更複雜的情況使用 Replace Type Code with State/Strategy
### Self Encapsulate Field
* 使用 getting 跟 setting method 存取 field
#### Motivation
* 有些人認為定義出來的 class variable 就是可以直接存取，另外一派認為一定要用 accessor 去存取
* 使用 accessor 去存取變數的好處是 subclass 可以 overridde 來提供彈性，例如 lazy initialization
* 直接存取變數的好處是很簡單，不需要多一個只是拿值的 method
* 一開始先用直接存取都遇到不好修改的地方再改用 accessort 存取，refactoring 讓我們可以隨時改變想法
* 當需要讀取 superclass 的 field 但想要在 subclass 用計算好值來 override 就可以先用 Self Encapsulate Field 
#### Mechanics
* 建立 getting 跟 setting 的 method
* 找出所有用到 field 的地方替換成 getting 跟 setting method
* 把 field 變成 private
* 在一次檢查有把所有 reference 修正
* 編譯跟測試
### Replace Data Value with Object
* Data item 需要額外的 data 或 behavior 的時候換成 object
#### Motivation
* 初期通常都用簡單的資料型態代表 data item，但隨著功能增加發現需要更行為則把該 data item 換成 object
#### Mechanics
* 建立 value 的 class，弄一個 final field 型態跟之前一樣，增加使用 field 為 argument 的 getter 跟 constructor
* 編譯
* 把 source class 的 type 改成 new class
* 把 source class 的 getter 改成用 new class 的 getter
* 如果 field 有在 source class 的 constructor 使用 new class 的 constructer 為 field assign
* 改變 getting method 產生新 instance
* 編譯跟測試
* 可能需要用 Change Value to Reference
### Change Value to Reference
* 想要用 object 換掉有很多一樣的 instance，就把 object 變成 reference object
#### Motivation
* Reference object 是指 objecy 在真實世界中就代表一種東西，Value object 完全根據資料來定義
* 一開始只用簡單的 value 後來可能需要讓他可以變而且可以反應到所有 reference，這時候就要變成 reference object
#### Mechanics
* 使用 Replace Constructor with Factory Method
* 編譯跟測試
* 決定 object 該提供那些讀取方式
* 決定要先建立好還是要用帶才建立
* 把 factory method 回傳的東西換成 reference object
* 編譯跟測試
### Change Reference to Value
* Reference object 已經單純到不需要管理就換成 value object
#### Motivation
* Reference object 都需要某些控制，但如果太單純才變成 value object 比較好，可以用在分散式系統或 conccurrent 系統
* Value object 好處是 immutable，這樣可以確保很多的 object 是一樣的，如果 mutable 需要確保所有 object 都有更新到才能確保都是一樣的
#### Mechanics
* 確認 object 可以變成 immutable
    * 如果不可以使用 Remove Setting Method 直到可以
    * 如果都不行應該要放棄這個 refactoring
* 建立 equal method 跟 hash method
* 編譯跟測試
* 移除 factory method 跟讓 constructor 公開
### Replace Array with Object
* Array 中有一些 element 代表不同的意思則要把 array 換成 object，用 field 代表不同 element
#### Motivation
* Array 只適合用來放相似的東西，而有時候會用 element 在 array 的哪個位置來代表不同意思，這樣很難記住這個東西，就算有註解也會過期，最好的方式就是用 object 然後給 element 名字或者使用 Move Method 增加行為
#### Mechanics
* 建立新 class 代表 array 的資訊，有一個 public field 給 array
* 把所有用 array 的改成用 class
* 編譯跟測試
* 為每個 element 都增加 getter 跟 setter，取個適合的名字，讓 client 使用 accessor，每做一次改動都測試
* 當所有 array 的存取都被換成 method 讓 array 變成 private
* 為每個 element 都建立 field，讓 accessort 用 field
* 每個 element 變動都測試
* 當所有 element 都被換成 field 則刪除 array
### Duplicate Observed Data
* Domain data 只能從 GUI 拿到而且 domain method 需要用到該 data
* 把 data 複製到 domain object，使用 observer 同步在不同地方的資料
#### Motivation
* User interface 的 code 會跟 business logic 的 code 分開因為可能有很多 interface 用到相同的 business logic，為了容易維護所以需要從 GUI 拉出 domain object
* 行為容易分離但是資料不行，需要利用 framework 保持資料的一致性
#### Mechanics
* 做一個 presentation class 給 domain class
* 在 GUI class 的 domain data 使用 Self Encapsulate Field 
* 編譯跟測試
* 在 event handler 增加對於 setting methdo 的呼叫以便更新
* 編譯跟測試
* Domain class 定義 data 跟 accessor method
* 讓 accessort 寫到 domain field
* 修改 observer 更新的 method 能夠從 domain field 複製資料到 GUI
* 編譯跟測試
### Change Unidirectional Association to Bidirectional
* 有兩個 class 需要對方的 feature 但只有 one-way link
* 增加back pointer 跟改變 modifier 可以同時更新 sets
#### Motivation
* 一開始兩個 class 只有其中一個 class 會 refer 另外一個但隨著功能增加需要用到被 refer class 的 feature，這時後可能會用其他 route 到需要的 class 但容易造成混亂，直接設定好 two-way reference 比較簡單
* 這個 refactoring 用 back pointer 實作 bidirectionality，其他技巧還有 link object
#### Mechanics
* 為 back pointer 增加 field
* 決定哪個 class 負責控制 association
* 在不負責控制的 class 建立 helper method
* 如果在控制端有 modifier 也要更新 back pointer
* 如果被控制端有 modifier 在控制端建立 method 讓他呼叫
### Change Bidirectional Association to Unidirectional
* 有一個 two-way 關係的 class 但卻不再需要另外 class 的 feature
* 移除掉不需要的關係
#### Motivation
* 雙向的關係雖然有用但也增加複雜度
* 很多 two-way link 容易造成 zombie 的出現
* 雙向關係造成互相依賴，太過多的依賴容易造成 coupling 導致些許改動就讓很多無法預測的後果
#### Mechanics
* 檢查所有 reader 用到的 field 的 pointer 是否可以移除掉
* Client 需要 getter 則使用 Self Encapsulate Field 然後在 getter Substitue Algorithm 編譯跟測試
* Client 不需要 getter 則改變所有用到 field 的方式，編譯跟測試
* 沒有 reader 的時候就把所有 update field 的地方移除後再把 field 移除
* 編譯跟測試
### Replace Magic Number with Symbolic Constant
* 有一個代表特殊意義的數值把它用一個 constant name 取代
#### Motivation
* 有代表特殊意義的數值不容易被發現，如果有在很多地方都用到又要改變則會非常困擾因為很難找出所有需要改的地方
* 宣告 constant 沒有代價還可以帶來很大的可讀性
* 先看看 magic number 有沒有更好替代方案，如果是 type code 則應該要用 Replace Type Code with Class
#### Mechanics
* 宣告 constant 然後把 value 設為 magic number
* 找出所以 magic number
* 看 magic number 是否適合取代成 constant
* 編譯
* 當所有 magic number 都改完就編譯跟測試
### Encapsulate Field
* 把 public field 變成 private 後改用 accessors 存取
#### Motivation
* Field 應該都要是 private 才能做到 encapsulation，如果外面的人可以直接存取 object 裡面的 data 就會讓 data 脫離行為
* Data 跟行為透過 object 聚在一起方便做修改
* Encapsulate Field 只是第一步，接下來需要看有哪些行為適合 Move Method 到 class
#### Mechanics
* 建立 field 的 getting 跟 setting methods
* 找出所有 client 用到的地方改成用 getting 跟 setting method
* 每個改變都要編譯跟測試
* 當所有 client 都改變後把 field 變 private
* 編譯跟測試
### Encapsulate Collection
* Method 回傳 collection 換成回傳 read-only view 跟增加 add/remove methods
#### Motivation
* Class 如果有 collection instance 很容易用來做 getter 跟 setter
* 不該回傳 collection 因為會在不知道 class 內部實作的狀況下做改動讓 client 知道太多事情，所以應該要回傳無法改動的東西
* 不應該用 setter 給 collection 要用 add 跟 remove method 讓 object 可以控制對於 collection 的增減
* 減少與 client 的 coupling
#### Mechanics
* 增加 collection 的 add 跟 remove method
* 用 empty collection 初始化 field
* 編譯
* 找出所有使用 setting method，改成使用 add 跟 remove operation
* 編譯跟測試
* 找出所有用 getter 然後改 collection，改成使用 add 跟 remove method，每個改變都編譯跟測試
* 所有會修改 getter 的 collection 都改完後把 getter 改成只會回傳無法改的 collection
* 找出使用 getter 的低方看看可不可以用 Extract Method 跟 Move Method 方到 host object
### Replace Record with Data Class
* 需要一個 record structure 的 interface 用 dumb data object 取代 record
#### Motivation
* 在 object-oriented program 會用 record structure 因為可以用傳統的 programming API 來操作，這時候用一個 interface 處理 external element 之後還可以把 methodd 搬進去
#### Mechanics
* 建立一個 class 代表 record
* 為每個 data item 建立 private field, getting method 跟 setting method
### Replace Type Code with Class
* Class 有數字 type code 但不影響行為，使用新 class 取代 type code
#### Motivation
* Numeric type code 雖然有 symbolic name 但是本質上還是數字 compiler 無法檢查，使用的時候都用數字所以造成可讀性降低
* 使用 class 取代數字可以讓 complier 進行 type check，提供 factory method 可以確保產生正確的 instance
* 只有在 type code 是單純的資料且不會有不同的行為才能使用，如果有其他狀況要考慮使用不同的 refactoring 方式
* 雖然 type code 的值不影響行為但把行為放到 type code class 是更適合的
#### Mechanics
* 建立 type code 的新 class
* 修改 source class 的實作改用新 class
* 編譯跟測試
* Source class 每個使用 type code 的 method 要在新 class 建立新 method 來取代原本的
* 把用到 source class 的 client 慢慢換成使用新的 interface
* 每個 client 改變後都要編譯跟測試
* 移除舊的 interface 跟 static declaration
* 編譯跟測試
### Replace Type Code with Subclasses
* Class 中有會影響行為的 type code
* 使用 subclasses 取代 type code
#### Motivation
* Type code 會影響行為則需要使用 polymorphism 去處理不同的行為
* 使用一堆 if-then-else 架構根據 type code 來決定行為，這種情形通常需要使用 Replace Conditional with Polymorphism，所以需要把 type code 替換成 inheritance structure 來達成 polymorphic 行為
* 為每個 type code 建立 class，但如果 type code 會改變跟已經是 subclass 就需要用 Replace Type Code with State/Strategy
* Repalce Type Code with Subclasses 主要是為了 Replace Conditional with Polymorphism，如果沒有 conditional statement 使用 Replace Type Code with Class 比較好
* 使用 Replace Type Code with Subclasses 為了讓 class 本身更貼近自己的 feature，之後可以使用 Push Down Method 跟 Push Down Field 來讓 feature 只存在於特定 class
* Replace Type Code with Subclasses 把不同行為的知識移到專屬的 class，如果有新的種類可以直接新增 subclass 就好而不需要去找每個 conditional 去改
#### Mechanics
* 把 type code encapsulate 起來，如果 type code 是由外面的傳進來需要建立 factory method 取代 contructor
* 為每個 type code 建立 subclass，override type code 的 getting method 回傳對應的 type code
* 每換一個 subclass 就編譯跟測試
* 移除 type code field，在 abstract 宣告 type code 的 accessor
* 編譯跟測試
### Replace Type Code with State/Strategy
* Class 使用到 type code 會影響行為但無法用 subclass 取代
* 使用 state object 取代 type code
#### Motivation
* 類似 Replace Type Code with Subclasses 使用在 type code 會改變或無法 subclassing 的情況
* State 跟 strategy 很像所以 refactoring 方式一樣，如果想要用 Replace Conditional with Polymophism 簡化一個 algoirthm 使用 strategy 比較好，如果 data 是 state-specific 可以使用 state pattern
#### Mechanics
* 把 type code encapsulate 起來
* 建立以 type code 為目的命名的新 class，是個 state object
* 為每個 state object 的 type code 增加 subclasses
* 在 state object 建立 abstract query 回傳 type code，在 subclass overriding 去回傳自己的 type code
* 編譯
* 舊的 class 上面建立為了 state object 的 field
* 調整 type code query 讓原本 class delegate 到 state object
* 調整 type code seeting method 讓原本 class assign 給適當的 state object
* 編譯跟測試
### Replace Subclass with Fields
* Subclasses 只有 method 回傳的 constant data 不同，把 method 轉成 superclass fields 然後移除 subclasses
#### Motivation
* Constant method 指回傳 hard-coded value，在 subclasses 讓他可以傳不同的 accessor 出來只要在 superclass 定好 accessor 就好了
* 如果 subclasses 只有 constant method 則該 class 的存在價值就很薄弱
#### Mechanics
* Subclass 上使用 Replace Constructor with Factory Method
* Code 有 refere 到 subclasses 的用 superclass 取代
* Superclass 為每個 constant method 定義 final field
* 宣告 superclass constructor 用來初始化 fields
* 增加或修改 subclass constructor 去呼叫 superclass constructor
* 編譯跟測試
* Superclass 實作每個 constant method 回傳 field 然嘔把 subclasses 的 method 移除
* 每一鋤一個就編譯跟測試
* 所有 subclass method 都被移除後使用 Inline Method 把 constructor 變成 factory method
* 編譯跟測試
* 移除所有 subclass
* 編譯跟測試
* 持續 inline constructor 跟刪除 subclass
## Chapter 9. Simplifying Conditional Expressions
* Conditional logic 容易讓事情變複雜，主要 refactoring 的方式是 Decompose Conditional 讓 switch logic 跟事情發生的細節分離
* Consolidate Conditional Expression 可以在遇到很多測試但都有相同效果，Consolidate Duplicate Conditional Fragment 可以移除 conditional code 中有 duplicate 的部分
* 對於使用 control flag 來達成 one exit point 的 code 使用 Replace Nested Conditional with Guard Clauses 讓特別的 conditional case 變得清楚，Remove Control Flag 移除各種 control flag
* Object-oriented 的程式比較少 conditional logic 因為可以用 polymorphism 來處理這件事，Polymorphism 讓 caller 不需要知道 conditional behavior 而且容一擴充，所以通常使用 Replace Conditional with Polymorphism
* Introduce Null Ojbect 來移除確認 null value 的 code
### Decompose Conditional
* 有複雜的 conditional statement 用從 condition 到 parts 使用 extract method
* 通常程式複雜的地方來自於複雜的 conditional logic，不僅會增加 method 長度也會使目的模糊讓程式難以閱讀
* 遇到很大 block 的 code 藉由使用有意義的 method 分解後讓目的凸顯出來
* 把 condition extract 成 method 再把其他部分都 extract 成 methods
* 如果是 nested logict 使用 Replace Nested Conditional with Guard Clauses
* Condition 雖然很短但很難表示出目的，所以也要 extract condition 讓人看到清楚的目的
### Consolidate Conditional Expression
* 很多 conditional tests 都是回傳一樣的結果，把他們都放到同一個 conditional expression 然後 extract
* 一連串的測試其實是組合在一起的所以要放在一起，而且放在一起就可以使用 Extract Method 把 conditional extract 出來讓 code 更清晰
* 如果這些測試都是獨立的就不需要做這個 refactoring
* 檢查所有的 conditional 都沒有 side effect
* 使用一個 conditional statement 取代
* 編譯跟測試
* 嘗試使用 Extract Method 把 condition extract 出來
### Consolidate Duplicate Conditional Fragments
* 所有的 branch 都有相似的一段 code，把這段 code 移出 fragment
* 讓人比較清楚這些 condition 有什麼差別
* 先確認那段 code 在 condition 之內跟之外執行都一樣
* 如果 code 在開頭則移到 condition 之前
* 如果 ode 在最後則移到 condition 之後
* 如果 code 在中間確認前後都沒有改變任何東西就去移到 condition 前面或後面都可以
* 如果不只有一個 statement 可以 extract 成 method
### Remove Control Flag
* 當有變數在一連串的 boolean expression 都像是 control flag，使用 break 或 return
* 為了達到 one entry point 跟 one exit point 會需要用到 control flag，但是不一定要遵守 one exit point，因為使用 flag 會讓 code 不清楚，使用 continue, break, return 可以讓真正的 condition 更清晰
* 先找到 control flag，把 assignment break-out value 的地方換成 break 或 continue，每個取代都編譯跟測試
* Extract log 成 method，找出 control flag 會跳出的 value，把 assignment break-out value 用 return 取代，每個取代都編譯跟測試
### Replace Nested Conditional with Guard Clauses
* Condition 讓執行的路很不清晰，使用 guard clauses 取代所有特殊的 cases
* 想要區別正常跟異常情況應該使用 if 跟 else，想要區別常見跟少見情況要讓 check condition return True，這類 check 稱為 guard clause
* If-then-else 這三種通常會被認為都是同樣重要的，因此應該使用 guard clause 要讓人知道這是處理少見或異常情況
* one exit point 不是很好的規則，應該要以簡潔為主
* 把每個 check 都放到 guard clause，每次替換都編譯跟測試
### Replace Conditional with Polymorphism
* Condition 根據 object type 來決定行為，把每個 condition 的分支都放到 subclass 的 overriding method，讓原本的 method 變成 abstract
* Polymorphism 讓 condition 指出現在一個地方，當需要新的 type 只要加 subclasses 就好
* 需要有 inheritance 架構，如果沒有使用 Replace Type Code with Subclasses 跟 Replace Type Code with State/Strategy
* 如果 condition 在一個大的 method 先用 Extract Method 把 conidtion 分離出來，需要的畫使用 Move Method 讓 condition 放在 inheritance 的上層。選一個 subclass 去 override conditional statement method，複製 condition 的分支到 subclass method，編譯跟測試後，移除複製過來的部分，在編譯跟測試，condition 分支都不斷做這個動作直到所有到放到 subclass method，讓 superclass method 變 abstract
### Introduce Null Object
* 不斷重複檢查 null value，使用 null object 取代 null value
* Polymorphism 本質在於去直接使用行為而不是先確認什麼型態再去做什麼事情
* 建立 null 版本的 subclass，在 source class 建立 isNull operation 回傳 False 而 subcalss 回傳 True，編譯，找出所有可能產生 null 的地方替換成 null object，找出所有比較 null 的地方替換成 isNull，編譯跟測試，找出 client 使用 null operation 替換行為，移除 condition check，編譯跟測試
### Introduce Assertion
* 有段 code 有預設特定的狀態，使用 assertion 讓假設顯示出來
* 有些假設是用註解說明，可以轉換成 assertion
* Assertion 幫助讓問題更接近源頭
* Assertion 只用在檢查東西是否一定要是對的，不要過度使用不然會造成 duplication，如果 assertion fail 但 code 還是可以正常運作就把 assertion 拿掉
## Chapter 10. Making Method Calls Simpler
* 如何讓 Object 開出容易使用跟理解的 interface 是關鍵
* 最簡單的方法就是 rename method 把理解到的知識放到 method name
* Interface 的 parameter 越少越好，可以使用不同方式的 refactoring 減少 parameter 的數量
* Method 同時修改 object 狀態跟查詢狀態需要分開
### Rename Method
* 從 method 的名字看不出來要做什麼，換個可以看出來要做什麼的名字
* 使用小的 method 但沒有取好名字會讓人需要不斷跳來跳去看 method 到底在做什麼，使用能夠清楚表達意圖的 method 就能跟讀者建立良好的溝通管道
* 取好的名字需要經過時間練習，不要輕易放棄取個好名字，因為好的名字可以讓人更省力，不斷練習到可以取好的名字才有資格成為好的 programmer
* 檢查 method signature 是否有被 superclass 或 subclass 使用，如果有先宣購新的名字的 method 把舊的 method 內容抄過來，編譯，讓舊的 method 呼叫新的 method，編譯跟測試，找出所有用舊的 method 的地方改成新的，每次都編譯跟測試，移除舊的 method，編譯跟測試
### Add Parameter
* Method 需要 caller 的更多資訊，多加一個 parameter
* 沒有其他的方式才用這個因為太長的 parameter list 不容易記得跟常常有 data clumps
* 檢查 method signature 是否有被 superclass 或 subclass 用到， 如果有則宣告一個新的 method 加上 parameter，複製舊的 method 的內容到這邊，編譯，把舊的 method 內容換成呼叫新的 method，編譯跟測試，找出所有用到舊 method 的地方換成新的，每次變動都編譯跟測試，移除舊的 method
### Remove Parameter
* Method body 沒用到 parameter 則移除
* Parameter 代表該 method 需要用到的資訊，如果留下沒用的則 caller 需要多花時間思考要傳什麼進去
### Separate Query from Modifier
* Method 同時回傳東西跟改變 object 狀態，使用兩個 method 分別處理
* function 只回傳值而沒有其他 side effect 則使用的人可以放心的經常呼叫
* Method 應該要區分為有 side effect 跟沒有 side effect，只要回傳值的 method 就不該有 side effect
* Side effect 是指可以明顯察覺到的，所以使用 cache 優化查詢速度是不在此類
* 建立 query 回傳跟原本 method 的值一樣，修改原本的 method 使用 query method 回傳值，編譯跟測試，把呼叫原本的 method 換成 query method，然後新增呼叫原本的 method，每次都編譯跟測試，讓原本的 method 不回傳東西
### Parameterize Method
* 很多 method 都做類似的事情差別只在於 method 裡面的值不同，使用一個 method 可以讓 parameter 可以有不同值
* 減少 duplicate code 然後增加彈性，之後增加不同的值也不用新增 method
* 建立可以替換掉重複 method 的 parameterized method，編譯，一個一個替換 method 然後編譯跟測試
### Replace Parameter with Explicit Methods
* Method 使用不同的 parameter value 來決定回傳什麼值，建立讓每個 parameter 不同的 method
* 避免有 conditional 還可以再編譯的時候發現錯誤，使用者也不需要知道該傳什麼東西進去，而且介面也比較乾淨
* 當 paramter 很常變動就不適合使用
* 為每個 parameter value 建立 explicit method，每個 condition 裡面呼叫新的 method，每次替換掉一個 condition 都編譯跟測試，把呼叫 conditional method 替換成新的 method，編譯跟測試，替換掉所有後移除 conditional method
### Preserve Whole Object
* 先從 object 得到很多值之後再傳進 method 裡面，直接傳整個 object 就好
* 避免 long parameter list 的問題，讓 parameter list 容易修改
* 缺點是會造成兩個 object 的 coupling，如果會讓 dependency structure 變得混亂則不應該使用
* 當 method 使用了很多別的 object 的東西可以思考使用 Move Method
* 建立新的 parameter 給整個 object 傳進去，編譯跟測試，確定要從 object 拿什麼 parameter，依次拿 parameter 取代本來 method 用到的地方，刪除 paramter，編譯跟測試，不斷重複直到都從 object 拿 parameter，刪除沒用到的 temp
### Replace Parameter with Method
* Object 把 method 的結果當成 parameter 傳進 method 但是其實可以自己使用 method 拿到結果，移除掉 paramter 讓 method 自己呼叫得到結果
* 盡量減少 paramter，讓 method 不使用 parameter 就可以移除，所以只要能夠自己呼叫拿到需要的東西就可以移除掉 parameter
* 有些 parameter 是為了之後 method 的參數化，除非會影響整體架構不然都先不要這樣用
* 把使用 parameter 計算的部分放到 method，把使用 parameter 的地方換成 method，每次替換都編譯跟測試，移除 parameter
### Introduce Parameter Object
* 有一堆適合再一起的參數，把他們變成 object
* 常常有一群 parameter 一起傳進 method 就是有 data clump 的味道把它用 object 取代掉，不僅減少 parameter list 也讓 code 更一致更容易修改
* 有了新 object 就可以把行為移進去，可以減少 duplicate code
* 建立新的 class 準備取代那些 parameter，編譯，增加一個 paramter 到 data clump，依次把每個 parameter 移除掉改用 object，每次都編譯跟測試，最後看看是否可以把行為移到新的 object
### Remove Setting Method
* Field 如果要在建立的時候就設定好然後就不能改變，就需要移除掉所有 setting method
* 提供可以設定 field method 會讓人覺得這是可以改變的，所以不能有 setting method 另外把 field 設為 final 可以清楚表達意圖
* 檢查 setting method 只有被 constructor 使用，讓 constructor 直接存取 variables，編譯跟測試，移除 setting method 跟把 field 設 final，編譯
### Hide Method
* Method 沒有被其他 class 用到則把它變成 private
* Refactoring 過程常常需要調整 method 的可見度，但不容易察覺到 method 沒有 class 使用需要將他藏起來，理想上是有工具可以找出來沒有的話需要自己定期檢查
* 常見情況是當 class 有越多行為則 getting 跟 setting method 就漸漸不會被用到，這時候就可以藏起來
* 定期檢查 method 是否有機會變成 private，讓 method 變 private，改一堆後就編譯
### Replace Constructor with Factory Method
* 建立 object 想要有更多的操作則使用 factory method
* Replace type code with subclassing 常常用到，建立 object 都是使用 type code 但現在需要 subclasses，所以要用 factory method 取代 constructor
* 讓人知道根據不同的 parameter 會有不同建立的方法
* 建立 factory method 讓裡面直接呼叫現有的 constructor，替換掉呼叫 constructor 的地方，每次替換都編譯跟測試，把 constructor 宣告成 private，編譯
### Encapsulate Downcast
* Method 回傳的東西需要讓 caller 自己 downcast，讓 method 裡面自己做 downcast
* Strongly typed OO language 需要 downcast 告訴 compiler，雖然需要做 downcast 但最好越少越好，所以 method 自己做 downcast 比起讓 caller 自己 downcast 好
* 找出需要 downcast 的地方，把那些地方移進 method
### Replace Error Code with Exception
* Method 使用特殊的 code 代表錯誤要使用 exception 取代
* 程式發生錯誤就需要做處理，發生錯誤的地方可能不知道該如何處理就讓呼叫的人知道去處理
* Expcetion 可以跟一般流程做區隔作為錯誤處理的流程，讓程式容易理解
* 決定那些 exception 需要被檢查，找出所有呼叫的人讓他們使用 exception，改變 method signature 讓人知道新用法
### Replace Exception with Test
* 丟出一個 checked exception 而 caller 可以先檢查，讓 caller 自己先檢查
* Exception 應該要是例外的處理而不是 conditional test 的替代品
* 把測試放在前面然後複製 catch block 的內容，catch blcock 加上 assertion 讓自己知道 catch block 有沒有被執行到，編譯跟測試，移除 catch block 跟 try block，編譯跟測試
## Chapter 11. Dealing with Generalization
* 要做到 Generalization 會需要一堆 refactoring，主要是把 method 移動到不同的 hierarchy
    * Pull Up Field 跟 Pull Up Method 把 function 提高 hierarchy
    * Push Down Method 跟 Push Down Field 把 function 降低 hierarchy
    * Pull Up Contrstor Body 特別對 contructor 做 pull up
    * Replace Constructor with Factory Method 用來取代把 constructor push down 的情況
* Method 只有一點不太一樣使用 Form Template Method 把不一樣的地方分開
* 除了把 function 在不同 hierarchy 移動還可以改變 hierarchy
    * Extract Subclass, Extract Superclass, Extract Interface 都是創造新的 hierarchy
    * Collapse Hierarchy 把不需要的 call 從 hierarchy 移除
* 有時候發現 inheritance 不是最適合的方法就使用 Replcae Inheritance with Delegation 或者相反 Replace Delegation with Inheritance
### Pull Up Field
* 兩個 subclasses 有相同的 field 則把 field 放到 superclass
* Subclass 可能分別開發所以會有 duplicate features，field 被使用的方式很類似則可以 generalize field
* 找出所有可能可以移到 superclass 的 field 確認被使用的方式一樣，field name 不一樣需要先 rename 成一樣，編譯跟測試，superclass 建立新的 field，刪除 sublass fields，編譯跟測試，嘗試使用 Self Encapsulate Field
### Pull Up Method
* Subclasses 有相同的 method 回傳相同的結果應該移到 superclass
* 移除 duplicate 是重要的因為雖然現在運作正常但未來可能只會對其中的一個作修正而忽略其他造成問題產生
* 通常在經過其他步驟才有機會 Pull Up Method
* 檢查 method 是一樣的，如果不同 signature 要先改成一樣，在 superclass 建立 method 然後複製 method body 到裡面後編譯，刪除其中一個 subclass method，編譯跟測試，繼續刪除剩下的 subclass method 直到只剩下 superclass method
### Pull Up Constructor Body
* Subclass 的 constructor 都很相似，在 superclass 建立 constructor 讓 subclass 使用來 construct
* Constructor 無法 extract 相同的地方後 pull up 到 superclass，所以需要在 superclass 新增 constructor
* 定義 superclass constructor，把相同的 construtor code 從 subclass 搬到 superclass，subclass construtor 呼叫 superclass construtor，編譯跟測試
### Push Down Method
* Superclass 的行為只跟一些 subclass 有關，把那些行為放到 subclass
* 因為那些行為只對 subclass 有意義，通常在 Extract Subclass 使用
* 在所有 subclass 宣告 method 然後複製 body 到每個 subclass，移除 superclass 的 method，編譯跟測試，移除不需要該 subclass 的 method，編譯跟測試
### Push Down Field
* Field 只被某些 subclass 使用，把 field 放到那些 subclass
* 在所有 subclasses 宣告 field，移除 superclass 的 field，編譯跟測試，移除 subclass 不需要的 field，編譯跟測試
### Extract Subclass
* Class 的 feature 只在某些 instance 才有使用，把這些 feature 建立 subclass 
* 沒有 type code 無法使用 Replace Type Code with Subclasses 就可以用 Extract Subclass
* Extract Class 是另一種方法取決於想要用 delegation 還是 inheritance，Extract Subclass 限制在於當 object 產生後就無法改變 class-based 行為，但 Extract Class 可以藉由 plugin 來改變 class-based 行為
* 定義新的 subclass，加上 constructor，找出所有呼叫 superclass 的 constructor 如果需要用到 subclass 就換成新的 constructor，使用 Push Down Method 跟 Push Down Field 把 feature 放到 subclass，找出 field 提供 hierarchy 資訊使用 Self Encapsulate Field 消除，使用 Replace Conditional with Polymorphism 來 refactored，每個 push down 都做編譯跟測試
### Extract Superclass
* 兩個 class 有相似的 feature 則建立 superclass 然後把共同的 feature 移進 superclass
* 兩個 class 做相似的事情就很有可能有 duplicate code，使用 inheritance 移除 duplicate code
* 另一種替代方案是 Extract Class 就看想要用 inheritance 或者 delegation 解決，就算選錯也可以用 Replace Inheritance with Delegation
* 建立一個空的 superclass 讓原本的 class 變成 subclass，一個接著一個使用 Pull Up Field, Pull Up Method, Pull Up Constructor Body 把共同部分移到 superclass 每次都需要編譯跟測試，檢查 subclass 剩下的 method 如果還有共同的部分先用 Extract Method 再用 Pull Up Method 那些部分移到 superclass
### Extract Interface
* 一些 client 都用 class subset 的 interface，把 subset 變成 interface
* 只用 class 的一部分將這一部份弄成可以代表自己責任的東西讓人比較清楚該如何使用
* OO language 通常提供繼承來支援也有 interface 用 single inheritance 就可以辦到
* Extract Interface 只提供共同的 interface 而不能消除 duplicate code，想要消除 duplicate 還需要用到 Extract Class
* Extract Interface 適合使用在 class 在不同情況有不同的角色
* 建立空的 interface，宣告共同的 operation，宣告 class 實作 interface，調整 client type 可以使用 interface
### Collapse Hierarchy
* Superclass 跟 subcalss 沒有太大的差異則合併成一個
* Refactoring hierarcchy 的時候移動 method 或 field 可能最後發現 subclass 並沒有增加任何價值就可以合併
* 選擇需要被移除的 class，移動所有 method 跟 field 到要被合併的 class，每次移動都編譯跟測試，調整 reference 到要被移除的 class，移除空的 class，編譯跟測試
### Form Template Method
* 兩個 method 在不同 subclass 使用類似的步驟，但步驟有點不同，讓每個步驟都有相同的 signature 才可以讓原本的 method 一樣再去 pull up
* Inheritance 是很有用的工具幫忙消除 duplicate behavior 但如果沒有完全一樣需要把相似的地方保留後只剩下不一樣的地方
* 把類似的步驟放到 superclass 而 subclass 實作步驟細節
* 分離 method 讓分離出來的 method 都一樣，使用 Pull Up Method 把一樣的 method 放到 superclass，不同的 method 使用 Rename Method 讓所有步驟一樣，每次改變 signature 都要編譯跟測試，把原本的 method 用 Pull Up Method 定義不同的 abstract method 在 superclass，編譯跟測試，移除其他 method，每次都要編譯跟測試
### Replace Inheritance with Delegation
* Subclass 只有用 superclass 一部分的 interface 或者不想繼承 data，在 superclass 建立 field 然後調整 method delegate to superclass 然後移除 subclassing
* 繼承後發現有很多 superclass 的 method 都不適用或繼承一堆資料不適用於 subclass
* 雖然可以繼續使用宣稱只有 superclass 的一部分但是會讓 code 變得混淆
* 使用 delegation 讓 code 清楚顯示只用一部份的 class，只需要考慮一部分的 interface，而且很簡單可以做出 delegating method
* 在 subclass 建立 field 指向 superclass，改變每個在 subclass 的 method 去用 delegate field，每次改變都要編譯跟測試，移除宣告 subclass 換成 delegate，每個被 client 用到的 superclass method 都新增 delegating method，編譯跟測試
### Replace Delegation with Inheritance
* 用太多 delegation 讓 interface 充滿很多簡單的 delegation，讓 delegating class 變成 delegat 的 subclass
* 如果沒有用 delegating 的 class 的所有 method 則不該使用 Replace Delegation with Inheritance，因為 subclass 應該要跟 superclass 的 interface，如果不是可以改用 Remove Middle Man 讓 client 直接用或者用 Extract Superclass 把相同的 interface 分離出來
* 如果 delegate 被多個 object 共用到而且 mutable 則不該用 delegat 替換 inheritance 因為就不能使用共用的 data，如果 object 是 immutable 則 data sharing 就不是問題可以直接複製
* 讓 delegating object 是 delegat 的 subclass，編譯，設好 delegate field，移除簡單的 simple delegation methods，編譯跟測試，替換所有其他 delegation，移除 delegate field
## Chapter 12. Big Refactorings
* 之前章節都是關於 refactoring 的步驟但沒有說明如何從全局透過 refactoring 達到特定目的
* The Nature of the Game
    * 因為情況各有不同所以 refactoring 步驟都限定在特定的範圍內才能夠說明步驟
    * 六到十一章節的 refactoring 都可以在一小時內完成但有些需要幾個月還是幾年才有辦法完成
    * 因為大型的 refactoring 短期看不到效果要稍微相信有用才能持續下去
    * 大型 refactoring 需要團隊一起確認目標確保有朝同一個目標前進
* Why Big Refactorings Are Important
    * 雖然大型 refactor 無法立即產生價值但如果缺少這部分就會讓之前學習的 refactoring 無法發揮該有的效益
    * 沒有充分理解所下的設計決定會限制程式的發展，透過 refactoring 確保程式有反映出設計
* Four Big Refactorings
    * 有四種大型 refactoring 的範例
    * Tease Apart Inheritance 處理複雜的 inheritance hierarchy
    * Convert Procedural Design to Objects 處理用 object-oriented language 但還是使用 procedural code
    * Separate Domain from Presentation 處理使用 user interface 跟 databases 的設計但想要把 business logic 跟 user interface code 分開
    * Extract Hierarchy 簡化太過負責的 class 讓他轉變成多個 subclasses
### Tease Apart Inheritance
* 用一個 inheritace hierarchy 一次做兩件事情，建立兩個 hierarchies 跟使用 delegation 使用對方
* Inheritance 讓一個 method 可以用少少的 code 建立重要的位置
* 很容易不小心用錯 inheritance，例如不斷的加上 subclass 做一點工作最後會讓 code 很散亂
* 複雜的 inheritance 讓 code 會 duplication 所以很難改動，因為解決特定問題的 code 散落在各處
* 如果同一層級的 subclass 都有類似的形容詞則應該就是用一個 inheritace 處理兩件事
* 確認 hierarchy 處理不同事情就建立 two-dimensinal grid 然後標註工作類型，決定哪件事比較重要需要保持在目前的 hierarchy， superclass 使用 Extract Class 去建立 object 跟新增 instance variable 來使用這個 object，為 extracted object 建立 subclass，用 instance variable 初始化，Subclasses 使用 Move Method 把行為一道 extracted object，移除沒有 code 的 subclass，持續做到 subclasses 都消失
### Convert Procedural Design to Objects
* Code 用 procedural 的方式寫，把 data record 變成 object 分離行為放到 object
* Class 有一個 long method 只有一點點資料跟 dumb data object 就是典型的 procedural method
* 把每個 record type 變成 dumb data object 有著 accessors，把所有 procedural code 放到一個 class，使用 Extract Method 在 long procedure 然後使用 Move Method 一道適合的 dumb data class，持續在原本的 class 移除行為最後刪除
### Separate Domain from Presentation
* GUI classes 包含 domain logic 應該需要分離 domain logic 到不同的 domain classes
* Model-view-controller 主要分離於 user interface code 跟 domain logic，presentation classes 只包含處理 user interface 的部分而 domain object 只包含 business logic，之後修改比較容易也讓多個 presentaion 可以使用相同 business logic
* 另一種設計方式是把 data 放到 database 而 logic 放到 presentation class，通常是環境所驅使的設計方式這會讓 logic 散落在各處
### Extract Hierarchy
* Class 裡面有部分有很多 conditional statement，使用 hierarchy 讓 subclass 個別處理特殊情況
* 一開始的 class 都以為只是處理一件事隨著時間越來越多處理的事情越來越多就變成一團混亂，當 conidtional logic 是 static 才能使用否則先用 Extract Class，可能需要花很久的時間
* 確定 variation，建立 special case 的 subclass 然後使用 Replace Constructor with Factory Method，複製在 conditional logic 的 method 到 subclass，持續把 special mcase 獨立的 subclass 直到 superclass 可以是 abstract，刪除 sperclass 已經被 overridden 的 method
* 為每個 variation 建立 subclass，使用 Replace Constructor with Factory Method 到合適的 subclass，有 conditional logic 的 method 使用 Replace Conditional with Polymorphism
## Chapter 13. Refactoring, Reuse, and Reality
### A Reality Check
* 系統的挑戰來自於改變而不出初期的開發，讓改動簡單才是能為公司帶來好處
* 自己在意的事情不被別人重視是自己沒有好好闡述自己的理念
### Why Are Developers Reluctant to Refactor Their Programs?
* 通常需要面對的是要在現有系統上增加功能，在不夠理解要做的事情加上時間壓力沒有太多選擇
* 使用自己的設計經驗來重寫系統但無法保證現有功能可以持續運作
* 單純複製修改現有系統的 code 就可以在不理解的情況下做到 reuse但會造成問題蔓延最後讓改動成本越來越高
* Refactoring 介在兩者之中，可以讓 design 更貼近現況、合理的 reuse code、釐清程式架構、讓新增 code 變得容易
#### Understanding How and Where to Refactor
* 有工具可以自動檢查程式裡架構脆弱的部分，例如 function 有太多 argument 或太長，兩個 function 相似部份太多
* 判斷工具給予的建議是否可以讓程式變得讓以後的改動更容易
#### Refactoring to Achieve Near-term Benefits
* 短期好處在於因為只能在一個地方修改如果讓其他人壞掉就可以盡早發現
* 中期可以藉由 abstraction 設計出之後的架構
#### Reducing the Overhead of Refactoring
* 有工具跟技巧可以讓 refactoring 變得快速又簡單
* Refactoring 可以幫助減少之後需要的成本
#### Refactoring Safely
* 安全是指程式不會壞掉然後行為跟 refactoring 之前一樣
* 如何安全 refactor 有幾種選擇，相信 coding 能力、相信 compiler 能夠抓到錯誤、相信 test 抓到 compiler 漏掉的錯誤、相信 code review 抓到 compiler 跟 test 都漏掉的錯誤
### A Reality Check (Revisited)
* Refactored code 是不同 programmer 所擁有的，但如果有測計好應該是可以 decoupled 讓 refactoring 只影響小部分
* 有多個版本則需要確認那些會受到影響
### Resources and References for Refactoring
### Implications Regarding Software Reuse and Technology Transfer
* 不知道怎麼 reuse
* 短期沒幫助就不 reuse
* 還要花時間找怎麼 reuse
* 有其他事情要忙
* 過早採用新技術容易失敗
### A Final Note
* 盡量使用 refactoring 在工作中讓開發更順利
## Chapter 14. Refactoring Tools
### Refactoring with a Tool
* 用手動 refactoring 很花時間就會讓人不想做，需要工具來降低 refactoring 的成本提高意願
* Tool 幫助 refactoring 融入日常開發流程
* Extract Method 需要考量很多事情但工具可以幫忙確認瑣碎的事情很快速的做完 refactoring
* 當 refactoring 變輕鬆則做出設計錯誤的成本也降低，設計的時候只需要考量該如何解決問題跟保留彈性就可以了而不需要預測未來的可能性
* 有自動化的 refactoring 就不需要跑那麼多次的測試，但還是有無法自動化的 refacotring 所以還是需要測試
### Technical Criteria for a Refactoring Tool
* Refactoring tool 讓我們做 refactor 不需要重新測試就可以加速過程
#### Program Database
* 要能夠找出 program entities 之前的關聯性，語言環境要能提供搜尋的功能，只要有改動就更新資料讓搜尋可以找到最新的關聯
* Semantic analysis 可以分析各種元素的關聯性
#### Parse Trees
* Parse trees 是一種資料結構代表內部 method
* 改變後藉由 parse trees 找出需要跟著變動的地方
#### Accuracy
* Refactoring 要盡量保持程式的行為但無法完全的保持，可能會快一點或慢一點
* 雖然 tool 無法百分百保證行為不改變但還是可以手動修正
