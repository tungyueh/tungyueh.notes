# Code Complete Note

## Part I: Laying the Foundation
### Chapter 2: Metaphor of Software Development

很多偉大的發現是由聯想到不同事物所獲得的啟發，一個好的譬喻可以精準描述所要表達的意思，藉由譬喻也可以使人對於事物有更快速的理解，因為藉由已知的來連結未知的是人類所擅長的，工程師可以將問題已譬喻來描述他藉此來統合遇到的不同問題，之後遇到新的問題可以快速套用之前思考過的譬喻找出解決方式，而不是為這個問題設計出演算法後遇到新問題又繼續不斷重複流程而無法累積經驗，一個好的譬喻必定是可以從很多方面來對應也可以完美說明，像是一開始有人將寫程式比喻為寫信，稍微考慮一下就開始撰寫，寫好之後就放入信封寄送出不再變動，但寫程式是多人合作的項目而不是自己一個人的單打獨鬥，寫程式也不是有一個明顯的結束必須不斷維護跟修改，又有人將寫程式比喻像種田，必須不斷維護跟規劃，但寫程式不像種田一樣只要把種子放下去定期灌溉就可以等待豐收，於是乎人類將寫程式比喻成牡蠣生產珍珠的過程，一層一層的疊上去蛋白質以形成珍珠，最後最常比喻寫程式就像蓋房子一樣，有一開始的規劃，蓋什麼樣的房子，用什麼建材之類的，總之一個好的譬喻可以藉由多面向來驗證，善用好的譬喻可以事半功倍。

### Chapter 3: Measure Twice, Cut Once: Upstream Prerequisites

軟體開發流程由檢視需求開始，檢視完需求後根據需求打造架構，架構建立好之後開始設計，設計架構好就可以開始寫程式了。越前面階段發現缺陷所改動的成本可以越少，因此在打造軟體之前的準備工作是很重要的，如果再打造軟體途中才發現有缺陷可能會需要拋棄某些成果讓之前的工作浪費掉。身為軟體工程師需要教育老闆跟客戶我們打造軟體的流程跟解釋為什麼要花這麼多時間來準備而不是寫程式跟除錯，不只可以讓軟體界生態變更好也可以使接下來的工作更輕易的完成。
     首先要做的是釐清這個軟體需要解決的問題，需要把問題完整的描述清楚而不是直接提出可能的解法，以使用者的角度來描述問題而不是用工程師的思維來解決，有些問題可能是有更簡單的解決方法而不需要使用軟體來解決，
     問題列出後開始思考需求是什麼，列好需求不只可以幫助使用者更加了解自己需要解決的問題也可以幫助工程是在寫程式的過程中參考，不然工程師很容易在一邊寫程式的時候自己想需求造成爭吵，客戶如果想到有某些需求想要加入過改變必須擁有一套流程，幫助客戶冷靜思考跟提供改變所需要的成本以供客戶考量需求的重要性。雖然需求重要但還是要考量到需求是否真正為客戶帶來價值，不然單純的列出需求後沒有評估商業價值會讓許多需求都是白費工夫。
     需求決定後就可以開始來設計架構了，一個好的架構會將系統分成好幾個獨立的模組，每個模組都已定義好的功能跟輸出輸入，一個好的架構是簡單有條理的不能連工程師的覺得不合邏輯，一個好的架構可以針對每個需求都有一個解決方法，設計架構時不一定要將每一個模組都設計好這要能涵蓋 80% 的需求就可以了，

### Chapter 4:  Key Construction Decisions

每種語言都有不同的特性，根據需求來決定開發用的語言，組合語言的強項在提升效能而高階語言提供較多功能讓程式設計師使用，遇到問題時候要先自己思考該如何解決然後再思考如何使用該語言來實作，不然先想語言有提供哪些工具然後用這些工具來找出答案會容易讓人被語言所綁住，程式設計師應該是要使用語言而不是被語言所箝制住思考能力。

## Part II: Creating High-Quality Code
### Chapter 5: Design in Construction

Design 是個很困難的問題，不知道何時才算完成也不知道是否有完整思考過整個設計，所以 design 是一種推敲的過程，不斷重複思考來得出最好的方法。Design 最重要的是控制程式的複雜度，因為人類的腦袋不可能一次思考很多問題，只能專注在某一小部分上面，因為把複雜度降低讓人類得以專注於想要解決的問題上面，為了知道是否為好的設計可以從幾中指標來觀察
1. 最小化複雜度讓問題變得簡單而不是用很特殊的方法來解決，盡量保持所有的東西簡單且易懂
2. 讓程式易於維護，可以想像有人會不斷問你為什麼要這樣寫程式，這樣自然會把程式寫的好懂易於維護
3. 程式中的不同部分都盡量不相關，這樣改動某一部分的東西時候就可以不用考慮太多事情
4. 易於擴張
5. 可以重複利用程式碼
6. 一個 class 可以被很多人用到
7. 一個 class 用很少其他 class。 

Design 的流程為先將整體看過一遍大概要怎麼規劃，分成 sub system 後，在分成class，class 裡面會有 routines，對於每個部份要定義好功能，提供那些方法給其他人用，將那些資訊隱藏起來，若有類似功能可以用繼承的方法重複利用程式碼，對於可能變動的部分把這些隱藏起來，不要將實作寫在每個 class 裡面，不然之後做改動需要將每個 class 修改。可以利用 Design Pattern 來與其他人溝通減少說明設計的細節，也可以利用 design pattern 來作些許修改用來解決問題。Design 是不斷重複以上流程的行為，中途雖然會碰到錯誤的想法但可以從中學到東西進而在下次的 design 做得更好。

### Chapter 6: Working Classes

使用 Abstract data type (ADT) 將相關資料跟行為包裝起來，不僅可以降低複雜度也可以重複利用程式碼，還有隱藏實作細節、修改不必動到整個程式、集中化管理、程式本身就是文件的優點。一個好的 ADT 應該是所有 interface 都是是有關係，而且 interface 不能預設一些行為或者呼叫順序會有引響，不然使用的人都還要需要學習這些預設條件又會是複雜度上升。如果是 class 擁有的東西應該要用 containment，另外如果 class "是" 某個 class 時就可以使用 inheritance ，不需要再設計時就把 class 的繼承關係劃分得很細，目前需要什麼設計就要弄得簡單明瞭就可以了，還要注意只有一個 class 繼承的情形或者是 override function 卻不做事的情形，這時需要重新檢視設計。

### Chapter 7: High-Quality Routine
    
使用 routine 最大的好處就是可以降低複雜度，做好一個 routine 後使用者可以不用知道裡面的細節就得到需要的結果，routine 更可以為程式自我註解，只要 routine 的名字取的好就可以很輕易的看出在做什麼事，而不是需要花時間看懂這一坨 code 在做什麼，另外 routine 可以減少 code 的重複性跟修改優化只需要在這個 routine 做就可以而不是去修改散落在各地的 code。就算 routine 很小也不要因為心裡障礙就不去做，雖然此時很小但未來充滿不確定性，routine 建議的參數以不超過7個為準，routine 大小以大約一頁至兩頁作為準則。routine 名字通常是一個動詞搭配一個名詞，盡可能的將 routine 所做的事情濃縮在名字上，讓人可以一看名字就可以推測出 routine 的功能跟回傳的東西。

### Chapter 8: Defensive Programming

對於外部裝置給予的值需要檢查過後才繼續讓流程跑下去，如果發現不合法的值就立即警告並停止流程讓問題早點得到解決，不過一旦發現不合法的輸入時可以根據情形做出不同的回應，像是回傳正常的值、回傳上次的結果之類的。Assert 通常用於開發的時候，透過 assert 可以釐清 interface 跟 document，發生問題也可以早點排除，通常 Assert 是用於絕對不可能發生的情形，exception 用於不常發生的情形，當程式無法自己處理這個錯誤的時候可以 raise exception 期待有人可以幫助他解決，exception 必須符合自己的階層跟寫好原因，雖然 exception 會使複雜度提高但好好善用的話還是可以處理不少問題。但每次都必須檢查輸入的正確性是滿惱人的，所以可以將可信任跟不可信任的資料做分區，當資料從不可信任區流進可信任區時候必須做檢查，資料已經進入可信任區就不必再做檢查，通常從不可信任區到可信任區用 exception 的方式來處理，而可信任區中的流動是用 assert 的方式來處理，因為 assert 是不可能發生的情形，照理來說可信任區的資料一定是正確的所以用 assert 來處理。

### Chapter 9: The Pseudo-code  Programming Process

使用 pseudo-code 的好處是可以容易的讓別人 review 設計，寫好 pseudo-code 就等於寫好了文件。pseudo-code 不能綁定特定的語言，應該要通用到可以轉換成任何語言才是正確的。

## Part III: Variables
### Chapter 10: General Issues in Using Variables
    
變數要在使用之前才進行宣告跟出初始化，避免變數的 span time 跟 life time 太長造成程式的可讀性跟正確率降低，變數的 scope 也是也越小越好，從最小的範圍開始遇到必須擴大的時候想清楚後才能擴大，太早綁定變數與實際的數值會造成彈性變小，盡量越晚綁定越好，變數最好只有一個用途，不要用奇怪數字 -1 之類的數值表示異常狀態。

### Chapter 11: The Power of Variable Names

變數的名字是決定變數好壞的重要因素，賦予變數有意義的名字跟與本質相同可以讓人容易理解，盡量避免技術面的名詞反之使用描述問題本質的名字，將 total index first last 等等字詞放在變數的後面讓同一個變數有統一性，幫 boolean 變數命名時使用只有正面或反面意義的名字，例如 done, found ，避免使用像是 status 這種搞不清楚真假值的意思的名字。

### Chapter 12: Fundamental Data Types

避免使用 magic number, string, 因為使用特定的數字或字串代表某些意義會造成讓讀程式碼的人無法了解背後代表的意思降低可讀性，再者當需要改變 magic number 時候需要尋找所有用到此 magic number 的地方一一修改讓維護變得困難。

因為現在國際化的關係需要支援多種語言的需求，在設計一開始就需要考慮支援多種語言的情形。

妥善運用 Boolean 可以讓程式的可讀性提高，例如當需要判斷很多情況的時候可以用 boolean 變數代表判斷的意思，讓判斷式顯得很清楚。

Enum 可以讓 class 的 member 以英文描述，使用 Enum 可以增加程式碼的可讀性，還可以用在 function 需要回傳多種狀態的時候。

Array 是可以隨機存取的資料結構，但也是因為隨機存取的特性常常不小心 index 錯誤造成 bug，所以建議使用 array 還是用 sequential 的方式來運用避免錯誤。

### Chapter 13: Unusual Data Types

structure 將互相關聯哦資料組成一個資料結構，不但可以提高可讀性也可以讓人輕易理解資料的互相關聯性，雖然可以使用 class 來包裝但有時候用 structure 更容易理解，像是如果想要把資料結構互換可以用一般變數互換的方法，而且如果不把資料關聯成結構如果要新增或刪除某些資料就要搜尋整個程式找出所有需要修改的地方。如果 function 的參數有很多而且彼此都有互相關聯就可以包裝成一個結構，把此結構當成參數但切記這樣回提高 coupling，所以需要確認這些資料是有必要關聯後才包裝成結構。

Pointer 是程式最容易造成錯誤的地方，需要花很多心力才能找出問題，所以在寫程式就要有好習慣來避免嚴重的問題，最常見的問題就是修改到不該修改的記憶體導致整個程式發生預期外的行為。我們對於 pointer 的操作要在同一個 function 或 class，不要在一個 function 配置記憶體然後在其他 function 釋放記憶體。宣告 pointer 時候馬上給予值才不會被人誤用。使用 pointer 先確認記憶體是沒有 corrupt 的情況，接著再檢查記憶體代表的值是否合理。使用 pointer 還可以多加上 tag 來確保資料的完整性，配置記憶體時加上 tag，刪除記憶體檢查 tag，避免重複刪除的情形。避免對 pointer 做太多複雜的操作，例如 currentNode->next->prev = insertNode，會讓人不容易理解。可以藉由畫圖來理解指標的運作方式避免使用錯誤。刪除 pointer 要先確保還有方法可以找到指向的記憶體避免 garbage 產生。刪除記憶題時要把指向他的 poitner 指向 null。刪除記憶體前確認不是 null 避免重複刪除。

Global data 是指在整個程式內皆可以被讀取或修改，所以問題常發生於使用 global data 沒有意識到已經改變了，還有把 global data 取 alias 導致使用的人沒有意識到是 global data 所以沒有特別小心修改，多個 thread 環境之下也會讓 global data 產生 race condition。為何要使用聽起來很危險的 global data，理由是這個 data scope 就是整個程式所以變成 global data 也是合理的，另外還有需要用到 named constant 的時候但程式語言不支援。為了使用 global data 同時避免上述危險的情形，最基本的方法就是使用 access routine 來存取 global data，使用 access routine 可以實作 abstract data type 跟 information hiding，就算都不想要使用這兩種方法也可以得到下列好處，單純作為中央控管的方式，確保資料的存取修改都是有經過保護的。

## Part IV: Statements
### Chapter 14: Organizing Straight-Line Code

當 statements 有順序性時必須將 code 寫的容易看的出來有順序性，在重要的 code 上面還可以加上檢查確保有依照正確的順序執行。若沒有順序性的 statement 要將相關聯的 statement 寫在一起讓人容易閱讀，不要讓人需要不斷跳著程式碼才能得知想要的資訊。

### Chapter 15: Using Conditionals

使用 if-then-else statement 需要將正常的 case 寫在 if 裡面，越常見的 case 要寫在越上面的 if clasue。if clause 不要寫 null。確認 else clause 有被考慮過，如果覺得不需要處理 else 也要寫註解代表有思考過了。if clause 裡面的測試條件太複雜時候需要換成容易理解的 boolean 變數。

### Chapter 16: Controlling Loops

如果 loop content 可以預計跑的次數跟足夠簡單就使用 for loop, 反之則使用 while loop。把 loop content 想成是個黑盒子，把判斷 loop 執行與否的條件放在最上面讓人可以一眼看出什麼時候會執行 loop content，並且把控制 loop 的變因降到最低就可以減少出錯的次數。

進入跟離開 loop 的地方只有一個才不會讓人還要去找出所有進入點跟離開點增加閱讀的困難度，把 loop 初始化的 code 放在 loop 前面，使用 while true 代表無限迴圈而不使用 for i=1 to 99999 這種其實有限次數的 loop，盡量使用 for loop 因為 for loop 把 loop control 的方式都寫在開頭讓人容易理解也容易修改，如果 for loop 的 initiation, termination, iteration 沒有明確關聯請使用 while loop。

避免使用 empty loop，在 loop 開頭或結尾修改控制 loop 的變數，每個 loop 只做一項功能即使可以使用同一個 loop 達成兩項功能也不該這麼做，雖然分成兩個 loop 會有效率問題但等到真正成為效率瓶頸時才合併並且註解。

讓 loop 終止條件很明顯，不要使用奇怪的的手法跳出 loop，不要使用 loop 的 final value 來做判斷 loop 是否被完整執行。及早讓 loop 離開而不是先設好一堆 boolean 等到整個 loop content 都執行完才讓 loop header 判斷是否要離開，如果 loop 裡面有很多 break 四散在 loop content 就是一個危險的徵兆，使用 continue 可以跳過剩下的 loop content 減少縮排，但如果 continue 在 loop 中間或結尾就要使用 if 代替。

用頭腦跑過 loop 才是減少錯誤的方式，不要一直 try and error 直到 loop 運作正常，因為這只是把問題隱藏起來也沒有對於問題做出真正的了解。

使用有意義的變數名稱讓 nested loop 更容易理解，也可以避免誤用其他層的 loop 變數。

loop content 盡量在一頁內，層數不要大於3，如果太多層可以把 loop 變成 routine，越多行的 loop 增加越多複雜度所以要盡量寫的很明顯。

使用 inside out 的方式來建構複雜的 loop，先寫最內層的 loop 並且想像成 function，寫好後縮排加上 loop control，然後不斷重複這件事情。

### Chapter 17: Unusual Control Structures
若程式語言支援可以在 function 任何地方 return 請在可以增加可讀性的情況才使用，不然會讓人需要到處找所有 return 的地方。

recursion 使用在問題可以被分成很多個小問題而且小問題都是很簡單可以被解決，只有一小部分的問題適合用 recursion，更多一點的問題可以用 recursion 簡單解決但難以理解，對於多數問題使用 recursion 會使問題本身複雜很多。使用 recursion 要確保可以停下來，使用 safety counter 避免陷入無限迴圈，只在一個 routine 使用 recursion 因為若發生 routine A -> routine B -> routine C -> routine A 是非常難以偵測的，人對於處理一個 routine 的 recursion 已經是極限了，使用 recursion 之前要先確認其他方式不會比 recursion 更好才使用。

### Chapter 18: Table-Driven Methods
當遇到複雜邏輯處理或繼承時候可以試試看 table-driven method 來降低複雜度，使用 table-drive method 需要思考要怎麼 lookup table, direct access, indexed access, stair-step access，另外決定什麼 data 要放在 table 裡面，像是如果有某區間都是一樣的結果，一種做法是就把區段所有的 key 都存在 table，優點是很直覺而且改區段可以只改 table 就好，缺點是浪費空間，另一種方法可以將 key 轉換過後去存取，這樣只要存一個 key 在 table 裡面而不用浪費空間，轉換 key 的方法獨立包在 routine 裡面讓人知道這是 key 有經過轉換才去 lookup table。

Indexed access table 是先用 data 去 lookup table 找 index 再去找到結果，好處是當 data 大部分的結果都是不重要就可以節省空間，另外 data encode 在 table 裡面比起 data encode 在程式裡面好維護。

Stair-step access table 適合使用在 key 為某個範圍對應到結果的時候，當遇到 irregular data 也可以使用，但需要注意邊界問題還有使用 binary search 來尋找結果增加效率，如果願意使用空間換取速度也可以換成 indexed access table。

### Chapter 19: General Control Issues
Boolean expression: implicit 比較 boolean value，寫成 if(done) 不要寫成 if(done is True)，boolean test 太過於複雜可以把部分判斷變成一個 boolean 變數，如果複雜的判斷有一個意義且被常常使用就可以搬進一個 routine 但如果只有使用一次也是可以搬進 routine 增加可讀性，另外還使用 decision table 好處是資料變動只需要改動 table。

Deep nesting 會使人難以理解所以我們需要 refactor code 讓邏輯清晰，retest part of condition 可以減少 nested，當發現在成功情形下還有許多不同的情況可以把這些情況拉出來與成功情形一起測試減少 nested，如果在 loop 裡面有 deep nested if 可以將其中的 nested code 寫成 routine，另外還可以使用 object-oriented 的方式為情形寫好 class 各種類的情形為 sub class，

## Part V: Code Improvements

### Chapter 20: The Software-Quality Landscape
使用者跟 programmer 對於 Software quality 看法有不同的觀點，以外部的觀點來看 correctness, usability, efficiency, reliability, integrity, adapability, accuracy, robustness 代表 software quality 的指標，內部觀點來看則是 maintainability, flexibility, portability, reusability, readability, testability, understandability，但是不可能同時達成這些指標需要考量 project 的目標來決定要用那些指標評估 software quality。

增加 software quality 需要先定義好要達成的目標，需要明確指出 software quality 為優先的目標讓工程師內心會去努力達成，為 software 寫 unit test 或者 basic function test 可以當成評估 quality 的一項指標，藉由 test passed 跟 failed 與 code coverage 來清楚知道 software quality 是增加還是降低，code review 或者 pair programming 也是可以提高 software quality 的一種方式。開發過程中同時考慮 software quality 會比沒有考慮好，要在開發過程中保持 software quality 可以建立處理未預期的 spec 變更的流程。
  
利用不同的方式找出 defect，在越早的開發階段發現問題可以花越少時間處理，造成的傷害也會越少。

### Chapter 21: Collaborative Construction

### Chapter 22: Developer Testing

### Chapter 23: Debugging

### Chapter 24: Refactoring

### Chapter 25: Code-Tuning Strategies

### Chapter 26: Code-Tuning Techniques

## Part VI: System Considerations

### Chapter 27: How Program Size Affects Construction

### Chapter 28: Managing Construction

### Chapter 29: Integration

### Chapter 30: Programming Tools

## Part VII: Software Craftsmanship

### Chapter 31: Layout and Style

### Chapter 32: Self-Documenting Code

### Chapter 33: Personal Character

### Chapter 34: Themes in Software Craftsmanship

### Chapter 35: Where to Find More Information
