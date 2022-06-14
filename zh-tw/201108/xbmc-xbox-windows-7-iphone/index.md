# XBMC (XBox, Windows 7) + iPhone


  
[XBMC](http://xbmc.org/) 真是個好東西, 但是一方面 XBox (一代, 不是 XBox 360) 已經很老舊了, 再加上 [XBMC 決定不再維護 XBox 版本](http://xbmc.org/theuni/2010/05/27/farewell-xbox/)之後, 就一直在尋找替代產品. [Apple TV](http://www.apple.com/appletv/) 是個不錯的東西, 而且也有 [XBMC for iOS](http://wiki.xbmc.org/?title=XBMC_for_Mac_on_Apple_TV), 只是一來得花錢買, 二來還是得 JB. 另外 XBMC 也有 [Windows](http://xbmc.org/download/) 的版本, 不過幾年前用過是覺得不太穩定, 效能也沒有很好, 另外就是, 用 PC 的話, 就沒有遙控器了, 用滑鼠來操作 PC 然後接電視, 總是覺得怪怪的.  
  
前兩天剛好發現, 有 XBMC remote for iPhone 的軟體, 也有好幾個, 於是就想到了, 或許用 iPhone 來遙控 XBMC 也是個不錯的想法, 於是就測試了一下, 基本上沒有什麼大問題, 除了 iPhone 遙控器用起來還是有點怪怪的.  
  
  
  
**iPhone for XBMC**  

*   [XBMC Constellation](http://itunes.apple.com/app/xbmc-constellation/id437807301): 我其實最剛開始是看到這個, 可以看一下[這裡](http://forum.xbmc.org/showthread.php?t=104567)的介紹, 看來在 iPad 上面還真的滿炫的, 就是因為看到這個才想來試試看. 只是測過之後, 只有一點點功能可以用, 可以播放影片, 但無法 pause 或 stop, 所以後來就放棄了, 不知道是哪裡出了問題, 可能等下個版本再來試試看.
*   [SoulMote](http://itunes.apple.com/us/app/soulmote/id424477236): 這個用起來算是穩定的, 可以連上 XBMC, 可以放影片, 也可以放圖片. 播放影片時可以 pause, stop 等等. 也可以模擬遙控器的上下左右來控制 XBMC
*   [XBMoteC](http://itunes.apple.com/us/app/xbmotec/id386296182): 這個我連 XBMC 總是失敗

btw, 我只有看免費的, 要錢的倒是沒有研究.  
  
**XBMC for Windows 7**  
要用 XBMC for Windows, 有個和 XBox 版本不一樣的需求是, XBMC 要能在播放時自動偵測第二個螢幕, 並且使用全螢幕. 另外當然最好能夠是 [portable](http://zh.wikipedia.org/wiki/%E7%B6%A0%E8%89%B2%E8%BB%9F%E9%AB%94) 的. 一開始測試時發現 XBMC 並不會去自動偵測雙螢幕和使用全螢幕播放, 感覺有點不太方便, 但是找了一下, 發現已經有人試出來了. 用一個批次檔就搞定了. 不過請注意是 Windows 7 限定, 如果是其他版本, 就麻煩許多, 需要 3rd party 軟體幫忙控制螢幕切換.  
  
  
`@echo off C:\Windows\System32\DisplaySwitch.exe /extend start /WAIT xbmc.exe -p C:\Windows\System32\DisplaySwitch.exe /external`  

*   \-p 參數的目的是變成 portable 版本, 意思就是說, 所有的資訊都存到 XBMC 目錄
*   DisplaySwitch.exe 是 Windows 7 的工具程式, 用來快速切換螢幕模式

另外中文的部份, 研究了一下, 發現不難, 一樣把 arial.ttf 換成自己喜歡的中文字型就可以了.  
  
**XBMC for XBox**  
雖然 XBMC 已經不再維護 XBox 版本了, 但是就像大多數的 open source project 一樣, 總是會有有興趣的人接手. 新專案的名字很直覺, 就叫做 [XBMC4XBox](http://www.xbmc4xbox.org/). 要注意的是, XBMC.org 上面的 link 是指到 [http://xbmc4xbox.sourceforge.net/](http://xbmc4xbox.sourceforge.net/ "http://xbmc4xbox.sourceforge.net/"), 這個 link 的內容不多.  
  
比較特別的是, XBMC for XBox 因為 license 的關係, 不能直接放出 binary, 只能放 source code 然後有興趣的人自己用 XBox SDK 去 compile 出來用. 但是在討論區裡就看到 [http://sshcs.com/xbmc/](http://sshcs.com/xbmc/ "http://sshcs.com/xbmc/") 有放新版的 XBMC for XBox. 不知道是否因為 XBox (一代) 已經不再賣了所以放寬 license 限制了嗎?  
  
**結論**  
其實用起來還滿好玩的, 就在 iPhone 上面選你要播放的內容, 就可以播放了, 基本上沒什麼問題. 只是一般來講在看電視用遙控器時, 應該是不太會用眼睛去看按鈕的位置, 而是憑感覺去按. 只是 iPhone 是觸控的, 所以基本上不太可能有什麼感覺, 這部份可能要習慣一下.  
  
**參考:**  

*   [Can\_XBMC\_be\_run\_in\_portable\_mode](http://wiki.xbmc.org/index.php?title=XBMC_for_Windows_specific_FAQ#Can_XBMC_be_run_in_portable_mode.3F)
*   [How does displayswitch.exe work in Windows 7?](http://answers.microsoft.com/en-us/windows/forum/windows_7-hardware/how-does-displayswitchexe-work-in-windows-7/1d30ef62-881a-4a69-ac02-e496b1f794ef)
*   [[WINDOWS] HOW-TO start XBMC in fullscreen on secondary monitor with custom resolution](http://forum.xbmc.org/showthread.php?t=44617)
*   [XBMC Babylon 9.04 安裝筆記](http://dennys.tiger2.net/node/3082)
