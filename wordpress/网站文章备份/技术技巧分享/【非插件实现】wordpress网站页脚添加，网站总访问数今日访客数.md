【非插件实现】wordpress网站页脚添加，网站总访问数/今日访客数

```php
/**
* 统计全站总访问量/今日总访问量/当前是第几个访客
* @return [type] [description]
*/
function wb_site_count_user(){
$addnum = 1; //初始化访问人数
session_start();
$date = date('ymd',time());
if(!isset($_SESSION['wb_'.$date]) && !$_SESSION['wb_'.$date]){
$count = get_option('site_count');
if(!$count || !is_array($count)){
$newcount = array(
'all' => 0,
'date' => $date,
'today' => $addnum
);
update_option( 'site_count', $newcount );
}else{
$newcount = array(
'all' => ($count['all']+$addnum),
'date' => $date,
'today' => ($count['date'] == $date) ? ($count['today']+$addnum) : $addnum
);
update_option( 'site_count', $newcount );
}
$_SESSION['wb_'.$date] = $newcount['today'];
}
return;
}
add_action('init', 'wb_site_count_user');
//输出访问统计
function wb_echo_site_count(){
session_start();
$sitecount = get_option('site_count');
$date = date('ymd',time());
echo '<p>总访问量：<span style="color:#7df1ff">'.absint($sitecount['all']).'</span>    今日访问量：<span style="color:#7df1ff">'.absint($sitecount['today']).'</span>    您是今天第：<span style="color:#7df1ff">'.absint($_SESSION['wb_'.$date]).'</span> 位访问者</p>';
}
```
在function文件中插入以上代码

在footer文件中写入以下代码

```
<div style="text-align: center;background:#000;color:#FFF"> <?php wb_echo_site_count(); ?> </div>
```

效果如下：

![img](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/202301051505251.png)

参考自：https://www.laoliang.net/jsjh/technology/9383.html