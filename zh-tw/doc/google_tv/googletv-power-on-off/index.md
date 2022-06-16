# Google TV 開關 Infocus 電視


## 前言

[Chromecast With Google TV](https://store.google.com/tw/product/chromecast_google_tv) 要控制電視機的 ***開關*** 有 2 種方法

## 做法

1. 走紅外線, 這個 Infocus 電視行不通, mobile01 上面很多人也都測過了, 不支援.
1. 走 [HDMI-CEC](https://zh.wikipedia.org/zh-tw/CEC), 不過在 Infocus 電視預設是關閉的, 設定方式也很簡單:
    1. 進入設定畫面
    1. 選 HDMI 模式
    1. 將"智慧連接"打開
    1. 將"自動開機"打開

這樣就完成了, 之後把 Google TV 打開時, 大概 1 秒就會把電視也打開, 還是會有一點時間差, 而且是有感的, 但已經方便太多了.
**注意, 下面的貼圖是抓官方說明書, 裡面寫的是"關閉", 請勿照著設定, 要記得打開**

![ ](/zh-tw/images/software/google_tv/infocus_hdmi_setup.png)



