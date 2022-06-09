# Android 11 如何不要驗證 Wi-Fi CA 憑證


<p><mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-vivid-red-color"><strong>注意: 系統刷機有一定的風險, 可能導致手機無法開機和失去保固, 資料還會被清空, 執行前請先三思!!!</strong></mark></p>

<p><strong><mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-luminous-vivid-amber-color">注意: 系統刷機有一定的風險, 可能導致手機無法開機和失去保固, 資料還會被清空, 執行前請先三思!!!</mark></strong></p>

<p><strong><mark style="background-color:rgba(0,0,0,0);" class="has-inline-color has-luminous-vivid-orange-color">注意: 系統刷機有一定的風險, 可能導致手機無法開機和失去保固, 資料還會被清空, 執行前請先三思!!!</mark></strong></p>

<p>======================================================</p>

## 更新紀錄

1. 2022/05/30: 新增系統更新或 Wi-Fi 密碼修改後的處理方式

## Wi-Fi CA 憑證驗證在 Android 11 的改變

Android 10 以前, 如果是連到 Enterprise Wi-Fi (EAP/PEAP), CA 憑證是可以選 "不要驗證", 如下:

<a href="https://dennys.files.wordpress.com/2022/02/image-13.png"><img src="https://dennys.files.wordpress.com/2022/02/image-13.png?w=346" alt="" class="wp-image-394" width="233" height="297"/></a>

自 Android 11 起, 如果是連到 Enterprise Wi-Fi (PEAP), CA 憑證就無法選 "不要驗證", 原因可參考 https://www.xda-developers.com/android-11-break-enterprise-wifi-connection/. 這個方向是對的, 但如果網管不改, 一般使用者恐怕也無能為力. (<mark style="background-color:rgba(0, 0, 0, 0);" class="has-inline-color has-vivid-red-color">而且 iPhone 至少到 15.4 都沒有卡這個...</mark>)

爬了一下文, 看來是可以直接把這個設定加回去 (參考: https://android.stackexchange.com/questions/231859/), 但很不幸的, 檔案是放在 /data/misc/apexdata/com.android.wifi/WifiConfigStore.xm, 這裡的檔案<mark style="background-color:rgba(0, 0, 0, 0);" class="has-inline-color has-vivid-red-color">必須 Unlock 才能修改</mark>. 文章裡面是寫有可能透過 App 修改, 但我沒找到方法, 如果有人找到好方法了, 也麻煩分享一下.

## 修改步驟

<p>1. 首先, 要 Unlock, Pixel 5 (Android 12) 可參考 <a href="https://wordpress.com/post/dennys.wordpress.com/280">https://wordpress.com/post/dennys.wordpress.com/280</a>. (要做到把 boot.img 換掉)</p>

<p>2. 試著連上 Wi-Fi, 網域就隨便給他寫, 然後試著連線, 這<mark style="background-color:rgba(0, 0, 0, 0);" class="has-inline-color has-vivid-red-color">當然是不會成功</mark>, 目的只是要先寫入一版 Config 檔案, 等一下可以修改.</p>

<a href="https://dennys.files.wordpress.com/2022/02/1-1.png"><img src="https://dennys.files.wordpress.com/2022/02/1-1.png?w=373" alt="" class="wp-image-316" width="261" height="369"/></a>

<p>3. 先用下列指令檢查 WifiConfigStore.xml 是否正確, 這裡也可以用 adb pull 拉到電腦檢查, 但我一直沒成功, 就先用 cat 看. 若執行時發生</p>

<pre class="wp-block-code"><code>adb shell
su
cat /data/misc/apexdata/com.android.wifi/WifiConfigStore.xml</code></pre>

<p>若執行 su 時發生 inaccessible or not found (如下), 通常是 root 沒成功, 請再確認一次 root 步驟.</p>

<pre class="wp-block-code"><code>127|redfin:/system/bin $ su
/system/bin/sh: su: inaccessible or not found</code></pre>

讀出來的 WifiConfigStore.xml 內容如下, 可找 WifiEnterpriseConfiguration 這個關鍵字, 目的是要把紅色的設定值拿掉

<pre class="wp-block-code"><code>&lt;WifiEnterpriseConfiguration&gt;
&lt;string name="Identity"&gt;這裡是帳號名字&lt;/string&gt;
&lt;string name="AnonIdentity"&gt;&lt;/string&gt;
&lt;string name="Password"&gt;這裡是密碼&lt;/string&gt;
&lt;string name="ClientCert"&gt;&lt;/string&gt;
&lt;string name="CaCert"&gt;&lt;/string&gt;
&lt;string name="SubjectMatch"&gt;&lt;/string&gt;
&lt;string name="Engine"&gt;0&lt;/string&gt;
&lt;string name="EngineId"&gt;&lt;/string&gt;
&lt;string name="PrivateKeyId"&gt;&lt;/string&gt;
&lt;string name="AltSubjectMatch"&gt;&lt;/string&gt;
&lt;<mark style="background-color:rgba(0, 0, 0, 0);" class="has-inline-color has-vivid-red-color">string name="DomSuffixMatch"&gt;這裡是上面隨便輸入的網域&lt;/string&gt;
&lt;string name="CaPath"&gt;/system/etc/security/cacerts&lt;/string&gt;</mark>
&lt;int name="EapMethod" value="0" /&gt;
&lt;int name="Phase2Method" value="4" /&gt;
&lt;string name="PLMN"&gt;&lt;/string&gt;
&lt;string name="Realm"&gt;&lt;/string&gt;
&lt;int name="Ocsp" value="0" /&gt;
&lt;string name="WapiCertSuite"&gt;&lt;/string&gt;
&lt;boolean name="AppInstalledRootCaCert" value="false" /&gt;
&lt;boolean name="AppInstalledPrivateKey" value="false" /&gt;
&lt;null name="KeyChainAlias" /&gt;
&lt;null name="DecoratedIdentityPrefix" /&gt;
&lt;/WifiEnterpriseConfiguration&gt;</code></pre>

<p>4. 執行以下指令 (這指令是直接從上面那篇 stackexchange 抄過來的), 他就會把 <mark style="background-color:rgba(0, 0, 0, 0);" class="has-inline-color has-vivid-red-color">CaPath</mark> 和 <mark style="background-color:rgba(0, 0, 0, 0);" class="has-inline-color has-vivid-red-color">DomSuffixMatch</mark> 的內容改成空白. 最後一個步驟會重開, <mark style="background-color:rgba(0, 0, 0, 0);" class="has-inline-color has-vivid-red-color">要重開才會生效</mark>.</p>

<pre class="wp-block-code"><code>adb shell
su
sed -i 's%&lt;string name="CaPath"&gt;.*&lt;/string&gt;%&lt;string name="CaPath"&gt;&lt;/string&gt;%' /data/misc/apexdata/com.android.wifi/WifiConfigStore.xml
sed -i 's%&lt;string name="DomSuffixMatch"&gt;.*&lt;/string&gt;%&lt;string name="DomSuffixMatch"&gt;&lt;/string&gt;%' /data/misc/apexdata/com.android.wifi/WifiConfigStore.xml
reboot</code></pre>

<p>5. 重開完後, 檢查 /data/misc/apexdata/com.android.wifi/WifiConfigStore.xml 的內容, 確認下面紅色這段的設定被清空了</p>

<pre class="wp-block-code"><code>&lt;WifiEnterpriseConfiguration&gt;
&lt;string name="Identity"&gt;這裡是帳號名字&lt;/string&gt;
&lt;string name="AnonIdentity"&gt;&lt;/string&gt;
&lt;string name="Password"&gt;這裡是密碼&lt;/string&gt;
&lt;string name="ClientCert"&gt;&lt;/string&gt;
&lt;string name="CaCert"&gt;&lt;/string&gt;
&lt;string name="SubjectMatch"&gt;&lt;/string&gt;
&lt;string name="Engine"&gt;0&lt;/string&gt;
&lt;string name="EngineId"&gt;&lt;/string&gt;
&lt;string name="PrivateKeyId"&gt;&lt;/string&gt;
&lt;string name="AltSubjectMatch"&gt;&lt;/string&gt;
<mark style="background-color:rgba(0, 0, 0, 0);" class="has-inline-color has-vivid-red-color">&lt;string name="DomSuffixMatch"&gt;&lt;/string&gt;</mark>
<mark style="background-color:rgba(0, 0, 0, 0);" class="has-inline-color has-vivid-red-color">&lt;string name="CaPath"&gt;&lt;/string&gt;</mark>
&lt;int name="EapMethod" value="0" /&gt;
&lt;int name="Phase2Method" value="4" /&gt;
&lt;string name="PLMN"&gt;&lt;/string&gt;
&lt;string name="Realm"&gt;&lt;/string&gt;
&lt;int name="Ocsp" value="0" /&gt;
&lt;string name="WapiCertSuite"&gt;&lt;/string&gt;
&lt;boolean name="AppInstalledRootCaCert" value="false" /&gt;
&lt;boolean name="AppInstalledPrivateKey" value="false" /&gt;
&lt;null name="KeyChainAlias" /&gt;
&lt;null name="DecoratedIdentityPrefix" /&gt;
&lt;/WifiEnterpriseConfiguration&gt;</code></pre>

<p>6. 這時候應該就可以連線了</p>

<p>7. 因為實際上並不需要 root, 只是為了修改 WifiConfigStore.xml, 因此改完後可以 UnRoot (移除 Magisk 和刷回原廠 boot.img), 但因為解鎖會清空資料, 而且下次又遇到新的 Wi-Fi 又得重來, 我就維持解鎖狀態了. 方法可參考 https://dennys.wordpress.com/?p=330</p>

## 系統更新後如何處理?

不用處理, 至少目前我的 Pixel 5 更新到 2022 年 5 月更新 (12.1.0 (SP2A.220505.002, May 2022) 時, 都不需要處理.

## Wi-Fi 密碼更新後如何處理?

1. 如果系統有更新過, 就得先用 Magisk 重新 patch 一次.
1. 一樣要試著連線 (當然不會成功), 重點是把密碼寫進去.
1. 然後再用 adb shell 跑一次上面的 sed 指令修改 WifiConfigStore.xml 即可.

## 後記

這個方法並不是把不要驗證 (Do Not Validate) 的設定加回去, 而是先嘗試連線一個要驗證的 Wi-Fi (當然這裡會失敗), 等他把設定檔存下來之後, 再進系統把憑證的設定改掉, 所以還是和 Android 10 以前不太一樣. 而且<mark style="background-color:rgba(0, 0, 0, 0);" class="has-inline-color has-vivid-red-color">每次連上一個新的要使用 EAP 的 Wi-Fi 基地台都要重新執行一次</mark>, 會比較麻煩.

