wordpress自建博客站怎么在页脚添加网站总访问次数

笔者使用的主题是 GeneratePress 版本：3.1.3
----
**打开footer.php编辑**

```html
<div style="text-align: center;">
		<script async src="https://cdn.ly522.com/js/jilei.pure.mini.js"></script> 
		<span id="jilei_container_site_pv" style="color:#556ED3;">本站总访问量 
		<span id="jilei_value_site_pv"></span> 次</span> 
</div>
```

![image-20220727130916454](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220727130916454.png)