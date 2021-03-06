---
layout:     post
title:      "新电脑开发装机"
subtitle:   " \"谷歌浏览器，QQ、微信，百度网盘，Android studio，Git，XMind\""
date:       2018-11-21 17:52:00
author:     "Xiao"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Android
---

### 新电脑开发工具下载
>每次拿到新电脑装机都要重新挨着百度一边，极伤。记下，以备来日之需！
```
1、谷歌浏览器
2、QQ、微信
3、百度网盘
4、Android studio
5、Git
6、XMind
```
##### 谷歌浏览器
~~~
地址：https://chrome.en.softonic.com/
无法登陆：https://jingyan.baidu.com/article/7e440953191a2b2fc0e2ef0c.html
~~~
##### QQ、微信
~~~
QQ:    http://im.qq.com/pcqq/
微信:   https://weixin.qq.com/cgi-bin/readtemplate?t=win_weixin&platform=wx&lang=zh_CN
~~~
##### 百度网盘
~~~
地址： http://pan.baidu.com/download
~~~
##### Android studio

准备：
~~~
1、jre
2、jdk
3、sdk
4、Android studio
5、Color Themes (酷炫主题)
~~~
开始：
~~~
jre地址：   https://www.java.com/zh_CN/
jdk地址：  https://www.oracle.com/technetwork/java/javase/downloads/index.html

配置环境变量： 
JAVA_HOME:     C:\Program Files\Java\jdk1.7.0_80  (JDK路径)
PATH：%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin; （现在应该都自带有这个变量名，只不过为小写Path）
CLASSPATH：   .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;
验证环境变量配置： win+R -> cmd -> java / javac / java -version

sdk:  https://github.com/inferjay/AndroidDevTools#sdk-tools
android studio :    http://www.android-studio.org/
Color Themes： http://color-themes.com/?view=index
~~~
微调
~~~
主题：File -> Import Settings -> xxx.jar
xxx.jar：Color Themes下载的jar文件，文件名不可有中文，空格。
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181029154332207.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_27,color_FFFFFF,t_70)
~~~
编码格式：File -> settings -> Editor -> File Encodings -> UTF-8
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181029154600823.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_27,color_FFFFFF,t_70)
~~~
字体样式、大小：File -> settings -> Editor -> Color Scheme -> Color Scheme Font 
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181029154806891.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_27,color_FFFFFF,t_70)
~~~
方法注解快捷键：File -> Settings -> Keymap -> Other -> Fix doc comment 
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181029155318793.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_27,color_FFFFFF,t_70)
~~~
SDK、JDK、NDK路径设置：File -> Project Structure 
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181029155724211.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_27,color_FFFFFF,t_70)
~~~
启动：File -> Settings -> Appearance & Behavior -> System Settings -> Reopen last project on startup
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181029160235922.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_27,color_FFFFFF,t_70)
~~~
Git:File -> Settings -> Version Control -> Git 
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181029160514399.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_27,color_FFFFFF,t_70)
##### Git
~~~
地址：https://git-scm.com/downloads
~~~
##### XMind
准备
~~~
地址：https://www.xmind.cn/download/xmind8
补丁：https://stormxing.oss-cn-beijing.aliyuncs.com/files/XMindCrack.jar
~~~
使用
~~~
1、将 补丁 jar 粘贴至 XMind安装目录
2、修改XMind.ini文件内容：
文末添加  -javaagent:D:\Program Files (x86)\XMind\XMindCrack.jar （后面的为jar绝对路径）
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181029161641848.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_27,color_FFFFFF,t_70)

鸣谢：
[Git教程:廖雪峰](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
[Android studio 安装教程：杨璐昊](https://www.cnblogs.com/yanglh6-jyx/p/Android_AS_Configuration.html)
[Android studio 开发环境搭建 | 菜鸟教程](http://www.runoob.com/android/android-environment-setup.html)
[Android Studio中的字体设置](https://blog.csdn.net/student9128/article/details/73909895)
[Android Studio主题设置、颜色背景配置](https://blog.csdn.net/cc20032706/article/details/71598529)
[android studio 项目编码格式的设置](https://blog.csdn.net/lp120660021/article/details/47831199)
[Android Studio启动时可选最近打开过的工程](https://blog.csdn.net/ks7638246/article/details/80408816)
[XMind8破解](https://blog.csdn.net/qq_35911589/article/details/81901868)





