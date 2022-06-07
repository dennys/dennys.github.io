# 更新 Unlock 過的 Pixel 5


# 事前準備

1. 下載
    1. [Pixel OTA Images](https://developers.google.com/android/ota)
    1. [Pixel Factory Images](https://developers.google.com/android/images)
1. 把redfin-sq1a.220205.002-factory-734556ab.zip (檔案名稱看實際的版本) 解壓縮, 要解兩層, 直到解出 boot.img
1. 利用 Magisk 修補 boot.img, 產生的檔案名稱長類似這樣 magisk_patched-24201_LSQ4E.img, 再把這個檔案傳回電腦中
1. 進入開發者模式並確定 adb devices 可以連

<!-- wp:image {"id":256,"sizeSlug":"large","linkDestination":"media"} -->
<figure class="wp-block-image size-large"><a href="https://dennys.files.wordpress.com/2022/01/auth.jpg"><img src="https://dennys.files.wordpress.com/2022/01/auth.jpg?w=197" alt="" class="wp-image-256"/></a></figure>
<!-- /wp:image -->
<br>

# 更新

1. 執行以下指令重新開機進入sideload mode

    adb reboot sideload

1. 刷入安全性更新檔案, 這個步驟會比較久

    adb sideload redfin-ota-sq1a.220205.002-84ba2421.zip
    
1. 完成上傳後<strong><mark style="background-color:rgba(0, 0, 0, 0);" class="has-inline-color has-vivid-red-color">勿重新開機，"在手機上" 選擇Reboot to bootloader</mark></strong>

1. 刷入被 Magisk 修補後的 boot.img

    fastboot flash boot --slot=all magisk_patched-24201_LSQ4E.img
    
1. 執行以下指令重新開機

    fastboot reboot

1. 進入 設定-&gt;關於-&gt;Android 版本, 確定有更新成功.</li></ol>

# 參考
1. https://forum.xda-developers.com/t/4356219/
1. https://www.mobile01.com/topicdetail.php?f=565&t=6487485

