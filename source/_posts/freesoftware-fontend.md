title: 自由軟體心得<八>字音字形離線版開發心得
date: 2014-12-27 07:28:21
tags:
- 自由軟體
- 社群
category:
- 自由軟體與社群
---

因為自由軟體與社群發展這堂課，讓我有機會參與字音字形離線版的開發過程，真的覺得是個很難得的機會~雖然我個人認為目前的成果
離真正「完成」還有相當大的距離，但好歹也算是暫時有了階段性的成果，就把握機會分享一下吧。(其實只是為了衝文章數XD)

<!--more-->

一開始我會選擇這項project，就是想練習自己寫網站的能力。結果也真的讓我獲益良多。在一開始的討論裏，我們因為組員眾多，討論
起來有點繁瑣，也因此花了不少的時間才決定基礎的版型，每個人也都認領了自己的那部份工作。

![討論ㄉ照片](http://i.imgur.com/ZETAfs9.png)

還有一點我覺得很有意思的點，就是我們使用GitHub來做版本管理的任務，而在我們這組裡面，除了吳子晨之外都是第一次使用Git甚至
是GitHub，所以大家一開始其實都滿辛苦的，要同時學習不少東西，包括commit的格式要寫清楚，還有不同branch之間做切換，或者是
遇到兩個版本衝突時要怎麼解決等等，這一路用下來，我覺得Git雖然方便，但是要上手還真是有一定難度！

而我的第一份工作是製作選單的部分。由於對html和css還算有基礎的概念，加上bootstrap這個樣板的輔助，因此單純排版的部分我其實
滿快就做完了，雖然如此，我業在過程中體認到網頁的排版其實也不如想像中簡單，尤其是要同時考慮電腦與手機等不同裝置顯示的時候
。像我的選單是由許多按鈕組成，但每個選單按鈕的數量又不一樣，導致整個區域的高度會不斷變動，十分惱人，但如果固定高度的話，
在手機顯示時又會超出畫面，這個問題到最後也還沒一個完美的解決方式，只能說我們要學習的東西還有很多吧。

當大家把各自的排版都弄得差不多時，再來就是最麻煩的──邏輯的部分了。我們的網頁是用javascript來處理背後的邏輯，而我們通樣的
幾乎從來沒碰過javascript......因此一開始可以說是跌跌撞撞，比預定的時程還慢了一大截。

雖然對腳本語言不太熟悉，導致開發進度緩慢，但我遇到第一個最大的問題是在處理題庫的部分。為了方便javascript讀取，子晨提議
把題庫轉為JSON的格式，而這個工作不知如何就落到了我的頭上。一開始我試著將google試算表直接轉成JSON，但上網試了許多方法，
出來的結果都不盡人意；後來我才發現可以先將google試算表轉成CSV檔，再轉成JSON，整個流程就順利了不少。然而工作沒有就這樣
結束，要如何讀這些題目也是一大問題，我一開始直接把題庫當成一個全域變數，但不知為何都讀不到，後來才發現是因為javascript
會有非同步的狀況，最後採用getJSON函數才解決了這個問題。

另外一個問題就是在不同畫面之間做切換的時候，原本想說是不是要用參數來傳遞資訊，後來因為太麻煩了，乾脆全部用全域變數來存
，而因為我們的網頁是採用AJAX的架構，有時會產生一些難以察覺的問題，比方說網頁裡面嵌入的網頁又有另一個html標頭，導致某些
奇怪的狀態，又像是題目在切換不同版型時，會因為讀檔太慢而無法顯示正確題目等等，這些需要經驗才能發現問題到底在哪裡。

總體而言，在這個project中我學到了許多東西，一個網頁要寫得好，背後真的需要許多經驗和技術，雖然我們最後勉強做出了還能看的
成品，但我認為還有非常多改善的空間。除此之外，我也發現團隊合作中溝通的重要性，雖然Git很好用，但若沒有好好溝通仍會出現許
多問題，像是很多人同時修改同意個檔案，導致不知道該用哪個版本，或者是語法不一致讓邏輯難以處理等等，合作真的不管在哪裡都
是相當需要學習的一門學問呢！

最後附上我們的網站網址：http://ezgo-wordtest.github.io/Frontend/