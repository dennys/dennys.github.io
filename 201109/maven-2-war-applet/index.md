# Maven 2 - 打包 war (含 applet)


在 Maven 裡面打包 war 應該是最簡單的了, 照著 Maven 的[目錄結構](http://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html/)放好, 他就會自動去幫你把程式打包好. 唯一的問題是, 如果有用到 applet, 那就得另外處理了. 一個簡單的方法, 把 applet 設定成 war 的 dependency, 那就會把 applet 放到 WEB-INF/lib 下面, 不過這樣子似乎無法呼叫到 (這部分如果有解麻煩指正).

另外的方法是用 Maven 的 maven-dependency-plugin, 做法如下:  

1.  先把 applet 打包好, 並且記得在使用 Maven 時加上 install 這個 goal, 這樣就會把打包好的 applet 放到 local repository 裡面去.
2.  使用 maven-dependency-plugin 的 copy, 將 applet 放到指定的目錄. 這裡因為要放到 root, 所以指定的是 ${project.build.directory}/app-war-1.0. 後面的 "app-war-1.0" 請依照實際的專案修改.

```
  <build>
    <plugins>
      <plugin>
        <artifactid>maven-dependency-plugin</artifactid>
        <version>2.2</version>
        <executions>
          <execution>
            <id>copy</id>
            <phase>prepare-package</phase>
            <goals>
              <goal>copy</goal>
            </goals>
            <configuration>
              <artifactitems>
                <artifactitem>
                  <groupid>com.yourcompany</groupid>
                  <artifactid>app-client</artifactid>
                  <version>1.0</version>
                  <type>jar</type>
                  <overwrite>true</overwrite>
                  <destfilename>app-client.jar</destfilename>
                </artifactitem>
              </artifactitems>
              <outputdirectory>${project.build.directory}/app-war-1.0</outputdirectory>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
```

## Reference

[http://maven.apache.org/plugins/maven-dependency-plugin/](http://maven.apache.org/plugins/maven-dependency-plugin/)
