# 無聲無影遠端桌面連線 - VNC


## 需求

需要一個遠端登入的功能, 要做到以下幾項功能
1. 進去的時候不能有任何跳出的畫面, 就是不讓使用者有感覺的意思
1. 進去的時候, 預設是只有 "看" 而不能控制滑鼠鍵盤, 同樣也是不讓使用者有感覺
1. 有需要的話, 可以切換一下, 就可以控制滑鼠鍵盤了

## 方法 (Server 端)

測試了一下, [TeamViewer](https://www.teamviewer.com/) 和 [AnyDesk](https://anydesk.com/) 都做不出來這種功能, 目前是只有看到 [RealVNC](https://www.realvnc.com/)可以做到, 就先用這個測試.

1. 先下載 RealVNC server (從這裡下載: https://www.realvnc.com/en/connect/download/vnc/)
2. 打開設定, 把 show accept/reject prompt for each connection 關掉, 這樣連線上去時就不會跳一個視窗出來問是否同意連線.

<a href="https://dennys.files.wordpress.com/2022/06/image-3.png"><img src="https://dennys.files.wordpress.com/2022/06/image-3.png?w=608" alt="" class="wp-image-749"/></a>
<br>

3. 再關掉 Notify when users connect and disconnect, 這樣連線時就不會跳出視窗. 另外, 預設是如果一段時間沒動作, 就會自動斷線. 如果不希望這樣, 可以把 Disconnect users who have been idle for an extended period 取消, 這樣就算放一整天也不會斷線. (也可以在 Expert 裡面設定 idleTimeout 時間)

<a href="https://dennys.files.wordpress.com/2022/06/image-4.png"><img src="https://dennys.files.wordpress.com/2022/06/image-4.png?w=608" alt="" class="wp-image-751"/></a>
<br>

如果沒有關掉, 那有人連上來時, 就會跳出這個視窗, 就被發現了. :yum:

<a href="https://dennys.files.wordpress.com/2022/06/image-5.png"><img src="https://dennys.files.wordpress.com/2022/06/image-5.png?w=335" alt="" class="wp-image-754"/></a>
<br>

## 方法 (Client 端)

1. 先下載 RealVNC viewer (從這裡下載: https://www.realvnc.com/en/connect/download/viewer/)

2. 在連線以前, 把 Option 裡面的 View-only 打開, 這樣你連上去的時候, 就不會去動到對方的滑鼠.

<a href="https://dennys.files.wordpress.com/2022/06/image-7.png"><img src="https://dennys.files.wordpress.com/2022/06/image-7.png?w=516" alt="" class="wp-image-758"/></a>
<br>

## 成果

依照以上設定好, 就可以神不知鬼不覺得連到對方電腦了. 當然, 對方如果進去 VNC 看, 還是可以看到有誰連進來, 這個就沒招了.

<a href="https://dennys.files.wordpress.com/2022/06/image-8.png"><img src="https://dennys.files.wordpress.com/2022/06/image-8.png?w=618" alt="" class="wp-image-760"/></a>
<br>

## 如果臨時想去移動對方的鍵盤滑鼠怎麼辦?

很簡單, VNC 的工具列可以改設定, 直接把 View-only 拿掉就可以了.

<a href="https://dennys.files.wordpress.com/2022/06/image-9.png"><img src="https://dennys.files.wordpress.com/2022/06/image-9.png?w=469" alt="" class="wp-image-762"/></a>

