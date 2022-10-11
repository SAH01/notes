**使用爬虫等获取实时数据+Flume+Kafka+Spark Streaming+mysql+Echarts实现数据动态实时采集、分析、展示**

主要工作流程如下所示：

其中爬虫获取实时数据，并把数据**实时传输到Linux本地文件夹**中。

使用Flume**实时监控**该文件夹，如果发现文件内容变动则进行处理，将**数据抓取并传递到Kafka消息队列**中。

之后**使用Spark Streaming 实时处理Kafka通道中的数据**，并写入本地mysql数据库中，之后**读取mysql数据库中的数据并基于Echart图表对数据进行实时动态展示。**

![image-20220318161438182](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220318161438182.png)



----

启动hadoop集群	myhadoop.sh start 【脚本参考 https://www.cnblogs.com/rainbow-1/p/16774523.html】

启动zookeeper集群 	myzk.sh start	【脚本参考 https://www.cnblogs.com/rainbow-1/p/15319226.html】

启动kafka集群  kf.sh 	start 				【脚本参考 https://www.cnblogs.com/rainbow-1/p/16015749.html】

## 一、实时数据的模拟

案例简化了第一步的流程，使用模拟数据进行测试，代码如下：

```python
import datetime
import random
import time

import paramiko

hostname = "hadoop102"
port = 22
username = "root"
password = "000429"
client = paramiko.SSHClient()
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
client.connect(hostname, port, username, password, compress=True)
sftp_client = client.open_sftp()
# try:
#     for line in remote_file:
#         print(line)
# finally:
#     remote_file.close()
#获取系统时间
num1=3000
for i in range(1000):
    remote_file = sftp_client.open("/opt/module/data/test1.csv", 'a')  # 文件路径
    time1 = datetime.datetime.now()
    time1_str = datetime.datetime.strftime(time1, '%Y-%m-%d %H:%M:%S')
    print("当前时间：  " + time1_str)
    time.sleep(random.randint(1,3))
    num1_str=str(num1+random.randint(-1300,1700))
    print("当前随机数：  "+num1_str)
    remote_file.write(time1_str+","+num1_str+"\n")
    remote_file.close()
```

- 主要过程

1. 在/opt/module/data/路径下建立test1.csv文件

2. 代码实现远程连接虚拟机hadoop102并以root用户身份登录，打开需要上传的文件目录。

3. 使用一个for循环间隔随机1到3秒向文件中写入一些数据。

----



## 二、Flume实时监控文件

1. 进入/opt/module/flume/job路径编辑配置文件信息（myflume.conf）

   内容如下：其中指定了被监控文件的路径，Kafka服务主机地址，Kafka主题和序列化等信息

```bash
#给agent中的三个组件source、sink和channel各起一个别名，a1代表为agent起的别名
a1.sources = r1
a1.channels = c1
a1.sinks = k1

# source属性配置信息
a1.sources.r1.type = exec
#a1.sources.r1.bind = localhost
#a1.sources.r1.port = 44444
a1.sources.r1.command=tail -F /opt/module/data/test1.csv

# sink属性配置信息 
a1.sinks.k1.type = org.apache.flume.sink.kafka.KafkaSink
a1.sinks.k1.kafka.bootstrap.servers:hadoop102:9092,hadoop103:9092,hadoop104:9092
a1.sinks.k1.kafka.topic=first
a1.sinks.k1.serializer.class=kafka.serializer.StringEncoer

#channel属性配置信息
  #内存模式
a1.channels.c1.type = memory
a1.channels.c1.capacity = 1000
  #传输参数设置
a1.channels.c1.transactionCapacity=100

#绑定source和sink到channel上
a1.sources.r1.channels=c1
a1.sinks.k1.channel=c1
```

2. 在/opt/module/flume 路径下开启Flume，此时Flume开始监控目标文件**（job/myflume.conf）**

```bash
bin/flume-ng agent -c conf/ -n a1 -f job/myflume.conf -Dflume.root.logger=INFO,console
```



## 三、使用Spark Streaming完成数据计算

1. 新建一个名为first的消费主题（topic）

   ```bash
   bin/kafka-topics.sh --bootstrap-server hadoop102:9092 --create --partitions 1 --replication-factor 1 --topic first1
   ```

2. 新建Maven项目，编写代码，Kafka的topic主题的消费者

   **pom.xml配置如下：注意此处各个资源的版本号一定要与本机（IDEA编译器）的Scala版本一致，博主为Scala 2.12.11**

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
       <groupId>com.reliable.ycw</groupId>
       <artifactId>spark-test</artifactId>
       <version>1.0-SNAPSHOT</version>
   
       <dependencies>
           <!-- https://mvnrepository.com/artifact/org.apache.spark/spark-network-common -->
           <!--<dependency>-->
               <!--<groupId>org.apache.spark</groupId>-->
               <!--<artifactId>spark-network-common_2.12</artifactId>-->
               <!--<version>3.0.0</version>-->
           <!--</dependency>-->
           <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>8.0.18</version>
           </dependency>
           <dependency>
               <groupId>org.apache.spark</groupId>
               <artifactId>spark-core_2.12</artifactId>
               <version>3.0.0</version>
           </dependency>
           <!-- https://mvnrepository.com/artifact/org.apache.spark/spark-streaming -->
           <dependency>
               <groupId>org.apache.spark</groupId>
               <artifactId>spark-streaming_2.12</artifactId>
               <version>3.0.0</version>
           </dependency>
           <!-- https://mvnrepository.com/artifact/org.apache.spark/spark-streaming-kafka-0-10 -->
           <dependency>
               <groupId>org.apache.spark</groupId>
               <artifactId>spark-streaming-kafka-0-10_2.12</artifactId>
               <version>3.0.0</version>
           </dependency>
           <dependency>
               <groupId>org.scala-lang</groupId>
               <artifactId>scala-library</artifactId>
               <version>2.12.11</version>
           </dependency>
           <dependency>
               <groupId>org.scala-lang</groupId>
               <artifactId>scala-compiler</artifactId>
               <version>2.12.11</version>
           </dependency>
           <dependency>
               <groupId>org.scala-lang</groupId>
               <artifactId>scala-reflect</artifactId>
               <version>2.12.11</version>
           </dependency>
       </dependencies>
       <build>
       <plugins>
       <!-- 该插件用于将 Scala 代码编译成 class 文件 -->
       <plugin>
       <groupId>net.alchim31.maven</groupId>
       <artifactId>scala-maven-plugin</artifactId>
       <version>3.2.2</version>
       <executions>
       <execution>
           <!-- 声明绑定到 maven 的 compile 阶段 -->
           <goals>
               <goal>testCompile</goal>
           </goals>
       </execution>
       </executions>
       </plugin>
           <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-assembly-plugin</artifactId>
               <version>3.1.0</version>
               <configuration>
                   <descriptorRefs>
                       <descriptorRef>jar-with-dependencies</descriptorRef>
                   </descriptorRefs>
               </configuration>
               <executions>
                   <execution>
                       <id>make-assembly</id>
                       <phase>package</phase>
                       <goals>
                           <goal>single</goal>
                       </goals>
                   </execution>
               </executions>
           </plugin>
           <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-compiler-plugin</artifactId>
               <configuration>
                   <source>8</source>
                   <target>8</target>
               </configuration>
           </plugin>
       </plugins>
       </build>
   </project>
   ```

   消费者类代码如下：

   ```scala
   import org.apache.kafka.common.serialization.StringDeserializer
   import org.apache.spark.SparkConf
   import org.apache.spark.streaming.kafka010.ConsumerStrategies.Subscribe
   import org.apache.spark.streaming.kafka010.KafkaUtils
   import org.apache.spark.streaming.kafka010.LocationStrategies.PreferConsistent
   import java.sql.DriverManager
   import java.text.SimpleDateFormat
   import java.util.Date
   
   /**
     * 主要是计算X秒内数据条数的变化
     * 比如5秒内进来4条数据
     */
   
   import org.apache.spark.streaming.{Seconds, StreamingContext}
   /** Utility functions for Spark Streaming examples.*/
   object StreamingExamples extends  App{
     /** Set reasonable logging levels for streaming if the user has not configured log4j.*/
   //  def setStreamingLogLevels() {
   //    val log4jInitialized = Logger.getRootLogger.getAllAppenders.hasMoreElements
   //    if (!log4jInitialized) {
   //      // We first log Appsomething to initialize Spark's default logging, then we override the
   //      // logging level.
   //      logInfo("Setting log level to [WARN] for streaming example." +
   //        " To override add a custom log4j.properties to the classpath.")
   //      Logger.getRootLogger.setLevel(Level.WARN)
   //    }
   //  }
     val conf=new SparkConf().setMaster("local").setAppName("jm")
       .set("spark.streaming.kafka.MaxRatePerPartition","3")
       .set("spark.local.dir","./tmp")
       .set("spark.serializer", "org.apache.spark.serializer.KryoSerializer")
     //创建上下文，5s为批处理间隔
     val ssc = new StreamingContext(conf,Seconds(5))
   
     //配置kafka参数，根据broker和topic创建连接Kafka 直接连接 direct kafka
     val KafkaParams = Map[String,Object](
       //brokers地址
       "bootstrap.servers"->"hadoop102:9092,hadoop103:9092,hadoop104:9092",
       //序列化类型
       "key.deserializer"->classOf[StringDeserializer],
       "value.deserializer" -> classOf[StringDeserializer],
       "group.id" -> "MyGroupId",
       //设置手动提交消费者offset
       "enable.auto.commit" -> (false: java.lang.Boolean)//默认是true
     )
   
     //获取KafkaDStream
     val kafkaDirectStream = KafkaUtils.createDirectStream[String,String](ssc,
       PreferConsistent,Subscribe[String,String](List("first"),KafkaParams))
     kafkaDirectStream.print()
     var num=kafkaDirectStream.count()
     var num_1=""
     num foreachRDD (x => {
   //var res=x.map(line=>line.split(","))
        val connection = getCon()
        var time = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date).toString
        var sql = "insert into content_num values('" + time + "'," + x.collect()(0) + ")"
        connection.createStatement().execute(sql)
        connection.close()
      })
   //  print("sdfasdf")
   //  print(num_1)
     //根据得到的kafak信息，切分得到用户电话DStream
   //  val nameAddrStream = kafkaDirectStream.map(_.value()).filter(record=>{
   //    val tokens: Array[String] = record.split(",")
   //    tokens(1).toInt==0
   //  })
   //
   //  nameAddrStream.print()
   //  .map(record=>{
   //    val tokens = record.split("\t")
   //    (tokens(0),tokens(1))
   //  })
   //
   //
   //  val namePhoneStream = kafkaDirectStream.map(_.value()).filter(
   //    record=>{
   //      val tokens = record.split("\t")
   //      tokens(2).toInt == 1
   //    }
   //  ).map(record=>{
   //    val tokens = record.split("\t")
   //    (tokens(0),tokens(1))
   //  })
   //
   //  //以用户名为key，将地址电话配对在一起，并产生固定格式的地址电话信息
   //  val nameAddrPhoneStream = nameAddrStream.join(namePhoneStream).map(
   //    record=>{
   //      s"姓名：${record._1},地址：${record._2._1},邮编：${record._2._2}"
   //    }
   //  )
   //  //打印输出
   //  nameAddrPhoneStream.print()
     //开始计算
     ssc.start()
     ssc.awaitTermination()
     def getCon()={
       Class.forName("com.mysql.cj.jdbc.Driver")
       DriverManager.getConnection("jdbc:mysql://localhost:3306/spark?serverTimezone=UTC&useUnicode=true&characterEncoding=utf8","root","reliable")
     }
   }
   
   
   ```

   这段代码指定了虚拟机中Kafka的主题信息，并从中定时获取（博主设置的为5秒）期间变化的信息量，完成计算后把本机的时间和信息变化量存储到本地Mysql数据库中【库spark 表content_num 字段 type num】

   - 注意指定时区和编码

     ```bash
     jdbc:mysql://localhost:3306/spark?serverTimezone=UTC&useUnicode=true&characterEncoding=utf8
     ```

---

## 四、可视化

![](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20221010094734855.png)

使用Echarts平滑折线图完成数据的展示

1. 后台读取mysql的数据【spark_sql.py】

```python
import pymysql
def get_conn():
    """
    获取连接和游标
    :return:
    """
    conn = pymysql.connect(host="127.0.0.1",
                           user="root",
                           password="reliable",
                           db="spark",
                           charset="utf8")
    cursor = conn.cursor()
    return conn, cursor


def close_conn(conn, cursor):
    """
    关闭连接和游标
    :param conn:
    :param cursor:
    :return:
    """
    if cursor:
        cursor.close()
    if conn:
        conn.close()


# query
def query(sql, *args):
    """
    通用封装查询
    :param sql:
    :param args:
    :return:返回查询结果 （（），（））
    """
    conn, cursor = get_conn()
    print(sql)
    cursor.execute(sql)
    res = cursor.fetchall()
    close_conn(conn, cursor)
    return res


def dynamic_bar():
    # 获取数据库连接
    conn, cursor = get_conn()
    if (conn != None):
        print("数据库连接成功！")
    typenumsql = "select * from content_num order by num desc limit 11;"
    detail_sql = ""
    res_title = query(typenumsql)
    type_num = []  # 存储类别+数量
    for item1 in res_title:
        type_num.append(item1)
    return type_num
```

2. 路由获取后台数据

```python
@app.route('/dynamic_bar')
def dynamic_bar():
    res_list=spark_sql.dynamic_bar()
    my_list=[]
    list_0=[]
    list_1=[]
    for item in res_list:
        list_0.append(item[0])
        list_1.append(item[1])
    my_list.append(list_0)
    my_list.append(list_1)
    return {"data":my_list}
```

3. 前台绘制折线图 line.html

```html
<!DOCTYPE html>
<html style="height: 100%">
    <head>
        <meta charset="utf-8">
    </head>
    <body style="height: 100%; margin: 0">
        <div id="container" style="height: 100%"></div>
        <script type="text/javascript" src="../static/js/echarts.min.js"></script>
        <script src="../static/js/jquery-3.3.1.min.js"></script>
    </body>
</html>
<script>
    var dom = document.getElementById("container");
    var myChart = echarts.init(dom);
    var app = {};
    var option;
</script>

<script type="text/javascript">

    option = {
      tooltip: {
        trigger: 'axis',
        axisPointer: {
          type: 'shadow'
        }
      },
      grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        containLabel: true
      },
      xAxis: [
        {
          type: 'category',
          data: [],
          axisTick: {
            alignWithLabel: true
          }
        }
      ],
      yAxis: [
        {
          type: 'value'
        }
      ],
      series: [
        {
          name: 'Direct',
          type: 'line',
          barWidth: '60%',
          data: []
        }
      ]
    };

    if (option && typeof option === 'object') {
        myChart.setOption(option);
    }
        function update(){
        $.ajax({
            url:"/dynamic_bar",
            async:true,
            success:function (data) {
                option.xAxis[0].data=data.data[0]
                option.series[0].data=data.data[1]
                myChart.setOption(option);
            },
            error:function (xhr,type,errorThrown) {
                alert("出现错误！")
            }
        })
    }
    setInterval("update()",100)
</script>
```

可视化这里需要注意的点：

- 注意先引入echarts.min.js再引入jquery-3.3.1.min.js
- 注意指定放置图像的div块的大小
- 把赋值方法放在图像初始化配置代码的后面
- 注意设置方法循环执行：setInterval("update()",100)

----

小结：整个流程的关键在于对实时数据的监控和展示，首先要保证**数据传输的动态性**，其次要**保证Flume实时监控数据的变化**。其中使用Kafka的目的在于当数据量足够大的时候，往往会出现数据的监控和采集速度跟不上数据的变化，所以采用Kafka消息队列机制，让其缓冲数据以实现大数据量的处理，后续需要编写Spark Streaming代码完成对消息的收集处理（存入本地mysql数据库），最后读取数据库数据并用折线图完成动态展示效果，数据库的数据是实时变动的，这就需要**在读取的时候要读到最新进来的数据**，这样才能看到图线的动态效果。（下图的图线会随着数据的变化动态改变！）

![image-20220318200827044](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220318200827044.png)

​	

​	
