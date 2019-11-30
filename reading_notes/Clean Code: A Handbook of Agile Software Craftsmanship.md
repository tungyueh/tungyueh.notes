# Clean Code: A Handbook of Agile Software Craftsmanship

## Introduction
* Craftmanship: 獲得足夠的知識並且努力不斷練習
* 學習寫程式也是需要不斷的練習才不會止步於讀完書上的技巧後自我感覺良好
* 書中有需多程式碼會不斷挑戰什麼是正確什麼是錯誤的 code
* 本書分為三段落
    * 第一段落介紹寫出 clean code 的原則、pattern與實踐的技巧
    * 第二段落包含許多 case studies，每個範例都會把 code 變得問題少一點，需要分析理解 code 找出變動的原因
    * 第三段落整理改動的啟發及線索
* 如果沒有仔細讀好第二段落的 case studies 則第一段落的價值將會很侷限
* 第二段落需要仔細了解每個步驟的決定，可以有助於加深理解第一段落所闡述的原則、pattern、實踐方法

## Chapter 1: Clean Code
* Code 說明需求的細節，將需求規範到讓機器可以跑就是 programming，而 code 就是 spec
* Bad code 會隨著 feature 的增加而使 code 更加糟糕最後無法控制
* 雖然知道 bad code 不好但是由於工作上的壓力都會覺得之後再做，但其實之後等於從不
* Messy code 雖然可以讓團隊在一開始前進速度很快但是隨著每次改動讓 code 越來越不直觀，藏有許多先要有背景知識才能理解的 code，最終會讓團隊無法修改任何 code，因為都無法知道為何 code 要如此寫的原因
* 當 code base 已經混亂到無法使團隊有任何生產力時候就會提出重新設計的建議，雖然開始重新設計後但最終發現新設計的系統還是一樣混亂，所以保持 code base clean 是很重要的
* 我們常常將 code 變糟怪罪到其他人，但其實往往是自己沒有誠實的揭露風險跟捍衛自己的工作準則，作為一個專業的程式設計師應該要維持住自己最低的底線在 schedule 壓迫下提醒 manager 可能會有的風險
* 為了要趕上 deadline 會製造 mess code 但是製造 mess 會推延速度導致無法趕上 deadline 所以要隨時讓 code 保持 clean 才能趕上 deadline
* 可以分辨 clean code 跟 dirty code 不代表就會寫 clean code
* 要能寫出 clean code 需要無數的小技巧與對於 clean 的 sense，這些可以讓我們分辨 code 的好壞與將準則應用於改變 bad code 上
* 有 code-sense 的 programmer 看到 messy module 會有不同的改變的選擇並且使用最佳的方法去改動
* Clean code 應該讓人讀起來是愉悅的
* Bad code 會誘惑人們助長 code 變得更雜亂 
* Clean code 只注重在一件事情上，bad code 一次做了太多事情
* Clean code 要有 unit test
* Clean code 要能讓人感覺到作者的用心
* Clean code 不要有 duplication
* Clean code 讓語言本身就像是為問題所產生的一樣
* code 的童子軍原則: 讓 code 變得比當初發現的更好，就可以避免 code 腐化

## Chapter 2: Meaningful Names
* 軟體開發過程中需要常常命名所以需要好好的命名
* 使用能表明意圖的名稱，雖然取好的名字需要花時間但是之後可以節省更多時間，使用有意義的名稱能快速幫助閱讀者知道 code 的作用跟意圖
* 避免使用誤導性的命名、包含資料結構的命名、與其他命名差異過小
* 讓命名都有明顯的區別，不要故意拼錯、使用 number-series naming 跟放 noise word 到名稱中，會讓讀者困惑
* 使用可以念出來的名稱，才能夠順暢的討論，而且人類對於可發音的語言更能好好的處理所以要使用這項優勢
* 使用容易被搜尋的命名才便於找出使用的地方，命名的長度可以根據出現的 scope 來決定，所以如果使用的 scope 很小就可以用短一點的命名
* 避免使用縮寫因為還需要解讀也不容易唸出來更容易拼錯
* 現代語言都要好的 type system 跟現代傾向寫出小的 class 跟 function 所以不太需要使用 Hungarian Notation 來幫助 programmer 記憶型態，而且也不太好更改命名跟比較難讀懂
* 有了現代 IDE 的幫助就不需要在 class 的 member 前面加 prefix m_ ，而且寫小的 class 也不太需要特別加入 prefix，另外讀者會自動忽略 prefix 才能快速知道命名所以 prefix 也有點累贅
* 比起 prefix interface 比較傾向 prefix implementation
* 避免讓讀者還要自己找出命名背後的意義，通常發生在不使用 problem domain teram 也不使用 solution domain term
* class 命名通常使用名詞
* Method 命名通常使用動詞
* 使用清楚的命名取代娛樂性的命名
* 固定使用一種命名的方式才不會讓使用者困惑，fetch, retrieve, get 中挑選一種，controller, manager, driver 中挑選一種
* 保持一個字只代表一種概念，不要使用同一個字但有兩種意思避免造成誤解
* 使用 programmer 都知道的名字來命名，因為讀 code 的人都是 programmer，如果使用從問題命名而來就需要去讀懂問題的本身
* 如果沒有 solution domain term 就使用 problem domain term 至少讓讀者可以去問該領域的專家，好好的把 solution 跟 problem domain 分開始 programmer 的工作之一
* 把命名結合再一起形成有意義的 class, function ，如果都無法至少在這些命名加上 prefix 讓讀者知道這是屬於結構的一部分
* 讓命名長度短到可以明確表達意思，不要加入累贅的部分，不僅會讓 IDE 的 auto complete 難以使用也無法重複使用該 class 或 function
* 命名需要有良好的描述能力跟同樣的文化背景

## Chapter 3: Functions
* Function 是組織程式的第一步所以把 function 寫好是重要的事情
* Function 越小越好，不要超過 100 行盡量在 20 行之內
* 把 if, else, while 底下的 block 包裝成 function 可以給這個 block 有意義的名稱增加描述性，也可以減少 function 過多的縮排讓 function 更好讀
* Function 下只做同一層概念的事情，因為 function 是為了把大概念分割成許多小概念的工具，所以如果 function 有不同層的概念就失去意義了
* Function 用相同層級的概念下閱讀起來比較容易，讀者不需要再不同概念下轉換
* switch (包含 if/else chain) 本性上就違反只做一件事的原則，但可以使用 polymorphisom 來把 switch 藏在下一層的概念裡避免重複
    ``` java
    public Money calculatePay(Employee e)
    throws InvalidEmployeeType {
        switch (e.type) {
            case COMMISSIONED:
                return calculateCommissionedPay(e);
            case HOURLY:
                return calculateHourlyPay(e);
            case SALARIED:
                return calculateSalariedPay(e);
            default:
                throw new InvalidEmployeeType(e.type);
        }
    }
    ```
    * 有新的 type 增加會使 function 變大
    * 不只做一件事情，做了找出相對應的 function 跟計算 pay
    * 違反 Single Responsibility Principle，當新種類的 employ 出現或改變計算薪資的方式都需要改這個 function
    * 違反 Open Closed Principle，因為有新形態就要修改
    * 最糟糕的是有其他很多 function 都會不斷出現這類結構，像是 `isPayday(Employee e, Date date)`, `deliverPay(Employee e, Money pay)`
    ``` java
    public abstract class Employee {
        public abstract boolean isPayday();
        public abstract Money calculatePay();
        public abstract void deliverPay(Money pay);
    }
    -----------------
    public interface EmployeeFactory {
        public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType;
    }
    -----------------
    public class EmployeeFactoryImpl implements EmployeeFactory {
        public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType {
            switch (r.type) {
                case COMMISSIONED:
                    return new CommissionedEmployee(r) ;
                case HOURLY:
                    return new HourlyEmployee(r);
                case SALARIED:
                    return new SalariedEmploye(r);
                default:
                    throw new InvalidEmployeeType(r.type);
            }
        }
    }
    ```
    * 使用 abstract factory 產生不同的 employ class 把 switch 永遠藏起來
    * 當 switch structure 只出現一次是可以接受的但是出現多次需要藏在繼承關係裡面讓其他人看不到
* 挑選有描述性的 fuction 名字有助於釐清設計的方向並且幫助改善
* Function Arguments
    * Function 參數越少越好，三個以上參數就需要仔細思考了
        * 參數讓讀者每次看到都需要耗費多餘的精力去理解
        * 參數也不容易對 function 做完整的測試，參數越多則測試組合數量會快速的增加
        * output 參數比 input 參數更讓人難以理解，一般都會認為由 function 回傳值而不是從參數回傳
    * 通常傳一個參數到 function 是為了問關於這個參數的問題或者對於參數做特定的轉換，挑選 function 名字需要能準確區分這兩種目的，另外稍微少見一點的是 event 的形式，傳一個參數來做特定的事情沒有回傳值，不要把回傳值放在 output argument
    * 不要使用 flag arugment，因為這樣就很清楚說明 function 不只做一件事情，也會讓 function 的目的模糊
    * 兩個參數的 function 需要再合適的時機使用，像是在 `Point p = new Point(0,0);`，不一定要轉換成單個參數的 function 要考慮轉換後是否有得到好處
    * 過多的參數可以用 object 來包裝減少參數的個數，包裝後的 object 可以為相關的參數取個名字增加可讀性
    * Function 跟參數名字要能形成動詞與名詞的關係才能表達清楚彼此的關係
* Function 不要有 side effect，不但違反 do one thing 的原則而且增加 coupling
* Output arguments 會讓讀者很疑惑需要再去確認宣告的地方知道用法，需要避免使用，如果 function 需要改變東西的狀態應該東西本身自己去改變狀態，例如 `appendFooter(s);` 轉換成 `report.appendFooter();`
* Function 不是做事就是回答問題，不要混用造成違犯 do one thing 也會讓可讀性降低
    * Bad: `if (set("username", "unclebob"))...` 
    * Good: 
        ``` java
        if (attributeExists("username")) {
            setAttribute("username", "unclebob");
            ...
        }
        ```
* 使用 exception 來處理錯誤而不是回傳的 error code，使用回傳的 error code 造成可讀性降低也會讓結構有很深的縮排
* 把 try/catch block 包成 function 比較容易閱讀
* Error handling 也算是一件事情所以 function 只能處理 try/catch 而不能做其他事情
* Error code class 會讓其他 class 都需要 import ，改變 Error code class 會需要重新 compile 跟 deploy 造成大家都不願意加新錯誤，但使用 exception 加新的 exception 是繼承所以不需要重新 compile 跟 deploy
* 雖然有人提倡 one entry and one exit 但是在小型的 function 就不適用，所以小型的 function 還是可以用多個 return, break, continue，但還是不要使用 goto 因為 goto 只在大的 function 才有意義

## Chapter 4: Comments
* 註解是因為能力不足以使用 code 表達所造成的失敗，所以當使用註解時應該要仔細想想是否可以使用 code 來表達
* Code 會不斷改變但註解就比較難跟隨著，所以註解常常沒有被更新，雖然可以要求注意註解的更新，但把精力放在把 code 寫到不需要註解比較好
* Code 永遠會有正確的資訊而註解不會，所以我們盡量把註減少
* 通常寫註解是覺得 code 很難懂，但應該要花時間把 bad code 改成 clean code 而不是幫 bad code 寫一堆註解

### Good Comments
* 最好的註解是找到一種方式不需要寫註解
* Legal Comments: 版權宣告之類的註解
* 提供基本的資訊，回傳的值要用來做什麼，regular expression 所要 match 的字串
* 解釋為何挑選這個實作方法跟如何實作
* 無法改 code 的情況下可以為參數跟回傳值做更好讀的註解
* 警告其他人改動 code 可能的後果
* TODO 註解標示現階段無法處理的原因，像是未來要丟棄的功能或是未解的問題或者因為相依性問題目前無法更動，但不能是 bad code 存在的理由，需要定期檢視 TODO
* 強調別人容易忽略的地方

### Bad Comments
* Mumbling: 別人都不太清楚意思需要在翻找其他 code 來搞清楚註解在說什麼，如果要寫註解就要確保是自己能寫出最好的註解
* Redundant Comments: 沒有提供比 code 更多的資訊、比 code 不好讀懂、比 code 還要不精確
* Misleading Comments: 提供錯誤的資訊誤導使用者
* Mandated Comments: 規定每個 function 或變數都需要有註解，這會讓註解變的沒有意義也會讓 code 的組織變得混亂跟讓不實到訊息到處散播
* Journal Comments：每次修改檔案都在開頭加上註解，在沒有版本控制系統以前是合理的方式，但現在已經不需要這類的註解
* Noise Comments: 沒提供任何有用的資訊只是突然增加雜亂
* 如果可以使用 function 或 variable 就不要使用註解
    * Bad
    ``` java
    // does the module from the global list <mod> depend on the
    // subsystem we are part of?
    if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem()))
    ```
    * Good
    ``` java
    ArrayList moduleDependees = smodule.getDependSubsystems();
    String ourSubSystem = subSysMod.getSubSystem();
    if (moduleDependees.contains(ourSubSystem))
    ```
* Position Markers: 使用註解標示出特定位置，但太常使用只會讓讀者自動性略過變成一種干擾，Ex: `// Actions //////////////////////////////////`
* Closing Brace Comments: 在 long loop 結尾加上相對應的註解以便於知道是屬於哪個 loop，但是應該要嘗試縮短 loop 才是比較好的方法
* Attributions and Bylines: `/* Added by Rick */`，版本控制系統才是比較好的選擇
* Commented-Out Code: 註解掉 code 會讓人不敢刪除，版本控制系統可以幫我們記住 code 所以可以放心刪掉
* HTML Comments: 從註解中不易閱讀，要使用其他工具較方便，如果是 tool 需要應該是 tool 的責任去把註解變成 HTML 而不是靠 programmer
* Nonlocal Information: 如果一定要寫要保證是描述附近的 code，不要把系統性的資訊放到 local 註解上，實際上沒有描述該 function，之後更改系統設定也容易忘記跟著改
* Too Much Information: 包含之前討論的細節或不相關的細節
* Inobvious Connection: 註解與 code 本身的關聯性要明顯而不需要再去問寫註解的人
* Function Headers: 短的 function 通常不需要註解，把 function name 取好一點就可以了

## Chapter 5: Formatting
* 程式碼排版對於溝通是很重要的，在程式碼不斷變動的情況下能夠與人溝通的程式碼比起能夠運作的程式碼還重要

* Vertical Formatting
    * source file 應該要像新聞段落一樣
        * source file 的名稱像是標題一樣簡單但足以解釋內容，讓人知道是否有興趣繼續讀內容
        * source file 上面部分要能提供不涉及細節的大致概念與演算法
        * 越往下讀 source file 能知道更多的細節
    * 每行程式碼像是個句子而多行程式碼像是個段落闡述一個目的，不同目的利用空行提醒讀者，相同目的的程式碼要放在一起顯示他們是相關的
    * 相關的 function 或有繼承關係的 class 盡量放在同一個 source file 才不需要在不同檔案間找出彼此的關係
    * 宣告的變數要靠近使用的地方，控制 loop 的變數通常宣告在 loop statement 裡面
    * class 的 member 要放在一開頭的地方，因為通常都會有很多 method 都會使用到這個 member
    * 呼叫與被呼叫的 function 要放在一起，呼叫的 function 排在被呼叫的 function 上面就能清楚知道彼此的關係
    * 有類似概念或目的的 function 要放在一起

* Horizontal Formatting
    * 使用空白區分相關與不相關的部分
        * Assignment 用空白隔在兩旁區分出左邊與右邊，`a = b`
        * 使用空白讓 operator 的優先順序更明顯，`return b*b - 4*a*c`
    * 不要對齊宣告的變數名稱或 assignment 的 rvalue，容易讓人忽略型態或 assignment operator
    * 縮排呈現程式的階層架構讓人可以輕易了解每個的範圍以及上下的階層關係
    * 不要為了單行的 if statement 或小的 while loop 或小的 function 而打破縮排的規定
    * 無可避免需要使用沒有 body 的 loop 的時候需要清楚將齊縮排好，而不要藏在最尾端
* 每個人都有自己喜歡的排版方式但在專案裡需要大家討論出一個排版方式並且遵守才能讓專案有一致性
