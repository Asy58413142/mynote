#标题待写(Windows版)

###**首先下载并安装Jenkins**

1. 去[http://jenkins-ci.org/](http://jenkins-ci.org/ "Jenkins")下载Jenkins
2. 下载完之后是：![](http://i.imgur.com/7uIHH89.png)点击安装，下一步即可。
3. 安装完成之后就在Windows的服务中多了一项服务：![](http://i.imgur.com/Rj3c7oo.png)启动该服务，然后打开浏览器输入：localhost:8080就可以打开Jenkins了。8080是默认端口，如果想改端口，就在Jenkins安装根目录下的jenkins.xml中修改：![](http://i.imgur.com/aNrlFsJ.png)
4. 还有另外一种方式启动Jenkins：用java -jar 命令运行Jenkins安装目录下的jenkins.war包，这种启动方式的端口是默认端口8080，即使你修改了jenkins.xml中的端口也不好使：![](http://i.imgur.com/bcdZ404.png)![](http://i.imgur.com/XxppTP6.png)看到命令号输出Jenkins is fully up and running，表示开启成功，在浏览器中访问localhost:8080即可。


###**安装插件Gralde、Git、Lint**
1. 启动成功之后打开Jenkins的界面：![](http://i.imgur.com/6oFz6R9.png)
2. 点击系统管理按钮，进入到系统管理界面：![](http://i.imgur.com/SI7G4Vr.png)
3. 点击管理插件：	![](http://i.imgur.com/sVEAAKy.png)
4. 安装需要用到的插件，需要用到Git、Gradle、Lint：![](http://i.imgur.com/v3DinV4.png)
5. 点击安装之后进入到安装插件的界面：![](http://i.imgur.com/HeSgD47.png)
6. 安装成功之后重启Jenkins：![](http://i.imgur.com/tyEfG7D.png)![](http://i.imgur.com/mg3WyyV.png)
7. 配置JDK，SDK，Gradle，git：
 ![](http://i.imgur.com/CCq1FxM.png)


###**创建任务**

1. 点击"新建"按钮，新建一个任务，选第一项，名字随便起：![](http://i.imgur.com/HDk9ECJ.png)
2. 