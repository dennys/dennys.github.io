# 如何下載 YouTube 私人影片 (使用 youtube-dl 或 ytl-dp)


## 需求
YouTube 預設是不能讓人下載影片的, 即使是你自己上傳的影片也是一樣. 然後我的影片大多都設成私人, 就更沒辦法用一堆線上工具下載了. 研究了一下兩年前曾被 GitHub 下架的 youtube-dl, 看來他支援下載私人影片 (不需要帳號密碼, 用 cookie).

## 方法

### 準備工具

1. [youtube-dl](https://github.com/ytdl-org/youtube-dl) 或是 [ytl-dp](https://github.com/yt-dlp/yt-dlp)
1. cookie 工具, 我是用 [Get cookies.txt](https://chrome.google.com/webstore/detail/get-cookiestxt/bgaddhkoddajcdgocldbbfleckgcbcid) 這個 Chrome extension
1. [FFmpeg](https://www.ffmpeg.org/download.html), 轉檔用

### 準備 cookie

既然這是私人影片, 就會卡權限, 因此要準備 cookie. (注意: 這個套件不會去抓無痕模式的 cookie). 安裝好後, 連上 youtube.com, 然後 export 檔案出來就好. 預設他取的名字會是 youtube.com_cookies.txt, 這邊就不修改檔案名稱.

<a href="https://dennys.files.wordpress.com/2022/05/image-14.png"><img src="https://dennys.files.wordpress.com/2022/05/image-14.png?w=616" alt="" class="wp-image-634"/></a>

### 使用 youtube-dl 下載

使用如下指令就可以下載檔案了

```cmd
youtube-dl.exe --format bestvideo+bestaudio --limit-rate 2M cookies=youtube.com_cookies.txt https://www.youtube.com/watch?v=********/
```

要注意的是, 如果沒有 FFmpeg, youtube-dl 就不會合併檔案, 最後就會有一個影片(mp4), 一個聲音 (m4a), 得要再另外 merge, 很麻煩, 如下圖.

<a href="https://dennys.files.wordpress.com/2022/05/image-12.png"><img src="https://dennys.files.wordpress.com/2022/05/image-12.png?w=1024"/></a>

因此, 直接在下載參數指定 FFmpeg 的目錄位置, 這樣在下載完後, 他就會去呼叫 FFmpeg 執行合併檔案的動作.

```cmd
youtube-dl.exe --ffmpeg-location C:\bin\ffmpeg\bin</mark> --format bestvideo+bestaudio --limit-rate 2M --cookies=youtube.com_cookies.txt https://www.youtube.com/watch?v=********/
```

如下, 他會把 mp4/m4a 合併成一個 mp4 檔案, 然後把原來的檔案刪除.

<a href="https://dennys.files.wordpress.com/2022/05/image-11.png"><img src="https://dennys.files.wordpress.com/2022/05/image-11.png?w=1024"/></a>

### 使用 youtube-dl 下載

[YT-DLP](https://github.com/yt-dlp/yt-dlp) 是 youtube-dl 的一個 fork, 他自己的官方說明是 "A youtube-dl fork with additional features and fixes", 兩者的指令也都差不多, 試用了一下也沒甚麼問題, 只是下載完是 .mkv

<a href="https://dennys.files.wordpress.com/2022/05/image-13.png"><img src="https://dennys.files.wordpress.com/2022/05/image-13.png?w=960"/></a>

