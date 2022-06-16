# Command Line Deploy on NetWeaver


## 前言
 
要把 ear 檔給 deploy 到 NetWeaver, 標準的作法應該是使用 [SDM Remote GUI Client](http://help.sap.com/SAPHELP_NW70/helpdata/EN/3c/7d9e2588827546a2b8f2e7d8a26434/frameset.htm) , 使用上是不困難, 但有幾個缺點:

1. 雖然說是 Remote GUI, 但是基本上你的環境得有安裝 NetWeaver 才行, 也不知道能不能只單單安裝 SDM. 因此我都是用 VNC 連上去再做 deploy
1. GUI 的好處是簡單, 缺點就是你如果常常要做這個動作, 然後一直在選檔案, 執行, 確認, ... 等等按來按去, 按久了也真的滿累的
1. 這個 GUI 有個很怪的地方, 沒法 re-deploy, 也就是說, 如果要更新版本時, 得先把舊版 un-deploy, 然後再 deploy, 這樣真的滿不方便的

## Command line deploy 的方法

因此呢, 需要研究如何在 NetWeaver 上使用 command line deploy. 研讀了一下使用手冊 (裝好 SDM 就有了, 放在 /program/doc/SDMCommandLineDoc\*\*\*\_en\_final.pdf), 指令如下  
  
步驟1: 把 SDM 停下來 (\*\*\* 代表你 instance 的名字, 通常是三個大寫字元)  

```sh
/usr/sap/\*\*\*/JC00/SDM/program/StopServer.sh  
```

步驟2: 把模式改成 standalone, 這樣才能做 command line deploy.  

```sh
sdm jstartup sdmhome=/usr/sap/\*\*\*/JC00/SDM/program mode=standalone
```
  
步驟3: 執行這個指令, 就可以 deploy 了. updateversions 這個參數的目的在於, 如果版本一樣, 還是可以 re-deploy.

```sh
cd /usr/sap/\*\*\*/JC00/SDM/program java -jar bin/SDM.jar deploy file=/tmp/AP.ear updateversions=all
```

要注意的是, 上面步驟執行之後, Remote GUI 就不能使用了, 得使用下列指令把模式切回來才能使用 GUI.  

```sh
sdm jstartup sdmhome=/usr/sap/\*\*\*/JC00/SDM/program mode=standalone  
/usr/sap/\*\*\*/JC00/SDM/program/StartServer.sh
```

至於 undeploy, 執行下列指令即可, compname/compvendor 請參考 ear 檔裡面的 SAP\_MANIFEST.MF  

```sh
java -jar bin/SDM.jar undeploy compname=Application\_Name compvendor=test.com
```

以上是 NetWeaver 7.0 的作法, 7.1 不知道會不會有什麼不同.  
參考:  
[NetWeaver Software Deployment Manager](http://help.sap.com/saphelp_nw70/helpdata/EN/22/a7663bb3808c1fe10000000a114084/frameset.htm)
