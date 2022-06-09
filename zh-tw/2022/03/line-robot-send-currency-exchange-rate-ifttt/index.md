# IFTTT 自動發 LINE 訊息 (免寫程式) - 以美元匯率為例


## 需求
用 LINE Bot 機器人每天自動發一個 LINE 訊息到群組, 要包含以下資訊.

1. 匯率
1. 一些連結到各銀行的匯率網站
1. 一些匯率的歷史圖表

## 做法: 使用 IFTTT + LINE

[IFTTT](https://ifttt.com/) 是個免費的服務, 他的名字是 if this then that 的縮寫. 以這個應用來講, 我們要的是, IF 拿到一個匯率, THEN 發一個 LINE 訊息, 作法如下:

<p>1. 在 IFTTT 建立一個新的 Applet</p>

<p>2. 新增一個 "If This"</p>

<a href="https://dennys.files.wordpress.com/2022/03/image-8.png"><img src="https://dennys.files.wordpress.com/2022/03/image-8.png?w=605" alt="" class="wp-image-432" width="382" height="180"/></a>

<p>3. 選擇 "Finance" service, 這個服務目前是由 Yahoo 提供的</p>

<a href="https://dennys.files.wordpress.com/2022/03/image.png"><img src="https://dennys.files.wordpress.com/2022/03/image.png?w=811" alt="" class="wp-image-416" width="446" height="274"/></a>

<p>4. 選擇"Today's exchange rate report"</p>

<a href="https://dennys.files.wordpress.com/2022/03/image-1.png"><img src="https://dennys.files.wordpress.com/2022/03/image-1.png?w=1024" alt="" class="wp-image-418" width="568" height="280"/></a>

<p>5. 在這裡輸入你需要的幣別和通知 (發 LINE) 的時間</p>

<a href="https://dennys.files.wordpress.com/2022/03/image-2.png"><img src="https://dennys.files.wordpress.com/2022/03/image-2.png?w=702" alt="" class="wp-image-420" width="385" height="373"/></a>

<p>6. 新增一個 "Then That"</p>

<a href="https://dennys.files.wordpress.com/2022/03/image-9.png"><img src="https://dennys.files.wordpress.com/2022/03/image-9.png?w=597" alt="" class="wp-image-434" width="288" height="148"/></a>

<p> 7. 選擇 "LINE" service</p>

<a href="https://dennys.files.wordpress.com/2022/03/image-13.png"><img src="https://dennys.files.wordpress.com/2022/03/image-13.png?w=651" alt="" class="wp-image-458" width="407" height="188"/></a>

<p>8. 目前他只有一個功能就是 Send message, 就選這個</p>

<a href="https://dennys.files.wordpress.com/2022/03/image-14.png"><img src="https://dennys.files.wordpress.com/2022/03/image-14.png?w=287" alt="" class="wp-image-459"/></a>

<p>9.1 這時 IFTTT 會需要取得和 LINE 做整合的權限, 請選 Connect</p>

<a href="https://dennys.files.wordpress.com/2022/03/image-20.png"><img src="https://dennys.files.wordpress.com/2022/03/image-20.png?w=599" alt="" class="wp-image-472" width="383" height="363"/></a>

<p>9.2 這裡請使用 LINE 的帳號密碼登入</p>

<a href="https://dennys.files.wordpress.com/2022/03/image-21.png"><img src="https://dennys.files.wordpress.com/2022/03/image-21.png?w=504" alt="" class="wp-image-473" width="285" height="432"/></a>

<p>9.3 這時請回到手機上輸入認證碼, 權限就會開通了</p>

<a href="https://dennys.files.wordpress.com/2022/03/image-22.png"><img src="https://dennys.files.wordpress.com/2022/03/image-22.png?w=499" alt="" class="wp-image-475" width="356" height="550"/></a>

<p>以上 9.1~9.3 的部分只有第一次和 IFTTT 整合需要, 以後就不需要這個動作了.</p>

<p>10. LINE, 建立一個群組, 譬如我這裡的 TEST1, 然後把 LINE Notify 加進這個群組</p>

<a href="https://dennys.files.wordpress.com/2022/03/image-17.png"><img src="https://dennys.files.wordpress.com/2022/03/image-17.png?w=278" alt="" class="wp-image-464" width="270" height="327"/></a>

<p>11. 設定好你要發訊息的群組 (Recipient) 還有內容</p>

<a href="https://dennys.files.wordpress.com/2022/03/image-15.png"><img src="https://dennys.files.wordpress.com/2022/03/image-15.png?w=430" alt="" class="wp-image-460" width="310" height="453"/></a>

<p>12. 之後 IFTTT 就會自動每天發訊息了</p>

<h2>注意</h2>

1. IFTTT 好像沒辦法手動 trigger, 而且每天只會發一次, 所以每次修改完, 得等第二天才知道結果, 這個不像 Zapier 可以直接測試, 得再看看有沒有好的解法.
1. 似乎只能發到群組, 他雖然有個 1 對 1 的訊息, 但我發到那個是沒用的
1. 如果沒有把 LINE Notify 加到群組, 他就會發個訊息告訴你

<a href="https://dennys.files.wordpress.com/2022/03/image-19.png"><img src="https://dennys.files.wordpress.com/2022/03/image-19.png?w=484" alt="" class="wp-image-468"/></a>

<h2>實際範例 (LINE)</h2>
設定好後, 每天在你設定的時間, 就會收到一個 LINE 訊息 (實測是會有 1~2 分鐘的延遲)</p>

<a href="https://dennys.files.wordpress.com/2022/04/image-5.png"><img src="https://dennys.files.wordpress.com/2022/04/image-5.png?w=381" alt="" class="wp-image-505"/></a>

<h2>實際範例 (手機通知)</h2>
其實很方便, 每天就會有個手機通知, 想看細節再點連結進去.</p>

<a href="https://dennys.files.wordpress.com/2022/04/image-4.png"><img src="https://dennys.files.wordpress.com/2022/04/image-4.png?w=361" alt="" class="wp-image-503"/></a>

<h2>實際範例 (電腦版 LINE 的截圖)</h2>

<a href="https://dennys.files.wordpress.com/2022/03/image-16.png"><img src="https://dennys.files.wordpress.com/2022/03/image-16.png?w=280" alt="" class="wp-image-462"/></a>

<h2>IFTTT</h2>
這是這個 IFTTT applet 的連結: <a href="https://ifttt.com/applets/pC2DyG9z-line">https://ifttt.com/applets/pC2DyG9z-line</a></p>

