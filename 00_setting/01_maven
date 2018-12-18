###设置JDK版本

##### POM文件中添加配置
<build>

       <plugins>

           <plugin>

              <groupId>org.apache.maven.plugins</groupId>

              <artifactId>maven-compiler-plugin</artifactId>

                    <!-- <version>3.7.0</version> -->  

                    <configuration>

                  <source>1.8</source>

                  <target>1.8</target>

                  <encoding>utf-8</encoding>

              </configuration>

           </plugin>

       </plugins>

    </build>


##### 本地maven仓库配置的settings.xml文件中添加配置

找到有<profile>标签的地方，加上如下配置：
<profile>
<id>jdk-1.8</id>    
    <activation>  
        <activeByDefault>true</activeByDefault>    
        <jdk>1.8</jdk>    
    </activation>    
    <properties>    
        <maven.compiler.source>1.8</maven.compiler.source>    
        <maven.compiler.target>1.8</maven.compiler.target>    
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>    
    </properties>    
</profile>
