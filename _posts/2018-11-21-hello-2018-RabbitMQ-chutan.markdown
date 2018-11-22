---
layout:     post
title:      "Android RabbitMQ之Android初探"
subtitle:   " \"RabbitMQ的简单使用\""
date:       2018-11-21 17:52:00
author:     "Xiao"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Android
    - RabbitMQ
---
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

### 代码部分

>前提条件
~~~
    private String userName = "dlw" ;
    private String passWord = "dlw" ;
    private String virtualHost = "/" ;
    private String hostName = "192.168.xx.xxx" ;
    private int portNum = 5672 ;
    private String queueName = "que" ;
    private String exchangeName = "demo" ;
    private String rountingKey = "key" ;
~~~
#### ①Rabbit配置
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
#### ②发消息
>①工厂创建一个新的连接 ---- factory.newConnection() ;
>②用连接创建一个通道 ---- connection.createChannel() ;
>③声明一个交换机 ---- channel.exchangeDeclare(String , String , Boolean) ;
>④声明一个独占服务器，并得到队列名称 ---- channel.queueDeclare().getQueue() ;
>⑤将交换机和队列绑定 ---- channel.queueBind(queueName , exchangeName , rountingKey) ;
>⑥发布消息 ---- channel.basicPublish(exchangeName , rountingKey  ,null , msg);
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
            //声明了一个交换和一个服务器命名的队列，然后将它们绑定在一起。
            channel.exchangeDeclare(exchangeName , "fanout" , true) ;//声明exchange
            String queueName = channel.queueDeclare().getQueue() ;//声明独占的服务器，并得到队列名
            channel.queueBind(queueName , exchangeName , rountingKey) ;//将exchange和queue绑定
            //消息发布
            byte[] msg = "hello word!".getBytes() ;
            channel.basicPublish(exchangeName , rountingKey  ,null , msg);
            
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        }

    }
~~~
###### API 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106145924774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106150017339.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106150124958.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106150532309.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
#### ③接受消息
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
            //实现Consumer的最简单方法是将便捷类DefaultConsumer子类化。可以在basicConsume 调用上传递此子类的对象以设置订阅：
            channel.basicConsume(queueName , false , "administrator" , new DefaultConsumer(channel){
                @Override
                public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                    super.handleDelivery(consumerTag, envelope, properties, body);

                    String rountingKey = envelope.getRoutingKey() ;
                    String contentType = properties.getContentType() ;
                    String msg = new String(body) ;
                    long deliveryTag = envelope.getDeliveryTag() ;

                    Log.e("TAG" , rountingKey+"：rountingKey") ;
                    Log.e("TAG" , contentType+"：contentType") ;
                    Log.e("TAG" , msg+"：msg") ;
                    Log.e("TAG" , deliveryTag+"：deliveryTag") ;

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
###### API
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106153510700.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106153657290.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106153754589.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181106153902932.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
#### 测试
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105191019707.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4,size_16,color_FFFFFF,t_70)
#### 完整代码
~~~
public class RabbitTwo_Activity extends AppCompatActivity {

    private String userName = "dlw" ;
    private String passWord = "dlw" ;
    private String virtualHost = "/" ;
    private String hostName = "192.168.xx.xxx" ;
    private int portNum = 5672 ;
    private String queueName = "que" ;
    private String exchangeName = "demo" ;
    private String rountingKey = "key" ;

    ConnectionFactory factory = new ConnectionFactory() ;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_rabbittwo);
        
        setupConnectionFactory();
        new Thread(new Runnable() {
            @Override
            public void run() {
                basicPublish();
            }
        }).start();
        new Thread(new Runnable() {
            @Override
            public void run() {
                basicConsume();
            }
        }).start();
    }

    /**
     * Rabbit配置
     */
    private void setupConnectionFactory(){

        factory.setUsername(userName);
        factory.setPassword(passWord);
        factory.setHost(hostName);
        factory.setPort(portNum);

    }

    /**
     * 发消息
     */
    private void basicPublish(){

        try {
            //连接
            Connection connection = factory.newConnection() ;
            //通道
            Channel channel = connection.createChannel() ;
            //声明了一个交换和一个服务器命名的队列，然后将它们绑定在一起。
            channel.exchangeDeclare(exchangeName , "fanout" , true) ;
            String queueName = channel.queueDeclare().getQueue() ;
            channel.queueBind(queueName , exchangeName , rountingKey) ;
            //消息发布
            byte[] msg = "hello word!".getBytes() ;
            channel.basicPublish(exchangeName , rountingKey  ,null , msg);

        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        }

    }

    /**
     * 收消息
     */
    private void basicConsume(){

        try {
            //连接
            Connection connection = factory.newConnection() ;
            //通道
            final Channel channel = connection.createChannel() ;
            //实现Consumer的最简单方法是将便捷类DefaultConsumer子类化。可以在basicConsume 调用上传递此子类的对象以设置订阅：
            channel.basicConsume(queueName , false , "administrator" , new DefaultConsumer(channel){
                @Override
                public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                    super.handleDelivery(consumerTag, envelope, properties, body);

                    String rountingKey = envelope.getRoutingKey() ;
                    String contentType = properties.getContentType() ;
                    String msg = new String(body) ;
                    long deliveryTag = envelope.getDeliveryTag() ;

                    Log.e("TAG" , rountingKey+"：rountingKey") ;
                    Log.e("TAG" , contentType+"：contentType") ;
                    Log.e("TAG" , msg+"：msg") ;
                    Log.e("TAG" , deliveryTag+"：deliveryTag") ;

                    channel.basicAck(deliveryTag , false);
                }
            });
        } catch (IOException e) {
            e.printStackTrace();
        } catch (TimeoutException e) {
            e.printStackTrace();
        }

    }



}

~~~
## 鸣谢
[官方指南](https://www.rabbitmq.com/api-guide.html)
[官方api](https://rabbitmq.github.io/rabbitmq-java-client/api/current/)
