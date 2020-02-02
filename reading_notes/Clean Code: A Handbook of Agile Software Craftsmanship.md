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

## Chapter 6: Objects and Data Structures
* 把變數變成 private 是為了不想要使用者依賴這些變數並且我們可以自由改變這些變數的實作方式，但還是有很多人為了讓外面的人讀取這些 private 的變數多加了 setter 跟 getter 來讓外面存取

### Data Abstraction
* 隱藏實作方式不只是將變數多一層分隔開來，主要目的是 abstraction
* 把 private 變數提供 setter 跟 getter 讓外界可以更改還是等於把變數設為 public
* 只把使用者需要操作的必要資料 expose 出去而隱藏內部實作細節
* 要思考 expose 出去的 interface 的最佳形式而不是單純把隱藏的資料 expose 出去
    * Concrete Vehicle: 
        ``` java
        public interface Vehicle {
            double getFuelTankCapacityInGallons();
            double getGallonsOfGasoline();
        }
        ```
    * Abstract Vehical(Prefer):
        ``` java
        public interface Vehicle {
            double getPercentFuelRemaining();
        }
        ```

### Data/Object Anti-Symmetry
* Object 把 data 隱藏在 abstraction 後面，提供 function 去對 data 操作
* Data structure 只 expose data 沒有任何 function
* 使用 data structure 容易增加 function 而不影響原本的 data structure，使用 OO 的方式容易增加 class 而不影響原本的 function
* 根據使用情境挑選合適的解法

### The Law of Demeter
* Module 不該知道 object 內部的行為
* class C 的 method f 只能呼叫
    * C 的 method
    * f create 出來的 object 的 method
    * 被當作參數傳進 f 的 object 的 method
    * C 的 instance variable 的 object 的 method
* 不使用 `a.b.Method()` 這類方式，因為呼叫的 `Method()` 是 `b` 所回傳的

#### Train Wrecks
* `final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();`
* 這類型的 code 像是一堆火車車廂擠在一起，應該要盡量避免
* 最好改寫成 
    ``` java
    Options opts = ctxt.getOptions();
    File scratchDir = opts.getScratchDir();
    final String outputDir = scratchDir.getAbsolutePath();
    ```
* 上面這段 code 沒違反 Law of Demeter 因為呼叫的 function 只呼叫自己所知道的 function
* 違反 Law of Demeter 端看是 object 還是 data structure，如果是 data structure 天生就 expose data 所以就跟 Law of Demeter 沒關係

#### Hybrids
* 混用 data structure 與 object 會同時得到兩種的缺點

#### Hiding Structure
* 如果 `ctxt`, `options,`, `scratchDir` 都是 object 就無法透過 internal structure 去找出 absolute path
* 雖然可以讓 `ctxt` 加入 `getAbsolutePathOfScratchDirectoryOption()` 但感覺不太好
* 應該要找出目的後讓 `ctxt` 為我們做事而不是詢問 `BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);`

### Data Transfer Objects
* 典型的 DTO 是一個 class 只有 public variable 沒有 function
* 通常使用在與 database 溝通或者解析 socket 的訊息
* Active Record 是 DTO 的一種特殊形式，除了 public variable 還有 navigational method 像是 save 或 find，提供直接的轉譯
* 把 active record 誤認為 object 而加入 business rule 會有混用 object 與 data structe 的弊病
* Active Reocrd 當成純粹的 data structure 在另外的 object 加入商業邏輯

## Chapter 7: Error Handling
* Error handling 是需要面對的事情，輸入或機器可能異常，所以要確保遇到異常情況能做需要的行為
* 雖然 error handling 很重要但一但讓原本邏輯模糊掉就是錯誤的

### Use Exceptions Rather Than Return Codes
* 早期程式語言沒有 exception 所以都利用 error flag 或 error code 來檢查呼叫的結果
* 用 error code 檢查呼叫結果缺點是 caller 需要立刻檢查 error code 的結果而且也容易忘記
* 使用 excpetion 處理異常情形可以將本身的邏輯與處理錯誤的邏輯分離開來

### Write Your Try-Catch-Finally Statement First
* Exception 可以創造出 scope，讓人知道 try 裡面的 code 都有可能發生 exception 接著會在 catch 裏回復
* 當遇到會發生 exception 的 code 使用 try-catch-finally 的寫法可以幫助使用者預測行為
* 先從寫預期發生 exception 的測試開始，接著增加 handler 去滿足測試，這樣可以先把 try block 先建立好，之後只要在裡面增加邏輯就好

### Use Unchecked Exceptions
* Checked exception 缺點是如果改變的時候需要重新 compile
* 當 low level throw checked exception 則中間的檔案都需要宣告這個 exception
* Checked exception 破壞了封裝的概念，因為上層需要知道下層會吐出哪種 excpe

### Provide Context with Exceptions
* 丟出去的 exception 要能夠提供足夠的資訊讓接住的能有辦法判斷要如何處理
* 雖然可以看 traceback 但是不容易看懂所以自己要轉換好資訊，通常包含做了什麼動作遇到什麼錯誤

### Define Exception Classes in Terms of a Caller’s Needs
* Define exception 時通常要考慮怎麼被接住
* 多個 excepction 都是處理相同的事情就是 duplicate，可以 define excpetion 跟多一個 wrapper 來處理
* 對於第三方 API 寫一個 wrapper 是好的作法，因為不需要遷就於原本作者的想法，也可以無痛的轉換不同的 API
* 通常一個 exception class 對應到一個範圍的 code，利用 exception 的訊息可以分辨錯誤，如果想要讓某些 exception pass 才去 define 多個 exception class

### Define the Normal Flow
* 不要使用 exception 來處理特殊情況因為為打亂邏輯
    ``` java
    try {
        MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
        m_total += expenses.getTotal();
    } catch(MealExpensesNotFound e) {
        m_total += getMealPerDiem();
    }
    ```
* 讓 object 去處理 special case 讓 client code 不需要特別處理使 special case 封裝在 object 中
    ``` java
    MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
    m_total += expenses.getTotal();
    public class PerDiemMealExpenses implements MealExpenses {
        public int getTotal() {
            // return the per diem default
        }
    }
    ```

### Don’t Return Null
* 處理錯誤時常用的方法是回傳 null 但是會造成 caller 多需要檢查回傳值，也有忘了檢查的風險
* 使用 exception 或 special case object 來處理

### Don’t Pass Null
* 不要傳 null 到 method 因為不保證 method 有對 null 做處理
* Method 檢查傳進的參數後如果是 null 就丟出 exception，而由誰來接 exception 又是個問題
* 使用 assert 來確保傳進 null 但還是會造成 runtime error

### Conclusion
* Clean code 不只是 readable 也要 robust，這兩者必不互相衝突
* 分離 main logic 與 error handling logic 可以達成 clean robust code

## Chapter 8: Boundaries
* 通常我們不會從無到有的打造軟體，都會使用 third-party package 或 open source，本章介紹如何讓我們的軟體與其他的有清楚的界線

### Using Third-Party Code
* 提供 interface 與使用 interface 本來就有一段差距，提供的人想要提供通用的 interface 讓更多人使用，而使用的人想要盡量專注在問題本身，拉鋸會造成系統界線模糊的問題
* Java Map 提供很多 method 面向通用的情況，但我們實際使用可能不想要某些 method 的功能並且希望任何人都不應該使用某些 method
* 使用 object 把使用 map 的實作封裝起來就可以畫清楚界線

### Exploring and Learning Boundaries
* Third-party code 幫助我們使用較少時間實作更多功能
* 同時學習跟使用 thirty-pary code 是很困難的，可以分開使用 learning test 的方式透過撰寫測試來了解，之後再去使用

### Learning Tests Are Better Than Free
* Learning test 是個簡單理解 third-party code 的方式，可以精準測試我們的理解是否有誤
* Learning test 可以在改版的時候及時發現是否預期行為有改變
* 無論是否有 learning test 都需要有 boundary test 來測試外部的行為來幫助 migration

### Using Code That Does Not Yet Exist
* 已知與未知也是一種界線，當某些領域已經不再我們的知識裡面或目前不知道那部分的 code 會是怎麼樣
* 為了不被其他團隊卡住所以先定義好自己希望的 API 後做好測試之後只要用 adapter 整合到 production code 就好

### Clean Boundaries
* Boundary 會發生 change 但好的設計應該不需要花太多研究與重製來對付變更，當 code 逐漸失去控制需要好好的仔細研究如何不讓未來改動花太多心力
* Boundary code 清楚的分開跟測試預期行為，不讓我們的 code 知道太多關於 third-party code
* 可以使用 wrap 或 adapter 的方式與 third-party code 畫清楚界線並將 interface 轉換成我們期待的樣貌，讓 code 更易讀也讓 interface 更一致

## Chapter 9: Unit Tests

### The Three Laws of TDD
* FIRST LAW: 寫 production code 之前先寫好 failing test case
* SECOND LAW: 寫更多 test case 之前要先把之前 failing test case 修好
* THIRD LAW: 再讓 failing test 通過之前不寫多餘的 production code
* 遵循著上述規則開發會得到需多的 test case 雖然可以幾乎覆蓋到全部的 production code 但數量過多的 test case 也造成管理困難

### Keeping Tests Clean
* Dirty tests 等於沒有 tests 因為 tests 需要跟著 code 做改變，當 tests 愈 dirty 則越難改動，讓修正 tests 的時間多餘改動 code 的時間，最後導致捨棄 tests
* Test code 與 production code 同等重要，跟 production code 一樣需要關懷

#### Tests Enable the -ilities
* Unit test 讓 code 有彈性、維護性與可重複利用
    * 沒有 unit test 則我們開始害怕改動 code 而失去彈性
    * 沒有 unit test 的改動都可能造成 bug 的產生而不易維護
* Unit test 讓設計與架構可以盡可能的保持 clean

### Clean Tests
* 可讀性是 clean test 的重點，所有讓 code 可以增加可讀性的方法都適用
* 建立 domain-specific language 來測試，在不斷 refactoring tests 途中可以找到特定 pattern 製造 test API 讓測試更加清楚

#### A Dual Standardh
* 測試不一定要像 production code 一樣有效率因為測試只會在測試環境下跑
* 多個測試結果需要測試可以違反 metal mapping 因為這樣才比較好確定測試結果: ```assertEquals("HBchL", hw.getState());```

### One Assert per Test
* 一個測試只能有一個 assert 聽起來很嚴格但好處是可以一目了然的檢視測試結果
* 使用 give-when-then patter 可以讓測試更易讀但會有很多 duplicate code，可以使用 template 來消除 duplicate
* 雖然一個測試只能有一個 assert 是好的建議但也不用刻意避免，只要注意最小化 assert 就好了
* 一個測試只測試一個概念

### F.I.R.S.T.
* Fast: 測試要跑得快才會常跑才會早點找到問題
* Independent: 測試要能獨立跑，如果測試彼此有 dependent 很難找問題點
* Repeatable: 測試要能在各種環境下跑，如果不行就會懷疑是環境的問題，在特定環境無法使用下就無法跑測試
* Self-Validating: 測試結果只能有通過或失敗，不需要讓跑測試的人親自去分析結果
* Timely: 測試要在 production code 完成之前寫好才能讓 production code 可以被測試

### Conclusion
* Tests 與 production code 同等重要或更重要，tests 讓 code 保持彈性、易維護性與可重複利用的特性
* 透過找出 testing API 讓 tests 可簡潔表達測試目的
* 如果讓 tests 腐爛則 code 也會跟著腐爛，所以要保持 tests clean

## Chapter 10: Classes
* 目前為止都注重在如何把某行或某區塊的 code 寫得好，也學習了如何適當讓 function 組合與了解彼此關係，但我們需要往更上一層的 code 組織概念才能讓 code 更 clean

### Class Organization
* Class 一開頭先宣告變數，順序如下
    1. Public static constant
    2. Private static variable
    3. Private instance variable
* Publice function 放在變數之後，將該 function 呼叫的 private utilities 放在下面

##### Encapsulation
* 我們都盡量把 variable 或 utilities 變成 private 但有時候為了測試會變成 protected 或 package scope
* 先需要尋找有沒有替代方案最後才去鬆綁 encapsulation

### Classes Should Be Small!
* Class 越小越好
* Class 大小由 responsibilites 來決定，不像 function 用行數來決定
* Class 名稱可以幫助我們知道大小，如果不能精確的取名就代表 class 承擔的責任太多，越模糊的命名則代表所承擔的責任越多，像是含有 Processor、Manager、Super 等字眼的命名
* 如果能不使用 if, and, or, but 來簡短描述 class 代表承擔的責任太多

#### The Single Responsibility Principle
* Class 或 Module 只能為了一種原因而改動，這個原則提供 responsibilites 的定義跟 class size 的原則
* SRP 是 OO 設計中最容易理解的原則但我們還是容易寫出做太多事情的 class
    * 讓軟體可以動跟讓軟體簡潔是兩種不同的思考模式，由於我們注重讓軟體可以動就忘記保持簡潔，分離不同的思考在撰寫程式的時候也很重要所以先讓軟體可以動也沒有錯
    * 問題是大部分的人都只停留在讓軟體可以動後就結束開發過程了而沒有轉換到讓軟體簡潔的思考
* 有些人會擔心太多小型單一目的 class 可能會讓人難以理解整體概念，為了知道如何達成目標需要再這些 class 中不斷尋找，但是其實不管系統裡的 class 多或少需要學習的東西都是一樣的
* 具有規模的系統都會包含大量的邏輯與複雜度，處理龐大複雜度的工具就是要組織管理讓開發者任何時候都可以知道去哪裡找到需要的地方並且只需要了解該地方的邏輯
* 總結來說我們希望我們的系統由許多小的 class 組成，每個小 class 都負責單一責任與其他 class 合作達成期望的系統行為

#### Cohesion
* Class 的 instance variable 應該要少一點
* Method 使用的 instance variable 的多寡可以代表此 method 與 class cohesion 程度
* 我們希望 method 與 class 的 cohesion 高一點比較好，因為這代表 method 與 variable 形成一整塊的邏輯
* 讓 function 跟 parameter list 越小會導致 instance variable 增加讓 method 只用某些 instance variable，這時候可以將 variable 跟 method 分離成不同 class 提高 class 內部的 cohesion

#### Maintaining Cohesion Results in Many Small Classes
* 把 large function 分成許多小 function 會讓 class 增加
    * 分開後的 function 需要用到在 large function 的變數，因為不要把參數全部傳給小 function 所以使用 class 把這些變數變成 instance varaible，所以分開 large function 的結果 class 數量增加
    * 隨著把 function 變數慢慢變成 class 的 instance varaible 會造成 class 的 cohesion 降低，此時要在將 class 切割成更小的 class 以提高 cohesion，結果又造成 class 數量增加

### Organizing for Change
* 我們需要以避免改動造成系統危機的目標來管理 code
* 改動 class 就有可能造成危機所以需要完整測試才能確保不影響其他 code
* 當 class 的 private method 只某些 public method 被用就可以分離出來成為新的 class，但只在有改動需求的驅動下才去改動，不然就先不動
* 當把一個有多個責任的 class 分成一堆 closed class 就可以將理解特定 class 時間等於幾乎沒有，也可以避免改變 function 造成其他 function 壞掉的機會降低，新增 function 也不會影響到現有 function，以測試觀點來看多個 closed class 比較方便測試邏輯
* Open-Closed Principle: Class 只能擴充而不能修改
* 理想的系統是增加新功能時只需要擴展系統而不是修改系統

#### Isolating from Change
* 需求會變動所以 code 會變動，在我們學的 OO 設計中 concrete class 包含實作細節而 abstract class 描述概念，當 client class 依賴於 concrete detail 就容易受到改動的影響也難以測試，所以要使用 interface 跟 abstract class 來將實作細節獨立出來
* 當系統被 decouple 成足以被測試的情況也代表系統是有彈性而 code 也容易被 reuse，系統中的 element coupling 程度不高代表彼此獨立不容易受到其他 element 改動的影響，同時也讓人比較容易理解每個 element 
* Dependency Inversion Principle: class 應該依賴於 abstraction 而不是 concrete detail


## Chapter 11: Systems
### How Would You Build a City?
* 由一個人管理城市是不可能的，城市運作是有不同的團隊負責不同的部分，一部分負責詳細實行規劃而另一部分負責整體概念
* 管理系統與城市有類似概念，目前 clean code 幫助我們達成 low level 的抽象化而本章更上一層讓系統層的抽象化更 clean

### Separate Constructing a System from Using It
* 建造與使用的過程是不一樣的，需要將 start up process 與 runtime logic 分開
``` java
public Service getService() {
    if (service == null)
        service = new MyServiceImpl(...); // Good enough default for most cases?
    return service;
}
```
* 直到使用才被建立出來有需多優點，如果沒用到就不需要建立、startup 時間可以縮短、也確保 service 不會回傳 null
* 但是我們就會依賴於 `MyServiceImpl` 與所需要的參數，如果真的沒用到也不能 compile
* 測試也是個問題，如果 `MyServiceImpl` 是個很大的 object 則需要 mock object 才能避開初始化 `MyServiceImpl`，因為把 construction logic 跟 runtine process 混再一起所以需要更多測試，也就是有多個 reponsibility 也就違反 SRP
* 也不確定是否 `MyServiceImpl` 適用於所有 case
* LAZY-INITIALIZATION 雖然不是壞事但把很多 setup 部分分散在系統各處不是很好
* 如果要打造堅固的系統就不要讓貪圖方便的小技巧打亂了整體模組化的設計，需要好好區分 start up process 與 runtime logic 讓我們有個一致的方法來解決相依性問題

#### Separation of Main
* 把 construction 的部分都放在同一個地方，在將產生出來的東西丟給 application 使用
* Application 不會知道所使用的東西是怎麼建立出來的，單純相信拿到的東西是對的

#### Factories
* 有時候需要讓 application 可以覺得什麼時候要製造東西出來，就像是一個訂單處理系統一樣
* 使用 abstract factory 的 pattern 讓 main 製造 factory 出來給 訂單處理決定如果要產生訂單就使用 factory 來產生，訂單處理不會知道 factory 內部怎麼產生訂單，所以就達成將 construction 與 runtime logic 分離了

#### Dependency Injection
* Dependency Injection 是一個 mechanism 將 construction 從 use 抽離出來
* Object 不需要負責初始化的責任而是交由另外的 authorative 的機制來產生
* Class 不直接解決 dependency 而是提供 setter 或 constructor argument 用來 inject dependency

### Scaling Up
* 設計系統不可能一次到位，先是根據目前情況設計好後，之後根據需求來 refactor 擴展系統
* 將各種 concern 都有分開維持好一定的空間就可以慢慢改進系統

#### Cross-Cutting Concerns
* Concern 會把原本的 domain natural object 切割掉，像是要使用 DBMS 或一般檔案來儲存資料需要再不同 object 都保持一致
* Cross-Cutting Concern: 很多處理 concern 的相同 code 散落放進不同的 object 中，但這些 concern 也可以模組化所以需要好好的調整與 domain 的交錯
* Aspect-oriented programming(AOP): 找出系統相同處理相同 concern 的 code 成為 modular construct 稱為 aspect 支援特定 concern

### Java Proxies
* 只適合用在簡單的情況，像是 wrap method call
* 用在複雜的情況中會造成 code 太多且複雜所以需要使用真正的 AOP

### Pure Java AOP Frameworks
* 大多數的 proxy boolerplate 可以由 tool 自動處理，只需要寫好 business logic 在 object 中，此 object 就單純處理這個 domain 的 user story 不會與 enterprise framework 有 dependency，所以也就容易測試
* 使用 config 或 API 來包含 application 需要的 cross-cutting concern

### AspectJ Aspects
* AspectJ 語言是 Java 的擴充支援 fist-class 提供做完整支援 seperating concerns through aspect，但需要採納新工具與學習新語言

### Test Drive the System Architecture
* 透過 aspect 把 concern 與 domain logic 分離好處很多
    * 可以使用測試來驅動架構成長
    * 不需要從一開始就精心設計 Big Design Up Front(BDUF)，如果一開始精心設計就會不容易捨棄不好的設計
* 需要用 BDUP 的方式來建造可能是因為建造途中因為實際情況才無法隨時修改，但只要把 concern 分離好修改就是可行的
* 我們可以在把各種 concern decouple 設計下從最簡單實現 user story 建造系統後才慢慢一步步擴充
* 我們也不應該一頭栽進去 project 裡開始建造，需要有對於 project 的目標、範圍、時程才能開始打造，也要規劃好容許改動的空間
* 一個好的系統應該由各種模組化的 domain concerns 所組成，不同 domain 盡可能不侵犯到其他 domain 讓整個系統可以被測試

### Optimize Decision Making
* 模組化與分好 concern 讓去中心化的管理與做決定可行，因為當如果系統太龐大則單獨一個人是無法做出所有決定
* 我們都知道由一群人做出來的決定會比單獨一個人的好但卻忽略了決定越晚做越好，因為當資訊不足的情況做出來的決定不一定是做好的
* 透過模組化來將做決定的時刻推延的最後一刻以期作出做好的決定，也可降低需要一次做出好多決定的複雜度

### Use Standards Wisely, When They Add DemonstrableValue
* 標準讓人可以更容易重複利用現有的東西、招募有經驗的人、encapsulate 好點子跟把各零件串接一起
* 但是創造標準的時間會趕不上實際使用的時機也可能會與現況脫節

### Systems Need Domain-Specific Languages
* 建造過程需要有 domain specific languages(DSLs) 來讓溝通精確且有效率
* 好的 DSLs 應該要能有效消除 domain concept 與實際的 code 的落差，如果使用與該 domain 專家一致的語言來實作也可以減少誤解
* DSLs 提升 code abstraction level 讓開發者可以透過適當的 level 顯示他們的意圖

### Conclusion
* 系統中的 domain logic 沒有分清楚會造成無法敏捷開發，很容易有 bug 隱藏在模糊的 domain logic 也難以實作 user story，也會失去 TDD 的好處
* 每個 abstraction level 都應該要清楚表示目標，透過單純的 object 再加上 aspect 機制才能讓不同 concern 分開
* 不管是系統還是 module 都不要忘記用最簡單的方式實作

## Chapter 12: Emergence
### Getting Clean via Emergent Design
* Kent Beck’s four rules of Simple Design
    * 通過所有測試
    * 不包含重複的 code
    * 表達意圖
    * 減少 class 跟 method 的數量
* 參考這四條規則可以幫助我們創造具有良好設計的軟體

### Simple Design Rule 1: Runs All the Tests
* 設計系統讓它可以照預期的運作是最重要的，如果無法證明系統可以正常運作則這個設計只是紙上談兵而已
* 當系統可以被完整的測試驗證才有開發的價值
* 當系統可以被測試自然會讓我們遵循 SRP 來開發，因為遵循 SRP 才會容易測試
* Tight coupling 讓我們難以測試，所以當我們要測試就會避免 tight coupling，為了避免 tight coupling 自然會採用 DIP, dependency injection, interface, abstraction 減少 coupling 程度
* 當我們有測試且不斷去跑測試會強迫我們向 low coupling 與 high cohesion 的目標前進，所以寫測試會讓我們有更好的設計

### Simple Design Rules 2–4: Refactoring
* 測試可以讓我們不害怕去 refactoring code，所以當增加 code 的時候可以去反思有沒有讓設計變糟，如果有就可以放心去 refactoring
* Refactoring 過程中可以使用任何好的設計，例如: increase cohesion, decrease coupling, seperate concern, modularize system concern, shrink functions and classes, choose better name 等等

### No Duplication
* Duplication 是好設計的大敵，代表增加多餘的工作量、多餘的危險、多餘的複雜度
* 從小地方的 refactoring 讓 code 可以 reuse 可以降低系統的複雜度，為了可以 reuse 大地方的 code 要從小地方開始學習
* TEMPLATE METHOD 常被用來去除 high-level 的 duplicate

### Expressive
* 在我們對於理解問題程度較深的時候容易寫出其他人較不容易懂的 code，因為其他人對於問題理解程度沒這麼深
* software project 成本最高的地方就是維護，在系統日漸複雜的情況下能讓維護的人一眼看出 code 的意圖是很重要的，這樣才能降低理解 code 的時間
* 取個好名字、用小的 function、design pattern 都可以很好的顯示意圖
* 好的 unit tests 讓人快速理解 code 的作用
* 完成 code 要多留一點時間思考有沒有辦法讓 code 變得更好讀，不要急著往下個問題前進

### Minimal Classes and Methods
* 雖然要保持 class 跟 method 小但可能會造成太多的 class 跟 method 所以應該也要保持 class 與 method 的數量少一點
* 太多的 class 與 method 有時候是太過遵守原則的問題，應該要依照實際狀況來採用
* 最終目標是要同時讓系統的 class 與 method 少而且 class 與 method 要小
* 這條原則是排序最後的所以要先遵守好前三條

### Conclusion
* 沒有簡單的原則可以取代經驗，參考作者透過多年經驗所濃縮的原則可以少走一些冤枉路

## Chapter 13: Concurrency
* 要寫出 clean concurrent program 是很難的，雖然寫出看起來不錯的 code 但當系統在壓力之下就會崩潰
* 本章討論 concurrent programming 的必要性跟困難之處，建議幾個方法來處理困難處，最後討論該如何測試

### Why Concurrency?
* Concurrency 把 what get done 跟 when get done decouple
* Concurrency 可以增加 throughput 跟 application 的架構，可以讓系統更容易理解也可以更簡單分離 concern
* 有些系統有 reponse time 與 throughput 的要求所以需要 concurrency 來達成

#### Myths and Misconceptions
* Concurrency 永遠會增加效能: 只有多個 thread 可以同時等待時間很多的時候才會
* Concurrency 的設計不需要變: 分離 what 與 when 的概念會很大影響設計
* 如果使用 container 就不太需要了解 concurrency: 最好了解 container 是如何處理 issue concurrent update 與避免 deadlock
* Concurrency 會有額外的 overload 包括效能或多的 code
* 正確的 concurrency 是很困難的
* Concurrency 的 bug 通常很難重現，常被誤解成一次性的問題
* Conccurency 通常需要從根本上採取不同的設計策略

### Challenges
``` java
public class X {
    private int lastIdUsed;
    public int getNextId() {
        return ++lastIdUsed;
    }
}
```
* 如果有一個 instance X 並設好 LastIdUsed 為 42 讓兩個 thread 共用
* 假設兩個 thread 同時呼叫 getNextId 可能會有三種可能的結果
    * Thread one 拿到 43, thread two 拿到 44, lastIdUsed 為 44
    * Thread one 拿到 44, thread two 拿到 43, lastIdUsed 為 44
    * Thread one 拿到 43, thread two 拿到 43, lastIdUsed 為 43
* 最後一種結果最讓人驚訝，會發生的原因是有很多可能的路徑過經過那行 code，到底有多少 path 就要去理解 compile 怎麼產生的 bytes code 跟 jave memory model 怎麼做好 atomic
* 雖然大多數的 path 產生正確的結果但是重點是有些 path 產生錯誤的結果

### Concurrency Defense Principles
#### Single Responsibility Principle
* Concurrency 足夠複雜到可以當成一個原因去改變，所以要與其他 code 做分離
* Concurrency-related code 有自己的 life cycle 包含開發，變動，微調
* Concurrency-related code 有自己的困難之處，跟平常會遇到的困難不一樣
* 有很多種撰寫 concurrency-based code 的錯誤方式在不增加 code 就已經足夠挑戰了
* 建議把 concurrency-related code 與其他 code 分離

#### Corollary: Limit the Scope of Data
* 使用 synchronized keyword 保護 critical section，並且限制 critical section 的數量
* 越多 shared data 則越容易忘記保護、到處都有保護的 code、難以找出錯誤的地方
* 把 data encapsulation 放在心上，嚴格限制 shared data 的讀取

#### Corollary: Use Copies of Data
* 不使用 shared data 最好的方式就是不去 share data，某些情形可以複製 object 出來成唯獨狀態，或者複製 object 出來在 multi thread 然後在單一 thread 整理結果
* 如果有簡單的方法避免 share data 則 code 就不會有太大問題，雖然使用複製的方式可以避免同步的問題但會額外造成 creation 與 garbage collection 的負擔

#### Corollary: Threads Should Be as Independent as Possible
* Threaded code 不去 share data 讓 thread 像是活在自己的世界不需要與其他 thread 交流
* 嘗試把 data 分割讓 thread 可以獨立執行，之後更有機會在不同 processor 跑

### Know Your Library
* Java 5 提供許多對於 concurrency 的支援，主要需要注意下列事項
    * 使用 thread-safe collection
    * 使用 executor framework 去執行彼此不相關的 task
    * 可以的話使用 non blocking solution
    * 有些 library class 不支援 thread safe

#### Thread-Safe Collections
* Java 的 ConcurrentHashMap 在大部分情況都比 HashMap 效能好
* Review 對你可用的 classes

### Know Your Execution Models
* 有很多種方式區分 concurrent application 的行為，所以要先定義以下名詞
    * Bound Resources: 有固定大小的 resource
    * Mutual Exclusion: 同一時間只有一個 thread 可以讀取 shared resource
    * Starvation: 一個以上的 thread 長時間無法執行
    * Deadlock: 兩個以上的 thread 都在等待對方結束
    * Livelock: thread 處在每次嘗試執行都需等待

#### Producer-Consumer
* 一個以上的 producer thread 產生 work 放到 queue
* 一個以上的 cosumer thread 從 queue 拿 work 出來完成
* Queue 是個 bond resource，所以 producer 需要等 queue 有空間才能放，consumer 需要等 queue 有東西才能拿
* Producer 與 Consumer 用 signal 通知 queue 的狀態

#### Readers-Writers
* Writer 更新 shared resource 會卡住 Reader 造成 throughput issue
* 注重 throughput 可能造成 starvation 與 information 過時
* 協調 Reader 不能讀 Writer 正在更新的資料是個困難的地方
* 要提供合理的 throughput 與避免 starvation 是個有挑戰性的任務
* writer 太常 update 會讓 throughput 降低，writer 等到 reader 都不需要 read 會讓 writer starve，取得之間的平行是很重要的問題

#### Dining Philosophers
* Thread 彼此搶 resource 會造成 starvation、deadlock、livelock、throughput、efficiency degradtion
* 研究 algorithm 並且寫出 solution 可以幫助解決 concurrency 問題

### Beware Dependencies Between Synchronized Methods
* Synchronized method 彼此有 dependency 會造成奇怪的問題
* 避免在 shared object使用兩個以上的 method 
* 當一定要使用兩個以上的 method 參考下列方式讓寫出的 code 是正確的
    * Client-Based Locking: Server 在呼叫第一個 method 之前 lock 起來直到呼叫最後一個 method
    * Server-Based Locking: 用一個 method 把 server lock 起來，呼叫完所有 method 才 unlock，讓 client 使用這個 method
    * Adapted Server: 創造中間層會去 lock，有點像是 server-based locking 但 server 無法改變的情況下使用

### Keep Synchronized Sections Small
* Lock 會製造 delay 與其他 overhead，所以要減少使用
* 不需要為了減少 critical section 數量而把 critical 變得很大，因為會增加競爭的機率與降低效能

### Writing Correct Shut-Down Code Is Hard
* 寫出一個永遠跑下去的程式跟寫出只跑一段時間後就結束的程式是不一樣的
* Graceful shutdown 很難正確因為可能會遇到 deadlock 或 thread 永遠等不到 signal
* 盡早考量如何 graceful shutdown 因為這會花滿多時間

### Testing Threaded Code
* 對於可能會引發問題的部分寫測試並且使用不同 config 常常跑，如果出現錯誤要找出原因而不是忽略
* 把 spurious failure 當成 threading issues
* 讓 nonthreaded code 先正常運作
* 讓 threaded code 可以決定是否使用
* 讓 threaded code 可以調整
* 使用比 processer 更多的 thread
* 不同平台測試
* 控制 code 可以強迫製造錯誤

#### Treat Spurious Failures as Candidate Threading Issues
* Threaded code 容易在不可能壞掉的地方壞掉，而且往往很久才發生一次
* 不要忽略這類錯誤，不然 code 只會建立在錯誤的方向

#### Get Your Nonthreaded Code Working First
* 確保 code 在不是 threaded 的環境下也可以運作
* 不要同時尋找 nonthreaded bug 跟 threaded bug，先確定 code 在不使用 thread 是正確的

#### Make Your Threaded Code Pluggable
* 支援 concurrency 的 code 應該要能在不同設定下跑
* 讓 thread-based code pluggable 才能在不同設定嚇跑

#### Make Your Threaded Code Tunable
* 透過反覆測試才能找出平衡的 thread 數量，所以要讓 thread 可以調整
* 最好是能在 run time 根據環境調整

#### Run with More Threads Than Processors
* 事情通常都發生在切換 task 的時候，所以使用比起 processor 更多的 thread 可以增加問題發生的機率

#### Run on Different Platforms
* 不同的 platform 有不同的 threading policies 所以需要再不同 platform 測試

#### Instrument Your Code to Try and Force Failures
* 缺陷常常藏在 concurrency code 中因為簡單的測試無法發現這類缺陷，需要很久時間才會發生一次
* 只有很少的 path 才會觸發問題出現造成偵測及除錯困難
* 可以操作 code 讓發生的機率提高
* Hand-Coded
    * 處理棘手的測試手動插入 `wait()`, `sleep()`, `yield()`, `priority`
    * 缺點是需要找出適當的地方插入、要知道該使用何種指令、加在 production code 會變慢、可能還是找不到問題
    * 只在測試做這件事，輕易的使用不同平台與設定來跑測試來增加問題發生的機率
* Automated
    * 使用 tool 來 instrument code
    * 插入在不同地方後隨機產生動作藉由多次測試找出原因

### Conclusion
* 很難寫出對的 concurrent code，面對 concurrent code 需要嚴謹的態度
* 依照 SRP 將 thread-aware code 與 thread-ignorant code 分開
* 知道 concurrency 的問題的來源
* 了解使用的 library 與 framework
* 知道如何 lock 該 lock 的區域，不該 lock 的區域不要 lock，讓 shared object 與 scope 越少越好
* Thread issue 不常發生所以要在不同平台用不同的設定反覆的測試
* 實際插入某些指令在 code 提高問題發生的機率以便除錯

## Chapter 14: Successive Refinement
* 程式一開始一開始很成功但是卻無法擴充，透過 refactored 跟 clean 的過程讓程式可以容易擴充
* 寫程式像是手工藝品一樣要慢慢精雕細琢，clean code 由 dirty code 慢慢 clean 後才產生出來的，因此不要停留在讓 code work 的階段而是要到達 clean code 的階段才算結束
* 當發現增加新功能需要改動 code 很多地方，多出很多 duplicate code 或讓 code 變得懂懂就先停下來，等到 refactoring 後才繼續
* Refactoring 不要一次大量的改動容易毀滅整個程式，要建立好測試一步步改動，每個改動後系統還是可以正常運作

## Chapter 17: Smells and Heuristics

### Comments

#### C1: Inappropriate Information
不要把不適當的資訊放在程式碼裡面像是 change history，而是要放到 source code control system，這些資訊只會把程式碼碎片化難以讀懂

#### C2: Obsolete Comment
Comment 很快的就會過時，最好的方法就是不要寫 comment，但如果發現過時的 comment 就要馬上更新或捨棄，不然過時的 comment 會誤導人對於 code 的理解

#### C3: Redundant Comment
如果 code 就已經足夠表達意圖就不要再加上多餘 comment，comment 應該只說明 code 無法說明的內容

#### C4: Poorly Written Comment
寫 comment 時要三思而後行確保寫出必須且最好的 comment

#### C5: Commented-Out Code
註解掉的 code 沒有人敢去動他，大家會預設有人之後需要用到，但隨著每天過去 code 會慢慢脫節導致不可用，所以看到註解掉的 code 就直接刪除因為 version control system 會記錄，真的需要時再去 revert 就好了

### Environment

#### E1: Build Requires More Than One Step
Building project 應該要簡單的步驟就可以成功，而不用檢查各種環境需求

#### E2: Tests Require More Than One Step
要能一個按鈕就可以跑所有的測試，最糟也要能一個 command 就能執行，能夠快速直覺簡單的執行測試是很基本的事情

### Functions

#### F1: Too Many Arguments
Function 要有越少參數越好，最多不要超過三個

#### F2: Output Arguments
Output arguments 很違反直覺的

#### F3: Flag Arguments
Boolean argument 清楚的說明 function 不只做一件事情

#### F4: Dead Function
不會被任何人呼叫的 method 就該被捨棄

### General

#### G1: Multiple Languages in One Source File
不要讓多種不同語言放在同一個 source file 裡面，最好只有一個語言

#### G2: Obvious Behavior Is Unimplemented
任何的 function 或 class 應該要實作其他人預期的行為，當沒有做好預期的行為會迫使人去看細節

#### G3: Incorrect Behavior at the Boundaries
雖然說 code 要能正確的運作但對於何謂正常運作的細節卻沒有深入思考，對於 corner case 應該要實際去證明有正常運作而不是依靠直覺

#### G4: Overridden Safeties
輕忽安全性是很危險的，關掉 compiler 的 warning 或者忽略失敗的測試都會導致往後更多的 debug 時間，所以不要忽視目前發生的警告避免悲劇發生

#### G5: Duplication
當發現 duplication 就是個 abstract level 提升的機會，讓其他人可以使用減少錯誤的發生
最明顯的 duplication 就是 code 看起來就是從其他地方不斷複製貼上，接著是 if/else chain 判斷相同的 condition，或者 module 都有相同的邏輯但不是用相同的 code

#### G6: Code at Wrong Level of Abstraction
Abstraction 要能分開 hight level 概念性的東西與 low level 的細節，要確保兩者分離清楚。
當有錯誤的 abstraction 產生後實作細節不要去欺騙，不然會產生其他問題

#### G7: Base Classes Depending on Their Derivatives
當我們有 Base 跟 Derivative class 就是為了把概念拆開到不同 class，如果發現 Base class 有提到 Derivative class 的字眼代表有問題

#### G8: Too Much Information
定義良好的 module 通常有很少的 interface 卻能讓使用者做很多事情，相反的定義糟糕的會有很多的 interface 讓使用者需要用很多步驟才能完成一件簡單的事情
好的 programmer 會限制 expose 出去的 interface，class 的 method 越少越好，Function 的 variable 越少越好
把 data, utility function, constants, temporaries 藏好，不要創造有好多 method 的 class，不要創造很多 protected variable 跟 function 在 subclass 裡面，專注於讓 interface 小而美，透過限制來降低 coupling

#### G9: Dead Code
永遠不會被執行到的就是 dead code，dead code 不會隨著設計所改變卻仍然 compile 卻不遵守新的 convention，所以看到 dead code 就把它刪掉

#### G10: Vertical Separation
Variable 或 function 定義與使用的地方要盡量靠近，private function 也要與使用的地方接近，雖然 private function 的 scope 是整個 class 但為了閱讀方便還是要接近使用的地方

#### G11: Inconsistency
一但決定使用某種 convention 就要保持一致讓 code 更好讀跟修改

#### G12: Clutter
移除掉所有造成 code 混亂的東西，像是從沒用到的變數、從不呼叫的 function、沒有意義的註解

#### G13: Artificial Coupling
不相關的東西不該刻意綁再一起，好好花時間思考該在哪邊宣告而不是貪圖方便宣告在共用的地方

#### G14: Feature Envy
當 method 使用到其他 object 的 method 去獲取資料代表該 clss 逾越自己的本分，所有 class 應該都只能關注於自己有的 variable 跟 method

#### G15: Selector Arguments
Selector argument 會包含很多 function 再一起也很難記住不同 flag 會造成的不同行為，應該要把 function 拆成很多小 function 而不是用 selector arguments 挑選需要的 function

#### G16: Obscured Intent
盡量讓 code 意圖明顯，避免使用縮寫或 magic number

#### G17: Misplaced Responsibility
把 code 放在讀者預期的地方

#### G18: Inappropriate Static
盡量使用 nonstatic method，如果要用 static method 確保永遠不需要 polymorphism

#### G19: Use Explanatory Variables
讓每個變數的名稱都有意義，就算是計算途中暫存的變數也一樣

#### G20: Function Names Should Say What They Do
Function 要能讓人不需要看內部就知道做些什麼事情

#### G21: Understand the Algorithm
要能清楚知道自己的演算法如何作用而不是不斷微調到可以運作，最終還是要把演算法 refactor 到能清楚看出如何運作的方式

#### G22: Make Logical Dependencies Physical
Module 與其他有相依性應該限於 physical 的而不是 logical

#### G23: Prefer Polymorphism to If/Else or Switch/Case
大部分人使用 switch 只是因為是種暴力解法而不是正確的解法，通常比較會增加 function 而不常新增 type 的情況比較少見所以使用 switch 要特別謹慎，針對特定 type 選擇的 switch 只能有一個，使用 polymorphism 來取代其他地方的 switch

#### G24: Follow Standard Conventions
團隊需要有 coding standard 來遵循，不需要有正式的文件因為 code 都是範例可以參考

#### G25: Replace Magic Numbers with Named Constants
不應該要單純數字出現，要把它隱藏在有意義的名稱之下

#### G26: Be Precise
每當在 code 下決定需要確保夠精確，知道為何做出這個決定跟如何處理 exception

#### G27: Structure over Convention
決定設計方式應該要優先考慮 structure 而不是 convention

#### G28: Encapsulate Conditionals
Boolean logic 很難懂所以需要提煉出意圖

#### G29: Avoid Negative Conditionals
盡量讓 boolean 為 positive

#### G30: Functions Should Do One Thing
Function 有很多 section 或做一連串的操作需要再細分成小 function 讓每個 function 只做一件事

#### G31: Hidden Temporal Couplings
當 statement 有順序關係要強迫使用者照順序使用，裡用傳遞參數的方式可以清楚表達順序關係，雖然比較複雜一點但是可以 expose 順序關係

#### G32: Don’t Be Arbitrary
思考為何如此架構 code，要能與現有的 code 一致才比較不容易被改掉

#### G33: Encapsulate Boundary Conditions
不要讓 boundary condition 散落在 code 的各處，封裝起來

#### G34: Functions Should Descend Only One Level of Abstraction
Function 中的 statement 應該都描述於比 function name 低一層的 abstract level

#### G35: Keep Configurable Data at High Levels
如果有 default 的 constant 或 config 應該要定義在 high level 的 abstraction 藉由 hight level function 當成參數傳給 low level function

#### G36: Avoid Transitive Navigation
我們不希望 module 對於其他 collaborator 知道太多，例如 A 與 B 合作，B 與 C 合作，而我們不希望 module 利用 A 來知道 C，因為如果我們想要在 B 跟 C 之前加入 Q 需要找出所以這樣使用的 pattern

### Java

#### J1: Avoid Long Import Lists by Using Wildcards
如果只要用到 package 某些 class 直接 import 整個 package，避免干擾讀者，直接說明用哪個 package 就好了
指定 import 特定 class 會有 dependency 的問題而 import 整個 package 就不會有 dependency 問題

#### J2: Don’t Inherit Constants
不要把 constant 藏在 base calls 用繼承的方式底下的 class 知道 constant，而是用 import 的方式

#### J3: Constants versus Enums
使用 Enum 取代 constant，enum 有很多的 method 跟 field 可以表達更多意思與擁有更多彈性

### Names

#### N1: Choose Descriptive Names
不要太快決定名稱，要確保是個有描述性的名字，並且隨時注意名字有沒有過時要隨著軟體做更新
不能只靠感覺來取名，軟體靠名字來提高可讀性，需要花時間挑選讓名字並讓名字彼此有關連

#### N2: Choose Names at the Appropriate Level of Abstraction
根據 abstraction level 挑選名稱，雖然很困難但需要不斷反覆思考來找出不合適 abstraction level 的名稱

#### N3: Use Standard Nomenclature Where Possible
使用常用的名稱可以讓人馬上理解作用，專案也會有自己專屬的語言，常用這些語言可以讓讀者更容易理解

#### N4: Unambiguous Names
避免不清楚在做什麼的名稱，直接說明做了什麼事情

#### N5: Use Long Names for Long Scopes
使用到的 scope 越長則需要取越長的名稱，短的名稱容易隨著距離越遠而失去意義

#### N6: Avoid Encodings
不要使用 encoding 的名稱，現代 IDE 都有能力做你想做的事，所以不需要加 prefix 之類的 encoding

#### N7: Names Should Describe Side-Effects
如果有 side-effect 也要在名稱中清楚描述

### Tests

#### T1: Insufficient Tests
測試要能快速的測試所有可能壞掉的地方

#### T2: Use a Coverage Tool!
使用 coverage tool 找出沒有測試到的地方

#### T3: Don’t Skip Trivial Tests
Trivial test 很容易撰寫而且帶來文件化的價值所以不要忽略

#### T4: An Ignored Test Is a Question about an Ambiguity
當不清楚行為的正確與否可以先寫出 ignore test 當成問題紀錄起來

#### T5: Test Boundary Conditions
仔細測試 boundary condition

#### T6: Exhaustively Test Near Bugs
Bug 通常都會聚在一起，所以當發現一個 bug 可以對附近做更密集的測試或許可以找出更多 bug

#### T7: Patterns of Failure Are Revealing
可以藉由 failed test 找出錯誤的原因

#### T8: Test Coverage Patterns Can Be Revealing
Passing test 中 code 有執行與沒執行的部分可以提供為何測試失敗的線索

#### T9: Tests Should Be Fast
慢的測試並不會被執行，所以要讓測試很快速

### Conclusion
這些清單或許永遠不會完成但卻可以拿來評估一個系統的好壞
