保姆级本地maven安装配置步骤【Windows】

## 一、前期准备

1、首先需要安装并配置好本地JDK（WIN+R输入cmd，输入java -version如下图）

![image-20230316174551118](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316174551118.png)

2、下载maven到本地（链接[Maven – Download Apache Maven](https://maven.apache.org/download.cgi)）

![image-20230316174650627](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316174650627.png)

其他历史版本在这里找：[Index of /maven/maven-3 (apache.org)](https://dlcdn.apache.org/maven/maven-3/)

![image-20230316174755705](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316174755705.png)

## 二、解压缩并配置环境变量

### 1、解压maven压缩包到一个**不包含空格以及中文的路径下**。

![image-20230316174919632](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316174919632.png)

### 2、复制解压后的**maven的bin的目录**路径，添加到系统环境变量中。

![image-20230316175034798](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316175034798.png)

![image-20230316175052069](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316175052069.png)

![image-20230316175116092](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316175116092.png)

一路确定就可以。

### 3、验证是否配置成功。

win+R 运行cmd 输入 mvn -version，如图所示则成功！

![image-20230316175257519](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316175257519.png)

![image-20230316175313929](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316175313929.png)

## 三、修改配置文件

记事本打开settings.xml【D:\environment\apache-maven-3.6.1-bin\apache-maven-3.6.1\conf】

![image-20230316175409568](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316175409568.png)

### 修改配置文件

**修改三个地方：本地仓库，镜像仓库，本地JDK。**

#### 1、本地仓库需要先创建一个目录，如图：

![image-20230316175549601](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316175549601.png)

```xml
<localRepository>D:/environment/maven_local_repository</localRepository>
```

注意斜杠【/】

#### 2、添加镜像

```xml
   <mirrors>
		<mirror>
            <id>alimaven</id>
            <mirrorOf>central</mirrorOf>
            <name>aliyun maven</name>
            <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
        </mirror>
  </mirrors>
```

#### 3、添加本地JDK路径

```xml
   <profiles>
  <!-- java版本 --> 
		<profile>
		<!-- maven使用jdk1.8 --> 
			  <id>jdk-1.8</id>
			  <!-- 开启jdk --> 
			  <activation>
				<activeByDefault>true</activeByDefault>
				<jdk>1.8</jdk>
			  </activation>
			
			  <properties>
			  <!-- 配置编译器信息 -->
				<maven.compiler.source>1.8</maven.compiler.source>
				<maven.compiler.target>1.8</maven.compiler.target>
				<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
			  </properties>
		</profile>
	</profiles>
```

#### 4、测试是否配置成功

win+r输入cmd，在命令行输入**mvn help:system**测试，出现**aliyun**的字样则成功！

IDEA集成Maven步骤参考：https://www.cnblogs.com/rainbow-1/p/17223835.html