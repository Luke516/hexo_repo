title: 用HEXO建立自己的部落格(4)發表文章!!
date: 2014-07-19 06:16:42
tags: hexo
category:
- 網路與部落格相關
---

這篇文章要談的是關於如何透過HEXO發表文章！

<!--more-->

##建立新文章

在HEXO的根目錄，執行以下的指令來建立新文章：
```bash
hexo new [layout] "<name>"
```
HEXO的layout就我所知有兩種，分別是post和page
一般我們要寫文章的話，要建立的是post，而post也是預設的layout，
簡單來說，如果你只是想新增一篇文章，你可以這麼做：
```bash
hexo new post "<name>"
hexo new "<name>"
```
兩種做法都可以，而
```bash
hexo new page "<name>"
```
應該是用來新增頁面的，我目前還沒試過。

##文章內容

在建立了一篇新文章後，進到HEXO資料夾裡的source資料夾，你應該會看到一個新的.md檔，
那就是你新增的文章了！打開它，你應該會看到如下的內容：
```bash
title: (your title)
date: 2014-07-19 06:16:42
tags: 
---
```
前三行的功用應該很明顯吧！分別就是標題、日期和標籤，都可以自由修改，
而在---底下的空間就是你寫文章的地方了！
在這裡寫文章使用的是markdown語法，底下就說明一下常用的功能吧~~

###標題
在markdown語法裡，標題的寫法有兩種，我個人喜歡用Atx語法，如下：
```bash
#title
###title
```
結果就像這樣：
#title
###title
只要在標題的前面加上井字號就好了，一個井字號最大，最小是六個井字號。

###粗體和斜體
在字的兩端加上星號，一個是斜體，兩個是粗體。
另外，刪除線是在兩端加上兩個~符號。
```bash
*斜體*
**粗體**
~~刪除線~~
```
結果：
*斜體*
**粗體**
~~刪除線~~

###插入圖片

先在HEXO的source裡開一個資料夾放你的圖片，假設叫images，
使用下列寫法：
```bash
![](/images/<picture_name>) 
```
如果是連網路空間上的圖片也一樣：
```bash
![](<網址>)
```
這樣應該就OK了~~

更詳細的介紹可以參考：
[markdown語法說明](http://markdown.tw/)
[markdown簡明語法(簡體)](http://ibruce.info/2013/11/26/markdown/)
