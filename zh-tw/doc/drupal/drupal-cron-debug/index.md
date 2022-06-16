# 如何對 Drupal 的 cron 做 Debug


## Drupal's cron

[Drupal](http://drupal.org/) 的 cron 是個好用的東西, 但有時候出問題時也很難 debug, 當看到這幾個訊息時, 通常是上次的 cron 執行失敗了.

```sh
Attempting to re-run cron while it is already running.  
Cron run exceeded the time limit and was aborted.  
Cron has been running for more than an hour and is most likely stuck.
```

## Debug 方法

在 variable 裡面, 有兩個相關參數, cron_last 是上次執行的時間, cron_semaphore 是代表 cron 正在執行. 所以如果執行 cron 的途中系統掛了, 可能就得手動清掉 semaphore, 不然即使你手動執行 cron, 系統也會回應已經有一個 cron 在執行了. 可用下列指令:

```SQL
DELETE FROM variable WHERE name="cron_semaphore";  
```  

另外如果要看 cron 是執行到哪裡出問題了, 可修改 includes/module.inc  
```php {hl_lines=3}
   foreach (module\_implements($hook) as $module) {  
    $function = $module .'\_'. $hook;  
    if ($hook == 'cron') watchdog('cron', "hit $module cron");   // add this line  
    $result = call\_user\_func\_array($function, $args);  
    if (isset($result) && is\_array($result)) {  
      $return = array\_merge($return, $result);  
    }  
    else if (isset($result)) {  
      $return\[\] = $result;  
    }  
  }
```  

## 執行結果

如下, 可以看出 cron 執行到哪裡了.  
  
[![Drupal cron](http://farm4.static.flickr.com/3200/2365667427_6dc881c1b9.jpg)](http://www.flickr.com/photos/dennys_blog/2365667427/ "Flickr 上 Dennys Blog 的 Drupal cron")
