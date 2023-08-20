 解决raw.githubusercontent.com无法访问的问题（仓库已存储成功，picgo+github配置图床图片不显示）

**关于如何配置picgo+github图床参考我的这篇文章**：https://www.cnblogs.com/rainbow-1/p/17224212.html

----
**Typora**编辑器【在我的公众号内（靠谱杨的挨踢生活）回复<**记事本**>免费获取已激活Win版】
## 问题描述

GitHub图床仓库中已经上传成功图片，但是访问链接打不开，无法显示该图片，如图所示：

![image-20230315112835255](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230315112835255.png)

## 解决方法

### 1）https://www.ipaddress.com/

https://www.ipaddress.com/ 打开该网站搜索raw.githubusercontent.com，如图所示：

![image-20230315113022507](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230315113022507.png)

### 2）查看下面的IPV4和IPV6地址，根据个人情况选择一个

![image-20230315113117514](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230315113117514.png)

### 3）修改电脑hosts文件

**C:\Windows\System32\drivers\etc**

![image-20230315113400846](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230315113400846.png)

输入以下内容，如图所示：

**185.199.108.133  raw.githubusercontent.com**

![image-20230315112601236](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230315112601236.png)