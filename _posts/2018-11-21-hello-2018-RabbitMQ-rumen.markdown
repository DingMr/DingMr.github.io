---
layout:     post
title:      "Android RabbitMQ入门第一章"
subtitle:   " \"Android-RabbitMQ的简单发送及接收\""
date:       2018-11-21 17:52:00
author:     "Xiao"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Android
    - RabbitMQ
---

## Android Rabbit发送与接收
>Android 使用Rabbit需要几个充要条件
>①IP地址
>②端口号
>③账号，密码
>④交换机名称（ExchangeName）

Rabbit远程配置如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106175404601.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106175607858.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
>注：交换机的交换类型选择【fanout】 
>配置可以参考[Android RabbitMQ使用之测试](https://blog.csdn.net/qq_36576738/article/details/83754575)，不用绑定queue队列！
## Android studio
最新版本
RabbitMQ Java客户端的当前版本是 5.5.0。

添加库依赖
在项目中使用RabbitMQ Java客户端的推荐方法是使用依赖关系管理系统。

如果您正在使用Maven，请将此依赖项添加到项目的POM文件中：
~~~
<dependency>
  <groupId>com.rabbitmq</groupId>
  <artifactId>amqp-client</artifactId>
  <version>5.5.0</version>
</dependency>
~~~
或者，如果使用Gradle：
~~~
dependencies {
  implementation 'com.rabbitmq:amqp-client:5.5.0'
}
~~~
>RabbitMQ Java客户端库允许Java代码与RabbitMQ进行交互。
该库的5.x版本系列需要JDK 8，用于编译和运行时。在Android上，这意味着仅支持Android 7.0或更高版本。4.x版本系列支持7.0之前的JDK 6和Android版本。

版本请看这里：http://repo1.maven.org/maven2/com/rabbitmq/amqp-client/
依赖请看这里：https://www.rabbitmq.com/java-client.html

### 代码
>配置：
>
~~~
	private String userName = "dlw" ;
    private String passWord = "dlw" ;
    private String hostName = "192.168.xx.xxx" ;
    private int portNum = 5672 ;
    private String exchangeName = "demo" ;
~~~
~~~
   /**
     * Rabbit配置
     */
    private void setupConnectionFactory(){

        factory.setUsername(userName);
        factory.setPassword(passWord);
        factory.setHost(hostName);
        factory.setPort(portNum);

    }
~~~
>发送消息
>channel.basicPublish(exchangeName , rountingKey  ,null , msg);
>exchangeName ----- 交换机名称 --- 已知
>rountingKey ------路由键 -- 自己随便填写一个
>msg -------- 要发送的消息
~~~
    /**
     * 发消息
     */
    private void basicPublish(){

        try {
            //连接
            Connection connection = factory.newConnection() ;
            //通道
            Channel channel = connection.createChannel() ;
            //消息发布
            byte[] msg = "hello girl friend!".getBytes() ;
            //rountingKey 自己任意填写
            channel.basicPublish(exchangeName , rountingKey  ,null , msg);

        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        }

    }
~~~
>接收消息
>	basicConsume​(String queue, boolean autoAck, String consumerTag, Consumer callback)
>queue ------ 队列名称 --- 这里我们创建一个临时队列（非持久，独占，自动删除）
>autoAck - 如果服务器应考虑一旦发送确认的消息，则为true; 如果服务器应该期望显式确认，则返回false           -- false
>consumerTag - 客户端生成的消费者标签以建立上下文 -- 任意写
>callback - 消费者对象的接口 -- 我们使用Consumer的子类DefaultConsumer
~~~
    /**
     * 收消息
     */
    private void basicConsume(){

        try {
            //连接
            Connection connection = factory.newConnection() ;
            //通道
            final Channel channel = connection.createChannel() ;
            //声明了一个交换和一个服务器命名的队列，然后将它们绑定在一起。
            channel.exchangeDeclare(exchangeName , "fanout" , true) ;
            String queueName = channel.queueDeclare().getQueue() ;
            Log.e("TAG" , queueName + " :queueName") ;
            channel.queueBind(queueName , exchangeName , rountingKey) ;
            //实现Consumer的最简单方法是将便捷类DefaultConsumer子类化。可以在basicConsume 调用上传递此子类的对象以设置订阅：
            channel.basicConsume(queueName , false , "administrator" , new DefaultConsumer(channel){
                @Override
                public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                    super.handleDelivery(consumerTag, envelope, properties, body);

                    String rountingKey = envelope.getRoutingKey() ;//路由密钥
                    String contentType = properties.getContentType() ;//contentType字段，如果尚未设置字段，则为null。
                    String msg = new String(body,"UTF-8") ;//接收到的消息
                    long deliveryTag = envelope.getDeliveryTag() ;//交付标记

                    Log.e("TAG" , rountingKey+"：rountingKey") ;
                    Log.e("TAG" , contentType+"：contentType") ;
                    Log.e("TAG" , msg+"：msg") ;
                    Log.e("TAG" , deliveryTag+"：deliveryTag") ;
                    Log.e("TAG" , consumerTag+"：consumerTag") ;
                    Log.e("TAG" , envelope.getExchange()+"：exchangeName") ;

                    channel.basicAck(deliveryTag , false);
                }
            });

        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        }

    }
~~~
## 注
>因为【exchange】的交换类型为【fanout】（扇形），所以【rountingKey】（路由键）的设置会被忽略。我们使用basicPublish发送消息，其实是向该【exchange】里面的所有【queue】（队列）发送消息。
>因此，如果我们使用这种方式发送与接收信息，那么你所有的设备都会接受到你某刻发送的消息。
>如果我们向里面exchange里面的queue发送消息时，exchange里面没有queue。那么是不影响的。
>