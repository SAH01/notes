python打包Windows.exe程序（pyinstaller）

---

## 基础命令

`pip install pyinstaller` 使用pip命令来安装pyinstaller模块。

-F：  `pyinstaller -F hello.py -p hello2.py`

-D：  `pyinstaller -D hello.py -p hello2.py`

-i  ：  `pyinstaller -i tb.ico -F hello.py -p hello2.py`

> 其中前一个文件hello是主文件，后一个文件是会被调用到的文件，可以有多个。

| 参数      | 描述                                           |
| --------- | ---------------------------------------------- |
| -F        | 生成一个可执行文件                             |
| -D        | 生成一个目录（包含多个文件）作为可执行文件     |
| -w        | 运行exe时，不显示命令行窗口（仅对Windows有效） |
| -i        | 该参数后跟可执行文件的icon图标路径             |
| –distpath | 该参数后跟可执行文件的路径                     |
| -n        | 该参数后跟可执行文件的新名字                   |

## -F和-D参数的区别

-F 是指生成**单个**可执行的.exe文件，

-D 是指把.exe需要的资源和这个文件放在一起，见下图：

![image-20230709143952223](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230709143952223.png)

## ![image-20230709144100237](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230709144100237.png)-i 参数

`pyinstaller -i tb.ico -F hello.py -p hello2.py`

![image-20230709145022512](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230709145022512.png)

可以看到，用这个命令生成的.exe文件的图标就已经变了。

![image-20230709145046334](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230709145046334.png)

## 生成的文件目录

![image-20230709144512648](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230709144512648.png)