---
layout:     post
title:      "Android RabbitMQ使用之RabbitMQ安装及配置"
subtitle:   " \"Windows本地RabbitMQ安装及配置\""
date:       2018-11-21 17:52:00
author:     "Xiao"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Android
	- RabbitMQ
---

## Rabbit安装

准备
~~~
Erlang: http://www.erlang.org/downloads
Rabbit: http://www.rabbitmq.com/download.html
~~~
#### Erlang
> 选择自己对应的版本

![](https://img-blog.csdnimg.cn/20181105175107219.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105180126487.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
>点击运行，根据提示步骤安装！

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105180322479.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
>环境配置
~~~
ERLANG_HOME :  	D:\Program Files\erl10.1
Path:  %ERLANG_HOME%\bin
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105180822580.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105180750526.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181108152947451.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
#### RabbitMQ
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105181257439.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105181308383.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
>安装RabbitMQ's Management Plugin 
~~~
①直接运行  rabbitmq-plugins.bat 
② win+R --->  cmd ---> "D:\Program Files\RabbitMQ Server\rabbitmq_server-3.7.8\sbin\rabbitmq-plugins.bat" enable rabbitmq_management 
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181108152917936.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
## 鸣谢
https://www.cnblogs.com/ericli-ericli/p/5902270.html
[二萌偏](https://blog.csdn.net/weixin_39735923/article/details/79288578)

