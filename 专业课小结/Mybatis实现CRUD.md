## Mybatis实现简单的CRUD（增删改查）

### 用到的数据库：

```sql
CREATE DATABASE `mybatis`;
USE `mybatis`;
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `id` int(20) NOT NULL,
  `name` varchar(30) DEFAULT NULL,
  `pwd` varchar(30) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
insert  into `user`(`id`,`name`,`pwd`) values (1,'靠谱杨','123456'),(2,'张三','abcdef'),(3,'李四','987654');
```

### 使用之前要在Maven中导入Mybatis依赖包

```xml
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
```

### 官方的MyBatis核心配置文件mybatis-config.xml

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
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?	
                	useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf8"/>
                <property name="username" value="root"/>
                <property name="password" value="000429"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="com/kuang/dao/userMapper.xml"/>
    </mappers>
</configuration>
```



---



***注意***

 <mappers>
        **<mapper resource="com/kuang/dao/userMapper.xml"/>**
 </mappers>

这里很有可能会报错，需要解决**Maven静态资源过滤问题**，Maven习惯大于配置

在pom.xml文件中加入以下代码：

```xml
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

注意配置完之后刷新maven！

---



#### 简单数据类型User.java：

```java
package com.kuang.pojo;
public class User {
	private int id;  //id
	private String name;   //姓名
	private String pwd;   //密码
	//构造,有参,无参
	//set/get
	//toString()

	@Override
	public String toString() {
		return "User{" +
				"id=" + id +
				", name='" + name + '\'' +
				", pwd='" + pwd + '\'' +
				'}';
	}

	public User(){}
	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public User(int id, String name, String pwd) {

		this.id = id;
		this.name = name;
		this.pwd = pwd;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getPwd() {
		return pwd;
	}

	public void setPwd(String pwd) {
		this.pwd = pwd;
	}
}
```

#### 数据接口层UserMapper.java：

```java
public interface UserMapper {
	//查询全部用户
	List<User> getUserList();
	//根据ID查询用户
	User getUserByID(int id);
	//insert一个用户
	int addUser(User user);
	//update一个用户
	int updateUser(User user);
	//delete一个用户
	int deleteUser(User user);
}
```

#### 用户定义userMapper.xml:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.dao.UserMapper">
    
    <select id="getUserList" resultType="com.kuang.pojo.User">
    select * from user
    </select>
    
    <select id="getUserByID" resultType="com.kuang.pojo.User" parameterType="int">
    select * from user WHERE id = #{id}
    </select>
    
    <!--对象中的属性可以直接取出来 -->
    <insert id="addUser" parameterType="com.kuang.pojo.User">
        insert into mybatis.user (id,name,pwd) values (#{id},#{name},#{pwd});
    </insert>

    <update id="updateUser" parameterType="com.kuang.pojo.User">
        update mybatis.user set name = #{name} , pwd=#{pwd} where id = #{id} ;
    </update>
    
    <delete id="deleteUser" parameterType="com.kuang.pojo.User" >
        delete from mybatis.user where id = #{id}
    </delete>
</mapper>
```

**这里我们可以发现：数据接口是和xml对应的，xml文件实例了这个接口和里面的方法，namespace就是这个接口的名字，里面的select标签对应这个接口里的一个方法，id就是方法名，resultType对应这个方法的返回值类型。**

**xml文件namespace的包名要和Dao/Mapper接口类名保持一致！**

- id（getUserList）：就是对应的namespace中的方法名（ List<User> getUserList(); ）
- resultType：是SQL语句执行后的返回值！（Class、基本数据类型）
- parameterType：参数类型！
- delete from mybatis.user where id = #{id} 分析这个sql语句，赋值方式和JDBC有所不同，使用这样的格式#{ } 里面的参数是这个方法参数User对象里的成员属性 id 可以直接放到这里用！

#### 配置工具类（MybatisUtils.java）获取连接对象:

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
			InputStream inputStream = Resources.getResourceAsStream(resource);
            //读取配置文件获取连接
			sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	//返回获取的SqlSession连接
	public static SqlSession getSession(){
		return sqlSessionFactory.openSession();
	}
}
```

**分析一下这个工具类：**

首先这个**SqlSessionFactory**类是一个工厂类，这个工厂可以产生传统JDBC方法中获得的**PreparedStatement**对象。也就是这个工具类要完成和JDBCutils同样的任务，首先获取和数据库的连接，那么第一步也就是读取用户配置的**mybatis-config.xml**文件，可以得到一个数据库连接对象，之后用这个对象去获得一个类似于**PreparedStatement**对象的**SqlSession**类的对象。



---



#### 下面给出具体的调用方法（UserDaoTest.java）：

```java
import com.kuang.dao.UserMapper;
import com.kuang.pojo.User;
import com.kuang.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;
import java.util.List;
public class UserDaoTest {
	@Test
	public void test(){
		//sqlSession对象相当于是一个JDBC的PreparedStatement对象,这个对象可以执行sql语句
		//我们现在做的事情就是：获取数据库连接，得到sqlSession对象，通过这个对象去实现我们定义的数据操作接口
		//UserMapper，这个接口再通过解析我们定义的userMapper.xml文件去执行带有SQL语句的特定方法。
		SqlSession sqlSession = MybatisUtils.getSession();
		UserMapper mapper = sqlSession.getMapper(UserMapper.class);
//		List<User> userList = mapper.getUserList();
//		for (User user : userList)
//		{
//			System.out.println(user);
//		}
		//根据ID取
		/*User user=mapper.getUserByID(1);
		System.out.println(user);
		sqlSession.close();*/
		//更新一条数据 id为1
//		updateUser();
		//删除一个用户
		deleteUser();
	}
	public void addUser(){
		SqlSession sqlSession = MybatisUtils.getSession();
		UserMapper mapper = sqlSession.getMapper(UserMapper.class);
		//插入一条数据
		User user = new User(4, "名字", "密码");
		int res = mapper.addUser(user);
		if (res>0){
			System.out.println("插入成功！");
		}
		//提交事务
		sqlSession.commit();
		sqlSession.close();
	}
	public void updateUser(){
		//获得连接coon和执行sql语句的对象 类似JDBC的PreparedStatement
		SqlSession sqlSession = MybatisUtils.getSession();
		//实现UserMapper接口 通过反射机制
		UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
		User user = new User(1, "修改后的名字", "resetpass01");
		userMapper.updateUser(user);
		sqlSession.commit();
		sqlSession.close();
	}

	public void deleteUser(){
		//获取连接
		SqlSession sqlSession = MybatisUtils.getSession();
		//实现接口
		UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
		//实现接口后的mapper调用自己的方法执行sql语句
		userMapper.deleteUser(new User(1,"sss","123"));
		sqlSession.commit();
		sqlSession.close();
	}
}
```



***分析一下这两行代码：***

```java
	SqlSession sqlSession = MybatisUtils.getSession();
	UserMapper mapper = sqlSession.getMapper(UserMapper.class);
```



- 第一行代码，通过**MybatisUtils**类的**getSession**方法获得一个**sqlSession**对象，这个对象是一个已经连接了数据库的可以执行sql语句的对象（类似JDBC的PreparedStatement对象）
- 这样还不够，这只是停留在工具类层面，下面要做的一件事情就是让这个工具类去实现用户自定义的数据层接口，从而可以根据用户定义的数据操作方法去执行相应的sql语句，进而得到相应的结果集。

