## 大数据Hadoop-Spark集群部署知识总结

### 一、启动/关闭 hadoop

**myhadoop.sh start/stop**

分步启动：

第一步：在hadoop102主机上	sbin/start-dfs.sh

第二步：在hadoop103主机上    sbin/start-yarn.sh

分步关闭：

第一步：在hadoop103主机上    sbin/stop-yarn.sh

第二步：在hadoop102主机上	sbin/stop-dfs.sh

#### myhadoop.sh脚本文件内容

```

#!/bin/bash

if [ $# -lt 1 ]
then
    echo "No Args Input..."
    exit ;
fi

case $1 in
"start")
        echo " =================== 启动 hadoop集群 ==================="

        echo " --------------- 启动 hdfs ---------------"
        ssh hadoop102 "/opt/module/hadoop-3.1.3/sbin/start-dfs.sh"
        echo " --------------- 启动 yarn ---------------"
        ssh hadoop103 "/opt/module/hadoop-3.1.3/sbin/start-yarn.sh"
        echo " --------------- 启动 historyserver ---------------"
        ssh hadoop102 "/opt/module/hadoop-3.1.3/bin/mapred --daemon start historyserver"
;;
"stop")
        echo " =================== 关闭 hadoop集群 ==================="

        echo " --------------- 关闭 historyserver ---------------"
        ssh hadoop102 "/opt/module/hadoop-3.1.3/bin/mapred --daemon stop historyserver"
        echo " --------------- 关闭 yarn ---------------"
        ssh hadoop103 "/opt/module/hadoop-3.1.3/sbin/stop-yarn.sh"
        echo " --------------- 关闭 hdfs ---------------"
        ssh hadoop102 "/opt/module/hadoop-3.1.3/sbin/stop-dfs.sh"
;;
*)
    echo "Input Args Error..."
;;
esac
```



### 二、启动/关闭 zookeeper

**myzk.sh start/stop**

分步启动/关闭：

bin/zkServer.sh start

bin/zkServer.sh stop

#### myzk.sh脚本文件内容

```
for host in hadoop102 hadoop103 hadoop104
do
        case $1 in
        "start")
                ssh $host "source /etc/profile;/opt/module/zookeeper-3.5.7/bin/zkServer.sh $1"
                echo "$host zk is running..."
                echo "-----------------------------"
        ;;

        "stop")
                ssh $host "source /etc/profile;/opt/module/zookeeper-3.5.7/bin/zkServer.sh $1"
                echo "$host zk is stopping..."
                echo "-----------------------------"
        ;;

        *)
                 echo '输入有误！'
        ;;
     esac
done

```



### 三、启动Hbase

bin/hbase-daemon.sh start master

bin/hbase-daemon.sh start regionserver

bin/hbase-daemon.sh stop master

bin/hbase-daemon.sh stop regionserver

bin/start-hbase.sh

bin/stop-hbase.sh

---



### 四、常见端口号总结

**50070：HDFSwebUI的端口号**

8485:journalnode默认的端口号

9000：非高可用访问数rpc端口

8020：高可用访问数据rpc

**8088：yarn的webUI的端口号**

8080：master的webUI，Tomcat的端口号

7077：spark基于standalone的提交任务的端口号

**8081：worker的webUI的端口号**

18080：historyServer的webUI的端口号

4040：application的webUI的端口号

2181：zookeeper的rpc端口号

**9083：hive的metastore的端口号**

**60010：Hbase的webUI的端口号**

6379：Redis的端口号

**8087：sparkwebUI的端口号**		sbin/start-master.sh 文件可以修改端口号，默认是8080，我改为8081

9092：kafka broker的端口

---



### 五、启动Hive

1. **启动metastore	hive --service metastore**
2. 启动hiveserver2  bin/hive --service hiveserver2
3. 启动hive                **(/opt/module/hive)：bin/hive**





hive建表：

```sql
create table test1
(InvoiceNo String, StockCode String, Description String, Quantity String, InvoiceDate String, UnitPrice String, CustomerID String, Country String)
ROW format delimited fields terminated by ',' STORED AS TEXTFILE;
```

导入数据：

```sql
load data local inpath '/opt/module/data/test.csv' into table test1;
```

sqoop导出到mysql：

```sql
bin/sqoop export \
--connect jdbc:mysql://hadoop102:3306/company \
--username root \
--password 000429 \
--table sale \
--num-mappers 1 \
--export-dir /user/hive/warehouse/sale \
--input-fields-terminated-by ","
```



sqoop导入到hive：

```sql
bin/sqoop import \
> --connect jdbc:mysql://hadoop102:3306/company \
> --username root \
> --password 123456 \
> --table staff \
> --num-mappers 1 \
> --hive-impo
> --fields-terminated-by "\t" \
> --hive-overwrite \
> --hive-table  数据库名.staff_hive
```



sql建表：

```sql 
USE `company`;
CREATE TABLE `sale1` (
  `day_id` VARCHAR(50) COLLATE utf8_bin DEFAULT NULL,
  `sale_nbr` VARCHAR(50) COLLATE utf8_bin DEFAULT NULL,
  `cnt` VARCHAR(50) COLLATE utf8_bin DEFAULT NULL,
  `round` VARCHAR(50) COLLATE utf8_bin DEFAULT NULL
) ENGINE=INNODB DEFAULT CHARSET=utf8 COLLATE=utf8_bin;

CREATE TABLE `sale2` (
  `day_id` varchar(50) COLLATE utf8_bin DEFAULT NULL,
  `sale_nbr` varchar(50) COLLATE utf8_bin DEFAULT NULL,
  `cnt` varchar(50) COLLATE utf8_bin DEFAULT NULL,
  `round` varchar(50) COLLATE utf8_bin DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin;
```



### 六、Spark

1. 安装Spark后配置 **classpath**

```
$ cd /usr/local/spark
$ cp ./conf/spark-env.sh.template ./conf/spark-env.sh   #拷贝配置文件
```

export SPARK_DIST_CLASSPATH=$(**/usr/local/hadoop/bin/hadoop** classpath)		这个路径是hadoop的安装路径

2. local模式启动spark：	./bin/spark-shell

![image-20220302182338978](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220302182338978.png)

3. 安装sbt

   **vim ./sbt**

   **启动脚本文件内容如下：**

   ```
   #!/bin/bash
   SBT_OPTS="-Xms512M -Xmx1536M -Xss1M -XX:+CMSClassUnloadingEnabled -XX:MaxPermSize=256M" java $SBT_OPTS -jar `dirname $0`/sbt-launch.jar "$@"
   
   ```

   增加可执行权限命令： **chmod u+x ./sbt**

4. **simple.sbt文件内容（注意版本号和名字）**

```
name := "Simple Project"
version := "1.0"
scalaVersion := "2.11.8"
libraryDependencies += "org.apache.spark" %% "spark-core" % "2.1.0"
```

### 七、配置Spark集群

1. **主机环境变量**

```
vim ~/.bashrc

export SPARK_HOME=/usr/local/spark
export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin

$ source ~/.bashrc
```

2. **从机环境变量**

```
$ cd /usr/local/spark/
$ cp ./conf/slaves.template ./conf/slaves

把默认内容localhost替换成如下内容：
hadoop103
hadoop104
```

3. 配置**spark-env.sh**

- 注意SPARK_MASTER_IP 要填自己的主机IP地址
- SPARK_DIST_CLASSPATH和HADOOP_CONF_DIR 都是主机的hadoop路径

```
$ cp ./conf/spark-env.sh.template ./conf/spark-env.sh

export SPARK_DIST_CLASSPATH=$(/usr/local/hadoop/bin/hadoop classpath) 
export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
export SPARK_MASTER_IP=192.168.1.104 

```

4. **分发到从机**（待分发的路径最好已经建立好且是空的）

```
cd /usr/local/
tar -zcf ~/spark.master.tar.gz ./spark
cd ~
scp ./spark.master.tar.gz hadoop103:/home/hadoop
scp ./spark.master.tar.gz hadoop104:/home/hadoop

```

在从机上进行如下操作：

```
sudo rm -rf /usr/local/spark/
sudo tar -zxf spark.master.tar.gz -C /usr/local
sudo chown -R 用户名 /usr/local/spark
```



### 八、测试运行

1. 首先启动hadoop集群
2. **启动spark的主机节点**

```
$ cd /usr/local/spark/
$ sbin/start-master.sh
```

3. **启动spark的从机节点**

```
$ sbin/start-slaves.sh
```

打开浏览器输入 http://master:8087 

![image-20220302183458125](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220302183458125.png)

注意端口号冲突问题：

可以在启动的脚本文件里修改WEBUI端口号：也就是**在sbin/start-master.sh中修改端口号！**



### 九、关闭退出

- 关闭spark主机

```
sbin/stop-master.sh
```

- 关闭Worker从机

````
sbin/stop-slaves.sh
````

- 关闭hadoop集群

---

### 补充命令：

- cp命令：cp 源文件 目标文件（夹）

负责把一个源文件复制到目标文件（夹）下。如下图所示，**复制到文件夹下，则文件名保持不变，复制到文件中，则文件名变更。**如果目标文件已经存在或目标文件夹中含有同名文件，则复制之后目标文件或目标文件夹中的同名文件**会被覆盖。**

- cp  -r 命令 :复制**源文件夹到目标文件夹下**

命令格式为：cp -r 源文件夹 目标文件夹

- mv 命令：用来移动文件或者将文件改名

  格式：mv   [选项]   源文件或目录   目标文件或目录

    选项：
  
    -b  若需覆盖文件，则在覆盖文件前先进行备份
    -f   强制覆盖，若目标文件已存在同名文件，使用该参数时则直接覆盖而不询问
    -i   若目标文件已存在同名文件，则提示询问是否覆盖
    -u  若目标文件已存在需移动的同名文件，且源文件比较新，才会更新文件
    -t   指定mv的目标目录，改选项使用于移动多个源文件到一个目录的情况，此时目标文件在前，源文件在后
- chmod

  sudo chmod -（代表类型）×××（所有者）×××（组用户）×××（其他用户）

  0 [000] 无任何权限
  4 [100] 只读权限
  6 [110] 读写权限
  7 [111] 读写执行权限

  sudo chmod 600 ××× （只有所有者有读和写的权限）
  sudo chmod 644 ××× （所有者有读和写的权限，组用户只有读的权限）
  sudo chmod 700 ××× （只有所有者有读和写以及执行的权限）
  sudo chmod 666 ××× （每个人都有读和写的权限）
  sudo chmod 777 ××× （每个人都有读和写以及执行的权限）

----



- chown (选项)(参数)

  **选项**											**描述**
  -c或——changes					效果类似“-v”参数，但仅回报更改的部分；
  -f或–quite或——silent			不显示错误信息；
  -h或–no-dereference				只对符号连接的文件作修改，而不更改其他任何相关文件；
  -R或——recursive						递归处理，将指定目录下的所有文件及子目录一并处理；
  -v或——version								显示指令执行过程；
  –dereference										效果和“-h”参数相同；
  –help														在线帮助；
  –reference=<参考文件或目录>			把指定文件或目录的拥有者与所属群组全部设成和参考文件或目录的拥有者与所属群组相同；
  –version													显示版本信息。
  



当只需要修改所有者时，可使用如下 chown 命令的基本格式：

**[root@localhost ~]# chown [-R] 所有者 文件或目录**

如果需要同时更改所有者和所属组，chown 命令的基本格式为：

**[root@localhost ~]# chown [-R] 所有者:所属组 文件或目录**

---

### 十、使用sbt对Scala程序进行打包并运行（Spark单机运行）

1. 在./sparkapp 中新建文件 **simple.sbt（vim ./sparkapp/simple.sbt）**，添加内容如下，声明该独立应用程序的信息以及与 Spark 的依赖关系：

```scala
name := "Simple Project"
version := "1.0"
scalaVersion := "2.11.8"
libraryDependencies += "org.apache.spark" %% "spark-core" % "2.1.0"
```

2. 在sparkapp目录下查看文件目录结构（find .）

![image-20220303212754039](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220303212754039.png)

3. 同样**在sparkapp目录下**使用如下命令把代码打包成JAR

```
/usr/local/sbt/sbt package
```



**生成的JAR包位置是：/sparkapp/target/scala-2.11/simple-project_2.11-1.0.jar**

4. 通过**spark-submit**提交应用程序

   在sparkapp目录下执行下面的命令：

```scala
/usr/local/spark/bin/spark-submit --class "SimpleApp" ~/sparkapp/target/scala-2.11/simple-project_2.11-1.0.jar
```

### 十一、Flume

1）开启Flume的监控端口

![image-20220316163840422](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220316163840422.png)

```
bin/flume-ng agent -c conf/ -n a1 -f job/myflume.conf -Dflume.root.logger=INFO,console
```

![image-20220316164347054](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220316164347054.png)

---

2）使用netcat工具向44444端口发送信号

```
nc localhost 44444
```

HDFS NameNode 内部通常端口号：**8020**/9000/9820
HDFS NameNode 对用户的查询端口：**9870**
Yarn  查看任务的运行情况：8088
历史服务器：90080

----

![image-20220316221124278](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220316221124278.png)

jpsall代码

```
#!/bin/bash

for host in hadoop102 hadoop103 hadoop104
do
        echo =============== $host ===============
        ssh $host jps
done
```

查看某个命令所在的路径

![image-20220316221810301](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220316221810301.png)

which 【命令名称】

whereis 用来查看一个命令或者文件所在的路径

**which命令的原理：在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。也就是说，使用which命令，就可以看到某个系统命令是否存在，以及执行的到底是哪一个位置的命令。**

**which命令的使用实例：**

　　**$ which grep**

**whereis命令原理：只能用于程序名的搜索，而且只搜索二进制文件（参数-b）、man说明文件（参数-m）和源代码文件（参数-s）。如果省略参数，则返回所有信息。**

**whereis命令的使用实例：**

　　**$ whereis grep**

下面举个例子来说明。假如你的linux系统上装了多个版本的java。如果你直接在命令行敲命令 "java -version" ，会得到一个结果。但是，你知道是哪一个路径下的java在执行吗？如果想知道，可以用 which 命令：

which java

返回的是 PATH路径中第一个JAVA的位置，也就是JAVA命令默认执行的位置

如果使用命令： whereis java

那么你会得到很多条结果，因为这个命令把所有包含java（不管是文件还是文件夹）的路径都列了出来。

![image-20220316222016763](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220316222016763.png)

----



### 十二、Kafka

#### （1）Topic

1）查看当前服务器中的所有topic

bin/kafka-topics.sh --bootstrap-server hadoop102:9092 --list

2）创建first topic

bin/kafka-topics.sh --bootstrap-server hadoop102:9092 --create --partitions 1 --replication-factor 3 --topic first

参数说明

--topic 定义 topic 名
--replication-factor 定义副本数
--partitions 定义分区数

3）查看first主题详情

bin/kafka-topics.sh --bootstrap-server hadoop102:9092 --describe --topic first

4）修改分区数（只可以增加不可以减少）

bin/kafka-topics.sh --bootstrap-server hadoop102:9092 --alter --topic first --partitions 3

5）删除topic

bin/kafka-topics.sh --bootstrap-server hadoop102:9092 --delete --topic first

#### （2）生产者

发送消息

bin/kafka-console-producer.sh --bootstrap-server hadoop102:9092 --topic first

参数描述
--bootstrap-server <String: server toconnect to>   连接的 Kafka Broker主机名称和端口号。
--topic <String: topic> 												 操作的 topic名称。

#### （3）消费者

1）消费主题first中的数据

bin/kafka-console-consumer.sh --bootstrap-server hadoop102:9092 --topic first

参数 描述
--bootstrap-server <String: server toconnect to>  连接的 Kafka Broker主机名称和端口号。
--topic <String: topic> 												操作的 topic名称。

--from-beginning 														从头开始消费。
--group <String: consumer group id> 					 指定消费者组名称。

2）把主题中的数据都读取出来（包括历史数据）

bin/kafka-console-consumer.sh --bootstrap-server hadoop102:9092 --from-beginning --topic first

---

**kf.sh**

```bash
#! /bin/bash
case $1 in
"start"){
   for i in hadoop102 hadoop103 hadoop104
   do
      echo " --------启动 $i Kafka-------"
      ssh $i "/opt/module/kafka/bin/kafka-server-start.sh -daemon /opt/module/kafka/config/server.properties"
   done
};;
"stop"){
    for i in hadoop102 hadoop103 hadoop104
    do
       echo " --------停止 $i Kafka-------"
       ssh $i "/opt/module/kafka/bin/kafka-server-stop.sh "
    done
};;
esac
```

