# Asuswrt Merlin Diversion (分享器擋廣告)


## Diversion

[Diversion](https://diversion.ch/) 是個可安裝在 Asuswrt Merlin 上的 AD block 軟體, 他的原理是直接在 Hub 上面裝一個 DNS, 然後當你的 client 要查 DNS 的時候, 他如果發現這個 DNS 在他的黑名單裡面, 就把他擋掉.  

## 安裝步驟

以下參考這篇 https://www.snbforums.com/threads/amtm-step-by-step-install-guide-l-ld.56237/

然後他為了要檔 https, 有安裝 pixelserv-tls, 可參考這個 [FAQ](https://diversion.ch/faq-reader/what-does-pixelserv-tls-do.html) 看一下這是甚麼東西他能做的事情

### Asus Merlin Web GUI 設定

Web GUI 只要設定這項就好

![](https://lh6.googleusercontent.com/tukvZsPBBrBjzmLtgIkggBHigfjFp_SH9d_-JCFjysGeW0AeOIgc2LcMy0JtmoE_Zb9HSY23cOHDwSdnUj60cVzaDTiCLRz8kuTU8TVN6AGCCq1WnazKwLd1Lk6zt-fnkGCiMU0v)

### Diversion 安裝

1.  一開始會掛在 /dev/sda1, 而且格式是 NTFS (可用 fdisk 看)

```
admin@RT-AC66U\_B1-B898:/tmp/home/root# df
Filesystem           1K-blocks      Used Available Use% Mounted on
/dev/root                33408     33408         0 100% /
devtmpfs                127732         0    127732   0% /dev
tmpfs                   127836      1532    126304   1% /tmp
/dev/mtdblock4           64256      2868     61388   4% /jffs
/dev/sda1             60058960     11712  60047248   0% /tmp/mnt/sda1
```

2.  安裝 AMTM, 可參考 [https://diversion.ch/amtm.html](https://www.google.com/url?q=https://diversion.ch/amtm.html&sa=D&ust=1580525769356000, 指令如下:

```
curl -Os https://diversion.ch/amtm/amtm&& sh amtm
```

3.  把 USB 格式化成 ext
    1.  執行 /jffs/scripts/amtm
    2.  執行 fd
    3.  然後照著說明格式化, 格式化完後會重開機
    4.  完成後 df 如下

```
admin@RT-AC66U\_B1-B898:/tmp/home/root# df
Filesystem           1K-blocks      Used Available Use% Mounted on
/dev/root                33408     33408         0 100% /
devtmpfs                127732         0    127732   0% /dev
tmpfs                   127836      1528    126308   1% /tmp
/dev/mtdblock4           64256      2860     61396   4% /jffs
/dev/sda1             59144340    184232  55955696   0% /tmp/mnt/usb
```

4.  一樣在 amtm 裡面執行 dc, 進行 disk check, 結果如下

![](https://lh4.googleusercontent.com/w8S_1GldhU_MCeQsCReKihYCczmWEyRHzgG961hPjo_bh4bkqRpSu5NooBt1iiS_X6pqlkwO23q29vx1xy7HGbffROw9XJQc5GOimSH_BjlaifctXdFZq65laEsKbQ_YHqCRE1M4)

5.  安裝 Diversion, 在 amtm 裡面執行 '1' (可先用 'i' 指令確認一下是用 1 來安裝 Diversion). 注意, 這裡本來應該先安裝 Entware, 但如果你嘗試安裝, amtm 會建議你直接裝 Diversion, 然後他在安裝過程中會自動安裝 Entware![](https://lh4.googleusercontent.com/d3FokoWGjjCVb5hbTM7V7ynQId8c2-TFRINHntncnbdPm5MZMYDd3OQPE_bjI6N1UKHzgym1SpYsLKbPFQnvvj0WxX25ruPmopdJ7QviokV_WDlYr4InFgVGSY_rw29gyaPC7t0O)

6.  Diversion 安裝結果如下, 注意, 他移除了LAN的DNS 8.8.8.8/8.8.4.4. 不過他沒有動到 WAN 的 DNS.

![](https://lh6.googleusercontent.com/H8pQRnMIaas9yTSlXmtVM-wCyXSm4Iie3R6AwVQpGFLjJPn5wwyojmgX2SqNW2s-MT0Yit2sVE3u68youQlJHhKI6pE7-7-Wyun5tuSjTg8HJEtu9UEzXDkMw9YHFVhL1RjobjQL)

7.  安裝 Diversio Standard

![](https://lh3.googleusercontent.com/ED9LZcxJQ9TbOnIE4V4CpmgZ45yJ5QdBziUFAT4Vo37HFrLtNIuHHX99lJ61GkH-NktK4zYwSW1IYEfm8sM3FS5j8fiFpSAsyJtx2wMopjje_iZxwMJwhW0VJ-6vatrUHyk2zicG)

8.  這邊注意, 要進入 Asuswrt-Merlin Web 設定

![](https://lh6.googleusercontent.com/keypYuXHnWmu0I4avokSRMaYRwf9IqdYSsVdwcA2maOQRnMQmq-SCFrAE3shwLN_8oJ1gL1yhZzKP3U_ol3PXTYwcD2HncTQlacFka3ieTRQVVid1rjNLOFJJOf77vjlwNpxn1Fc)

9.  Asus Merlin 要把 DHCP 給出去的 IP, 跳過 192.168.1.2, 如下圖是 192.168.1.11 也可

![](https://lh3.googleusercontent.com/QasKfCJT9_-CGhJpEY_zibQDyON3_2Z9kgaxqP5NkFIWJTg_qbXilZutSLI2mJyd5FbQTA7T7_7HL1UiEviVPobwb6AMIZP0V1nRAfjT_tdC3QgXCiEYGDhza_VUzlkx8rfz56zA)

10.  接下來要輸入 pixelserv-tls 的 IP, 這裡就避開 DHCP 網段就好

![](https://lh3.googleusercontent.com/-K22T7E3GI5rKiQxWerVjQzcHOedPUdPiKo4R_AQEeAjGRAlIMK1xF4OmYTjwFCLuRxJG1kJCkMgs6mcAxlzdafL7LQpzIfD3eV-AZ5L8drPzNE8wx_8WEwjttMQIfMN_97oKvV9)

11.  選擇 log 方式

![](https://lh4.googleusercontent.com/1B0TCneezPe7bt3lHHj79BoOuoaoqIgplRuHlmU2jtpAdEz8P2ERxzlzoya-hmOniHrERSst_IWhxM0X5p95u-mcTUaYGI3v5r0pxJSRDxIoCvgI-W2c4TXKqCosixrw9NxZzHiX)

12.  他會安裝到這個目錄 /tmp/mnt/usb/entware

### 特別注意

因為 Diversion 是使用 DNS 來 block AD, 因此 WAN 的地方可以指定 DNS

![](https://lh5.googleusercontent.com/hNHM9nTtdidgejAeACTYduF5nOqfcbW2a4qdsVYXVWzg2Sd7hm-LhpVPSOIqQoTBD3kjc-XcsGwfU-fks4QLRhoOqGA3ptFTKU1p-QKVlMVGa4QKJ2IO6Ktx5CJoXHJddTp3nKAx)

但 LAN 的地方必須清空, client 的 DNS 必須是 192.168.1.1, 否則就沒效了

![](https://lh6.googleusercontent.com/sISw_lsJrNFDCPV2VJM7PlhPWlR7HN9pVkpwose1lXadrvbLEDz7sqgZKsWycFd0zR6IA5lNuSEjpqp-q7IscNsDXknZzXbJEX2kREQagqwtJokEIhX3U3VblCrDGf7Hfgz9qBKY)

可用 ipconfig /all 檢查

![](https://lh6.googleusercontent.com/Gchww0d3cH0tqRDh0rAXm815Ay16VBa7p8ZMkMS5gcLJpfxjHI1Gts24B9pbrlIcWlG6XkjCanQi7LUlnxNXvJP-1yd91ZemhezLguVER1h3s31oewpcuffJmxOrhtamPtQZlB9h)

## Diversion 使用

1.  使用 putty 連進去後, 執行 diversion, 看到如下畫面就是安裝成功了

![](https://lh3.googleusercontent.com/7GGlp5ISzGoZyPoZ4P0dmqDFp5whAQAizmk0H8SNrxqoX24xr0Jx425wRJ-FfCudnOqlsqA6vrghEJ_dfeAa8ILDs3y7QYcIM4ZFm0sWMDbPn3nlLShbsOywkkl_EEU5qUxmiT2z)

2.  選擇 f (follow dnsmasq.log) 可以看 log, 有以下幾種模式可以選 (實際 log 在 /tmp/mnt/sda1/entware/var/log/dnsmasq.log, 紅色的部分要看一開始設定的名字)

![](https://lh3.googleusercontent.com/dOwr8ybF5ARO42i_BB6__4QftiL00c1Fn2BXfLMzxE1psNvchu6P0dtAGE9ZzfP3LnT9INu85XVTou1cqeocHEXqq5_BCDjm_pXqHTuL9bEAjHn3Kgoc-XxthS-4ynwaiowtCwrs)

3.  選擇 1, 可看到如下就是有成功了

![](https://lh6.googleusercontent.com/B0IQoBEt2umQoXitstSdMMhMCymi0imsMkr4hVrVQIb-G_rQZ7JzLqdHCpC7sOcJbvqCP66rljETXQqUABxEzHqdyAzh-k-6nX542NOvywHuDfbw9IW4RR1t08SyRySBJjpzbyNJ)

4.  瀏覽器會看到如下

![](https://lh4.googleusercontent.com/BhtZxDDIYtXjLMBP8RibzRdGABZj-RW9SVyPK_kkTg6U-WdxshalYPRJ32yhjy3b1JEr_njA8abB6xwbBTGpppOL1352jJOfrGyGGFayCc3cNMRMhoq3Ear9dGv25LAYg2ZERbD1)

## iOS Shortcut 安裝/使用

1.  要先打開安全性: 設定 -> 捷徑 -> 允許不受信任的捷徑
2.  打開 https://diversion.ch/ios-shortcut.html, 選擇最新的版本
