IDEA常用设置（提高开发效率）

---

本人也是IDEA编译器的忠实用户了，但是有时出于各种原因，比如更换设备等等，IDEA总是需要重新安装配置。这就让我比较苦恼，因为总是记不全自己之前都修改了哪些地方（原谅脑子不好使hh），所以就以此篇文章记录一下目前我的IDEA的设置情况。可能依旧不太全（后续会持续修改更新），但至少比我用脑子记要好得多了。

以下内容是集大家之所长，但也有部分设置是个人习惯，各位视情况自取。

IDEA2018激活可参考：[Java编译器IntelliJ IDEA 2018.2.4激活【免费永久极简】2023年亲测有效 – 靠谱杨技术博客_靠谱杨技术博客 (reliableyang.cn)](https://www.reliableyang.cn/1015.html)

IDEA2020激活可参考：[IntelliJ IDEA2020.3激活【2023.04亲测有效】 – 靠谱杨技术博客_靠谱杨技术博客 (reliableyang.cn)](https://www.reliableyang.cn/1047.html)

# 1、基础设置

## 1.1、JDK配置

配置**JDK**安装路径。

File ➡ Project Structure

![image-20230520105803721](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520105803721.png)

## 1.2、maven配置

maven安装配置保姆级教学可参考：

[保姆级本地maven安装配置步骤【Windows】 – 靠谱杨技术博客_靠谱杨技术博客 (reliableyang.cn)](https://www.reliableyang.cn/1041.html)

[IntelliJ IDEA集成本地Maven步骤 – 靠谱杨技术博客_靠谱杨技术博客 (reliableyang.cn)](https://www.reliableyang.cn/1042.html)

File ➡ Setrings 搜索maven，如图所示：

![image-20230520110045515](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520110045515.png)

# 2、显示设置

## 2.1、主题设置

File ➡ Setrings 搜索**Theme**，如图所示：

![image-20230520111357836](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520111357836.png)

支持自定义主题(插件商店下载)，我使用的是**One Dark**这款，类似 VS Code 风格。

![image-20230520110512737](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520110512737.png)

One Dark 主题效果如图：

![image-20230520111058589](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520111058589.png)

## 2.2、字体样式设置

### 2.2.1、非代码字体

非代码字体样式设置：File ➡ Setrings 搜索 UI Options

![image-20230520111727947](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520111727947.png)

### 2.2.2、代码字体

代码字体样式设置：File-->Settings-->Editor-->**Font**

字体样式可以在**插件商店**自由定制，我使用的是**JetBrains Mono**字体主题。

**行间距建议1.2**，字体大小看个人习惯。

![image-20230520111919979](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520111919979.png)

### 2.2.3、控制台字体

控制台也是开发人员非常关注的一个界面了，控制台的样式设置如下：File-->Settings 搜索**Color Scheme**，选择Console Font。

如图所示：

![image-20230520112344630](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520112344630.png)

### 2.2.4、编码格式设置

File-->Settings 搜索 **File Encodings** 如图所示：

![image-20230520112956584](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520112956584.png)

着重介绍一下**properties files**编码设置，我们可以看到后面的复选框内容为：Transparent native-to-ascii conversion，直译过来就是，透明地将本地编码转换成ascii编码。解释一下这个选项的作用：

如果没有勾选，当我们以GBK模式编辑了一个.properties文件之后，其他人用UTF-8的编码打开，会出现**中文乱码**的状况。但是如果勾选了这个设置，就不会出现中文乱码的情况。

![image-20230520124527655](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520124527655.png)

### 2.2.5、鼠标悬浮设置

见名知意，当我们把鼠标悬停在某个字段上的时候，会出现相应的介绍信息，如下图鼠标悬浮在@Override：

![image-20230520124712245](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520124712245.png)

File-->Settings 搜索 **Show quick** 如图所示：

![image-20230520113308026](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520113308026.png)

### 2.2.6、忽略大小写提示

IDEA默认是区分大小写的，比如我想输入一个str就给我提示出String，但是默认的设置对首字母大写是敏感的，所以无法实现我想要的效果：

![image-20230520125534806](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520125534806.png)

File-->Settings 搜索 **Code Completion** 如图所示：

![image-20230520113540083](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520113540083.png)

设置之后的效果如下：当我输入str的时候系统会给我提示String，这会从一定程度上提高开发效率（当然这要看个人，如果习惯了切换大小写，这个设置可能就不太需要了）。

![image-20230520125658830](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520125658830.png)

### 2.2.7、设置方法间分割线

在我们写代码的时候会出现方法过多，看着眼花缭乱的情况，这一条细细的**分割线**会起到一定的辅助作用。

File-->Settings 搜索 **show method** 如图所示：

![image-20230520113813326](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520113813326.png)

实现效果如下：可以看到两个方法之间存在一条分割线。

![image-20230520113859712](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520113859712.png)

### 2.2.8、设置标签页多行显示

当我们打开多个tab标签的时候，会出现某些标签页藏在右侧不显示的情况，这时候我们去切换标签页就非常不方便。所以我们可以设置标签页多行显示，这样方便我们切换查看，方法如下：

File-->Settings 搜索 **Editor Tabs** 如图所示：

![image-20230520114123723](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520114123723.png)

多行显示标签效果如下：

![image-20230520130052341](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520130052341.png)

### 2.2.9、快捷键设置

快捷键是每个开发人员必不可少的一个设置了。

如果有之前习惯了eclipse开发的伙伴，可以直接选择设置**eclipse快捷键**。

File-->Settings 搜索 **Keymap** 如图所示：

![image-20230520114742237](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520114742237.png)

### 2.2.10、设置类、方法模板注释

在我们创建类或者方法时，可以快速生成注释信息，以便于开发人员后续查看，这同时也是一个良好的编程习惯，关于这部分设置我写过一篇文章做了详细介绍，可以参考下面的链接。

参考：[IDEA设置自动生成模板类和方法注释 – 靠谱杨技术博客_靠谱杨技术博客 (reliableyang.cn)](https://www.reliableyang.cn/1049.html)

![image-20230520114513580](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520114513580.png)

### 2.2.11、设置显示行号

这个设置就比较容易理解了，在代码页左侧显示行号。

File-->Settings 搜索 **line numbers** 如图所示：

![image-20230520115024099](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520115024099.png)

# 3、开发设置

## 3.1、设置自动导入

File-->Settings 搜索 **auto import** 如图所示：

Optimize imports on the fly：IntelliJ IDEA 将在我们书写代码的时候自动帮我们优化导入的包，比如自动去掉一些没有用到的包。

Add unambiguous imports on the fly： IntelliJ IDEA 将在我们书写代码的时候自动帮我们导入需要用到的包。但是对于那些同名的包，还是需要手动 Alt + Enter 进行导入的

![image-20230520130608059](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520130608059.png)

## 3.2、代码缩略图

打开IDEA设置Settings，选择Plugins，搜索**CodeGlance**插件。下载完成之后重启就完成了。

![image-20230520140016800](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520140016800.png)

设置完成后会在代码页右侧显示一列代码缩略图，方便开发者快速定位某段代码。

![image-20230520131441502](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520131441502.png)

## 3.3、设置自动编译



![image-20230520132238344](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520132238344.png)

然后按SHIFT+CTRL+A，输入registry后单击打开。

![image-20230520132443126](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520132443126.png)

之后找到compiler.automake.allow.when.app.running并将其选中。

![image-20230520132414004](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520132414004.png)

至此配置完毕，重启后生效。

## 3.4、彩虹括号

在开发过程中，会出现非常多个花括号嵌套的情况，那就可能会出现不知道哪个括号对着哪个的情况，尤其是在想删除一段代码的时候。这款插件就比较好的解决了这个问题。

在插件商店搜索Rainbow Brackets后安装。

![image-20230520140037007](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520140037007.png)

效果如下，每一对括号会显示为不同的颜色，方便区分。

![image-20230520140109155](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520140109155.png)

此外，当我们把光标定位到前一个括号后，使用快捷键**ALT+鼠标右键**可以只查看当前括号中的内容，按ESC取消，效果如下图：

![image-20230520140056183](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520140056183.png)

## 3.5、代码格式化插件

安装Eclipse Code Formatter格式化插件。

![image-20230520133225613](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520133225613.png)

下载格式化配置文件：

GoogleStyle：eclipse-java-google-style.xml

网上可能不太好找，可以关注我的订阅号【靠谱杨的挨踢生活】后台回复【代码格式】免费获取该文件。

文件配置如下图所示。

![image-20230520133418901](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520133418901.png)

使用快捷键Ctrl+Alt+L格式化代码。

![image-20230520133319368](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520133319368.png)

## 3.6、设置import分组、排序规则

![image-20230520134053850](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520134053850.png)

## 3.7、换行及Tab大小设置

换行设置建议120字符，Tab建议设置为4个空格。

![image-20230520134221438](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520134221438.png)

![image-20230520134438277](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230520134438277.png)



-----------



更多分享尽在我的订阅号:【靠谱杨的挨踢生活】

![img](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/o_220905015159_qrcode_for_gh_b43a6022f2e4_258.jpg)