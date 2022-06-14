# PTool 修改 Panasonic G2


## PTool

[PTool](https://www.personal-view.com/faqs/ptool/ptool-faq) 是個可以修改 Panasonic Lumix 系列相機的工具程式. 使用這個程式的主要原因是, 我的相機是買香港的水貨, 問題在於香港是使用 PAL (25fps), 但台灣是 NTSC (30fps), 因此在日光燈下錄影出來就會有水波紋...

## 功能

### Basic

*   Version incremental (打勾)
*   Prevent version compare (打勾)

### FPS change
  
G2 預設錄影是 25fps, 而 NTSC 是 29.97fps, 打勾這兩個可以改成 30fps  

*   720p59,94fps->29.97
*   PAL <-> NTSC
  
### Manual movie mode

*   Manual Movie Modes (打勾)
  
手動錄影的操作方式如下 (不需要轉到錄影模式)  

1.  開機
2.  選 PASM 任一種
3.  按下 Menu/Set
4.  再按一下 Menu/Set
5.  這時候左下角的 icon 就會變成錄影 M, 就可以調整 ISO, 光圈, 快門了.

### 流量
  
這個研究中, 雖然改了 potplayer 可以看到 Overall Bitrate 變了, 但檔案大小和實際流量似乎還是一樣?  
  

*   Video Bitrate FHD/SH (設成 50)
*   Video Bitrate H: (設成 32)
*   Video Bitrate L: (設成 20)
*   Overall Bitrate (設成 52)
