Java学习路线之redis

## 1、redis

大数据时代三V：海量Volume、多样Variety、实时Velocity

大数据时代三高：高并发、高可用（无限套娃+彼此监控）、高性能

```bash
- Redis（Remote Dictionary Server )，即远程字典服务，是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。
# 默认有16个数据库 使用select切换
-----------------------------------
set get flushall flushdb 
```

![image-20220809181435756](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220809181435756.png)

redis可以用作**数据库、缓存和消息中间件**。

```bash 
keys * 查看所有key
exists 返回 0 / 1
move name 1 删除
expire [name] [时长] 设置过期时间
type 查看类型 
秒语法：ttl key 毫秒语法：pttl key 查询key的生命周期（秒）

设置过期时间
秒语法：expire key seconds
毫秒语法：pexpire key milliseconds

设置永不过期
语法：persist key

更改键名称
语法：rename key newkey

值递增/递减
如果字符串中的值是数字类型的，可以使用incr命令每次递增，不是数字类型则报错。
语法：incr key

追加内容
语法：append key value

获取值长度
语法：strlen key

获取部分字符
语法：getrange key start end

设置过期时间
setex key [时长] [name]

获取并设置值
getset
```



### redis持久化

RDB快照（snapshotting）

把当前内存中的数据集**快照**写入磁盘，也就是 Snapshot 快照（数据库中所有键值对数据）。恢复时是将快照文件直接读到内存里。

可以自动触发（关机触发、隔一段时间触发）也可以手动触发（save和bgsave）。**Redis进程执行fork操作创建子进程，RDB持久化过程由子进程负责，完成后自动结束。**

==恢复==：将备份文件 (dump.rdb) 移动到 redis 安装目录并启动服务即可，redis就会自动加载文件数据至内存了。Redis 服务器在载入 RDB 文件期间，会一直处于阻塞状态，直到载入工作完成为止。

----------

AOF（append-only-file）

默认不开启，默认使用的是RDB

**通过保存Redis服务器所执行的【写命令】来记录数据库状态**

![image-20220809211845304](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220809211845304.png)

RDB 持久化方式就是将 str1,str2,str3 这三个键值对保存到 RDB文件中，而 AOF 持久化则是将执行的 set,sadd,lpush 三个命令保存到 AOF 文件中。

AOF 文件恢复：

　　重启 Redis 之后就会进行 AOF 文件的载入。

　　异常修复命令：redis-check-aof --fix 进行修复

AOF 重写：

　　由于AOF持久化是Redis不断将写命令记录到 AOF 文件中，随着Redis不断的进行，AOF 的文件会越来越大，文件越大，占用服务器内存越大以及 AOF 恢复要求时间越长。为了解决这个问题，Redis新增了重写机制，当AOF文件的大小超过所设定的阈值时，Redis就会启动AOF文件的内容压缩，只保留可以恢复数据的最小指令集。可以使用命令 bgrewriteaof 来重新。

### redis发布订阅

发布者和订阅者都是Redis客户端，Channel则为Redis服务器端，发布者将消息发送到某个的频道，订阅了这个频道的订阅者就能接收到这条消息。Redis的这种发布订阅机制与基于主题的发布订阅类似，Channel相当于主题。简单来说和**对讲机**的意思差不多。

### redis主从复制

![image-20220809213718293](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220809213718293.png)

主节点写、从节点读。

![image-20220809213633442](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220809213633442.png)

### redis哨兵模式

监控主机故障，自动选举老大的模式

![image-20220809214349850](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220809214349850.png)

![image-20220809214358272](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220809214358272.png)

### redis缓存穿透

![image-20220809215329997](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220809215329997-16600532110041.png)

![image-20220809215450899](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220809215450899.png)

![image-20220809215504969](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220809215504969.png)

### redis缓存击穿

正在疯狂查询的某数据过期了，导致去大量查询持久层数据库。

![image-20220809220139047](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220809220139047.png)

### redis缓存雪崩

缓存集中失效，或者redis宕机。

限流降级：通过加锁来控制线程数或者停掉一些不必要的服务，保证主服务的运行。

高可用：多增设几台redis

数据预热：把在未来可能会频繁访问的数据人为提前加载到缓存里。