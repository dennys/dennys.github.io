# 如何還原 Android 12 的 Wi-Fi / 行動數據 快速開關 (免 Root)


## 需求

在 Android 11 以前, 只要從手機上面往下滑, 就可以看到快速開關來切換很多設定.

[![](https://dennys.files.wordpress.com/2022/01/old.png?w=300)](https://dennys.files.wordpress.com/2022/01/old.png)

但自 Android 12 開始, Wi-Fi 和行動數據的開關被整合成一個網際網路開關, 如下圖.

[![](https://dennys.files.wordpress.com/2022/01/internet.png?w=600)](https://dennys.files.wordpress.com/2022/01/internet.png)

所以要改變 Wi-Fi 或行動數據時, 得先進去網際網路, 然後才能修改, 改完之後還得去按那個完成. 這真的很難用也很慢...

[![](https://dennys.files.wordpress.com/2022/01/internet2.png?w=600)](https://dennys.files.wordpress.com/2022/01/internet2.png)

而且這個改變看來 Google 是有做過研究的, 可參考 https://support.google.com/pixelphone/thread/132446941/. 他的原因是很多人會因為 Wi-Fi 不穩定而改用行動數據, 然後忘了開回 Wi-Fi... 雖然我不是很懂這個改變能怎麼避免這件事情. (後面的留言也滿多人反映同樣事情)

[![](https://dennys.files.wordpress.com/2022/01/internet3.jpg?w=713)](https://dennys.files.wordpress.com/2022/01/internet3.jpg)

再往下看, 負評的人不少, 所以我也給他按負評了 :p

[![](https://dennys.files.wordpress.com/2022/01/image-10.png?w=939)](https://dennys.files.wordpress.com/2022/01/image-10.png)

不過客服也辛苦了, 被人抱怨 this sucks, 還會回 You're welcome... (參考論壇: https://support.google.com/pixelphone/thread/133649087/).

[![](https://dennys.files.wordpress.com/2022/01/image-11.png?w=915)](https://dennys.files.wordpress.com/2022/01/image-11.png)

<br><br>

## 解法 (免 root)

網路上找到解法如下 (參考 https://forum.xda-developers.com/t/4353717/ 和 https://9to5google.com/2021/12/08/android-12-internet-wifi-mobile-data-toggles/), 比較奇怪的是, XDA 上面說要 root, 但我沒 root 也成功了.

**要注意的是, 他會清除所有 toggle (就是說, 設完以後, 你的快速設定就只剩下 Wi-Fi 和行動數據兩個), 不過這再加回來就好了**.

1.  安裝 ADB (在這裡下載 https://developer.android.com/studio/releases/platform-tools), 如果 adb.exe 執行起來一直 crash, 可用舊版 https://dl.google.com/android/repository/platform-tools_r28.0.0-windows.zip
2.  打開手機的 USB debug mode (參考 https://blog.pulipuli.info/2019/01/adbfastbootandroid-sdk-platform-tools.html)
3.  執行 `adb devices`, 確定手機有連線成功
    *  如果 List of devices attached 後面沒東西代表沒找到手機 (那兩行 daemon... 只有第一次執行會出現)
        ```cmd
        C:\platform-tools>adb devices
        * daemon not running; starting now at tcp:5037
        * daemon started successfully
        List of devices attached
        ```
    *  如果看到 unauthorized 代表沒有權限 (可能是 USB debug mode 沒開)
        ```cmd
        C:\platform-tools>adb devices
        List of devices attached
        *************** unauthorized
        ```
    *  要看到這個畫面才代表連線成功
        ```cmd
        C:\platform-tools>adb devices
        List of devices attached
        *************** device
        ```
4.  連線成功後, 請執行以下指令 (請用 cmd, 用 PowerShell 比較麻煩), 他就會把 Wi-Fi 和行動網路的快速設定分開來了
    ```cmd
    adb shell settings put global settings_provider_model false  
    adb shell settings put secure sysui_qs_tiles 'wifi,cell,$(settings get secure sysui_qs_tiles)'  
    ```
<br>

## 成果

如下, 可以快速開關 Wi-Fi 或是行動網路

[![](https://dennys.files.wordpress.com/2022/01/new.png?w=400)](https://dennys.files.wordpress.com/2022/01/new.png)

最後請記得把 USB debug mode 關掉, 不然一些 App 會無法使用, 如下圖.

[![](https://dennys.files.wordpress.com/2022/01/usb-1.png?w=300)](https://dennys.files.wordpress.com/2022/01/usb-1.png)

## 補充

1. 這招還是沒法改回 Android 11 以前那種小 icon, 也就是說, 預設最多只能看到 4 個開關. 是有看到 App 可以做到這功能, 但他看起來是用一個可以跑在其他 App 上的權限, 然後再加上一個從上往下滑的手勢. 也就是說, 當你從上往下滑, 其實是開啟他的 App, 而不是系統內建的通知功能. 所以其實會兩個一起執行, 這樣怪怪的... 再找找有沒有其他解法.
1. 後來有次降版 Google App, 結果一連上藍芽就重開 (後來查到是降版造成, 更新到新版就好了), 但影響是所有 toggle 就又被重設了.

