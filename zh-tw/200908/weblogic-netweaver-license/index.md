# WebLogic 和 NetWeaver 的 License


又是一篇筆記, 又是一個吃虧兩次的問題...  
  
**[WebLogic](http://www.bea.com/framework.jsp?CNT=index.htm&FP=/content/products/weblogic)**  
  
先講 WebLogic, 免費的 developer license, 可以用一年, 但是只能有 5 個 IP 連上來, 而且我覺得他 server 本身應該就已經算上一個 IP 了. 如果超過了, 第 6 個以後的 IP 會看到錯誤訊息  
錯誤訊息如下:  
  
`<BEA-000211> <Connection rejected, the server license allows connections from only 5 unique IP addresses.>`

聽說 Bea 在被 Oracle 買下之後, 這些 license 檔案都開放自由下載了(當然, 該給錢的還是要給), 不過我是沒有找到就是了. (有人如果知道, 麻煩提供一下, 謝謝.  
  
解法:  
1. 重開機就會重新計算  
2. 當然, 放上 license 就可以了.  
  
參考: [WebLogic Overview](http://e-docs.bea.com/wls/docs92/intro/overview.html#wp1087177)  
[http://edocs.bea.com/platform/docs81/install/license.html](http://edocs.bea.com/platform/docs81/install/license.html "http://edocs.bea.com/platform/docs81/install/license.html")  
  
**NetWeaver**  
  
NetWeaver 安裝完之後, 會有一個 temporary license, 可以使用 28 天, 過了時間之後, 每 25 分鐘會給你自動停機. 但是如果直接 restart 是無法啟動的, 得先 stop 再 start, 而且重點是, 畫面上是看不到的, 因此得去 log 裡面翻箱倒櫃找...  
  
檔案位置: /usr/sap///j2ee/cluster/server0/log/defaultTrace\*  
  
錯誤訊息: 

`The installed license for software product "{0}" is NOT valid.`

參考:
1. http://help.sap.com/saphelp_nw04s/helpdata/en/14/5e533e5ff4d064e10000000a114084/frameset.htm
1. [How to check the default trace](https://wiki.sdn.sap.com/wiki/display/BPMT/How%20to%20check%20the%20default%20trace)
