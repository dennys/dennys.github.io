# Android 移除 Root 和 Magisk


<p><mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-vivid-red-color"><strong>注意: 系統刷機有一定的風險, 可能導致手機無法開機和失去保固, 資料還會被清空, 執行前請先三思!!!</strong></mark></p>

<p><strong><mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-luminous-vivid-amber-color">注意: 系統刷機有一定的風險, 可能導致手機無法開機和失去保固, 資料還會被清空, 執行前請先三思!!!</mark></strong></p>

<p><strong><mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-luminous-vivid-orange-color">注意: 系統刷機有一定的風險, 可能導致手機無法開機和失去保固, 資料還會被清空, 執行前請先三思!!!</mark></strong></p>

<p>======================================================</p>

## 需求

在上次這篇 [Android 11 如何不要驗證 Wi-Fi CA 憑證](http://localhost:1313/zh-tw/posts/202203/add-do-not-validate-android-11-peap-mschapv2-wifi/) 有說明如何修改系統設定檔, 修改完後, 其實就不需要 Root 了, 因此可以 UnRoot. 方法如下:

<p>1. 將手機重開, 並同時按下 電源+音量下鍵進入 bootloader 模式</p>

<p>2. 用 fastboot 刷回原始的 boot.img</p>

<pre class="wp-block-code"><code>fastboot boot boot.img</code></pre>

<p>2.1 其實 Magisk 有這個功能刷回原始的 boot.img</p>

<a href="https://dennys.files.wordpress.com/2022/02/image-3.png"><img src="https://dennys.files.wordpress.com/2022/02/image-3.png?w=289" alt="" class="wp-image-327"/></a>

<a href="https://dennys.files.wordpress.com/2022/02/image-4.png"><img src="https://dennys.files.wordpress.com/2022/02/image-4.png?w=296" alt="" class="wp-image-333"/></a>

<p>2.2 但我實測按下還原原始映像檔是沒作用的, 所以只好自己刷</p>

<a href="https://dennys.files.wordpress.com/2022/02/image-5.png"><img src="https://dennys.files.wordpress.com/2022/02/image-5.png?w=293" alt="" class="wp-image-335"/></a>

<p>3. 刷回原來的 boot.img 之後, 絕大多數 App 都可以用了, 不過實測發現沒有移除 Magisk, 還是有某些 App 譬如 Costco 或國泰世華銀行無法使用, 所以最後還是把 Magisk 移除. 但手機還是維持在 Unlock 狀態.</p>

