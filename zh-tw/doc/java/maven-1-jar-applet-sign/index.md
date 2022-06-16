# Maven 1 - 打包 Jar, Applet 並簽名 Sign


Maven 1 - 打包 Jar, Applet 並簽名 Sign','Applet 程式在 package 時有兩個重點, 一是要把所有用到的 jar 程式庫都包在一起, 因為 applet 不像 ear 或 war 有固定目錄可以放 \*.jar. 二是如果需要用到 browser 機器的一些資源 (如印表機), 則必須經過簽名.  
  
以下是要把 Applet 程式打包的寫法, 幾個重點:  

*   descriptorRef 使用 jar-with-dependencies: 會把用到的 class 都一起打包進最後的 jar 檔. 預設會打包到另一個檔案, 原來的名字如果是 app-client-1.0.jar, 則會打包成 app-client-1.0-jar-with-dependencies.jar
*   appendAssemblyId 設成 false, 這樣產生出來的 jar 檔案名稱才不會有 ***-with-dependencies*** 的字
*   phase 請指定 package, 這樣才會把需要的 class 都打包進去
*   goal 請使用 assembly:single (不指定也可以, 有 package 也會執行到這裡)
*   注意, maven-assembly-plugin 一定要用 2.2-beta-5, 用 2.2 或 2.2.1 都會有問題 (這感覺不太合理, 不過測試結果目前就是這樣, 或許是我哪裡弄錯了)

```xml
<pre class="brush: xml" title="Client 的 pom.xml (打包用)">
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
        <artifactid>maven-assembly-plugin</artifactid>
        <version>2.2-beta-5</version>
        <configuration>
          <descriptorrefs>
            <descriptorref>jar-with-dependencies</descriptorref>
          </descriptorrefs>
          <finalname>${project.build.finalName}</finalname>
          <appendassemblyid>false</appendassemblyid>
          <archive>
            <index>true</index>
            <manifest>
              <adddefaultimplementationentries>true</adddefaultimplementationentries>
            </manifest>
          </archive>
        </configuration>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
```

簽名的部分, 使用 maven-jarsigner-plugin 來做簽名即可.  

```xml
      <plugin>
        <groupid>org.apache.maven.plugins</groupid>
        <artifactid>maven-jarsigner-plugin</artifactid>
        <version>1.2</version>
        <executions>
          <execution>
            <id>sign</id>
            <goals>
              <goal>sign</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <keystore>${basedir}\\key\\xxxx.store</keystore>
          <alias>xxxxkey</alias>
          <storepass>yourpassword</storepass>
        </configuration>
      </plugin>
```

實際上他會呼叫 jarsigner.exe 來做簽名的動作, 可以在 console 看到 log如下 (因為是 debug level, 所以要把 Maven 開到 debug mode 才看得到)```
  
`[debug] Executing: cmd.exe /X /C \"...\\jarsigner.exe -verify ...\\app-client-1.0.jar\"` 

另外, 也可以使用 maven-jar-plugin, 效果和 maven-jarsigner-plugin 是一樣的. 但是在 jar-plugin 2.3 以後, [jar:sign 已經不被建議使用](http://maven.apache.org/plugins/maven-jar-plugin/sign-mojo.html/), 請改用 jarsigner-plugin. 比較特別的是, jar-plugin 可以指定 signedjar 把簽名過的檔案放到其他目錄, 而 jarsigner-plugin 似乎沒有這個功能.

```xml
      <plugin>
        <groupid>org.apache.maven.plugins</groupid>
        <artifactid>maven-jar-plugin</artifactid>
        <version>2.3.1</version>
        <executions>
          <execution>
            <goals>
              <goal>sign</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <keystore>${basedir}\\key\\xxxx.store</keystore>
          <alias>xxxxkey</alias>
          <storepass>yourpassword</storepass>
          
          <verify>true</verify>
        </configuration>
      </plugin>
```

喔, 講了那麼多, 都沒有提到 Jar... 其實 Applet 就是一個 Jar, 所以用上面 Applet 的方式就可以了. 差別在於 Jar 一般來說不需要簽名就是了.  
  
## 參考

*   http://maven.apache.org/plugins/maven-assembly-plugin/single-mojo.html
*   http://maven.apache.org/plugins/maven-jar-plugin/
*   http://maven.apache.org/plugins/maven-jarsigner-plugin/
*   http://stackoverflow.com/questions/2027753/how-to-deploy-applet-with-dependencies-jar-using-maven-and-sign-it

## Reference for assembly for standalone jar

*   http://stackoverflow.com/questions/1832853/is-it-possible-to-create-an-uber-jar-containing-the-project-classes-and-the-pro
*   http://blog.csdn.net/mozhenghua/archive/2010/06/14/5671133.asp
