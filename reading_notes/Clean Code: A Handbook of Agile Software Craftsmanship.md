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
