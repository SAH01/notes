IntelliJ IDEA集成本地Maven步骤

## 一、前期准备

Maven已经在本地环境配置完成，步骤可以参考我的这篇文章：

https://www.cnblogs.com/rainbow-1/p/17223811.html

## 二、IDEA maven设置路径和参数

### 1、设置路径

![image-20230316185724144](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316185724144.png)

![image-20230316185934022](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316185934022.png)

### 2、配置参数

等同执行mvn archetype:generate命令，该命令执行时，需要指定一个archetype-catalog.xml文件，该命令的参数-DarchetypeCatalog，可选值为：remote，internal ，local等，用来指定archetype-catalog.xml文件从哪里获取。默认为remote，即从 http://repo1.maven.org/maven2/archetype-catalog.xml路径下载archetype-catalog.xml文件。http://repo1.maven.org/maven2/archetype-catalog.xml 文件约为3-4M，下载速度很慢，导致创建过程卡住。所以改成了internal使用内置的xml配置文件，这样一下子就能创建项目。
参考原文：https://blog.csdn.net/weixin_38568503/article/details/121045947

```xml
-DarchetypeCatalog=internal
```

![image-20230316190050089](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316190050089.png)