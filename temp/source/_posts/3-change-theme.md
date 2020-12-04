title: 用HEXO建立自己的部落格(3)更換主題!!
date: 2014-07-17 01:07:40
tags: hexo
category:
- 網路與部落格相關
---

其實我覺得原本預設的主題landscape就很好看了，不過大家多少都會想要對自己的部落格有些自訂的改變吧！
所以這裡就來稍微講一下如何更換主題吧！
<!--more-->
這裡就以一個相當有名的主題[pacman](https://github.com/A-limon/pacman)來當範例吧！
其他主題也可以到HEXO的[網站](https://github.com/hexojs/hexo/wiki/Themes)上找喔！

##更換pacman主題

首先進到你HEXO的根目錄，執行指令
```bash
$ git clone <repository> themes/<theme-name>
```
這裡的repository和theme-name不同主題會不同，像我現在要裝的pacman主題就是
```bash
$ git clone https://github.com/A-limon/pacman.git themes/pacman
```
這個指令會到你指定的repository把檔案複製到你的themes資料夾裡面！

完成之後，打開你根目錄的_config.yml，更改theme的位置，把原本的landscape改成pacman
接著重新部署，應該就會看到更換主題之後的樣子了！

###遇到的一點小狀況

值得一提的是，本人在第一次試著換主題時一直遇到一個惱人的問題，
就是只要換成其他主題，在deploy的時候都會出現以下的錯誤：
```bash
[error] { name: 'HexoError',
  reason: 'incomplete explicit mapping pair; a key node is missed',
  mark:
   { name: null,
     buffer: 'categories: Categories\nsearch: Search\ntags: Tags\ntagcloud: Tag
Cloud\nprev: Prev\nnext: Next\ncomment: Comments\ncontents: Contents\narchive_a:
 Archives\narchive_b: Archives: %s\npage: Page %d\nrecent_posts: Recent Posts\nm
enu: Menu\nlinks: Links\nrss: RSS\nshowsidebar: Show Sidebar\nhidesidebar: Hide
Sidebar\nupdated: Updated\n\u0000',
     position: 167,
     line: 9,
     column: 19 },
  message: 'Process failed: languages/default.yml',
  ```
  這是什麼問題我到現在還不知道，也查不到相關的資料QWQ
  總之權宜的解決之道就是把出問題的檔案：主題資料夾/languages/default.yml給刪掉！
  不過我可以這樣做是因為我是用中文版本，所以用不到預設的語言設定
  假如你遇到了這樣的問題而且想繼續用預設語言，那......我也不知道該怎麼辦就是了XD


  ##自定義顏色和圖片

  當然，換了主題之後，應該也會有把主圖修改的更漂亮的想法，尤其是預設的配色或
  圖片可能不是那麼令人滿意，所以這裡就稍微講一下修改圖片和配色的方法吧！

  ###修改圖片

  其實只是要改圖片的話非常簡單，以我用的主題pacman為例，所有的圖片都存在
  pacman\source\img 這個資料夾裡，你只要把裡面的圖片換成你想要的就好了！

  而如果你想改變預設檔名的話，就打開pacman\_config.yml，裡面應該有一段內容：
  ```bash
  #### Image
imglogo:
  enable: true             ## display image logo true/false.
  src: img/logo.svg        ## `.svg` and `.png` are recommended,please put image into the theme folder `/pacman/source/img`.
favicon: img/favicon.ico   ## size:32px*32px,`.ico` is recommended,please put image into the theme folder `/pacman/source/img`.     
apple_icon: img/pacman.jpg ## size:114px*114px,please put image into the theme folder `/pacman/source/img`.
```
把裡面的檔名改成你想要的就好了！

###更改配色

配色的設定在pacman\source\css\_base\variable.styl這個檔案裡，把它打開來，
你應該會看到這樣的內容：
```bash
//Color
color-background = #dddddd
color-font = #817c7c
color-white = #ffffff
color-blue = #2ca6cb
color-theme = #ea6753
color-font-nav =#E9CD4C
color-section = #fafafa
color-footer = #1f1f1f
color-gray = #CCC
color-heading = #333333
color-code = #eee
color-twitter = #00aced
color-facebook = #3b5998
color-weibo = #eb182c
color-google = #dd4b39
color-qrcode= #49ae0f
color-top = #762c54
```
這裡就是修改配色的地方囉~~至於哪一行是改哪裡的配色，推敲一下或試試看應該就知道了！
順帶一題，這裡的配色使用的是16進位的色碼喔！

附上色碼表：
![](/image/color-code.png) 

