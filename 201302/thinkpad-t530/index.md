# Thinkpad T530


## 購買

Lenovo Coupon: http://www.dealigg.com/story-Lenovo-Coupons-Promotions  

## 使用手冊

http://support.lenovo.com/en_US/downloads/detail.page?DocID=UM014841  

## 硬體

### 硬碟

SATA3, 7mm規格 (注意不是 9mm)

### mSATA

很可惜的, mSATA 只支援 SATA 2, 而且官方文件只建議用來當 cache 而不建議拿來當開機碟? 雖然網路上也看到很多人用來開機沒問題. 似乎是因為 Ivy Bridge 只支援兩個 SATA3, 而 HDD, DVD 就用掉 2 個了.  

### WiFi

*   Thinkpad 1x1 bgn
*   Intel Centrino Wireless-N 2200 2x2 BGN
*   [Intel Centrino Advanced-N 6205 AGN](http://www.intel.com/content/www/us/en/wireless-products/centrino-advanced-n-6205.html) : dual stream (2x2) and up to 300Mbps (2 x 150Mbps per stream), 2.4GHz and 5GHz
*   [Intel Centrino Ultimate-N 6300 AGN](http://www.intel.com/content/www/us/en/wireless-products/centrino-ultimate-n-6300.html): triple stream (3x3) and up to 450Mbps (3 x 150Mbps per stream), 2.4GHz and 5GHz

### **外接螢幕:**

mini Display: 有 ++, 有含聲音輸出  
  

## 軟體設定

### **Battery:**

Win 7: 使用 Power Manager  
Win 8: 沒有 Power Manager 可用  

1.  安裝 Power Management Driver
2.  安裝 Lenovo Settings Dependency Package
3.  修改 registry (記得要用 10 進位)
4.  重開 (重裝 driver 後似乎會被 reset ?)

```
Windows Registry Editor Version 5.00  
  
\[HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Lenovo\\PWRMGRV\\Data\]  
"ChargeStartControl"=dword:00000050  
"ChargeStopControl"=dword:0000005a  
"ChargeStartPercentage"=dword:00000050  
"ChargeStopPercentage"=dword:0000005a  

```  
**參考文章**  

*   [ThinkVantage-Power Manager 導覽 (ThinkPad電源管理懶人包)](http://www.mobile01.com/topicdetail.php?f=240&t=2460214)
*   [LENOVO 電池使用教學及注意事項](http://www.mobile01.com/topicdetail.php?f=240&t=2620283)
*   [Changing Charge Threshold on ThinkPads under Windows 8 without Power Manager](http://forums.lenovo.com/t5/Windows-8-Knowledge-Base/Changing-Charge-Threshold-on-ThinkPads-under-Windows-8-without/ta-p/920657)
*   [\[分享\] 在Windows 8中， 如何更改充電池的管理限制 (X230測試過)](http://www.mobile01.com/topicdetail.php?f=240&t=3090945)

### 修改 Power button 功能:

http://www.tomsitpro.com/articles/windows\_8-sleep-hibernation-power\_settings-hiberfil.sys,2-229-2.html  

### 自動登入:

1.  執行 netplwiz
2.  把"必須輸入使用者名稱和密碼，才能使用這台電腦(E)"的打勾取消掉
3.  確定後會需要輸入這個帳號以及密碼, 這樣下次就不需要 login 了

參考文章: http://changyang319.pixnet.net/blog/post/31344187  

### 中文輸入:
[Windows 8 如何設定 Ctrl+Shift 或 Alt+Shift 切換輸入法](http://idaiwan.pixnet.net/blog/post/38297889)  
[透過外掛程式讓 Windows 8 使用 Ctrl+Space 切換輸入法](http://idaiwan.pixnet.net/blog/post/38337105-%E9%80%8F%E9%81%8E%E5%A4%96%E6%8E%9B%E7%A8%8B%E5%BC%8F%E8%AE%93-windows-8-%E4%BD%BF%E7%94%A8-ctrl%2Bspace-%E5%88%87%E6%8F%9B%E8%BC%B8)  
[繁體中文語言如何變更預設輸入法(英文) ](http://blog.miniasp.com/post/2012/06/30/Windows-8-Tips-How-to-change-default-input-method-for-languages.aspx)  
    
## 內建的讀卡機沒法開機? 

同一張 SD 卡拿去別的電腦就可以開機, 然後把 SD 卡插在外接的 USB 讀卡機, 在開機時偵測的到(就是會有 SD 卡開機的選項出來), 但按下去還是沒用, 似乎他有嘗試去讀, 但無法開機的問題知道原因了, 是 UEFI BIOS 不支援 NTFS 的 USB 開機, 所以要把 SD 卡 format 成 FAB32. 但幾個製作開機光碟的程式又只支援 NTFS, 網路上找到的解法如下, 外接讀卡機可以開機, 但內建那個還是無法開機.  
  
1. 格式化成 fat32  
2. 用 WinRAR 或任何軟體把 ISO 解開, 檔案都複製到 USB  
3. 假設 USB 在 E:, 使用 administrator 權限在 CMD 執行 bootsect /nt60 E: (我執行時有發生無法寫入的錯誤, 後來改成 bootsect /nt60 /force E: 就可以了.  
  
還有些方法是關掉 UEFI 以及用 xcopy 直接建立開機檔案之類的, 可以 google 一下. 關鍵字可用 windows usb boot fat32 之類的即可.
