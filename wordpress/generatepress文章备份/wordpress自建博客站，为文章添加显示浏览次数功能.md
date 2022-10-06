# wordpress自建博客站，为文章添加显示浏览次数功能

笔者使用的主题是 GeneratePress 版本：3.1.3
----
## 1、后台文章管理列表添加浏览次数列

	效果如图：

![image-20220727125437290](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220727125437290.png)

	实现：

**编辑functions.php文件，在末尾添加如下代码**

```php
//在后台文章列表增加一列数据
add_filter( 'manage_posts_columns', 'customer_posts_columns' );
    function customer_posts_columns( $columns ) {
    $columns['views'] = '浏览次数';
    return $columns;
}
```

## 2、文章详情页显示阅读数

	效果如图：

![image-20220727125713293](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220727125713293.png)

安装插件：**WP-PostViews**	并启用



![image-20220727125911906](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220727125911906.png)

打开设置 -》 浏览次数

==模板即是会展示的样式代码，下面的 the_views( ) 放在你需要的位置就可以展示模板对应的内容==

![image-20220727130013134](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220727130013134.png)



==编辑 content-single.php 文件，在headers标签里添加 the_views( ) 函数即可==

不同的主题可能添加的位置不一样，但是道理是相通的。

![image-20220727130141094](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220727130141094.png)