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
