## wordpress搭建个人博客（阿里云服务器、宝塔面板、Elementor）

-------

笔记根据bilibili比得汪视频教程整理

https://www.bilibili.com/video/BV1Vg411w7os?p=18&spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=797a1ce924b64a3da083dec6dad97ed5

### 01 购买服务器

打开阿里云官网 https://www.aliyun.com/minisite/goods?taskPkg=amb618all&pkgSid=356704&recordId=3651197&userCode=nbq6yopo 选择购买ECS云服务器（**如果有备案域名需求，最好买包年包月的那种，三个月以上或者一年**）

==我买的是ECS.S6 年费==

![image-20221007160256754](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20221007160256754.png)

### 02 域名购买和解析

打开阿里云万网：https://wanwang.aliyun.com/?spm=5176.19720258.J_3207526240.53.18d976f400P8cg

选择喜欢的域名进行购买。

**解析**：打开你的域名控制台，解析时填入你的云服务器公网IP和你定义的域名

![image-20221007160305738](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20221007160305738.png)

### 03 安装宝塔

配置秘钥对远程连接你的云服务器（记得保存好生成的秘钥文件）

![image-20221007160321205](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20221007160321205.png)



```bash
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
```



【其他Linux操作系统的安装命令】https://www.bt.cn/btcode.html

### 04 宝塔安装套件

![image-20221007160330076](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20221007160330076.png)

### 05 创建站点

打开宝塔面板并登入，找到左侧创建站点！

![image-20221007160342374](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20221007160342374.png)

### 06 安装wordpress

到官网下载wordpress https://cn.wordpress.org/download/

打开宝塔面板选择文件上传这个压缩包并解压：

![image-20221007160353907](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20221007160353907.png)

### 07 安装一些插件

![image-20221007160408844](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20221007160408844.png)

### 08 网站备案

打开域名控制台选择ICP备案，之后按照流程去填！

![image-20220716200133011](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220716200133011.png)

![image-20221007160421913](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20221007160421913.png)

### 09 GeneratePress网站主题底部添加备案号

打开WordPress后台，点击左侧导航栏 —— 外观 —— 主题编辑器 —— 在右侧的文件列表，找到functions.php，拉到最底部粘贴代码，点击更新文件保存。

![image-20221007160432614](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20221007160432614.png)

