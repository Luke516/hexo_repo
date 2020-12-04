title: 自由軟體與社群發展心得<五>
date: 2014-11-21 22:19:26
tags:
- 自由軟體
- 社群
category:
- 自由軟體與社群
---

今天部落格的速度終於跟上筆記的速度了XD



我們今天的主題是放在OpenStreetMap這個開放街圖的自由軟體上，而來為我們介紹的講者是Cindy李昕迪(@mcdlee)小姐，她除了是
OpenStreetMap的貢獻者之外，還是高雄榮總核子醫學科的住院醫師喔！
<!--more-->

先來看看OpenStreetMap長什麼樣子吧~~

<iframe width="425" height="350" frameborder="0" scrolling="no" marginheight="0" marginwidth="0" src="http://www.openstreetmap.org/export/embed.html?bbox=120.19701719284058%2C22.989903614727677%2C120.22096395492554%2C23.002268657162283&amp;layer=mapnik" style="border: 1px solid black"></iframe><br/><small><a href="http://www.openstreetmap.org/#map=16/22.9961/120.2090">查看更大的地圖</a></small>

備註：這次的筆記有點混亂，想看更詳細的內容可以看看講者的投影片~ [介紹OSM](http://mcdlee.github.io/20141121_NCKU/) 跟 [編輯地圖](http://mcdlee.github.io/editor_tutorial/) QWQ

###OpenStreetMap###

OpenStreetMap(中文譯作開放街圖)是2004年由Steve Coast成立的一個專案，open source和wikipedia的精神，整個網站都是開放授權
的自由軟體，並且格式也是開放的xml格式。

OpenStreetMap裡面包含了包羅萬象的地理資料庫：
1.最基本的：像是街道、路名、地名
2.還有更詳細的：商店、設施、限速等等
3.甚至還會有一些神奇的資訊如：飲水機、板凳、樹種等等！

與google map一樣，OpenStreetMap可以拿來作為地圖的底圖。

OpenStreetMap的台灣社群：
社群官網：[http://openstreetmap.tw/](http://openstreetmap.tw/)
FB：[https://www.facebook.com/OpenStreetMap?fref=ts](https://www.facebook.com/OpenStreetMap?fref=ts)

OpenStreetMap的貢獻者稱為mapper，圖客。"自己的地圖自己畫"。手機GPS加上合適的軟體。紀錄tracker，做時間地點或照片的比對。

OpenStreetMap還有另一個來源，就是空照圖。Cindy提到之前微軟的Bing就曾和OpenStreetMap合作，提供空照圖，讓OpenStreetMap的
資料有了大幅的成長。

來源一定要有合法授權，

美國政府Tiger計畫，也提供了OpenStreetMap大量的美國區域地圖。

還有一個網站[Field Papers](http://fieldpapers.org/)，可以印出一張地圖，讓你簡單的在上面編輯，之後再拍照上傳，就可以對OpenStreetMap做出貢獻！

OpenStreetMap成長速度很快，已經有將近兩百萬註冊用戶，但只有約1%的活躍貢獻者，有許多人只使用它的功能，卻只有少數人協助
讓OpenStreetMap變得更好，相當可惜。

2007台灣開始比較多人使用，到2014約有400多個ip在台灣活動。

OpenStreetMap是以類似維基百科的方式，讓所有人都能編輯(Wiki-style)因為開放所有人編輯，難免會有人出問題。

好處：一些不被官方承認的地方也有人去做地圖。
壞處：也會有人來亂，塗鴉，鐵軌被破壞。

![鐵軌被破壞 Q_Q](http://mcdlee.github.io/20141121_NCKU/pics/ch.png)

Wiku style的特色：
1.可能會發生悲劇，但多半有解決方案
2.所有紀錄都有跡可循
3.半成品也很歡迎(Release early, release often)

開放授權

資料：ODbL
圖磚：CC-BY-SA

###應用###

wheelmap.org，標示輪椅無障礙空間的友善程度，用OpenStreetMap作底圖

素食地圖、蠟筆/手繪地圖、救災應用(菲律賓海燕颱風，標示房屋受損程度)、導航(OSMand，可離線)、FOURSQUARE、
蘋果日報、open historical map

![蠟筆地圖，滿可愛的(但是不知道有什麼用 QWQ)](http://mcdlee.github.io/20141121_NCKU/pics/crayon.png)

###格式###

交換格式OMS XML，標籤用Key and value的方式來儲存，可以自由添加標籤，但也因此建議用統一的格式，否則可能會很混亂。

可輸出geoJSON、KML


###實做###

前面的筆記很凌亂，連我自己都看不太懂，所以也懶得改了 QWQ

最後來說一下要怎樣實際的編輯OpenStreetMap吧！如果你是用手機或平板的話，有一個叫做[go map!!](http://wiki.openstreetmap.org/wiki/Zh-hant:Go_Map!!)的軟體，可以讓你當場拍照，然後
馬上編輯，還滿方便的！有興趣的可以到他們的[網站](http://wiki.openstreetmap.org/wiki/Zh-hant:Go_Map!!)去看看更詳細的教學~~

而如果是用電腦的話，有兩種方式可以選擇，分別是iD跟JOSM。

今天上課我們先用iD做編輯，因為iD的好處就是方便，不需要下載額外的軟體，只需要瀏覽器就可以直接使用，並且介面也相當人性化；
相對的，假如你想要編輯更複雜的關係或是更多自訂功能的話，就可以試試看下載JOSM的軟體！

而自己實際編輯的感想是，iD的操作真的還滿好上手的，不論是點、線、面或是基本的添加標籤都相當方便。不過有個小問題就是地圖
載入的速度感覺有些緩慢，有時候你看到沒東西想編輯，結果編輯完才發現其實有仁已經做了，只是剛剛還沒跑出來= =。這種感覺其實
滿煩人的，不知道是不是太多人同時編輯的關係？

除此之外，編輯完的結果你也無法馬上看到，因為OpenStreetMap是全球同步更新，好像一天左右才會全部更新一次，沒辦法馬上看看
自己的成果，也是有點可惜。

最後，聽說要附上課的照片當證明，哭哭。

[想看得自己點啦(傷眼慎入)](http://i.imgur.com/1c1Hp8t.png)