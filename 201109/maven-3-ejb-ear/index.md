# Maven 3 - 打包 EJB, EAR


## Ear 的打包

Ear 的打包分成兩階段, 第一個打包各個 ejb, 第二是把各個 ejb 加上 library 打包成 ear. 打包 ejb 的部分相對簡單, 如下. 最後會打包出 app-ejb1.jar (依據 finalName 的設定) 這個 ejb 檔案.

### EJB Client

generateClient 是指定要產生一個 app-ejb1-client.jar 的檔案, 不過如果只加 generateClient 而沒有任何其他參數, 預設是只會過濾某幾個 pattern 而已, 請再加上 clientIncludes, clientExcludes 來過濾. 詳情可參考
1. http://maven.apache.org/plugins/maven-ejb-plugin/examples/generating-ejb-client.html
1. http://maven.apache.org/plugins/maven-ejb-plugin/examples/ejb-client-dependency.html  

```
 <build>
    <plugins>
      <plugin>
        <groupid>org.apache.maven.plugins</groupid>
        <artifactid>maven-compiler-plugin</artifactid>
        <version>2.3.2</version>
        <configuration>
          <source></source>1.6
          <target>1.6</target>
        </configuration>
      </plugin>
      <plugin>
        <groupid>org.apache.maven.plugins</groupid>
        <artifactid>maven-ejb-plugin</artifactid>
        <version>2.3</version>
        <configuration>
          <ejbversion>2.1</ejbversion>
          
          <generateclient>true</generateclient>
        </configuration>
      </plugin>
    </plugins>
    <finalname>app-ejb1</finalname>
  </build>
```
  
打包 ear 檔的部分則要注意一下. 首先是要設定好 dependency 如下, 注意雖然上面打包的 ejb 其副檔名是 .jar, 但這裡的 type 必須設定成 ejb  
```
  <dependencies>
    <dependency>
        <groupid>com.yourcompany</groupid>
        <artifactid>app-ejb1</artifactid>
        <version>1.0</version>
        <type>ejb</type>
    </dependency>
    <dependency>
        <groupid>com.yourcompany</groupid>
        <artifactid>app-ejb2</artifactid>
        <version>1.0</version>
        <type>ejb</type>
    </dependency>
  </dependencies>
```

打包 ear 的設定要注意幾個地方:`  

1. 雖然 dependency 已經有指定了, 但是 ejb 還是要在 ejbModule 的地方再指定一次, 不同的是, 這裡不用指定 version 和 type, 不過要指定 bundleFileName, 這個是 ejb 最後會使用的檔案名字.
2. 請記得設定 defaultLibBundleDir, 這樣會把用到的 library 放到 APP-INF/lib (預設是通通放在跟目錄, 和 ejb 混在一起, 那就沒用了)
3. 如果有特別的檔案要直接放進來, 可以用 jarModule`

```
  <build>
    <finalname>app-ear</finalname>
    <plugins>
      <plugin>
        <groupid>org.apache.maven.plugins</groupid>
        <artifactid>maven-ear-plugin</artifactid>
        <version>2.5</version>
        <configuration>
          <generateapplicationxml>true</generateapplicationxml>  
          <defaultlibbundledir>APP-INF/lib</defaultlibbundledir>
          <modules> 
            <ejbmodule> 
              <groupid>com.yourcompany</groupid> 
              <artifactid>app-ejb1</artifactid> 
              <bundlefilename>app-ejb1.jar</bundlefilename> 
            </ejbmodule>
            <ejbmodule> 
              <groupid>com.yourcompany</groupid> 
              <artifactid>app-ejb2</artifactid> 
              <bundlefilename>app-ejb2.jar</bundlefilename> 
            </ejbmodule>
            
          </modules> 
        </configuration>
      </plugin>
    </plugins>
  </build>
```

### 最後 app-ear 的結構如下

  ├── app-ejb1.jar  
  ├── app-ejb2.jar  
  └── WEB-INF  
      └── lib  
          ├── log4j-xxxx.jar  
          ├── axis-xxxxjar  
          └── jdbc-xxxx.jar  

如果要把 war 也放進來, 使用 webModule 即可, 如下. 要注意的是, 原來的 war 裡面的 contextRoot 會失去作用, 如果要指定, 請在 webModule 裡面新增一個
```
  <modules>
    <webmodule>
      <groupid>artifactGroupId</groupid>
      <artifactid>artifactId</artifactid>
      <contextroot>/custom-context-root</contextroot>
    </webmodule>
  </modules>
```

目前的 ear 並沒有把 war 打包進去, 如果要打包也很簡單, 在 ear 的 pom 加上一段 warModule 就可以了. 基本上沒有什麼問題, 可以改進的地方只有, 如果 ear 和 war 有共用的 .jar 檔, 那就會重複打包. 可參考 [Creating Skinny WARs](http://www.blogger.com/%5C%22http://maven.apache.org/plugins/maven-war-plugin/examples/skinny-wars.html%5C%22)  
  
## 注意

1. 如果有使用 JPA, 而這個 JPA 是放在 EJB 或某個共用的 jar, 納在包成 EAR 的時候就要注意, 如果這個 jar 會同時出現在 war 的 WEB-INF/lib 以及 ear 的 APP-INF/lib, 那就會出現 duplicate persistence units with name 的錯誤. 解法:
1. ejb 會重複出現, 解法:無, 但似乎沒什麼影響

## 參考
1. http://maven.apache.org/plugins/maven-ejb-plugin/
1. http://maven.apache.org/plugins/maven-ear-plugin/
