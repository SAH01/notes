# 整合SSM

![image-20211128165153948](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20211128165153948.png)

![image-20211128165202990](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20211128165202990.png)

![image-20211128165216713](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20211128165216713.png)



-----



## 01 基本配置文件的关系

![image-20211128165552011](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20211128165552011.png)

web.xml配置**DispatcherServlet**

![image-20211128165659202](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20211128165659202.png)



---

## 02 需要的maven依赖

```xml
<!--依赖
1、junit
2、数据库连接池
3、servlet
4、jsp
5、mybatis
6、mybatis-spring
7、spring
-->
<dependencies>
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
    <!--Junit-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
    <!--数据库驱动-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.47</version>
    </dependency>
    <!-- 数据库连接池 -->
    <dependency>
        <groupId>com.mchange</groupId>
        <artifactId>c3p0</artifactId>
        <version>0.9.5.2</version>
    </dependency>
    <!--Servlet - JSP -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
    </dependency>
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.2</version>
    </dependency>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency>
    <!--Mybatis-->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.2</version>
    </dependency>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.2</version>
    </dependency>
    <!--Spring-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.1.9.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.1.9.RELEASE</version>
    </dependency>
</dependencies>
<!--资源过滤-->
<build>
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
</build>
```



## 03 web.xml配置DispatcherServlet以及中文编码过滤

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--DispatcherServlet-->
    <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <!--一定要注意:我们这里加载的是总的配置文件，之前被这里坑了！-->
            <param-value>classpath:applicationContext.xml</param-value>
        </init-param>
        <!--和tomcat一起启动-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    <!--encodingFilter-->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>
            org.springframework.web.filter.CharacterEncodingFilter
        </filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    <!--Session过期时间-->
    <session-config>
        <session-timeout>15</session-timeout>
    </session-config>
</web-app>
```



## 一、spring整合mybatis

### 01 mybatis核心配置文件mybatis-config.xml

- 起别名
- 添加mapper映射
- 添加setting

```xml 
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <typeAliases>
        <package name="com.kuang.pojo"/>
    </typeAliases>
    <mappers>
        <mapper resource="com/kuang/dao/BookMapper.xml"/>
    </mappers>
</configuration>
```

### 02 spring整合配置文件spring-dao.xml

- 关联数据库配置信息，配置连接池
- 配置SqlSessionFactory对象
- **配置扫描Dao接口包，动态实现Dao接口注入到spring容器中**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">
    <!-- 配置整合mybatis -->
    <!-- 1.关联数据库文件 -->
    <context:property-placeholder location="classpath:database.properties"/>
    <!-- 2.数据库连接池 -->
    <!--数据库连接池
        dbcp  半自动化操作  不能自动连接
        c3p0  自动化操作（自动的加载配置文件 并且设置到对象里面）
    -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <!-- 配置连接池属性 -->
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
        <!-- c3p0连接池的私有属性 -->
        <property name="maxPoolSize" value="30"/>
        <property name="minPoolSize" value="10"/>
        <!-- 关闭连接后不自动commit -->
        <property name="autoCommitOnClose" value="false"/>
        <!-- 获取连接超时时间 -->
        <property name="checkoutTimeout" value="10000"/>
        <!-- 当获取连接失败重试次数 -->
        <property name="acquireRetryAttempts" value="2"/>
    </bean>
    <!-- 3.配置SqlSessionFactory对象 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 注入数据库连接池 -->
        <property name="dataSource" ref="dataSource"/>
        <!-- 配置MyBaties全局配置文件:mybatis-config.xml -->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
    </bean>

    <!-- 4.配置扫描Dao接口包，动态实现Dao接口注入到spring容器中 -->
    <!--解释 ： https://www.cnblogs.com/jpfss/p/7799806.html-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 注入sqlSessionFactory -->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <!-- 给出需要扫描Dao接口包 -->
        <property name="basePackage" value="com.kuang.dao"/>
    </bean>
</beans>
```



- **到这里dao层的数据获取部分已经配置完成，其中获取sqlSession的方式有所不同，之前有两种方式获取sqlSession对象：**

1. **利用构造器注入**

```java
package com.kuang.dao;

import com.kuang.pojo.User;
import org.mybatis.spring.SqlSessionTemplate;

import java.util.List;

public class UserMapperImpl implements  UserMapper {
	//sqlSession不用我们自己创建了，Spring来管理
	private SqlSessionTemplate sqlSession;
	public void setSqlSession(SqlSessionTemplate sqlSession) {
		this.sqlSession = sqlSession;
	}
	public List<User> selectUser() {
		UserMapper mapper = sqlSession.getMapper(UserMapper.class);
		return mapper.selectUser();
	}
}

```



```xml
<import resource="spring-dao.xml"></import>
 <!---->
 <bean id="userMapper" class="com.kuang.dao.UserMapperImpl">
     <property name="sqlSession" ref="sqlSession"></property>
 </bean>
```



```xml 
<!--注册sqlSessionTemplate , 关联sqlSessionFactory-->
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
    <!--利用构造器注入-->
    <constructor-arg index="0" ref="sqlSessionFactory"/>
</bean>
```

2. **继承SqlSessionDaoSupport，直接使用getSqlSession().getMapper(UserMapper.class);获取**

   **引用spring-dao.xml的sqlSessionFactory，代理生成sqlSession对象**

   

```xml
<!--配置SqlSessionFactory-->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <!--关联Mybatis-->
    <property name="configLocation" value="classpath:mybatis-config.xml"/>
    <property name="mapperLocations" value="classpath:com/kuang/dao/*.xml"/>
</bean>
```



```java
package com.kuang.dao;

import com.kuang.pojo.User;
import org.mybatis.spring.support.SqlSessionDaoSupport;

import java.util.List;

public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper  {
	public List<User> selectUser() {
		UserMapper mapper = getSqlSession().getMapper(UserMapper.class);
		return mapper.selectUser();
	}
}

```



```xml 
<bean id="userMapper2" class="com.kuang.dao.UserMapperImpl2">
    <property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
</bean>
```

- **现在配置的方法是最简单的，不需要再手动获取sqlSession对象，由Spring动态生成并执行sql语句！**

---



## 二、整合springmvc

![image-20211128172749725](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20211128172749725.png)

### 01 springmvc主要管理的是controller层和service层

- controller层主要负责决定要从dao层获取什么数据，还有这些数据要渲染到什么视图上。
- service层主要负责实现dao层的方法，定义一个新接口，这个接口里的方法来自dao层接口的方法，然后写一个Impl实现这个接口。

### spring-mvc.xml整合controller层

- 开启注解支持
- 默认过滤静态资源
- 配置ViewResolver视图解析器
- 扫描controller层的bean完成注入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd
    http://www.springframework.org/schema/mvc
    https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!-- 配置SpringMVC -->
    <!-- 1.开启SpringMVC注解驱动 -->
    <mvc:annotation-driven />
    <!-- 2.静态资源默认servlet配置-->
    <mvc:default-servlet-handler/>
    <!-- 3.配置jsp 显示ViewResolver视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView" />
        <property name="prefix" value="/WEB-INF/jsp/" />
        <property name="suffix" value=".jsp" />
    </bean>
    <!-- 4.扫描web相关的bean -->
    <context:component-scan base-package="com.kuang.controller"/>
</beans>
```



### spring-service.xml整合service层

- 扫描service层的bean完成注入
- 把Impl实现方法注入到IOC容器，主要是给参数private BookMapper bookMapper 赋值
- 配置事务管理器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">
    <!-- 扫描service相关的bean -->
    <context:component-scan base-package="com.kuang.service" />
    <!--BookServiceImpl注入到IOC容器中-->
    <bean id="BookServiceImpl" class="com.kuang.service.BookServiceImpl">
        <property name="bookMapper" ref="bookMapper"/>
    </bean>
    <!-- 配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!-- 注入数据库连接池 -->
        <property name="dataSource" ref="dataSource" />
    </bean>
</beans>
```



---

## 三、Spring配置整合文件，applicationContext.xml

```xml 
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    <import resource="spring-dao.xml"/>
    <import resource="spring-service.xml"/>
    <import resource="spring-mvc.xml"/>
</beans>
```



---

## 四、补充service层的实现方法

```java
package com.kuang.service;
import com.kuang.dao.BookMapper;
import com.kuang.pojo.Books;
import java.util.List;

public class BookServiceImpl implements BookService {
   //调用dao层的操作，设置一个set接口，方便Spring管理
   private BookMapper bookMapper;
   public void setBookMapper(BookMapper bookMapper) {
      this.bookMapper = bookMapper;
   }
   public int addBook(Books book) {
      return bookMapper.addBook(book);
   }
   public int deleteBookById(int id) {
      return bookMapper.deleteBookById(id);
   }
   public int updateBook(Books books) {
      return bookMapper.updateBook(books);
   }
   public Books queryBookById(int id) {
      return bookMapper.queryBookById(id);
   }
   public List<Books> queryAllBook() {
      return bookMapper.queryAllBook();
   }
}
```



- //调用dao层的操作，设置一个set接口，方便Spring管理
     private BookMapper bookMapper;

在spring-service.xml中的

```xml
<!--BookServiceImpl注入到IOC容器中 主要是给参数private BookMapper bookMapper;赋值-->
<bean id="BookServiceImpl" class="com.kuang.service.BookServiceImpl">
    <property name="bookMapper" ref="bookMapper"/>
</bean>
```

## 五、补充controller层的方法

```java
package com.kuang.controller;

import com.kuang.pojo.Books;
import com.kuang.service.BookService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;

import java.util.List;

@Controller
@RequestMapping("/book")
public class BookController {
   @Autowired
   @Qualifier("BookServiceImpl")
   private BookService bookService;
   @RequestMapping("/allBook")
   public String list(Model model) {
      List<Books> list = bookService.queryAllBook();
      model.addAttribute("list", list);
      return "allBook";
   }

   @RequestMapping("/toAddBook")
   public String toAddPaper() {
      return "addBook";
   }
   @RequestMapping("/addBook")
   public String addPaper(Books books) {
      System.out.println(books);
      bookService.addBook(books);
      return "redirect:/book/allBook";
   }

   @RequestMapping("/toUpdateBook")
   public String toUpdateBook(Model model, int id) {
      Books books = bookService.queryBookById(id);
      System.out.println(books);
      model.addAttribute("book",books );
      return "updateBook";
   }
   @RequestMapping("/updateBook")
   public String updateBook(Model model, Books book) {
      System.out.println(book);
      bookService.updateBook(book);
      Books books = bookService.queryBookById(book.getBookID());
      model.addAttribute("books", books);
      return "redirect:/book/allBook";
   }

   @RequestMapping("/del/{bookId}")
   public String deleteBook(@PathVariable("bookId") int id) {
      bookService.deleteBookById(id);
      return "redirect:/book/allBook";
   }
}
```



## 六、总结

- **mybatis-config.xml** 完成dao层的mapper和mapper.xml的映射（扫描这个包并配置mapper）
- **spring-dao.xml** 帮助mybatis整合到spring中，主要完成获取数据库连接以及获取sqlSession对象并完成dao层mapper的自动注入。
- **spring-mvc.xml** 完成controller层的配置，扫描controller层的bean扫描，完成视图解析。
- **spring-service.xml** 把service层的方法注入到IOC容器中，完成配置事务管理。
- **web.xml** 完成DispatcherServlet的配置以及字符编码过滤（UTF-8）。
- **applicationContext.xml** 完成所有配置文件的整合引入。

