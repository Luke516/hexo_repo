title: C4 Labs參加心得<一>
date: 2014-10-17 23:39:19
tags:
- 自由軟體
- 社群
category:
- 自由軟體與社群
---

###前言###

這是這學期第C4 Labs的第二次聚會，不過因為本人第一次沒參加到，所以也算是我這學期的第一次！

今天的主題主要由KK分享終端機環境下各種方便或有趣的程式，以及子晨大大分享一個名叫Zend的網頁Framework，但在進入內容之前，
還是先來介紹一下C4 Labs到底是什麼東西吧！
<!--more-->

###C4 Labs###

C4 Labs是一個成大校園內，一群對資訊相關領域有興趣的學生們聚在一起所形成的社群組織。討論的範圍從硬體到軟體甚至到網路相關
都有，並且特別重視open source(開源)與maker(自造)的精神。

![C4 Labs](/image/C4Labs.jpg)

話說C4 Labs原本取名的用意似乎只是拿"come for Labs"的諧音來取的，後來的人又穿鑿附會(?)的加上了"cross college colLabsorative 
community"這樣的意思。而C4 Labs目前的核心成員雖然不多，但幾乎每個人都非常厲害，在不同領域上都有專長之處，從程式語言、
作業系統甚至到硬體，都可以找到人來請教，令人自嘆不如 QWQ

目前C4 Labs大概一到兩個禮拜會舉辦一次聚會，讓成員們能分享自己在資訊各領域的知識。有深入的技術，也有一些比較好理解的常識，
總之能夠大家一起討論學習，是個滿不錯的地方。

###終端機(Terminal)###

今天的第一個主題是由KK大大分享終端機環境下一些好用、有趣的程式。

什麼是終端機呢？一般電腦的使用者可能對什麼是終端機可能不是很有概念。事實上，我們可以把終端機想成一種與圖形化介面相對的
環境。大部分的終端機都是以黑底白字的畫面來呈現，並且內容通常全部都是文字，操作上也是輸入一行一行的指令，沒有圖示可以點。

![terminal 示意圖](/image/terminal.jpg)

一般人可能對terminal的環境比較陌生，但做資訊這方面的人，特別是使用linux相關系統的，應該都很常跟terminal打交道。而講者
今天就是介紹一些在terminal環境下好用的程式了！不過我個人目前還是用windows居多，所以也沒實際用過，就當參考吧！

1.[ranger](http://ranger.nongnu.org/)
這是個終端機上的檔案總管。提供了檔案總管的功能，讓你可以快速在資料夾之間切換，不然切換目錄要一直cd，是有一點麻煩。
除此之外，還支援UTF-8編碼、熱鍵、滑鼠控制和書籤等功能，還能自動偵測不同類型的檔案要用什麼程式開啟。

2.[catimg](https://github.com/posva/catimg)
這個程式讓你在終端機上觀看圖片。剛剛提過，terminal應該是沒辦法開圖片的，但這個catimg程式，讓你能模擬顯示圖片的效果
，雖然畫質有點......嗯，你懂得。看個範例圖：

![catimg 範例圖](/image/catimg.jpg)

我個人覺得可能實用性不高，而且開解析度太高的圖片會大到完全看不出來......不過滿有趣的 XD

3.[CMus](http://www.tuxarena.com/static/cmus_guide.php)
可以在終端機上播放音樂

4.[nano](http://www.nano-editor.org/)
文字編輯器

以上兩個我沒有認真聽，有興趣的自己點進去網站上看吧 QWQ

接下來的程式比較偏趣味性~

5.[cmatrix](http://www.asty.org/cmatrix/)
![cmatrix](/image/cmatrix.gif)
它很酷，沒了。(還很吃資源)

6.[yes](http://zh.wikipedia.org/wiki/Yes_%28Unix%29)
其實這只是一個指令而已，在linux環境下只要輸入yes它就會一直幫你輸出yes，在安裝東西時還滿好用的？！

7.[cowsay](http://zh.wikipedia.org/zh-tw/Cowsay)
同樣是個指令，可以(用文字)印出一隻說話的牛牛，滿可愛的。

![cowsay](/image/cowsay.png)

好了，其實以上的這些東西都不是重點！今天的精華其實根本就在下面這一段啊！

8.[ponysay](https://github.com/erkin/ponysay)
![ponysay](/image/ponysay.png)

嗚哇，是小馬耶！超可愛的小馬~~雖然說是cowsay的加強版，但是可愛的程度根本不能比啊！而且幾乎所有主要角色都有喔！
可惜windows不能裝 QAQ，這大概是我上大學以來最想換作業系統的時候......

![再來個pinkie pie吧](/image/ponysay2.png)

9.rogue-like game
以為在純文字界面下就沒辦法玩遊戲了嗎？大錯特錯~~terminal環境下的遊戲雖然也許不是那麼好看，但數量和品質可能都會讓一般人
大吃一驚喔！其中講者主要介紹的是rogue-like的遊戲，比較有名的像是NetHack、drawf fortess(矮人要塞)等！

有興趣的自己去查吧！


###Zend###

後半段是一個網路應用程式的開發框架Zend framework的介紹。

[Zend官網](http://framework.zend.com/)

說實在的，網路相關的知識我目前真的很淺薄......除了它是物件導向，在PHP5的環境下執行外，其他我真的沒啥印象 QWQ

以下說明來自[WIKI](http://zh.wikipedia.org/wiki/Zend_framework)：

1.所有元件完全物件導向，符合E STRICT錯誤報表。
2.松耦合（Use-at-will）設計可以讓開發者獨立使用元件，每個元件幾乎不依賴其他元件。
3.預設提供了強壯而高效的MVC實現和基於PHP的樣板。
4.經由PDO，支援多種資料庫，如MySQL，Oracle，IBM DB2，Microsoft SQL Server，PostgreSQL，SQLite和Informix Dynamic Server。
5.支援多種信件收發系統，如mbox，Maildir，POP3和IMAP4
6.靈活的快取機制，支援多種快取方式，可以將快取寫入記憶體或是檔案系統。

此外聽說PHP在Zend框架下，效能並不是很好，講者子晨大大也曾跟我說，Zend拿來當練習不錯，但實際上要做東西時它會想用別的
framework......總之我是不懂，有興趣的人也自己去看看吧(不負責任ㄏ)


以上，介紹完畢！