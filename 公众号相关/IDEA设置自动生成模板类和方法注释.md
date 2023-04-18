IDEA设置自动生成模板类和方法注释

# 一、模板类注释

![image-20230404203354442](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230404203354442.png)

在右侧粘贴如下代码：

```java
/**
*@BelongsProject: ${PROJECT_NAME}
*@BelongsPackage: ${PACKAGE_NAME}
*@Author: chuanwei.yang 42624
*@CreateTime: ${YEAR}-${MONTH}-${DAY}  ${HOUR}:${MINUTE}
*@Description: TODO
*@Version: 1.0
*/
```

![image-20230404203427185](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230404203427185.png)

# 二、模板方法注释

![image-20230404203529791](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230404203529791.png)

## 1、新建模板

如图**新建一个模板组名为userDefine**，选中新建好的这个组，在其目录下新建一个模板名为 * 。

![image-20230404203709217](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230404203709217.png)

![image-20230404203648862](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230404203648862.png)

## 2、在下方粘贴如下代码

```java
*
 * @description:
 * @author: chuanwei.yang 42624
 * @date: $date$ $time$
 * @param: $param$
 * @return: $return$
 **/
```

## 3、点击右侧编辑参数

如图所示：

![image-20230404203819548](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230404203819548.png)

![image-20230404203826371](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230404203826371.png)

## 4、为当前模板指定语言

![image-20230404204018176](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230404204018176.png)

选择**Java**：

![image-20230404204034499](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230404204034499.png)

![image-20230404204121766](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230404204121766.png)

# 三、实现效果

新建一个类：

![image-20230404204221400](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230404204221400.png)

在方法的上方输入 /** 并回车

![image-20230404204306529](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230404204306529.png)

# 四、补充

方法模板定义中的参数和返回值可以自定义效果：

**参数分行显示**

**返回值显示类型**

参数：

```java
groovyScript("def result=''; def params=\"${_1}\".replaceAll('[\\\\[|\\\\]|\\\\s]', '').split(',').toList(); for(i = 0; i < params.size(); i++) {result+='* @param: ' + params[i] + ((i < params.size() - 1) ? '\\n ' : '')};return result", methodParameters()) 
```

返回值：

```java
groovyScript("def returnType = \"${_1}\"; def result = '* @return: ' + returnType; return result;", methodReturnType());
```

![image-20230404210126625](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20230404210126625.png)