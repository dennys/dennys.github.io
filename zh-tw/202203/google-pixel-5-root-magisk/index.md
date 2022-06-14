# Google Pixel 5 Root & Magisk


<p><mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-vivid-red-color"><strong>注意: 系統刷機有一定的風險, 可能導致手機無法開機和失去保固, 資料還會被清空, 執行前請先三思!!!</strong></mark></p>

<p><strong><mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-luminous-vivid-amber-color">注意: 系統刷機有一定的風險, 可能導致手機無法開機和失去保固, 資料還會被清空, 執行前請先三思!!!</mark></strong></p>

<p><strong><mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-luminous-vivid-orange-color">注意: 系統刷機有一定的風險, 可能導致手機無法開機和失去保固, 資料還會被清空, 執行前請先三思!!!</mark></strong></p>

<p>======================================================</p>

## 環境準備

### 安裝 Google USB driver

1. 首先要在電腦上安裝 Google USB driver, 可在[這裡](https://developer.android.com/studio/run/win-usb)下載
1. 解開壓縮檔 (應該是 usb_driver_r13-windows.zip), 在 android_winusb.inf 按右鍵, 然後一直往下就好了

<a href="https://dennys.files.wordpress.com/2022/02/image-11.png"><img src="https://dennys.files.wordpress.com/2022/02/image-11.png?w=485" alt="" class="wp-image-383"/></a>

ps: 沒有裝 driver 的話, 在執行 fastboot 的時候, 就會遇到 waiting for any device 的訊息, 如下
```
C:\platform-tools>fastboot boot magisk_patched.img
< waiting for any device >
```

===================================================

### 安裝 Android SDK Platform Tools

從這裡下載, 一直 next next 就好.

https://developer.android.com/studio/releases/platform-tools#downloads

===================================================

## 手機解鎖 Unlock

<mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-vivid-red-color"><strong>請記得先備份資料, 因為解鎖會清空資料</strong></mark>

<strong><mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-luminous-vivid-orange-color">請記得先備份資料, 因為解鎖會清空資料</mark></strong>

<strong><mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-luminous-vivid-orange-color">請記得先備份資料, 因為解鎖會清空資料</mark></strong>

1. 手機進入開發者模式並確定 adb devices 可以連上手機

```
C:\platform-tools>adb devices
List of devices attached
*************** device
```
2. Enable OEM 解鎖

![](/images/software/android/android_oem_unlock.webp)

3. 執行下列指令, 手機會出現要你確認是否要 unlock, 按下去, 手機就解鎖了. <mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-vivid-red-color"><strong><span style="text-decoration:underline;">注意: 這裡會把手機資料清空</span></strong></mark><

```
C:\platform-tools>adb reboot bootloader
C:\platform-tools>fastboot flashing unlock
OKAY [  0.068s]
Finished. Total time: 0.071s
```

=============== 以上完成 Unlock, 以下為 Root 的步驟 ===============

## 手機 Root

<p>1. 用手機下載並安裝 Magisk <s>(https://github.com/topjohnwu/magisk_files/blob/canary/app-debug.apk)</s> (更新: 請改從這裡抓 https://magiskmanager.com/</p>

<p>2.1 檢查手機版本 (設定 -&gt; 關於, 然後拉到最下面), 譬如我這支是 SQ1A.220105.002</p>

<a href="https://dennys.files.wordpress.com/2022/02/image-2.png"><img src="https://dennys.files.wordpress.com/2022/02/image-2.png?w=235" alt="" class="wp-image-325"/></a>

<p>2.2 用電腦連到 https://developers.google.com/android/images, 下載對應的 boot image (按 Link) </p>

<a href="https://dennys.files.wordpress.com/2022/01/image-17.png"><img src="https://dennys.files.wordpress.com/2022/01/image-17.png?w=848" alt="" class="wp-image-298"/></a>

<p>2.3 解開裡面的 vbmega.img 和 boot.img. </p>

<a href="https://dennys.files.wordpress.com/2022/01/image-16.png"><img src="https://dennys.files.wordpress.com/2022/01/image-16.png?w=497" alt="" class="wp-image-295" width="360" height="216"/></a>

<p>3. 將前一步驟解出來的 boot.img 放到手機上, 然後使用 Magisk 對 boot.img 做 patch, 並把 patch 完後的檔案傳回電腦</p>

<a href="https://dennys.files.wordpress.com/2022/02/image.png"><img src="https://dennys.files.wordpress.com/2022/02/image.png?w=295" alt="" class="wp-image-319"/></a>

<a href="https://dennys.files.wordpress.com/2022/02/image-1.png"><img src="https://dennys.files.wordpress.com/2022/02/image-1.png?w=291" alt="" class="wp-image-321"/></a>

<p>4. 將手機重開, 並同時按下 電源+音量下鍵進入 bootloader 模式</p>

<p>5.1 用 fastboot 刷 vbmega.img, 不然刷完後重開機, 手機會出現 failed to load/verify boot images (應該是 Pixel 4A 5G 以後的都需要這個)</p>

```
C:\platform-tools>fastboot flash --disable-verity --disable-verification vbmeta vbmeta.img
```

<a href="https://dennys.files.wordpress.com/2022/01/image-15.png"><img src="https://dennys.files.wordpress.com/2022/01/image-15.png?w=760" alt="" class="wp-image-293"/></a>

<p>5.2 用 fastboot 刷剛剛用 Magisk 給 patch 過的 image. 注意, <mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-luminous-vivid-orange-color">這裡用的是 fastboot boot, 不是 fastboot flash</mark>. 刷完以後會自動重開機.</p>

```
fastboot boot magisk_patched.img
```

<a href="https://dennys.files.wordpress.com/2022/01/image-14.png"><img src="https://dennys.files.wordpress.com/2022/01/image-14.png?w=824" alt="" class="wp-image-291"/></a>

<p>6. 使用 Magisk 檢查是否 Root</p>

<p>若你的 Masgisk 長這樣</p>

<a href="https://dennys.files.wordpress.com/2022/02/image-6.png"><img src="https://dennys.files.wordpress.com/2022/02/image-6.png?w=59" alt="" class="wp-image-353"/></a>

<p>然後執行時出現這個畫面, 這時候按確定, 他應該會去嘗試下載, 然後最後說剖析套件時發生問題.</p>

<a href="https://dennys.files.wordpress.com/2022/02/image-7.png"><img src="https://dennys.files.wordpress.com/2022/02/image-7.png?w=242" alt="" class="wp-image-354" width="214" height="101"/></a>

<p>還是得裝前面講的版本 (https://github.com/topjohnwu/magisk_files/blob/canary/app-debug.apk), 執行後看到下面畫面, 就是 Root 成功了.</p>

<a href="https://dennys.files.wordpress.com/2022/02/image-3.png"><img src="https://dennys.files.wordpress.com/2022/02/image-3.png?w=289" alt="" class="wp-image-327"/></a>

<p>7. Root 之後可再刷第三方 ROM, Pixel5 有一堆可刷, 可參考 https://forum.xda-developers.com/f/11343/. 比較意外的是, 本來以為最多人用的是 <a rel="noreferrer noopener" href="https://www.google.com/search?q=LineageOS" target="_blank">LineageOS</a>, 不過看來 <a rel="noreferrer noopener" href="https://www.google.com/search?q=ProtonASOP" target="_blank">ProtonASOP</a> 受到的關注更多 (以 XDA 上面的 views/replies 數量來看). 還有 <a rel="noreferrer noopener" href="https://forum.xda-developers.com/t/4192641/" target="_blank">CleanSlate</a> 在沒有 Root 的情況下也可以使用, 這個值得研究一下.</p>

<a href="https://dennys.files.wordpress.com/2022/02/image-9.png"><img src="https://dennys.files.wordpress.com/2022/02/image-9.png?w=600" alt="" class="wp-image-361"/></a>

## 參考:

1. https://www.mobile01.com/topicdetail.php?f=565&amp;t=6233575 (這篇是 Pixel 4XL, 所以不需要 vbmega.img)
1. https://www.droidwin.com/how-to-root-pixel-devices-via-magisk-on-android-12/
1. https://android.gadgethacks.com/how-to/root-pixel-5-still-pass-safetynet-full-guide-for-beginners-intermediate-users-0348101/
