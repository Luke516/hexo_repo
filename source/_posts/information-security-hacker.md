title: 資訊安全<駭客入侵>
date: 2014-11-22 19:58:23
tags:
- 資訊安全
- hacker
category:
- 資訊安全與駭客技巧
---

這裡要分享的是在這學期的資訊安全課程裡，跟駭客入侵有關的作業心得。
<!--more-->

###作業一：stack buffer overflow###

這個作業的內容就是要我們利用stack buffer overflow的技巧，讓老師給的執行檔輸出"Hacked!!"的字樣。

[執行檔可在這裡下載](https://trello.com/c/1UdUowfE/31-homework3)

這個執行檔是個叫做gets的ELF，在linux環境下才能執行。而檔名也明顯的提示我們程式內是用gets這個函式來達成overflow的目標。

一開始(其實是老師的提示)，我們用objdump -M intel -d gets這個指令來觀察它：

![反組譯的結果](http://i.imgur.com/8y65UOw.png)

從上圖我們可以看到，main跟hacked兩個函式。其實從這裡我們就可以很明顯的看出，hacked就是我們的目標函式，我們要做的就是用
buffer overflow設法讓main跳到hacked的位置。

而從上圖也可以看到，hacked的位址在0804844d這個地方，因此我們就要用這個位址覆蓋過原本回傳的值。

但在那之前，我們得先知道stack的大小來決定輸入的字串長度。因此，就暴力猜測，使用不同長度的輸入字串來觀察程式的反應。結果
發現大約長度在21、22個字元左右的字串，就會讓程式產生異常(21會卡住，22就記憶體區段錯誤)。因此，我們可以知道，我們要覆蓋
的值大約就在這個範圍左右。

接著就來試試看吧！還記得hacked的位址是0804844d，因此我們關鍵的4個byte分別是4d、84、04、08(16進位)。注意因為stack的填入
順序是從右到左，所以順序是相反的。而因為裡面有些字是鍵盤打不出來的，所以再用一個小小的c程式產生我們input的字串。

產生字串後，在前面填入不同長度的任意字串做測試，最後發現在總長度26時成功Hack！

![成功Hack！](http://i.imgur.com/THdbF8o.png)

上面可以看到產生input的code，實際input的內容與程式印出Hacked！的字樣。不過因為正常的返回程序被破壞，程式最後還是會產生
記憶體區段錯誤的問題，無法避免。


###作業二：SQL injection###

第二項作業是要我們用SQL injection的方式，破解[redtiger hackit]或[hack this site]裡面的題目，由於兩題的內容其實非常類似
，因此這裡就用hack this site的內容做講解。

首先大概提一下題目的內容(雖然不怎麼重要)：就是有個網站濫殺無辜的動物做成包包或皮鞋來賣，委託人拜託你駭入他們的網站取得
用戶的電子郵件，這樣他就可以發信告訴他們這個網站的惡行(當然，這個背景是虛構的XD)

點進去他們的網站，可以看到如下的內容：

![fischer's animal products](http://i.imgur.com/NlRHaC8.png)

我們可以看到有一欄可以輸入電子郵件，另外還有兩個連結。我們先隨便打個虛構的郵件名稱看看，輸入12345@gmail.com：

![登錄12345@gmail.com](http://i.imgur.com/vGx0nUL.png?1)

登錄成功，沒有任何特別的事，檢查原始碼也看不出什麼端倪。

試著輸入非法字元看看，輸入@@@@@@@@@@：

![登錄非法email](http://i.imgur.com/EnnnxPd.png?1)

這裡我們知道了有個名叫email的table，但沒有其他特殊的訊息，從電子郵件這邊著手似乎行不通。

那來試試看另外兩個連結吧！點進去第一個會看到如下的頁面：

![毛皮大衣QWQ](http://i.imgur.com/TXzFRVZ.png)

這裡可以從URL看出是category的不同造成內容差異，那我們是否可以從這裡著手呢？

為了測試SQL injection是否可行，我們先在網址後方加上" or 1=1"，結果：

![加 or 1=1](http://i.imgur.com/baEgyEP.png)

神奇的事發生了，category=1和2的內容同時顯示出來，可見這個方向很有可能是正確的。

這時候，我們剛剛知道的email這個table名就可以派上用場了。讓我們試試看union all這個指令能不能化腐朽為神奇吧。

在原始網址的後方，讓我們加上" union all select 1,2,3,4 from email"看看會怎樣：

![union](http://i.imgur.com/Sj2XPQq.png)

出現了神奇的東西！事實上，select 1,2,3,4 也是試了幾次才試出來的，因為我們知道，union必須在欄數箱等的情況下才能進行，
因此從1欄開始試，最後發現在4欄時網頁才能正確地顯示。

現在我們已經找到了email這個table，離成功只剩一步之遙。從頁面的顯示情況我們可以發現，第2欄和第3欄的內容會以文字的方式
顯示出來。那麼，就讓我們把第二欄改成\*看看吧！(也就是 union all select 1,\*,3,4 from email)

![WOW](http://i.imgur.com/VYGl5cb.png)

WOW！email就這樣跑出來了！任務成功！

事實上，假如在試幾次會發現正確的欄名也叫email，但由於原始table似乎只有一欄，輸入*也可以得到email的結果。

這裡列出幾個能injection成功的寫法：
union all select 1,email,3,4 from email
union all select 1,2,\*,4 from email
union all select 1,\*,\*,4 from email
union all select NULL,\*,\*,NULL from email
其實只要讓第二欄或第三欄跑出email的值就好了(email或\*都可以)，其他填任意數字都OK，而NULL也是一種保險的寫法。


###其他###

其實除了這題之外，我還跑去做了一些其他題目，還滿有意思的，在此附上成績圖：
![hack this site](http://i.imgur.com/rmPIBbk.png?1)
![hack this!!](http://i.imgur.com/T7A2IwL.png?1)
![WeChall總成績](http://i.imgur.com/vW9Wwnv.png?1)


###心得###

經由這樣的練習，使我更了解所謂"駭客"實際上到底是在做什麼，又需要什麼樣的技巧。過程中因為缺乏經驗，往往卡了很久都理不出
頭緒，還要上網查別人的解法如何，才有恍然大悟的感覺。

除此之外，我也了解SQL injection不過是駭客技巧的其中一種，駭客所需要的其實是相當廣泛的知識，包括資料庫、密碼學、腳本語言
甚至是網頁語法等等，唯有全面了解網站的架構，才有辦法看出漏洞。

最後希望期末的專題可以做這個，感覺好有趣啊啊啊QWQ