# Command Line Deploy on WebLogic


## 前言

在 ear 檔愈長愈大之後 (30mb左右), deploy 程式變成一個有點累的動作, 因為 [Ant](http://ant.apache.org/) 只能做到把 ear 檔給 build 好, 之後的動作 (開 browser, login, deploy, active, start ap, ...) 需要不少時間, 不但要按好幾個按鍵, 中間還得等待, 因此, 得找個方法讓 deploy 的動作自動化, 以便放進 Ant 或是批次檔. 翻了一下 [WebLogic](http://www.bea.com/framework.jsp?CNT=index.htm&FP=/content/products/weblogic) 的文件, 看來不難做到, 而且還滿方便的.

## Command line deploy 的方法

如果 WebLogic 就裝在**自己的電腦**上, 假設程式放在 D:\\app\_name.ear, 使用下列的指令就可以很方便的直接 deploy 上去.  

```cmd
java -cp D:\\bea\\wlserver\_10.0\\server\\lib\\weblogic.jar weblogic.Deployer -adminurl t3://localhost:7001 -user weblogic -password weblogic -deploy "D:\\app\_name.ear"
```

如果 WebLogic 是裝在**遠端的電腦**, 指令基本上還是一樣, 只要多加上 -upload 參數就可以了.  
 
```cmd
java -cp D:\\bea\\wlserver\_10.0\\server\\lib\\weblogic.jar weblogic.Deployer -adminurl t3://remotehost:7001 -user weblogic -password weblogic -deploy -upload "D:\\app\_name.ear"
```
  
需要注意的是, 如果之前已經有用 WebLogic 的 console 把程式給 deploy 上去, 請先刪除之前 upload 的檔案, 原因如下:  
  
A. 使用 WebLogic 的 console 來作本機 deploy, 程式最後會被放到下面這個位置  
`user\_projects\\domains\\base\_domain\\servers\\AdminServer\\upload\\app\_name.ear`

B. 使用 WebLogic 的 deployer 來作遠端 deploy, 程式最後會被放到下面這個位置  
`user\_projects\\domains\\base\_domain\\servers\\AdminServer\\upload\\app\_name.ear\\app\\app\_name.ear`
  
使用 A 方法產生的檔案如果沒有刪除, 則 B 方法在建立目錄的時候就會失敗 (這是 Windows 環境測試的結果, UNIX 環境不太確定)  
  
另外就是如果有人正在使用 WebLogic 的 console 修改系統參數而把 console 給 lock 時, 一樣會 deploy 失敗. 錯誤訊息為: The domain edit lock is owned by another session in non-exclusive mode. 因為 WebLogic 10 在 console 的 lock 並不是 by session 的, 也就是說, user 1 把 WebLogic 改成 edit 模式時, 在 active 之前, 如果 user 2 從另一個地方連進來, 也會看到目前是 edit 模式. 而這時如果 user 2 直接給他 rollback 了, user 1 就會發現他的東西不見了... 有點怪的設計, 不知道是不是我不會用.  
  
TBD:  
\-stage 參數是做什麼的?  
  
## 參考
1. http://edocs.bea.com/wls/docs100/deployment/deploy.html#wp1031460

