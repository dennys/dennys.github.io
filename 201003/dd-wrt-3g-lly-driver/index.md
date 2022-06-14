# DD-WRT 之 3G 上網嘗鮮 (使用 lly driver)


[![](http://4.bp.blogspot.com/-rCd8h7VM0WI/ToJhcAfqcxI/AAAAAAAABkc/dog5uxnIWxs/s320/Asus_500gp_1.jpg)](http://4.bp.blogspot.com/-rCd8h7VM0WI/ToJhcAfqcxI/AAAAAAAABkc/dog5uxnIWxs/s1600/Asus_500gp_1.jpg)
 
  
**注意: 這是給想要用 DD-WRT 3G 上網"嘗鮮"所用, 所有的設定在重開機後都會消失...  
**  
**前言:**  
DD-WRT 官方版本目前並沒有支援 3G, 不過已經有高手弄出來了. 這幾天照著 [http://digiland.tw/viewtopic.php?id=914](http://digiland.tw/viewtopic.php?id=914 "http://digiland.tw/viewtopic.php?id=914") 的作法把 DD-WRT 的 3G 環境也弄起來了, 在這裡重新整理一下. 和原來的方法基本上是一樣的, 只是我在測試過程中全部都用放在 RAM 的 /tmp, 沒有動到 flash, 也沒有外接任何 storage. 當然, 意思就是說, 重開機就什麼都沒了. 至於為什麼要這樣呢? 其實只是因為不確定這條路通不通, 又不想把現在的 HDD 給動到, 所以就這樣子測試了. 如果您也只是想嘗鮮一下, 可以試試看我的方法.  
  
**參考文章:**  

*   [數位天堂: DD-WRT 3G 測試](http://digiland.tw/viewtopic.php?id=914)
*   [數位天堂: Lly 韌體 (Oleg 韌體分支)](http://digiland.tw/viewtopic.php?pid=4215#p4215)

**軟硬體:**  

*   Hub: [Asus WL-500gP V1](http://tw.asus.com/product.aspx?P_ID=8el2DcrRjLoHNdQ8&templete=2)
*   USB 網卡: [Huawei E169](http://www.huawei.com/mobileweb/en/products/view.do?id=1181)
*   Firmware: [DD-WRT](http://dd-wrt.com/)

**前置準備:**  

*   DD-WRT firmware 已經刷好 (我是用[這個](http://dd-wrt.com/routerdb/de/download/Asus/WL500G%20Premium/v1/dd-wrt.v24_mega_generic.bin/1958))
*   在 Hub 開機前把 USB 網卡插入 (這地方還需要多測試, 目前開機後再插入常常會抓不到)
*   要打開DD-WRT的PPPoE (這裡不懂為何一定要開, 因為後面反正還是要 kill 掉. 或許是有某些設定檔案會產生?)
*   把 SIM 卡的 pin code 給 disable

**步驟:**  
  
1\. 建立暫時的ipkg/jffs環境  

執行下列指令  

```
$ mkdir /tmp/jffs  
$ mount -o bind /tmp/jffs /jffs
```

2\. 準備 kernel library (從 lly 取得)  

因為我使用的 DD-WRT 是 2.4.37, 因此要抓一樣版本的 lly, 路徑如下:  
[http://wl500g.googlecode.com/files/modules-1.9.2.7-d-r240.tgz](http://wl500g.googlecode.com/files/modules-1.9.2.7-d-r240.tgz "http://wl500g.googlecode.com/files/modules-1.9.2.7-d-r240.tgz")

3\. 準備 package (從 openwrt 取得)  

需要以下這些軟體:

*   chat: 使用 AT command 撥號
*   [usb-modeswitch](http://www.draisberghof.de/usb_modeswitch/): 切換 USB 網卡模式
*   usbutils
*   libusb
*   zlib

因為 3G 網路沒通, 所以沒法直接用 ipkg 線上安裝, 因此我的作法是分兩段, 先用 PC 下載以下檔案, 再上傳到 500gp, 並用 ipkg install \*\*\*\*\*\*.ipk 安裝起來. 如果你的 500gp 已經有可以用的網路 (i.e. ADSL), 可以直接安裝, 不用這麼麻煩.  

[http://downloads.openwrt.org/snapshots/trunk/brcm-2.4/packages/chat\_2.4....](http://downloads.openwrt.org/snapshots/trunk/brcm-2.4/packages/chat_2.4.4-4_brcm-2.4.ipk "http://downloads.openwrt.org/snapshots/trunk/brcm-2.4/packages/chat_2.4.4-4_brcm-2.4.ipk")  
[http://downloads.openwrt.org/snapshots/trunk/brcm-2.4/packages/libusb\_0....](http://downloads.openwrt.org/snapshots/trunk/brcm-2.4/packages/libusb_0.1.12-2_brcm-2.4.ipk "http://downloads.openwrt.org/snapshots/trunk/brcm-2.4/packages/libusb_0.1.12-2_brcm-2.4.ipk")  
[http://downloads.openwrt.org/snapshots/trunk/brcm-2.4/packages/usb-modes...](http://downloads.openwrt.org/snapshots/trunk/brcm-2.4/packages/usb-modeswitch_1.1.0-1_brcm-2.4.ipk "http://downloads.openwrt.org/snapshots/trunk/brcm-2.4/packages/usb-modeswitch_1.1.0-1_brcm-2.4.ipk")  
[http://downloads.openwrt.org/snapshots/trunk/brcm-2.4/packages/usbutils\_...](http://downloads.openwrt.org/snapshots/trunk/brcm-2.4/packages/usbutils_0.86-1_brcm-2.4.ipk "http://downloads.openwrt.org/snapshots/trunk/brcm-2.4/packages/usbutils_0.86-1_brcm-2.4.ipk")  
[http://downloads.openwrt.org/snapshots/trunk/brcm-2.4/packages/zlib\_1.2....](http://downloads.openwrt.org/snapshots/trunk/brcm-2.4/packages/zlib_1.2.3-5_brcm-2.4.ipk "http://downloads.openwrt.org/snapshots/trunk/brcm-2.4/packages/zlib_1.2.3-5_brcm-2.4.ipk")

4\. 載入 kernel driver  

把前面下載的 modules-1.9.2.7-d-r240.tgz 解開, 然後切換目錄到解開之後的 lib/modules/2.4.37/kernel/drivers/usb/serial. 載入時要注意順序, 使用 insmod 的話, 要先載入usbserial, 再載入option. 

首先載入 usbserial, 執行下列指令  
  
```
$ insmod usbserial.o
```
  
這時候用dmesg應該會看到下列訊息  
  

usb.c: registered new driver serial  
usbserial.c: USB Serial support registered for Generic  
usbserial.c: USB Serial Driver core v1.4

  
再來載入 option, 執行下列指令  
  
```
$ insmod option.o
```
  
這時候用dmesg應該會看到下列訊息  
  

usbserial.c: USB Serial support registered for Option GSM modem  
option.c: USB Driver for GSM modems: v0.7.2a

  
執行lsusb看driver是否有抓到usb裝置, 最後一行(12d1代表Huawei, 1001代表E169) 表示抓到了. (前面第一行的錯誤訊息應該可以忽略吧?)  
  

$ lsusb  
lsusb: cannot open "/usr/share/usb.ids", No such file or directory  
Bus 004 Device 001: ID 0000:0000  
Bus 003 Device 001: ID 0000:0000  
Bus 002 Device 001: ID 0000:0000  
Bus 001 Device 001: ID 0000:0000  
Bus 001 Device 002: ID 12d1:1001

  
最後確定/dev/usb/tts/0, /dev/usb/tts/1 有被建立起來  
  

$ ls -l /dev/usb/tts/  
crw------- 1 root root 188, 0 Jan 1 00:01 0  
crw------- 1 root root 188, 1 Jan 1 00:00 1

5\. 使用usb\_modeswitch將網卡模式從 ZeroCD 切換到 modem  

編寫 /tmp/usb-modeswitch.conf, 內容如下 (可參考 [http://www.draisberghof.de/usb\_modeswitch/usb\_modeswitch.conf](http://www.draisberghof.de/usb_modeswitch/usb_modeswitch.conf "http://www.draisberghof.de/usb_modeswitch/usb_modeswitch.conf"), 將 DefaultVendor 和 DefaultProduct 修改成合適的值)  
  

$ more /tmp/usb-modeswitch.conf  
\# Huawei E169  
DefaultVendor=0x12d1;  
DefaultProduct=0x1001  
;TargetClass=0xff  
  
\# choose one of these:  
;DetachStorageOnly=1  
HuaweiMode=1

  
執行下列指令, 使用usb\_modeswitch將網卡切換到modem模式  
  
```
$ usb\_modeswitch -c /tmp/usb-modeswitch.conf  
```

Looking for default devices ...  
Found default devices (1)  
Accessing device 002 on bus 001 ...  
Using endpoints 0x02 (out) and 0x82 (in)  
Not a storage device, skipping SCSI inquiry  
  
USB description data (for identification)  
\-------------------------  
Manufacturer:   
Product: HUAWEI Mobile  
Serial No.:   
\-------------------------  
Sending Huawei control message ...  
OK, Huawei control message sent  
\-> Run lsusb to note any changes. Bye.

  

如果執行 usb\_modeswitch之後的訊息和上面不同, 而是像下面這段一樣, 則代表沒有抓到裝置  
  

$ usb\_modeswitch -c /tmp/usb\_modeswitch.conf  
Looking for default devices ...  
No default device found. Is it connected? Bye.

**到這個步驟如果沒有抓到 USB 網卡, 那後續的恐怕就不用測了...**  
  
6\. 使用pppd, chat撥號  

編寫 /tmp/e169.chat. 這邊是中華電信, 所以用的是 internet 和 \*99#

$ more /tmp/e169.chat  
'' ''  
'' 'ATZ'  
'OK' 'ATI'  
'OK' 'AT+COPS?'  
'OK' 'AT+CGDCONT=1,"IP","internet"'  
'OK' 'ATD\*99#'  
'CONNECT' ''

  

編寫 /tmp/ppp/options.pppoe  
  

$more /tmp/ppp/options.pppoe  
/dev/usb/tts/0  
460800  
debug  
crtscts  
noipdefault  
ipcp-accept-local  
lcp-echo-interval 60  
lcp-echo-failure 5  
usepeerdns  
noauth  
nodetach  
user ""  
connect "/jffs/usr/sbin/chat -s -S -V -t 30 -f /tmp/e169.chat 2>/tmp/chat.log"

  
先停掉redial, pppd. 這裡要注意的是, 我用 killall pppd 沒有作用, 得用 kill -9 \*\*\* 才行.  
  
```
$ killall redial  
$ killall pppd
```
  
使用pppd撥號  
  
```
$ pppd file /tmp/ppp/options.pppoe
```
  
檢查 /tmp/chat.log  
  

$ more /tmp/chat.log  
ATZ

  
OK  
ATI  
Manufacturer: huawei  
Model: E169  
Revision: 11.314.13.00.00  
IMEI: \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*  
+GCAP: +CGSM,+DS,+ES  
  
OK  
AT+COPS?  
+COPS: 0,2,"46692",2  
  
OK  
AT+CGDCONT=1,"IP","internet"  
OK  
ATD\*99#CONNECT

看到最後面的 ATD 代表撥號, 後面出現 CONNECT 就代表成功了, YA!   
  
**故障排除**  
  
1\. dmesg 是最基本的  
2\. 把 syslog 打開 (從 Web UI), 然後看 /tmp/message 也是個方法.  
3\. minicom 我抓下來的版本似乎已經 hard code 要把設定檔放在 /etc, 但是 /etc 又是唯讀的, 所以搞不定.  
  
**Next Step:**  
  
因為用的是 DD-WRT 的 mega 版本, 基本上已經沒有什麼空的 flash 了. 下一步是嘗試一下 size 比較小的 DD-WRT, 然後把這些 3G 相關程式放到 flash 裡面. 最終目的是把 DD-WRT 弄成一個開機即可上 3G 網路的環境, 而且不需要外接任何 storage.
