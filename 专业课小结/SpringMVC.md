## 01 初识SpringMVC

实现步骤其实非常的简单：

1. 新建一个web项目
2. 导入相关jar包
3. 编写web.xml , 注册DispatcherServlet
4. 编写springmvc配置文件
5. 接下来就是去创建对应的控制类 , controller
6. 最后完善前端视图和controller之间的对应
7. 测试运行调试

### 使用springMVC必须配置的三大件：

**处理器映射器、处理器适配器、视图解析器**

通常，我们只需要**手动配置视图解析器**，而**处理器映射器**和**处理器适配器**只需要开启**注解驱动**即可，而省去了大段的xml配置

### 注解实现SpringMVC

### 常见注解

```
@Component	组件
@Service	服务
@Controller	控制
@Respository	dao层
```

---

#### 控制器

```java
package com.kuang.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
//@Controller注解的类会自动添加到Spring上下文中
@Controller
@RequestMapping("/test2")
public class ControllerTest2{
	//映射访问路径
	@RequestMapping("/t2")
	public String index(Model model){
		//Spring MVC会自动实例化一个Model对象用于向视图中传值
		model.addAttribute("msg", "ControllerTest2");
		//返回视图位置
		return "test";
	}
}
```

- @Controller是为了让Spring IOC容器初始化时自动扫描到；
- @RequestMapping是为了映射请求路径，这里因为类与方法上都有映射所以访问时应该是/test2/t2；

### 标准maven依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.reliable</groupId>
    <artifactId>SpringMVC2</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>springmvc-04-controller</module>
    </modules>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
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
    </dependencies>
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
</project>
```



### 一、配置pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>SpringMVC2</artifactId>
        <groupId>com.reliable</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>springmvc-04-controller</artifactId>
    <dependencies>
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
    </dependencies>
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
</project>
```

### 二、配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--1.注册servlet-->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!-- 启动顺序，数字越小，启动越早 -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <!--所有请求都会被springmvc拦截 -->
    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

### 三、配置springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
    <context:component-scan base-package="com.kuang.controller"/>
    <!-- 让Spring MVC不处理静态资源：html 等-->
    <mvc:default-servlet-handler/>
    <!--
    支持mvc注解驱动
        在spring中一般采用@RequestMapping注解来完成映射关系
        要想使@RequestMapping注解生效
        必须向上下文中注册DefaultAnnotationHandlerMapping
        和一个AnnotationMethodHandlerAdapter实例
        这两个实例分别在类级别和方法级别处理。
        而annotation-driven配置帮助我们自动完成上述两个实例的注入。
		<mvc:annotation-driven /> 完成了映射和适配（支持用注解完成）
     -->
    <mvc:annotation-driven />
    <!-- 视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
          id="internalResourceViewResolver">
        <!-- 前缀 -->
        <property name="prefix" value="/WEB-INF/jsp/" />
        <!-- 后缀 -->
        <property name="suffix" value=".jsp" />
    </bean>
</beans>
```



---

## 02 RestFul 风格

**概念**
Restful就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。

**功能**
资源：互联网所有的事物都可以被抽象为资源
资源操作：使用POST、DELETE、PUT、GET，使用不同方法对资源进行操作。
分别对应 添加、 删除、修改、查询。

### RestFulController(@PathVariable)

```java
package com.kuang.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class RestFulController {
	//映射访问路径
	@RequestMapping("/commit/{p1}/{p2}")
	public String index(@PathVariable int p1, @PathVariable int p2, Model model){
		int result = p1+p2;
		//Spring MVC会自动实例化一个Model对象用于向视图中传值
		model.addAttribute("msg", "结果："+result);
		//返回视图位置
		return "test";
	}

	//映射访问路径,必须是Get请求
	@RequestMapping(value = "/hello",method = {RequestMethod.GET})
	public String index2(Model model){
		model.addAttribute("msg", "hello!");
		return "test";
	}
}
```



![image-20211127212117120](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20211127212117120.png)

----

### 使用method属性指定请求类型

用于约束请求的类型，可以收窄请求范围。指定请求谓词的类型如GET, POST, HEAD, OPTIONS, PUT, PATCH, DELETE, TRACE等。

```java
	//映射访问路径,必须是Get请求
	@RequestMapping(value = "/hello",method = {RequestMethod.GET})
	public String index2(Model model){
		model.addAttribute("msg", "hello!");
		return "test";
	}
```



**除了添加method，还可以使用注解**

@GetMapping
@PostMapping
@PutMapping
@DeleteMapping
@PatchMapping

```java
	//映射访问路径,必须是Get请求
	@GetMapping(value = "/hello")
	public String index2(Model model){
		model.addAttribute("msg", "hello!");
		return "test";
	}
```

**@GetMapping**
用于将HTTP GET请求映射到特定处理程序方法的注释。具体来说，@GetMapping是一个作为快捷方式的组合注释
@RequestMapping(method = RequestMethod.GET)。

**@PostMapping**
用于将HTTP POST请求映射到特定处理程序方法的注释。具体来说，@PostMapping是一个作为快捷方式的组合注释@RequestMapping(method = RequestMethod.POST)。

**@RequestMapping**
一般情况下都是用@RequestMapping（method=RequestMethod.），因为@RequestMapping可以直接替代以上两个注解，但是以上两个注解并不能替代@RequestMapping，@RequestMapping相当于以上两个注解的父类！

----



## 03 转发和重定向

### 视图解析器

```xml
 <!--视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
          id="internalResourceViewResolver">
        <!-- 前缀 -->
        <property name="prefix" value="/WEB-INF/jsp/" />
        <!-- 后缀 -->
        <property name="suffix" value=".jsp" />
    </bean>
```



### ResultSpringMVC2

```java
package com.kuang.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class ResultSpringMVC2 {
	@RequestMapping("/rsm2/t1")
	public String test1(){
		//转发
		return "test";
	}
	@RequestMapping("/rsm2/t2")
	public String test2(){
		//重定向
		return "redirect:/index.jsp";
	}
}
```



---

## 04 数据处理

**提交的域名称和处理方法的参数名不一致**

提交数据 : localhost:8080/hello?username=xxx	使用@RequestParam

```java
//@RequestParam("username") : username提交的域的名称 .
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name){
    System.out.println(name);
    return "hello";
}
```

```
Model 只有寥寥几个方法只适合用于储存数据，简化了新手对于Model对象的操作和理解；
ModelMap 继承了 LinkedMap ，除了实现了自身的一些方法，同样的继承 LinkedMap 的方法和特性；
ModelAndView 可以在储存数据的同时，可以进行设置返回的逻辑视图，进行控制展示层的跳转。
```



**Model**

```java
@RequestMapping("/ct2/hello")
public String hello(@RequestParam("username") String name, Model model){
    //封装要显示到视图中的数据
    //相当于req.setAttribute("name",name);
    model.addAttribute("msg",name);
    System.out.println(name);
    return "test";
}
```

**ModelMap**

```java
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name, ModelMap model){
    //封装要显示到视图中的数据
    //相当于req.setAttribute("name",name);
    model.addAttribute("name",name);
    System.out.println(name);
    return "hello";
}
```

**ModelAndView**

```java
public class ControllerTest1 implements Controller {
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        //返回一个模型视图对象
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","ControllerTest1");
        mv.setViewName("test");
        return mv;
    }
}
```



### 中文乱码

![image-20211128053007658](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20211128053007658.png)

![image-20211128053016204](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20211128053016204.png)

---------

### web.xml添加配置处理乱码

```xml
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```



---

## 05 Controller返回JSON数据

pom.xml导入Jackson

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.8</version>
</dependency>
```

- JSON 是 JavaScript 对象的字符串表示法，它使用文本表示一个 JS 对象的信息，本质是一个字符串。

```java
var obj = {a: 'Hello', b: 'World'}; //这是一个对象，注意键名也是可以使用引号包裹的
var json = '{"a": "Hello", "b": "World"}'; //这是一个 JSON 字符串，本质是一个字符串
```

- JSON 和 JavaScript 对象互转

1. JSON->JavaScript对象： JSON.parse() 

```java
var obj = JSON.parse('{"a": "Hello", "b": "World"}'); 
//结果是 {a: 'Hello', b: 'World'}
```

2. JavaScript 对象转换为JSON字符串： JSON.stringify()

```java
var json = JSON.stringify({a: 'Hello', b: 'World'});
//结果是 '{"a": "Hello", "b": "World"}'
```

### Controller案例

- 注意添加@ResponseBody注解
- 在类上直接使用 @RestController ，这样里面所有的方法都只会返回 json 字符串了，不用再每一个都添加 @ResponseBody

```java
package com.kuang.controller;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.kuang.pojo.User;
import com.kuang.utils.JsonUtils;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

import java.util.Date;

@RestController
public class UserController {
   @RequestMapping("/json1")
   @ResponseBody
   public String json1() throws JsonProcessingException {
      //创建一个jackson的对象映射器，用来解析数据
      ObjectMapper mapper = new ObjectMapper();
      //创建一个对象
      User user = new User("秦疆1号", 3, "男");
      //将我们的对象解析成为json格式
      String str = mapper.writeValueAsString(user);
      //由于@ResponseBody注解，这里会将str转成json格式返回；十分方便
      return str;
   }
   @RequestMapping("/json5")
   public String json5() throws JsonProcessingException {
      Date date = new Date();
      String json = JsonUtils.getJson(date);
      return json;
   }
}
```



---

## 06 SpringMVC需要的最简单的配置

### 首先web.xml配置DispacherServlet

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--1.注册servlet-->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:applicationContext.xml</param-value>
        </init-param>
        <!-- 启动顺序，数字越小，启动越早 -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <!--所有请求都会被springmvc拦截 -->
    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```

![image-20211208223339121](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20211208223339121.png)

### 其次配置applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--1.注册servlet-->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:applicationContext.xml</param-value>
        </init-param>
        <!-- 启动顺序，数字越小，启动越早 -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <!--所有请求都会被springmvc拦截 -->
    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```

![image-20211208223415778](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20211208223415778.png)

### 最后新建controller包，写相应的controller方法

![image-20211208223250551](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20211208223250551.png)



----



## 07 文件上传下载

### 上传

1. 导入依赖

```xml
<!--文件上传-->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.3</version>
</dependency>
```



2. 写一个上传的页面

```html
<form action="" enctype="multipart/form-data" method="post">
    <input type="file" name="file"/>
    <input type="submit">
</form>
```

3. 配置bean：multipartResolver

```xml
<!--文件上传配置-->
<bean id="multipartResolver"  class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <!-- 请求的编码格式，必须和jSP的pageEncoding属性一致，以便正确读取表单的内容，默认为ISO-8859-1 -->
    <property name="defaultEncoding" value="utf-8"/>
    <!-- 上传文件大小上限，单位为字节（10485760=10M） -->
    <property name="maxUploadSize" value="10485760"/>
    <property name="maxInMemorySize" value="40960"/>
</bean>
```



4. 写一个Controller

```java
package controller;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.multipart.commons.CommonsMultipartFile;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.*;
import java.net.URLEncoder;

@Controller
public class FileController {
	//@RequestParam("file") 将name=file控件得到的文件封装成CommonsMultipartFile 对象
	//批量上传CommonsMultipartFile则为数组即可
	@RequestMapping("/upload")
	public String fileUpload(@RequestParam("file") CommonsMultipartFile file , HttpServletRequest request) throws IOException {
		//获取文件名 : file.getOriginalFilename();
		String uploadFileName = file.getOriginalFilename();
		//如果文件名为空，直接回到首页！
		if ("".equals(uploadFileName)){
			return "redirect:/index.jsp";
		}
		System.out.println("上传文件名 : "+uploadFileName);
		//上传路径保存设置
		String path = request.getServletContext().getRealPath("/upload");
		//如果路径不存在，创建一个
		File realPath = new File(path);
		if (!realPath.exists()){
			realPath.mkdir();
		}
		System.out.println("上传文件保存地址："+realPath);
		InputStream is = file.getInputStream(); //文件输入流
		OutputStream os = new FileOutputStream(new File(realPath,uploadFileName)); //文件输出流
		//读取写出
		int len=0;
		byte[] buffer = new byte[1024];
		while ((len=is.read(buffer))!=-1){
			os.write(buffer,0,len);
			os.flush();
		}
		os.close();
		is.close();
		return "redirect:/index.jsp";
	}
	/*
	 * 采用file.Transto 来保存上传的文件
	 */
	@RequestMapping("/upload2")
	public String  fileUpload2(@RequestParam("file") CommonsMultipartFile file, HttpServletRequest request) throws IOException {
		//上传路径保存设置
		String path = request.getServletContext().getRealPath("/upload");
		File realPath = new File(path);
		if (!realPath.exists()){
			realPath.mkdir();
		}
		//上传文件地址
		System.out.println("上传文件保存地址："+realPath);
		//通过CommonsMultipartFile的方法直接写文件（注意这个时候）
		file.transferTo(new File(realPath +"/"+ file.getOriginalFilename()));
		return "redirect:/index.jsp";
	}
}
```



### 下载

页面

```html
<a href="/download">点击下载</a>
```

controller

```java
//下载
	@RequestMapping(value="/download")
	public String downloads(HttpServletResponse response , HttpServletRequest request) throws Exception{
		//要下载的图片地址
		String  path = request.getServletContext().getRealPath("/upload");
		String  fileName = "1632824890.jpg";
		//1、设置response 响应头
		response.reset(); //设置页面不缓存,清空buffer
		response.setCharacterEncoding("UTF-8"); //字符编码
		response.setContentType("multipart/form-data"); //二进制传输数据
		//设置响应头
		response.setHeader("Content-Disposition",
				"attachment;fileName="+ URLEncoder.encode(fileName, "UTF-8"));
		File file = new File(path,fileName);
		//2、 读取文件--输入流
		InputStream input=new FileInputStream(file);
		//3、 写出文件--输出流
		OutputStream out = response.getOutputStream();
		byte[] buff =new byte[1024];
		int index=0;
		//4、执行 写出操作
		while((index= input.read(buff))!= -1){
			out.write(buff, 0, index);
			out.flush();
		}
		out.close();
		input.close();
		return "ok";
	}
```



