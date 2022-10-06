给你的wordpress添加文章内图片鼠标点击放大浏览的功能吧~

注：笔者已启用**WP Githuber MD**插件使用Markdown语法进行文章编辑，启用的主题为**generatepress**。

----

### 1、进入你的宝塔面板首页

点击文件选项：
![](http://123.56.22.121/wp-content/uploads/2022/07/image-20220718180321560.png)


### 2、分别找到以下几个文件进行修改

#### 2.1、header.php

```php
<!-- 图片放大 -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/fancyapps/fancybox@3.5.7/dist/jquery.fancybox.min.css" />
```

#### 2.2、footer.php

```php
<!-- 图片放大 -->
<script src="https://cdn.jsdelivr.net/gh/fancyapps/fancybox@3.5.7/dist/jquery.fancybox.min.js"></script>
```

#### 2.3、functions.php

```php
/**图片灯箱自动给图片加链接**/
add_filter('the_content', 'fancybox');
function fancybox($content){ 
    $pattern = array("/<img(.*?)src=('|\")([^>]*).(bmp|gif|jpeg|jpg|png|swf)('|\")(.*?)>/i","/<a(.*?)href=('|\")([^>]*).(bmp|gif|jpeg|jpg|png|swf)('|\")(.*?)>(.*?)<\/a>/i");
    $replacement = array('<a$1href=$2$3.$4$5 data-fancybox="gallery"><img$1src=$2$3.$4$5$6></a>','<a$1href=$2$3.$4$5 data-fancybox="images"$6>$7</a>');
    $content = preg_replace($pattern, $replacement, $content);
    return $content;
}
```

保存退出之后再次点击查看你的文章会出现如下效果：
![](http://123.56.22.121/wp-content/uploads/2022/07/image-20220718180652897.png)