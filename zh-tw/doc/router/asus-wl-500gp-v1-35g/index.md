# 使用 Asus WL-500gP v1 的 3.5G 無線上網


[![](http://4.bp.blogspot.com/-rCd8h7VM0WI/ToJhcAfqcxI/AAAAAAAABkc/dog5uxnIWxs/s320/Asus_500gp_1.jpg)](http://4.bp.blogspot.com/-rCd8h7VM0WI/ToJhcAfqcxI/AAAAAAAABkc/dog5uxnIWxs/s1600/Asus_500gp_1.jpg)

**注意：所有改機流程皆有其風險，請各位朋友在開始動作以前自行評估!!**  

**前言
一般來說, 3G 上網, 是只能用手機, 或是直接透過 USB 接到電腦上, 所以只能給一個裝置上網. 當然, 一台電腦上網之後, 還是有許多花樣可以玩. 一般最簡單的就是 NAT 了, 不過那台電腦就得有兩張網路卡才能這樣玩. 如果是無線網路, 就還可以用 [ad-hoc mode](http://www.mobile01.com/topicdetail.php?f=61&t=1466&last=11879). 其實在以前分享器還比較貴的時候, 我也用過 NAT 一段時間, 不過說實在的, 並不是很方便, 而且那台電腦得開機才能使用, 重開機時大家就都斷線了.  
  
因此呢, 想找一個可以讓 3G 上網也可以透過分享器給家裡其他電腦也使用的方法. 查了一下, 原來已經有人做出這種產品了, 譬如 [EDIMAX 3G-6200n](http://store.pchome.com.tw/wellcome/M02587782.htm) 或是 [ZALiP CDW530AM](http://shopping.pchome.com.tw/?m=item&f=exhibit&IT_NO=DRAF0Z-A44360058-000&SR_NO=DRAF0Z), 價錢也還好. 不過呢, 一來都是沒聽過的廠牌, 二來則是, 這樣就不好玩了啊, 所以最後還是決定找一個可以自行改機的方案來實現透過分享器的 3G 上網來玩玩.  
ps: Vigor 的 hub 也有[支援 3G](http://www.draytek.co.uk/support/kb_vigor_3g.html), 不過目前還沒有研究.  
  
  
  
**硬體

* Hub: [Asus WL-500gP V1](http://tw.asus.com/product.aspx?P_ID=8el2DcrRjLoHNdQ8&templete=2)
* USB 網卡: [Huawei E169](http://www.huawei.com/mobileweb/en/products/view.do?id=1181)

選用 500gP V1 的原因在於 (1) 他有 8MB firmware, 可以玩的花樣比較多 (2) 有人已經改機改到 128MB RAM 了  
選用 E169 的原因在於, 他可以拿來打電話, 只要電腦有麥克風和喇叭就可以了. 當然 [E220](http://www.huawei.com/mobileweb/en/products/view.do?id=282) 也可以, 只是剛好碰到有人願意用半價賣一隻保固還有半年的 E169, 我就買了.  
其實如果要更便宜的 3G 組合, 買 Asus WL-HDD + E220 也可以.  
  
**Firmware

目前比較流行的 firmware 大概有這幾個系列, [Oleg](http://oleg.wl500g.info/), [Tomato](http://www.polarcloud.com/tomato), [OpenWrt](http://openwrt.org/) 以及 [DD-WRT](http://www.dd-wrt.com/) (參考[無線路由器韌體關係圖](http://digiland.tw/viewtopic.php?pid=2796))  

* Oleg 系列:
    * Oleg: 無內建 3G 支援
    * lly: 無內建 3G 支援, 但可外掛, 參考[這裡](http://digiland.tw/viewtopic.php?id=706&p=2)
    * [CDMA@wifi](http://koppel.cz/cdmawifi/english/): 內建支援 3G

* Tomato 系列:
    * Tomato: 無內建 3G 支援
    * [Tomato DualWan](http://dualwan.cn/): 內建支援 3G

* OpenWrt 系列:
    * 我沒碰過這個系列的 firmware...

* DD-WRT 系列:
    * [DD-WRT](http://dd-wrt.com/): 無內建 3G 支援, 但可外掛
    * [DD-WRT mod](http://www.dd-wrt.com/phpBB2/viewtopic.php?t=50531): 這個是有人修改的 DD-WRT, 內建 3G 支援, 但似乎還不完整, 我沒用過, 可參考[這裡](http://digiland.tw/viewtopic.php?id=893)

**前置準備

*   設定 SIM 卡
要注意的是, 必須把 SIM 卡的 pin code 關掉 (就是說不要設定密碼, 預設一般是 0000, 這個也不行, 要把他 disable). 可以用手機設定,如果使用的是 Huawei 的 3G 網卡, 也可以用他的 Mobile Partner 來改. 其實這部份或許也可以保留 pin code, 只是目前不知道如何設定 firmware 去輸入 pin code, 所以只能先把他 disable.

**步驟
  
以下以 CDMA@wifi 為例子, 基本上各家的刷法都差不多. 原則上就是第一次刷要用 Asus Recovery Utility, 之後的版本升級就直接用 Web UI 即可.  
  
1. 準備 Asus Recovery Utility  

如果直接使用 Asus 的 Web UI 是無法上傳非 Asus 的 firmware 的, 因此要使用 Asus 的 Recovery Utility, 可在[這裡](http://support.asus.com/download/download.aspx?model=WL-500gP&f_name=UT_WL500gP_3500.zip&f_type=14&os=17)下載.

2. 下載 firmware  

3rd party 的 firmware 有很多家, 目前應該是 [CDMA@wifi](http://koppel.cz/cdmawifi/english/)的用起來最簡單, 所以先測試這版本, 之後會再嘗試其他版本的 firmware. 首先從 [http://koppel.cz/cdmawifi/download/](http://koppel.cz/cdmawifi/download/) 下載 firmware, 因為我的是 500gP V1, 所以使用 [WL500gp-1.9.2.7-10-USB-1.71.trx](http://koppel.cz/cdmawifi/download/171/WL500gp-1.9.2.7-10-USB-1.71.trx)

3. 更換 firmware  

詳細的步驟可以參考 DD-WRT 上的 [Asus Recovery Utility](http://www.dd-wrt.com/wiki/index.php/All_Asus_WL-500xx_series_routers#Asus_Recovery_Utility), 簡單寫起來如下:

    1.  把 route 的電源拔掉
    2.  按著 reset (或是 500gP 上的 restore) 按鈕不放, 同時插上電源
    3.  當你看到 power LED 是一閃一閃的時候, 就代表 router 已經成功進入 recover 模式
    4.  如果這時候無法使用 192.168.1.1 連上 router (用 ping 的, web ui 這時不作用), 請自行設定 IP 192.168.1.10, subnet mask 255.255.255.0
    5.  使用 Asus 的 Recovery tool 把 WL500gp-1.9.2.7-10-USB-1.71.trx 傳上去
    6.  完成, 收工

4. 設定 128MB 卡 (Optional)  

這個步驟是給有 128MB 的才需要, 如果是 64MB 以下的, 應該會自動偵測到.

**注意：這個動作有可能會把機器變磚, 請小心!!**

指令很簡單, 只有兩行.  
  
```sh
nvram set sdram\_init=0x0011  
nvram commit
```
  
如果要回復成 64MB 也很簡單  
  
```sh
nvram set sdram\_init=0x0009  
nvram commit
```
  
一些關於 128MB 的參考文件:  
[http://oleg.wl500g.info/sdram.html](http://oleg.wl500g.info/sdram.html "http://oleg.wl500g.info/sdram.html")  
[http://svn.dd-wrt.com:8000/dd-wrt/ticket/466](http://svn.dd-wrt.com:8000/dd-wrt/ticket/466 "http://svn.dd-wrt.com:8000/dd-wrt/ticket/466")  
[http://oleg.wl500g.info/wl500gp\_ram.html](http://oleg.wl500g.info/wl500gp_ram.html "http://oleg.wl500g.info/wl500gp_ram.html")

5\. 設定 3G (因為各家步驟不一, 因此我會另外拆開成幾篇)  

*   CDMA@wifi
*   Tomato DualWan
*   DD-WRT

6\. 心得比較  

*   CDMA@wifi
這個最好裝, 也很穩定, 把 USB 網卡拔來拔去也都能很快抓到.  
缺點是他的 QoS 只有看到 bandwidth  
*   Tomato DualWan
這個也很好裝, 但是把 USB 網卡拔來拔去之後, 常常會抓不到. 不過一旦抓到了, 倒是也滿穩定的.  
但是有個問題是, 他會有一堆錯誤訊息的 log, 而且多到幾分鐘被 purge 掉, 因此用起來會很不方便.  
*   DD-WRT
這個因為要外掛, 還在測試.

**Next Step:**  
  
  
  
*   把 USB storage 掛上去, 裝一些軟體來玩玩看. 基本的如 MRTG, Samba, FTP 是一定要的, 再來就是 [Apache](http://httpd.apache.org/ "Apache"), [MySQL](http://www.mysql.com/), [PHP](http://www.php.net/) 了. 目前本站是住在一個 Linux 的 VMWare 裡面 (Host OS 是 Windows), 之前請教過數位天堂的站長, 我的流量遠小於數位天堂, 這樣應該撐得住才是. 如果成功的話, 就可以不用整天開電腦了. 比較擔心的是, 一般似乎是比較推薦使用 [lighttpd](http://www.lighttpd.net/) 在這種平台上, 不過我沒用過
