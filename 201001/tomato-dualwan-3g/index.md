# Tomato DualWan 之 3G 上網


[![](http://4.bp.blogspot.com/-rCd8h7VM0WI/ToJhcAfqcxI/AAAAAAAABkc/dog5uxnIWxs/s320/Asus_500gp_1.jpg)](http://4.bp.blogspot.com/-rCd8h7VM0WI/ToJhcAfqcxI/AAAAAAAABkc/dog5uxnIWxs/s1600/Asus_500gp_1.jpg)

  

**注意：所有改機流程皆有其風險，請各位朋友在開始動作以前自行評估!!**  
  
**前言:**  
無意中發現 Tomato DualWAN 自 [1.23.0410](http://bbs.dualwan.cn/thread-9022-1-1.html) (2009/11/09 的版本) 開始, 已經有支援 3G 上網了, 因此就拿來測試了一下. 要注意的是, 雖然這個 firmware 強調的是 Dual Wan, 不過目前只可以同時接一個 3G網卡（或手機）, 沒辦法接兩個. 也就是說, 無法同時使用譬如中華電信+威寶3G兩個來loading balance. 但是可以同時使用 ADSL (PPPoE) + 3G, 我有測試過.  
  
  
  
**軟硬體:**  

*   Hub: [Asus WL-500gP V1](http://tw.asus.com/product.aspx?P_ID=8el2DcrRjLoHNdQ8&templete=2)
*   USB 網卡: [Huawei E169](http://www.huawei.com/mobileweb/en/products/view.do?id=1181)
*   Firmware: [Tomato DualWan](http://www.dualwan.cn/) (1.23.0410 和 1.23.0441)

**前置準備:**  

*   將 firmware 刷為 Tomato DualWan, 請參考[官方教程](http://www.dualwan.cn/index.php/manual-overview)
*   把 SIM 卡的 pin code 給 disable

**設定步驟:**  
  
1\. 設定比 CDMA@wifi 簡單許多, 如下圖, 先把 USB 給 enable 起來 (紅框部份). 把這幾個都打開, 要確定 USB 網卡有抓到(如藍框部份). 然後設定的部份, 可能不見得需要把這些全部 enable, 有興趣的朋友或許可以測試一下哪些是可以 disable 的.  
  
[![Tomato DualWan 3G](http://farm3.static.flickr.com/2680/4394875396_f4933b89fd.jpg "Tomato DualWan 3G")](http://www.flickr.com/photos/dennys_blog/4394875396 "Tomato DualWan 3G")  
  
2\. 再來就只要一個設定就可以了. 請參考下圖, 將連線類型選為"GPRS/EDGE/UMTS/EVDO" 並設定撥號號碼為 \*99# 就可以了, APN 不用設定 (也沒找到地方可以設定?) 要注意的是, 斷線重連時間, 官方是建議 60 秒以上.  
  
[![Tomato DualWan 3G](http://farm5.static.flickr.com/4019/4394117833_7aa516fd42.jpg "Tomato DualWan 3G")](http://www.flickr.com/photos/dennys_blog/4394117833 "Tomato DualWan 3G")  
  
3\. 接下來就會開始自動撥號了(官方文件是說要手動去按連線, 不過我發現是不需要).  
觀察 /tmp/var/log/message, 首先會看到下列這一段  
  
  

Jan  1 08:00:13 user.info kernel: usbserial.c: Option GSM modem converter now attached to ttyUSB2 (or usb/tts/2 for devfs)  
Jan  1 08:00:14 user.warn kernel: Vendor: HUAWEI Model: Mass Storage Rev: 2.31  
Jan  1 08:00:14 user.warn kernel: Type:   CD-ROM ANSI SCSI revision: 02  
Jan  1 08:00:17 user.info hotplug\[293\]: device 12d1/1001/0 switch to modem mode

  
以上代表偵測到 USB 網卡了  
因為是設定 60 秒重新連線, 因此在上面 log 出來後, 會停下來, 60 秒之後會再看到下列這一段, 這樣就代表成功了.  
  
  

Jan  1 08:01:17 user.info redial\[92\]: ALL WAN down. Reconnecting...  
Jan  1 08:01:17 user.info redial\[312\]: Started. Time: 60  
Jan  1 08:01:19 daemon.info pppd\[311\]: Serial connection established.  
Jan  1 08:01:19 daemon.info pppd\[311\]: Using interface ppp0  
Jan  1 08:01:19 daemon.notice pppd\[311\]: Connect: ppp0 <--> /dev/usb/tts/0  
Jan  1 08:01:23 daemon.warn pppd\[311\]: Could not determine remote IP address: defaulting to 10.64.64.64  
Jan  1 08:01:23 daemon.notice pppd\[311\]: local  IP address 114.136.\*\*\*.\*\*\*  
Jan  1 08:01:23 daemon.notice pppd\[311\]: remote IP address 10.64.64.64  
Jan  1 08:01:23 daemon.notice pppd\[311\]: primary   DNS address 168.95.1.1  
Jan  1 08:01:23 daemon.notice pppd\[311\]: secondary DNS address 168.95.192.

  
**測試心得:**  
有試著連續開好幾天, 基本上連線是 ok 的, 但是有幾個地方很困擾.  
  
1\. keventd 的 loading 很高, 開個 ftp (100KB/ 左右) 就可以接近 100%. 原因不知, 已向官網[反應](http://bbs.dualwan.cn/thread-15752-1-1.html), 但似乎沒有別人碰到類似的問題... 我覺得應該是 3G 的問題, 因為有在內部網路傳檔 (1.5MB/s), 那時候的 loading 都滿低的... 下面是用 [Firefox](http://www.mozilla.com/firefox/) 開 [GMail](http://gmail.com/ "Google Mail") 時的 top 結果.  
  
  

Mem: 52020K used, 10944K free, 0K shrd, 2216K buff, 34944K cached  
CPU:  15% usr  84% sys   0% nic   0% idle   0% io   0% irq   0% sirq  
Load average: 1.34 0.46 0.17 5/35 671  
  PID  PPID USER     STAT   VSZ %MEM %CPU COMMAND  
    2     1 root     RW       0   0%  67% \[keventd\]  
  238     1 root     R     1224   2%  21% klogd  
  230     1 root     R     1228   2%  10% syslogd -L -s 50  
  657   633 root     R     1236   2%   0% top

  

  
2\. 在 3G 連線成功後, /tmp/var/log/message 會一直固定有同樣的重複錯誤訊息 log 如下, 量多到非常驚人, 基本上其他的訊息都被蓋掉了, 所以會很不方便. 其實是這個問題造成前面的問題也說不定, 也向官網[反應](http://bbs.dualwan.cn/viewthread.php?tid=12742)了.  
  
  

Jan 17 23:01:31 unknown user.err kernel: usb-uhci.c: ENXIO c0010400, flags 0, urb 877ad920, burb 877ad620  
Jan 17 23:01:31 unknown user.err kernel: usb-uhci.c: ENXIO c0010400, flags 0, urb 877ad820, burb 877ad620  
Jan 17 23:01:31 unknown user.err kernel: usb-uhci.c: ENXIO c0010400, flags 0, urb 877ad8a0, burb 877ad620  
Jan 17 23:01:31 unknown user.err kernel: usb-uhci.c: ENXIO c0010400, flags 0, urb 877ad920, burb 877ad620

  
3\. 連線是穩定的, 但是重新插拔網卡之後, 重新連線時不夠穩定, 常常會連不上線, 得插插拔拔重開好幾次... 這部份 CDMA@wifi 就很棒了, 隨便怎麼弄, 大概都是 10 幾秒就重新連線成功.  
官方說法是 "需要手動點擊網頁上的連接按鈕才能撥號", 不過我實測時發現他幾乎都會自動連線, 只是問題在於, 常常連線失敗.  
官方說法是 "已知問題：重啟路由器後有些上網卡需要重新插拔才能被認到，有些上網卡斷開連接後需要重新插拔才可以再次撥號。另外，由於上網卡撥號過程相對較長，所以請將斷線重撥的時間由30秒改為60秒或者更高，以免撥號時間過長系統誤判為撥號沒成功"  
  
**結論:**  
在 CPU, log 以及重新連線問題解決之前, 看來還需要再觀望一段時間... 可是我總覺得, 好像只有我碰到這個問題啊 ...
