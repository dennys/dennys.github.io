# Samsung Galaxy S7 Edge 刷 LineageOS


<p><mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-vivid-red-color"><strong>注意: 系統刷機有一定的風險, 可能導致手機無法開機和失去保固, 資料還會被清空, 執行前請先三思!!!</strong></mark></p>

<p><strong><mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-luminous-vivid-amber-color">注意: 系統刷機有一定的風險, 可能導致手機無法開機和失去保固, 資料還會被清空, 執行前請先三思!!!</mark></strong></p>

<p><strong><mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-luminous-vivid-orange-color">注意: 系統刷機有一定的風險, 可能導致手機無法開機和失去保固, 資料還會被清空, 執行前請先三思!!!</mark></strong></p>

<p>======================================================</p>

## 需求

Samsung Galaxy S7 Edge 是 2016 年初的手機, 最多可更新到 Android 8, 最後的 patch 是 2020 年 9 月.

可以刷的 ROM 看來有很多 (可參考 https://forum.xda-developers.com/f/5186/)

* 最有名的 LineageOS 15~19 (Android 8~12)
* FloydQ: 這是基於 Android 10 的 Samsung OneUI 2.5, 這應該是可以用到比較多 Samsung 手機特有的功能

## 作法

### 請先在電腦安裝以下兩個程式

1. Samsung USB driver: http://developer.samsung.com/galaxy/others/android-usb-driver-for-windows
1. ODIN: https://odindownloader.com/

### 下載 LineageOS 和 Open GApps

1. LineageOS 19: https://forum.xda-developers.com/t/4369001/
1. LineageOS 18: https://forum.xda-developers.com/t/4172727/
1. Open Gapps: https://sourceforge.net/projects/opengapps/files/arm64/ =&gt; 注意, 這有綁 OS 版本, 在 2022/2/15 的版本還沒有支援 Android 12.

### 下載 TWRP

https://dl.twrp.me/hero2lte/twrp-3.6.0_9-0-hero2lte.img.tar.html, 注意, 一開始使用 3.1.1 版, 結果都讀不到 SD card, 升到最新版後就好了.

### 安裝 TWRP

1. 開啟工程模式
    1. 設定→關於裝置→軟體資訊→版本號碼處連點8下
    1. 進入開發人員選項後，USB偵錯以及OEM解鎖選項要打勾。
1. 重開機進入 download mode
    1. 執行 ODIN, 把剛才的 twrp 給刷進去, 再重開就可以了

### 刷 LineageOS 和 Open Gapps

執行 adb reboot recovery 或是先關機, 然後按音量上+Home+Power 鍵進入 TWRP Recovery. 選 INSTALL, 把LineageOS 和 Open Gapps 的 zip 檔案裝上去, 重開機就可以了.

如果要用 ADB, 指令如下:

```cmd
adb sideload open_gapps-arm64-11.0-nano-20220202.zip
```

## TWRP 被 LineageOS Recovery 蓋掉的問題

自 LineageOS 17 開始, 他自己有帶一個 Recovery, 嘗試刷回 TWRP 又常常失敗.

可試試看 https://forum.xda-developers.com/t/3334084/ 的 step 6~8

## NikGapps 測試 (**失敗**)

一開始用 LineageOS 自己的 Recovery, 刷 NikGapps 一直失敗

```cmd
adb sideload NikGapps-basic-arm64-12-20211224-signed.zip
adb: sideload connection failed: closed
adb: trying pre-KitKat sideload method…
adb: pre-KitKat sideload connection failed: closed
```

用以下指令可解決

```cmd
C:\platform-tools\fastboot devices
C:\platform-tools\adb usb
C:\platform-tools\adb sideload NikGapps-basic-arm64-12-20211224-signed.zip
serving: 'NikGapps-basic-arm64-12-20211224-signed.zip' (~32%)
```

但最後又一直卡在空間不足, 就放棄了.

