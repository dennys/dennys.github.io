# JDBC Leaked Connection in WebLogic


## 前言
  
最近發現某個 [WebLogic](http://www.bea.com/framework.jsp?CNT=index.htm&FP=/content/products/weblogic) 的 JDBC connection 只會增加, 不會減少, 意思是說, 程式和 WebLogic 拿了很多 JDBC connection, 但是都沒有還給系統. 在程式執行一段時間之後, 就會拿不到 JDBC connection 了, 然後就掛了. WebLogic 的 console 可以看到, connection 的數字一直增加.  
  
[![jdbc_monitor](http://farm4.static.flickr.com/3596/3373931575_16a47e0632_o.jpg "jdbc_monitor")](http://www.flickr.com/photos/dennys_blog/3373931575 "jdbc_monitor")  

## 解決方法

WebLogic 的 log 如下, 整個 stack trace 就不全列出來了, 重點在於, 當 WebLogic 的 JDBC connection pool 的 connection 都被要光之後, 後面的程式就無法和 database 連線了.  
  
```sh
java.sql.SQLException: Internal error: Cannot obtain XAConnection  
weblogic.common.resourcepool.ResourceDeadException  
```

因為在上圖中的 WebLogic 的 console 看到的 "leaked connection count" 一直都是 0, 所以我也沒有往這個方向思考過. 不過後來才發現需要做一些設定才能偵測 leaked connection, 如下  
  
1. 設定 WebLogic 的 "Inactive Connection Timeout", 這個參數的意義是, 如果有個 JDBC connection 被拿走之後, 超過一段時間沒有使用 (如下圖, 代表 10 分鐘), 則系統就會自動收回. (其實設定這個之後, 這個問題基本上就已經暫時解決了)  
  
    [![jdbc_connection_pool](http://farm4.static.flickr.com/3578/3374748492_ca4a5aa6b2_o.jpg "jdbc_connection_pool")](http://www.flickr.com/photos/dennys_blog/3374748492 "jdbc_connection_pool")

2. 打開偵測的工具, 如下圖, 系統就會在 log 顯示 leaked connection 的資訊  
  
    [![jdbc_diagnostics](http://farm4.static.flickr.com/3587/3374748472_7f27e6d7da_o.jpg "jdbc_diagnostics")](http://www.flickr.com/photos/dennys_blog/3374748472 "jdbc_diagnostics")  
  
3. 設定好之後 (不用重開 WebLogic) , 觀察系統一段時間, 結果如下, 看到 "leaked connection count" 的值不再是 0 了, 而 active connection 在沒人使用時, 也會降到 0 了.

    [![jdbc_monitor_leaked](http://farm4.static.flickr.com/3554/3374748508_d7bd3e285c_o.jpg "jdbc_monitor_leaked")](http://www.flickr.com/photos/dennys_blog/3374748508 "jdbc_monitor_leaked")  
  
4. 之後 WebLogic 的 log (放在 user\_projects/domains/%domainname%/%servername%/logs, 而不是自己 AP 的 log) 如下, 整個 stack trace 滿大的, 重點在第 28 行, 問題發生在 "com.test.dao.getStatus" 這個 method, 意思就是說, 有某個 JDBC connection 在這個程式被拿走之後, 一直沒有還給系統, 於是就找到兇手了.

```sh
1.  ####<Mar 11, 2009 11:14:36 PM CST> <Warning> <JDBC> <testserver1> <Server1> <\[ACTIVE\] ExecuteThread: '4' for queue: 'weblogic.kernel.Default (self-tuning)'> <<WLS Kernel>> <> <> <1236748476017> <BEA-001153> <Forcibly releasing inactive connection "\[weblogic.jdbc.wrapper.JTAConnection\_weblogic\_jdbc\_wrapper\_XAConnection\_oracle\_jdbc\_driver\_LogicalConnection-TEST\_DS-311, oracle.jdbc.driver.LogicalConnection@42a794\]" back into the connection pool "TEST\_DS", currently reserved by: java.lang.Exception
2.          at weblogic.jdbc.common.internal.ConnectionEnv.setup(ConnectionEnv.java:291)
3.          at weblogic.common.resourcepool.ResourcePoolImpl.reserveResource(ResourcePoolImpl.java:314)
4.          at weblogic.common.resourcepool.ResourcePoolImpl.reserveResource(ResourcePoolImpl.java:292)
5.          at weblogic.jdbc.common.internal.ConnectionPool.reserve(ConnectionPool.java:425)
6.          at weblogic.jdbc.common.internal.ConnectionPool.reserve(ConnectionPool.java:316)
7.          at weblogic.jdbc.common.internal.ConnectionPoolManager.reserve(ConnectionPoolManager.java:93)
8.          at weblogic.jdbc.common.internal.ConnectionPoolManager.reserve(ConnectionPoolManager.java:61)
9.          at weblogic.jdbc.jta.DataSource.getXAConnectionFromPool(DataSource.java:1474)
10.          at weblogic.jdbc.jta.DataSource.refreshXAConnAndEnlist(DataSource.java:1303)
11.          at weblogic.jdbc.jta.DataSource.getConnection(DataSource.java:426)
12.          at weblogic.jdbc.jta.DataSource.connect(DataSource.java:383)
13.          at weblogic.jdbc.common.internal.RmiDataSource.getConnection(RmiDataSource.java:339)
14.          at org.springframework.orm.hibernate3.LocalDataSourceConnectionProvider.getConnection(LocalDataSourceConnectionProvider.java:81)
15.          at org.hibernate.jdbc.ConnectionManager.openConnection(ConnectionManager.java:423)
16.          at org.hibernate.jdbc.ConnectionManager.getConnection(ConnectionManager.java:144)
17.          at org.hibernate.jdbc.AbstractBatcher.prepareQueryStatement(AbstractBatcher.java:139)
18.          at org.hibernate.loader.Loader.prepareQueryStatement(Loader.java:1547)
19.          at org.hibernate.loader.Loader.doQuery(Loader.java:673)
20.          at org.hibernate.loader.Loader.doQueryAndInitializeNonLazyCollections(Loader.java:236)
21.          at org.hibernate.loader.Loader.doList(Loader.java:2220)
22.          at org.hibernate.loader.Loader.listIgnoreQueryCache(Loader.java:2104)
23.          at org.hibernate.loader.Loader.list(Loader.java:2099)
24.          at org.hibernate.loader.custom.CustomLoader.list(CustomLoader.java:289)
25.          at org.hibernate.impl.SessionImpl.listCustomQuery(SessionImpl.java:1695)
26.          at org.hibernate.impl.AbstractSessionImpl.list(AbstractSessionImpl.java:142)
27.          at org.hibernate.impl.SQLQueryImpl.list(SQLQueryImpl.java:152)
28.          at com.test.dao.getStatus(dao.java:194)
29.          at sun.reflect.GeneratedMethodAccessor755.invoke(Unknown Source)
30.          at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
31.          at java.lang.reflect.Method.invoke(Method.java:585)
32.          at org.apache.axis2.rpc.receivers.RPCUtil.invokeServiceClass(RPCUtil.java:194)
33.          at org.apache.axis2.rpc.receivers.RPCMessageReceiver.invokeBusinessLogic(RPCMessageReceiver.java:102)
34.          at org.apache.axis2.receivers.AbstractInOutMessageReceiver.invokeBusinessLogic(AbstractInOutMessageReceiver.java:40)
35.          at org.apache.axis2.receivers.AbstractMessageReceiver.receive(AbstractMessageReceiver.java:100)
36.          at org.apache.axis2.engine.AxisEngine.receive(AxisEngine.java:176)
37.          at org.apache.axis2.transport.http.HTTPTransportUtils.processHTTPPostRequest(HTTPTransportUtils.java:275)
38.          at org.apache.axis2.transport.http.AxisServlet.doPost(AxisServlet.java:133)
39.          at javax.servlet.http.HttpServlet.service(HttpServlet.java:727)
40.          at javax.servlet.http.HttpServlet.service(HttpServlet.java:820)
41.          at weblogic.servlet.internal.StubSecurityHelper$ServletServiceAction.run(StubSecurityHelper.java:226)
42.          at weblogic.servlet.internal.StubSecurityHelper.invokeServlet(StubSecurityHelper.java:124)
43.          at weblogic.servlet.internal.ServletStubImpl.execute(ServletStubImpl.java:283)
44.          at weblogic.servlet.internal.ServletStubImpl.execute(ServletStubImpl.java:175)
45.          at weblogic.servlet.internal.WebAppServletContext$ServletInvocationAction.run(WebAppServletContext.java:3395)
46.          at weblogic.security.acl.internal.AuthenticatedSubject.doAs(AuthenticatedSubject.java:321)
47.          at weblogic.security.service.SecurityManager.runAs(Unknown Source)
48.          at weblogic.servlet.internal.WebAppServletContext.securedExecute(WebAppServletContext.java:2140)
49.          at weblogic.servlet.internal.WebAppServletContext.execute(WebAppServletContext.java:2046)
50.          at weblogic.servlet.internal.ServletRequestImpl.run(ServletRequestImpl.java:1366)
51.          at weblogic.work.ExecuteThread.execute(ExecuteThread.java:200)
52.          at weblogic.work.ExecuteThread.run(ExecuteThread.java:172)
``` 

## 測試環境

WebLogic 10.0 MP1, JDBC driver 使用的是內建的 Oracle thin driver. 另外也發現一件事情, 如果改用 [Bea 的 Oracle driver](http://download.oracle.com/docs/cd/E12840_01/wls/docs103/jdbc_drivers/usedriver.html), 則 connection 成長的速度會放慢, 但最後還是一樣會爆掉的啦


