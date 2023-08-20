windows系统设置状态栏时间显示读秒

---

要实现的效果如下图：

![image-20230413103944689](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230413103944689.png)

## 一、打开注册表

WIN+R输入【cmd】之后输入【regedit】回车

![image-20230413103101943](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230413103101943.png)

![image-20230413103120857](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230413103120857.png)

## 二、修改注册表

在注册表地址栏输入：

`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced`

![image-20230413103243002](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230413103243002.png)

在该目录【Advanced】下，右侧列表中寻找【ShowSecondsInSystemClock】。

如果没有找到，在右侧空白处右键单击【新建】，选择【DWORD(32位)值(D)】，创建名为【ShowSecondsInSystemClock】的值，之后右键单击修改，将数值改为【1】即可（修改为0，即不显示读秒）。**最后重启资源管理器或电脑完成配置。**

![image-20230413103512822](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230413103512822.png)

![image-20230413103739104](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230413103739104.png)

## 三、补充

如果【WIN11】完成以上步骤后时间仍然没有显示读秒，可以选择下载工具【StartAllBack】

下载地址：https://www.xitongzhijia.net/soft/228742.html

**也可关注我的订阅号【靠谱杨的挨踢生活】回复【StartAllBack】获取。**

下载完成后解压，双击安装即可。

![image-20230413104234023](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230413104234023.png)

**工具简介**：StartAllBack是一款Win11开始菜单任务栏增强工具，可以在保留Win11系统的功能和外观等基础上对其进行修改，让其回归到Win7或者是Win10系统的样式，同时软件还可以自定义外观、任务栏、资源管理器等等，并且其体积是非常迷你的，在后台上运行完全不会影响让电脑在使用过程中变的卡顿，是一款几乎不占电脑内存的软件哦。

**使用方式**：在桌面下方状态栏右键单击，选择**属性**即可定制样式。

![image-20230413104435520](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230413104435520.png)