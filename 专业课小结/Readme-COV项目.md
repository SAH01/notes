![image-20220325125108542](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220325125108542.png)



---



![image-20220325125328514](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220325125328514.png)



---



![image-20220325125438208](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220325125438208.png)

![image-20220325125448679](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220325125448679.png)

![image-20220325125500930](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220325125500930.png)

![image-20220325125515031](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220325125515031.png)

![image-20220325125533840](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220325125533840.png)

![image-20220325125554481](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220325125554481.png)



----

单击**左上角按钮**可以切换中国和世界展示界面！

![image-20220325125639669](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220325125639669.png)

![image-20220325125654250](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220325125654250.png)

![image-20220325125706991](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220325125706991.png)

![image-20220325125738593](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220325125738593.png)





-----



## 项目简介：

项目分为两个板块：

- 一个是中国的疫情数据可视化，包括全国累计趋势折线图、全国新增趋势折线图、全国累计数据、全国疫情地图、非湖北城市TOP5柱状图、**全国地图实现省市下钻数据展示**。
- 另一个是世界疫情数据可视化，包括世界累计数据、疫情趋势折线图、世界地图可视化展示、**实现了地图和表格展示图表联动**（鼠标放在地图或表格上对应部分高亮显示）。

## 代码结构：

1. sql下是后台数据库备份文件
2. 下面四个文件是静态资源

![image-20220325130529748](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220325130529748.png)

---

![image-20220325130620815](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220325130620815.png)

3. china对应中国疫情板块、world对应世界疫情板块。
4. 下面的app.py是flask框架的主要实现部分。
5. spider.py是爬虫文件，主要负责爬取疫情数据（来自腾讯）。
6. utils.py实现前后端数据交互（mysql数据库传送数据到前台页面展示）。

