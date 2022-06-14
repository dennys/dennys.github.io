# Maven 4 - Others


## Package source code
在 multi module 的架構裡面, 因為都只有打包 .class 到 local repository 的 .jar 檔裡面. 因此在 Eclipse 裡面按 F3 是無法連結到 source code 的, 要使用 maven-source-plugin 把 source code 也打包到 local repository 才可以. 如果原始的檔名是 app-1.0.jar, 則 source 會被打包到 app-1.0-sources.jar goal 請使用 source:jar

```
  <build>
    <plugins>
      <!-- Attach sources to repository -->
      <plugin>
        <artifactId>maven-source-plugin</artifactId>
        <executions>
          <execution>
            <id>attach-sources</id>
            <phase>verify</phase>
            <goals>
              <goal>jar</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
```  
  
## 參考
1. http://maven.apache.org/plugins/maven-source-plugin/jar-mojo.html
