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
