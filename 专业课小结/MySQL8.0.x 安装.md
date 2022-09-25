# MySQL8.0.x 安装

## 一、下载

MySQL官网下载链接：https://downloads.mysql.com/archives/community/

选择版本后下载zip文件

![image-20220408194432886](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220408194432886.png)

博主选择的是8.0.13

## 二、安装

### 1 解压

把下载好的zip包在你想要的路径下直接解压。

解压完成后得到这个界面：

![image-20220408194638608](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220408194638608.png)

### 2 配置环境变量

#### 右击此电脑选择属性

![image-20220408194811149](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220408194811149.png)



![image-20220408194826022](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220408194826022.png)

#### 双击系统环境变量的Path

![image-20220408194901568](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220408194901568.png)



**新建一个刚刚你解压的路径（注意要到bin路径下）**



---



### 3 配置my.ini

![image-20220408195151023](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220408195151023.png)

在D:\Program Files (x86)\mysql-8.0.13-winx64\mysql-8.0.13-winx64路径下新建一个 **my.ini** 文件。

==这里有一个注意点：可能会埋坑，可以参考下面这个链接操作一下==

https://www.cnblogs.com/rainbow-1/p/16119461.html



文件内容如下：

```bash
[mysqld]

# 设置3306端口

port=3306

# 设置mysql的安装目录

basedir=D:\\Program Files (x86)\\mysql-8.0.13-winx64\\mysql-8.0.13-winx64

# 切记此处一定要用双斜杠\\，单斜杠这里会出错。

# 设置mysql数据库的数据的存放目录

datadir=D:\Program Files (x86)\mysql-8.0.13-winx64\\Data
# 此处同上

# 允许最大连接数

max_connections=200

# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统

max_connect_errors=10

# 服务端使用的字符集默认为UTF8

character-set-server=utf8

# 创建新表时将使用的默认存储引擎

default-storage-engine=INNODB

# 默认使用“mysql_native_password”插件认证

default_authentication_plugin=mysql_native_password

[mysql]

# 设置mysql客户端默认字符集

default-character-set=utf8

[client]

# 设置mysql客户端连接服务端时默认使用的端口

port=3306

default-character-set=utf8
```

### 4 命令行启动mysql

#### WIN+R 输入cmd 进入window命令行

![image-20220408195429947](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220408195429947.png)



#### cd 进入安装mysql的bin目录下

![image-20220408195540269](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220408195540269.png)

输入：==mysqld --initialize --console==

等待片刻会输出一堆东西，推荐先把这些输出复制一下，放到一个记事本里，因为里面会有你需要的**数据库初始密码**。

一般会在==root@localhost：之后==（是一堆像乱码一样的东西，这是初始的随机密码，后续我们会进行更改！）



记下密码之后，执行命令：==mysqld --install==  安装mysql

正常会输出 successfully



之后执行命令：==net start mysql==  启动mysql服务



### 5 修改登录密码

命令：==mysql -u root -p==  之后复制你刚刚保存在txt文件的初始密码进入mysql

命令：==alter user root@localhost identified by '123456';==  这个 123456是我随便打的 可以自定义修改，如果是mysql8以上，推荐别使用纯数字密码，别问我为什么，都是被坑出来的教训！



注：如果出现忘记初始随机密码的情况，可以重新执行==mysqld --initialize --console==这个命令，**但是前提是删除之前生成的Data文件夹**



![image-20220408200303037](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220408200303037.png)





