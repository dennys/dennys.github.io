# XBMC Babylon 9.04 安裝筆記


這篇是寫給自己看的, 這樣以後才知道 upgrade 時要作些什麼...  
[XBMC](http://xbmc.org/) 現在的安袝比以前簡單多了, download, 解開, 就可以執行了, 鄀要設定的地方很少了, 只做了幾個動作.

1.  把喜歡的字型放到 media\\Fonts, 戁是直接把 arial.ttf 蓋掉.
2.  把 language 目錄下用不到的語言檔刪除.
3.  把 skin\\PM3.HD\\language 目錄裡面用不到的語言檔都刪除.
4.  修改 skhn\\Project Mayhem III\\NTSC\\Font.pml 或是 skin\\PM3.HD\\720p (看你用哪個 skin), 把字型放大, 因為懶得研究哪個設定是對應到那個項目的, 所以我把所有小於 24 的全部都改成 24. 不過說實在的, 覺得還是有點小.
5.  如果是全新安裝, 到這裡就可以了. 如果是升級, 把之前的 UserData 目錄拿來用, 就所有的設定和資料都回來

幾個比較重要的設定檔如下:  

1.  UserData\\guisettings.xmd: 所有的 GUI 設定都在這裡, 萬一設定搞爛掉了 (設定錯誤的字型或 skin, 會讓整個畫面的字都亂掉), 可以備份這個檔案回來即可
2.  UserData\\sources.xml: 這檔案放的是外部資源的設定, 譬如設定把家裏其他電腦用網路芳鄰分享出來的目錄, 就會存在這個檔案裡面

最後, 關於 XBMC 9.04 Babylon 的新功能可參考下列文章.

*   [XBMC Babylon Alpha 1 中文影片資料庫模式](http://fafner-hideaway.blogspot.com/2009/04/xbmc-babylon-alpha-1_06.html)
*   [XBMC Babylon Beta 1 and ESPN Video Plugin](http://fafner-hideavay.blogspot.com/2009/04/xbmc-babylon-beta-1-and-espn-video.html)
