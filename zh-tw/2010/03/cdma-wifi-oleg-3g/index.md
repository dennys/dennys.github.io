# CDMA@wifi (Oleg) 之 3G 上網


[![](http://4.bp.blogspot.com/-rCd8h7VM0WI/ToJhcAfqcxI/AAAAAAAABkc/dog5uxnIWxs/s320/Asus_500gp_1.jpg)](http://4.bp.blogspot.com/-rCd8h7VM0WI/ToJhcAfqcxI/AAAAAAAABkc/dog5uxnIWxs/s1600/Asus_500gp_1.jpg)

**注意：所有改機流程皆有其風險，請各位朋友在開始動作以前自行評估!!**  
  
**前言:**  
[Oleg](http://oleg.wl500g.info/)官方的 firmware 並沒有支援 3G 上網, 而且也已經很久沒更新了. [CDMA@wifi](http://koppel.cz/cdmawifi/english/) 是個修改自 Oleg 的 firmware, 有特別支援 3G 上網, 雖然也有一陣子沒更新了, 不過這是目前 Oleg 系列 firmware 中, 唯一內建支援 3G 的了.  
  
  
  
**參考文章:**  

*   [數位天堂: CDMA@wifi 韌體 (支援 3.5G 網卡上網)](http://digiland.tw/viewtopic.php?id=455)
*   [數位天堂: WL-500g 系列改機基本步驟](http://digiland.tw/viewtopic.php?id=158)

**軟硬體:**  

*   Hub: [Asus WL-500gP V1](http://tw.asus.com/product.aspx?P_ID=8el2DcrRjLoHNdQ8&templete=2)
*   USB 網卡: [Huawei E169](http://www.huawei.com/mobileweb/en/products/view.do?id=1181)
*   Firmware: [CDMA@wifi](http://koppel.cz/cdmawifi/english/) [1.9.2.7-10-USB-1.71](http://koppel.cz/cdmawifi/download/171/WL500gp-1.9.2.7-10-USB-1.71.trx)

**前置準備:**  

*   將 firmware 改刷成 CDMA@wifi (方法請參考上面的 WL-500g 系列改機基本步驟)
*   把 SIM 卡的 pin code 給 disable

**步驟:**  
1\. 設定 3G 上網  

1.  將 IP Config -> WAN Connection Type 設定成 USB Connection 模式
2.  將 System Setup -> Operation Mode 設定成 Home Gateway 模式
3.  將 USB Connection -> Connection Mode設定成 GPRS/EDGE/UMTS 模式
4.  \[重要\] 這時請按下 F5 將整個畫面 refresh, 左邊功能表才會出現下面會用到的項目
5.  將 USB Connection -> GPRS/EDGE/UMTS Config 設定如下:

**GPRS**  
Username: 空白  
Password: 空白  
APN: internet  
Dial Number (usually \*99\*\*\*1#): \*99#  
USB device serial speed (usually 115200): 460800  
USB device location ID: 空白  
**Custom USB device parameters**  
USB device Vendor ID (0xabcd): 0x12d1  
USB device Product ID (0xefgh): 0x1001  
USB device packet size (0 for default): 4096  
**Zero CD Configuration**  
Modem type: Huawei E169
<br>
  
以上設定如下圖所示:  
  
[![Asus WL-500gp V1 3.5G](http://farm5.static.flickr.com/4047/4258857614_639c8a09d5.jpg "Asus WL-500gp V1 3.5G")](http://www.flickr.com/photos/dennys_blog/4258857614 "Asus WL-500gp V1 3.5G")  
  
有些設定可能會依據情況而不太一樣

    1. Dial Number 也有人設定 \*99\*\*\*# 或是 \*99\*\*1#, 我是已經可以用 Mobile Partner 上網, 直接參考裡面的值  
    2. USB device packet size 的數值是從數位天堂看來的, 這樣會快很多  
    3. USB device Vendor ID 的部份, 似乎有人沒有設定也可以使用, 我是有找到一個[列表](http://www.linux-usb.org/usb.ids)
<br>

2. 按下 Finish, 等待 router 重新啟動 (這裡只按 Apply 雖然會存檔, 但是似乎不 work)  
3. 插入 E169, 按下 connect, 應該就會開始 ppp 撥號, 然後就可以看是否連線成功了, 如果成功了, log 應該如下:  
  
```

Jan  1 01:18:48 kernel: hub.c: new USB device 01:03.0-1, assigned address 4  
Jan  1 01:18:48 kernel: Manufacturer:   
Jan  1 01:18:48 kernel: Product: HUAWEI Mobile  
Jan  1 01:18:48 kernel: SerialNumber:   
Jan  1 01:18:48 kernel: usbserial.c: Generic converter detected  
Jan  1 01:18:48 kernel: usbserial.c: Buffer size for bulk\_in is 4096 modem reports 64  
Jan  1 01:18:48 kernel: usbserial.c: Buffer size for bulk\_out is 4096 modem reports 64  
Jan  1 01:18:48 kernel: usbserial.c: Generic converter now attached to ttyUSB0 (or usb/tts/0 for devfs)  
Jan  1 01:18:48 kernel: usbserial.c: Generic converter detected  
Jan  1 01:18:48 kernel: usbserial.c: Buffer size for bulk\_in is 4096 modem reports 64  
Jan  1 01:18:48 kernel: usbserial.c: Buffer size for bulk\_out is 4096 modem reports 64  
Jan  1 01:18:48 kernel: usbserial.c: Generic converter now attached to ttyUSB1 (or usb/tts/1 for devfs)  
Jan  1 01:18:48 kernel: usbserial.c: Generic converter detected  
Jan  1 01:18:48 kernel: usbserial.c: Buffer size for bulk\_in is 4096 modem reports 64  
Jan  1 01:18:48 kernel: usbserial.c: Buffer size for bulk\_out is 4096 modem reports 64  
Jan  1 01:18:48 kernel: usbserial.c: Generic converter now attached to ttyUSB2 (or usb/tts/2 for devfs)  
Jan  1 01:18:52 pppd\[227\]: pppd 2.4.2 started by admin, uid 0  
Jan  1 01:18:55 pppd\[227\]: Serial connection established.  
Jan  1 01:18:55 pppd\[227\]: Using interface ppp0  
Jan  1 01:18:55 pppd\[227\]: Connect: ppp0 <--> /dev/usb/tts/0  
Jan  1 01:19:03 pppd\[227\]: Could not determine remote IP address: defaulting to 10.64.64.64  
Jan  1 01:19:03 pppd\[227\]: local  IP address 114.xxx.xxx.xxx  
Jan  1 01:19:03 pppd\[227\]: remote IP address 10.64.64.64  
Jan  1 01:19:03 pppd\[227\]: primary   DNS address 168.95.1.1  
Jan  1 01:19:03 pppd\[227\]: secondary DNS address 168.95.192.1  
Jan  1 01:19:03 dnsmasq\[73\]: read /etc/hosts - 5 addresses  
Jan  1 01:19:03 dnsmasq\[73\]: reading /tmp/resolv.conf  
Jan  1 01:19:03 dnsmasq\[73\]: using nameserver 168.95.192.1#53  
Jan  1 01:19:03 dnsmasq\[73\]: using nameserver 168.95.1.1#53  
Jan  1 01:19:06 USB Connection: connected to ISP
```
  
4\. 無意中發現的好處  
測試了幾天, 發現透過 hub 的 3G 上網, 似乎比直接用 Mobile Partner 還快滿多的, 不知道是不是 USB device packet size 的功勞?  
  
**故障排除**  
1\. dmesg 是最基本的  
2\. /tmp/syslog.log  
  
**測試心得:**  
用了快兩個月, 基本上就是很好, 很穩, 很不錯!!
