如何用Github搭建免费图床【详细图文步骤】

PicGo+Typora+Github图床配置步骤（一键上传本地图片）

-------

## 一、配置前的准备

首先你需要有一个**Github**账号【[GitHub](https://github.com/)】。

然后下载**PicGo**图片上传工具【[PicGo](https://picgo.github.io/PicGo-Doc/zh/)】和**Typora**编辑器【在我的公众号内（靠谱杨的挨踢生活）回复<**记事本**>免费获取已激活Win版】。

## 二、创建一个Github仓库

![image-20230316205457543](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316205457543.png)

## 三、创建一个Github授权码token

**切记，Token只能看到一次，复制之后要保存一下，之后还有用。**

![image-20230316205721360](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316205721360.png)

点击Settings设置，然后滑到最下面。

![image-20230316205805302](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316205805302.png)

然后点击Tokens

![image-20230316205832025](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316205832025.png)

![image-20230316205844541](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316205844541.png)

![image-20230316205854310](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316205854310.png)

![image-20230316205949804](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316205949804.png)

## 四、配置Typora

点击【文件】->【偏好设置】->【图像】

![image-20230316210346861](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316210346861.png)

## 五、配置PicGo

打开PicGo，选择Github图床设置。

![image-20230316210747496](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316210747496.png)

## 六、验证是否成功

到这里基本上整个配置流程就结束了，可以打开Typora测试一下。

【文件】->【偏好设置】>【图像】

![image-20230316211041813](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316211041813.png)

有时候这里测试会显示失败，那么可以去新建一个Typora文档，里面放一张图片。

右键单击选择**上传该图片**，如果几秒钟后，该图片路径变成如下图所示，那么说明你已经配置完成了，图片此时已经上传到了Github仓库。

![image-20230316211227719](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316211227719.png)

上传方式：

第一种，在单张图片上右键单击，选择【上传图片】，就可以把这张图片上传到Github仓库。

第二种，选择【格式】->【图像】->【上传所有本地图片】。

第三种，打开PicGo，首页会显示上传区域，在该处上传即可。

![image-20230316212003529](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230316212003529.png)

## 七、可能会遇到的问题

1、仓库需要设置为公有Public。

2、申请Token时，下面的框框都要勾选。

3、国内访问Github加载缓慢，可能会出现**仓库上传成功，但是图片显示不出的情况**。

4、关于其他问题，可以尝试重新操作一遍，细心一些应该可以成功，加油！

【补】第3个问题会显示<raw.githubusercontent.com无法访问>，我找到了一个解决办法，具体可以查看我的这篇**文章**：https://www.reliableyang.cn/1040.html