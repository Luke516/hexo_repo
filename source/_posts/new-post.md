title: 用HEXO建立自己的部落格(1)安裝HEXO!!
date: 2014-07-15 22:34:34
tags: hexo
category:
- 網路與部落格相關
---

一直以來都很想寫一個部落格試試看，但是因為太懶了加上學期中太忙，所以一直沒什麼機會......
剛好最近暑假閒閒沒事，想說就來用個部落格吧！剛好最近在網路上看到其他人用HEXO做的部落格，
看起來就很厲害，加上HEXO這個FrameWork還是台灣人開發的呢，想說就來試試看吧！
經過一番折騰，結果就是現在這個網站了！

那麼，就先來介紹一下HEXO吧！
<!-- more -->

##什麼是HEXO?

HEXO是一套基於node.js的部落格程式，開源而且免費，作者是台灣的tommy351，
它可以產生靜態網頁，跟其他有名的jekyll、Octopress、Wordpress等程式相比，它的特點是快、輕量、高擴展性，
熟悉之後所需要的指令也很簡單。
雖然這麼說，HEXO還是比較適合有程式或電腦相關底子的人使用，畢竟它會用到Github,Git,Markdown,Node.js等
工具，還有許多插件之類的要自己處理，對於沒有程式背景的人可能稍嫌複雜了一點。

那麼，接下來簡單介紹一下安裝過程吧。

##安裝HEXO

安裝HEXO本身非常簡單，只是在安裝前，你還需要先裝兩樣東西：git和node.js
安裝git:你可以在[這裡](http://msysgit.github.io/)找到Windows版的git，安裝程序應該不難
安裝node.js:到它的[官網](http://nodejs.org/)下載安裝檔然後照著執行就好了

這兩樣東西都裝好後，就可以開始安裝HEXO了！
安裝HEXO需要以下指令：

``` bash
$ mkdir <folder>
```
在你想要的地方創建資料夾

``` bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```
初始化環境，HEXO會自己裝好基本的東西
執行完之後，你的資料夾裡應該會出現以下的東西:
```bash
.
├── _config.yml
├── package.json
├── scaffolds
├── scripts
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
這樣安裝就完成了！還滿簡單的吧！
假如你這時候想測試看看目前的成果，可以用以下的指令：
```bash
$ hexo generate
$ hexo server
```
其中generate會生成所有靜態文件，server會在預設的本地端執行
這時候在打開瀏覽器，輸入localhost:4000，你應該就會看到一個漂亮的預設頁面了！





