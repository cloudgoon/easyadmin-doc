## 10.1.使用IDEA打包    :id=package
点击右侧的Maven/package即可开始打包，打好的包在target下面：

![](https://s2.ax1x.com/2019/08/31/mzrIZn.png)


## 10.2.部署到linux   :id=linux
先把jar包上传到linux里面，建议一个项目(jar包)建一个文件夹，然后通过如下命令启动：

```
nohup java -jar easyweb-shiro-1.2.1.jar --spring.profiles.active=prod
```

!> 命令执行后直接关闭此窗口，不要用Ctrl+C停止

如果要实时查看打印信息，使用如下命令：

```
tail -f nohup.out
```

![](https://s2.ax1x.com/2019/11/06/MPG4Tf.png)

关闭项目使用如下命令：
```
# 查看端口号的进程id
netstat -anp |grep 8083

# 显示如下信息
tcp   0    0 0.0.0.0:8083     0.0.0.0:*    LISTEN   22812/java  

# 关闭进程
kill 22812
```


## 10.3.部署到windows   :id=win

在windows上部署建议创建一个bat文件，这样只要双击bat文件就可以启动了：

```
title EasyWeb快速开发框架
java -jar easyweb-shiro-1.2.1.jar --spring.profiles.active=test
```

![](https://s2.ax1x.com/2019/08/31/mz2Va9.png)

> bat文件前面加一个title的好处就是打开的命令窗口可以显示项目的名字，
> 这样可以避免部署太多项目后无法区分哪个命令窗口对应哪个项目的问题