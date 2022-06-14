# 設定 WebLogic 的 memory 參數


  
這是第二次吃虧了, 把他記下來, 免得下次又忘記了...  
  
問題的起源在於記憶體不足, 通常訊息如下. 一次是某個單一 XML 訊息太大, 另一次是 ear 檔案太大, re-deploy 的時候會出問題.

`java.lang.OutOfMemoryError: PermGen space`

如果是簡單的 Java 程式, 那解法很簡單, 改參數就好了, 把 -XX:PermSize=*** 放大即可. 不過在 WebLogic 裡面, script 包了好幾層, 就很容易漏掉了. 這次就是因為呆呆的直接在最外層設定 MEM_ARGS, 然後被裡層的 script 蓋掉了, 所以當然沒作用...  
  
建議的作法是修改 USER_MEM_ARGS, 可參考 Modifying JVM Parameters in Server Start Scripts  
  
最後, 在 WebLogic 10, 設定環境變數的程式在 domains//bin/setDomainEnv.sh, 可以看出不同平台, 甚至 production/develop mode 的不同也會導致參數不同, 不過不建議改這裡.

```

if [ "${JAVA_VENDOR}" = "Sun" ] ; then
       if [ "${PRODUCTION_MODE}" = "" ] ; then
               MEM_DEV_ARGS="-XX:CompileThreshold=8000 -XX:PermSize=48m "
               export MEM_DEV_ARGS
       fi
fi

# Had to have a separate test here BECAUSE of immediate variable expansion
on windows

if [ "${JAVA_VENDOR}" = "Sun" ] ; then
       MEM_ARGS="${MEM_ARGS} ${MEM_DEV_ARGS} -XX:MaxPermSize=128m"
       export MEM_ARGS
fi

if [ "${JAVA_VENDOR}" = "HP" ] ; then
       MEM_ARGS="${MEM_ARGS} -XX:MaxPermSize=128m"
       export MEM_ARGS
fi

# IF USER_MEM_ARGS the environment variable is set, use it to override ALL
MEM_ARGS values

if [ "${USER_MEM_ARGS}" != "" ] ; then
       MEM_ARGS="${USER_MEM_ARGS}"
       export MEM_ARGS
fi
```
