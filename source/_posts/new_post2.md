title: 用HEXO建立自己的部落格(2)把網站放到GitHub Pages上!!
date: 2014-07-16 06:47:00
tags: hexo
category:
- 網路與部落格相關
---

上一篇文章講了安裝HEXO的基本流程，現在就來說明如何把你的部落格部署到GitHub Pages上吧！
<!-- more -->

##部署到GitHub Pages

首先你需要先有一個GitHub帳號，沒有的人可以到[這裡](https://github.com/)申請~~很簡單的
第二步，用你的GitHub帳號新增一個repository，它的名字必須是<你的帳戶名稱>.github.io
再來去你安裝HEXO的資料夾，裡面應該有個檔案叫_config.yml，這個檔案裡存了你一些基本的設定，
打開它，移到最底下有個deployment的地方，改成下列的格式：
```bash
deploy:
  type: github
  repository: https://github.com/<帳號名稱>/<帳號名稱>.github.io.git
  branch: master
```
最後執行下列的指令
```bash
$ hexo generate
$ hexo deploy
```
基本上這樣就OK了！接著只要到你的GitHub Pages(http://<帳號名稱>.github.io/)，
應該就能看到之前的HEXO範例網頁囉！
不過，剛申請GitHub Pages可能會有一小段時間的延遲，所以可能要等一下(最多大概10分鐘)就是了！

##一些可能遇到的問題

這裡講一下我在過程中遇到的一些問題！主要是發生在執行
```bash
$ hexo deploy
```
的時候

###問題1.出現fatal: Not a git repository (or any of the parent directories): .git
解決方法：
移除deploy檔再重來一次，也就是
```bash
$ rm -rf .deploy
$ hexo deploy
 ```
 這樣應該就OK了！

###問題2.出現錯誤Error: spawn ENOENT
```bash
Error: spawn ENOENT
    at errnoException (child_process.js:980:11)
    at Process.ChildProcess._handle.onexit (child_process.js:771:34)
```
因為因為我一開始都是在node.js的命令列執行指令，後來發現這樣會產生上面的錯誤

解決方法：
在git bash中執行指令就好了

###問題3.中文無法正確顯示，出現亂碼
解決方法：
不要用windows內建的筆記本或wordpad來編輯相關文件，會有編碼的錯誤
有一個叫Sublime免費文件編輯器還不錯，可到[這裡](http://www.sublimetext.com/)下載







