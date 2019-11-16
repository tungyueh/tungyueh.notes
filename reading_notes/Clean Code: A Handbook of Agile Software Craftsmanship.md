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
