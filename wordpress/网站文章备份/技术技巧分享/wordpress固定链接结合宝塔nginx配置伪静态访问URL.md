wordpress固定链接结合宝塔nginx配置伪静态访问URL

# 一、站点设置

![img](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/2090080-20220907222533810-1887068523.png)

 

**打开站点设置，选择伪静态，选择wordpress**

 

![img](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/2090080-20220907222620926-432976683.png)

 

#  二、wordpress设置

打开wordpress后台，选择**设置 ---》固定链接**

**![img](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/2090080-20220907223026389-2082424293.png)**

 

选择一个你喜欢的格式点击保存

 

![img](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/2090080-20220907223102055-1412414145.png)

之后打开你的文章就可以看到这样的链接格式：

![img](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/2090080-20220907223233265-2040435099.png)

https://www.reliableyang.cn/778.html

 

**如果发现出现了404，那可以尝试下面这个方法：**

# 方法2 直接修改nginx配置文件

在如图所示的位置插入下面的代码：注意要放在最后一个花括号里面

```php
1      location / {
2     try_files $uri $uri/ /index.php?$args;
3 }
4  
5     # Add trailing slash to */wp-admin requests.
6     rewrite /wp-admin$ $scheme://$host$uri/ permanent;
```

 

![img](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/2090080-20220907223433149-1137804618.png)

 