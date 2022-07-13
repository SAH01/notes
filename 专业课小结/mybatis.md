### 结果集映射：

resultMap**解决数据库字段名和属性名不一致的问题**

```java
id	name	pwd
id	name	password
column 是数据库的字段名 property 是实体类的属性名
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.dao.UserMapper">
    <!--结果集映射 -->
    <resultMap id="UserMap" type="User">
        <result column="id" property="id"></result>
        <result column="name" property="name"></result>
        <result column="pwd" property="password"></result>
    </resultMap>
    <select id="getUserByID" resultMap="UserMap" parameterType="int">
      select * from user WHERE id = #{id}
    </select>
</mapper>
```

### 动态SQL

1. 什么是动态SQL？

   根据不同的条件生成不同的SQL语句

```java
官网描述：
    MyBatis 的强大特性之一便是它的动态 SQL。如果你有使用 JDBC 或其它类似框架的经验，你就能体会到根据不同条件拼接 SQL 语句的痛苦。例如拼接时要确保不能忘记添加必要的空格，还要注意去掉列表最后一个列名的逗号。利用动态 SQL 这一特性可以彻底摆脱这种痛苦。
    虽然在以前使用动态 SQL 并非一件易事，但正是 MyBatis 提供了可以被用在任意 SQL 映射语句中的强大的动态 SQL 语言得以改进这种情形。
    动态 SQL 元素和 JSTL 或基于类似 XML 的文本处理器相似。在 MyBatis 之前的版本中，有很多元素需要花时间了解。MyBatis 3 大大精简了元素种类，现在只需学习原来一半的元素便可。MyBatis 采用功能强大的基于 OGNL 的表达式来淘汰其它大部分元素。
    -------------------------------
    - if
    - choose (when, otherwise)
    - trim (where, set)
    - foreach
    -------------------------------
```



---

### MybatisUtils工具类

```java
package com.kuang.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class MybatisUtils {
	//从SqlSessionFactory中获得SqlSession的实例，SqlSession完全包含了SQL命令的执行方法
	private static SqlSessionFactory sqlSessionFactory;
	static {
		try {
			String resource = "mybatis-config.xml";
			//SqlSessionFactoryBuilder去创建sqlSessionFactory 创建完就不再需要它了
			//sqlSessionFactory 可以理解为数据库连接池
			//sqlSessionFactory 创建 sqlSession
			InputStream inputStream = Resources.getResourceAsStream(resource);
			sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	//获取SqlSession连接
	public static SqlSession getSession(){
		return sqlSessionFactory.openSession();
	}
}
```

### mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf8"/>
                <property name="username" value="root"/>
                <property name="password" value="000429"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <!--<mapper resource="com/kuang/dao/userMapper.xml"/>-->
        <!--
        使用映射器接口实现类的完全限定类名
        需要配置文件名称和接口名称一致，并且位于同一目录下
        -->
        <!--<mapper class="com.kuang.dao.UserMapper"/>-->
        <!--
        将包内的映射器接口实现全部注册为映射器
        但是需要配置文件名称和接口名称一致，并且位于同一目录下
        -->
        <package name="com.kuang.dao"/>
    </mappers>
</configuration>
```



-----

### typeAliases优化

```xml
<!--配置别名,注意顺序-->
<typeAliases>
    <typeAlias type="com.reliable.pojo.User" alias="User"/>
</typeAliases>
```

当这样配置时，`User`可以用在任何使用`com.reliable.pojo.User`的地方。

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean，比如:

```xml
<typeAliases>
    <package name="com.reliable.pojo"/>
</typeAliases>
```

在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名。若有注解，则别名为其注解值。

```java
@Alias("userX")
public class User {
    ...
}
```

### 测试类

```java
@Test
	public void test(){
		//sqlSession对象相当于是一个JDBC的PreparedStatement对象,这个对象可以执行sql语句
		//我们现在做的事情就是：获取数据库连接，得到sqlSession对象，通过这个对象去实现我们定义的数据操作接口
		//UserMapper，这个接口再通过解析我们定义的userMapper.xml文件去执行带有SQL语句的特定方法。
		SqlSession sqlSession = MybatisUtils.getSession();
		UserMapper mapper = sqlSession.getMapper(UserMapper.class);
		//根据ID取
		User user=mapper.getUserByID(2);
		System.out.println(user);
		sqlSession.close();
		//更新一条数据 id为1
//		updateUser();
		//删除一个用户
//		deleteUser();
	}
```

### 目录结构

![image-20211122101200663](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20211122101200663.png)



### db.properties

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=utf8
username=root
password=000429
```

#### 在mybatis-config.xml中

```xml
<!--导入properties文件-->
<properties resource="db.properties"/>
```

### log4j.properties

```properties
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
log4j.rootLogger=DEBUG,console,file
#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n
#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log/reliable.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n
#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```

#### 在mybatis-config.xml中

```xml
<!--日志-->
<settings>
    <setting name="logImpl" value="LOG4J"/>
</settings>
```

依赖

```xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

测试

```java
//注意导包：org.apache.log4j.Logger
static Logger logger = Logger.getLogger(MyTest.class);
@Test
public void selectUser() {
    logger.info("info：进入selectUser方法");
    logger.debug("debug：进入selectUser方法");
    logger.error("error: 进入selectUser方法");
    SqlSession session = MybatisUtils.getSession();
    UserMapper mapper = session.getMapper(UserMapper.class);
    List<User> users = mapper.selectUser();
    for (User user: users){
        System.out.println(user);
    }
    session.close();
}
```



---

### 多对一

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.StudentMapper">
    <select id="getStudent" resultMap="studentTeacher">
        select * from mybatis.student
    </select>
    <resultMap id="studentTeacher" type="student">
        <result property="id" column="id"></result>
        <result property="name" column="name"></result>
        <!--
        对象用association  多对一
        集合用collection   一对多
        -->
        <association property="teacher" column="tid" javaType="Teacher" select="getTeacher"/>
    </resultMap>
    <select id="getTeacher" resultType="Teacher">
        select * from teacher where id=#{id};
    </select>
    <!--     
        按照结果嵌套处理  
    -->
    <select id="getStudent2" resultMap="StudentTeacher2" >
        select s.id sid, s.name sname , t.name tname
        from student s,teacher t
        where s.tid = t.id
    </select>
    <resultMap id="StudentTeacher2" type="Student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <!--关联对象property 关联对象在Student实体类中的属性-->
        <association property="teacher" javaType="Teacher">
            <result property="name" column="tname"/>
        </association>
    </resultMap>
</mapper>
```

### 一对多

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.dao.TeacherMapper">
    <select id="getTeacher1" resultType="Teacher">
        select * from mybatis.teacher
    </select>
    <!--
    思路:
        1. 从学生表和老师表中查出学生id，学生姓名，老师姓名
        2. 对查询出来的操作做结果集映射
            1. 集合的话，使用collection！
            JavaType和ofType都是用来指定对象类型的
            JavaType是用来指定pojo中属性的类型
            ofType指定的是映射到list集合属性中pojo的类型。
    -->
    <select id="getTeacher2" resultMap="TeacherStudent">
        select s.id sid, s.name sname , t.name tname, t.id tid
        from student s,teacher t
        where s.tid = t.id and t.id=#{id}
    </select>
    <resultMap id="TeacherStudent" type="Teacher">
        <result property="id" column="tid"></result>
        <result  property="name" column="tname"/>
        <collection property="students" ofType="Student">
            <result property="id" column="sid" />
            <result property="name" column="sname" />
            <result property="tid" column="tid" />
        </collection>
    </resultMap>
</mapper>
```



### 标准mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.mapper.BlogMapper">
    
</mapper>
```

### 标准mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>


</configuration>
```

### mybatis依赖pom.xml

```xml
<dependencies>
        <!--导入Mybatis依赖包-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
        </dependency>
        <!--log4j-->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <!--lombok插件-->
        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.16.10</version>
        </dependency>
    </dependencies>
    <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>
    </build>
```



----



### foreach

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT *
  FROM POST P
  WHERE ID in
  <foreach item="item" index="index" collection="list"
      open="(" separator="," close=")">
        #{item}
  </foreach>
</select>
```



你可以将任何可迭代对象（如 List、Set 等）、Map 对象或者数组对象作为集合参数传递给 *foreach*。当使用可迭代对象或者数组时，index 是当前迭代的序号，item 的值是本次迭代获取到的元素。当使用 Map 对象（或者 Map.Entry 对象的集合）时，index 是键，item 是值。

```xml
　　select * from user 

　　<trim prefix="WHERE" prefixoverride="AND |OR">

　　　　<if test="name != null and name.length()>0"> AND name=#{name}</if>

　　　　<if test="gender != null and gender.length()>0"> AND gender=#{gender}</if>

　　</trim>
```



```xml
　update user

　　<trim prefix="set" suffixoverride="," suffix=" where id = #{id} ">

　　　　<if test="name != null and name.length()>0"> name=#{name} , </if>

　　　　<if test="gender != null and gender.length()>0"> gender=#{gender} ,  </if>

　　</trim>
```



---



### mybatis缓存

- MyBatis包含一个非常强大的查询缓存特性，它可以非常方便地定制和配置缓存。缓存可以极大的提升查询效率。

- MyBatis系统中默认定义了两级缓存：

  **一级缓存和二级缓存**

  - 默认情况下，只有一级缓存开启。（SqlSession级别的缓存，也称为本地缓存）
  - 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。
  - 为了提高扩展性，MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存

---



#### 一级缓存失效的四种情况

1. 没有使用到当前的一级缓存，效果就是，还需要再向数据库中发起一次查询请求！sqlSession不同。
2. sqlSession相同，查询条件不同
   1.   User user = mapper.queryUserById(1);
   2.  User user2 = mapper2.queryUserById(2);
3. sqlSession相同，两次查询之间执行了增删改操作！
4. sqlSession相同，手动清除一级缓存
   - session.clearCache();		//手动清除缓存

![img](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2021/04/01/kuangstudy203221f0-73d7-4d4c-bb81-84b1af9a63db.png)



#### 小结

- 只要开启了二级缓存，我们在同一个Mapper中的查询，可以在二级缓存中拿到数据
- 查出的数据都会被默认先放在一级缓存中
- 只有会话提交或者关闭以后，一级缓存中的数据才会转到二级缓存中







