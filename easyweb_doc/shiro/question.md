## 11.1.IDEA热更新    :id=q1
如果修改了页面代码，需要重启服务器才能更新，对IDEA做如下设置即可：

1. 按住 `Shift`+`Ctrl`+`Alt`+`/`，选择Registry
2. 找到`compiler.automake.allow.when.app.running`并勾选

    ![](https://s2.ax1x.com/2019/08/31/mz33M8.png)
    
3. 打开`file`/`settings`/`Compiler`，勾选`Build project automatically`：

    ![](https://s2.ax1x.com/2019/08/31/mzUhAU.png)

完成上面三步设置，重启服务器即可热更新，如果还不行，再做如下设置：

![](https://s2.ax1x.com/2019/08/31/mzaZ4g.png)


## 11.2.修改包名后无法运行     :id=q2
这个问题一般都是包名没有修改彻底，修改包名建议使用全局替换的方式，对着src目录右键：

![](https://s2.ax1x.com/2019/08/31/mzlhkR.png)

![](https://s2.ax1x.com/2019/08/31/mzlT1K.png)

> 然后还有pom.xml的groupId和artifactId，当热这个改不改都不影响


## 11.3.部署后字体xls等文件损坏     :id=q3
修改pom.xml文件如下：
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
    <!-- 添加如下代码 -->
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <filtering>true</filtering>
            <excludes>
                <exclude>**/assets/**</exclude>
            </excludes>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <filtering>false</filtering>
            <includes>
                <include>**/assets/**</include>
            </includes>
        </resource>
    </resources>
</build>
```
原因就是因为项目的编码问题，maven打包的时候会对项目进行转码，导致文件损坏，
因为assets目录下的所有文件都是静态资源，不需要maven进行处理，添加上面代码排除即可。
