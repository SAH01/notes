wordpress搭建个人博客（阿里云服务器、宝塔面板、Elementor）

<p class="md-end-block md-p"><span class="md-plain">笔记根据bilibili比得汪视频教程整理</span></p>
<p class="md-end-block md-p"><span class="md-link md-pair-s" spellcheck="false"><a href="https://www.bilibili.com/video/BV1Vg411w7os?p=18&amp;spm_id_from=333.1007.top_right_bar_window_history.content.click&amp;vd_source=797a1ce924b64a3da083dec6dad97ed5">https://www.bilibili.com/video/BV1Vg411w7os?p=18&amp;spm_id_from=333.1007.top_right_bar_window_history.content.click&amp;vd_source=797a1ce924b64a3da083dec6dad97ed5</a></span></p>

<h3 class="md-end-block md-heading"><span class="md-plain">01 购买服务器</span></h3>
<p class="md-end-block md-p"><span class="md-plain">打开阿里云官网 </span><span class="md-link md-pair-s" spellcheck="false"><a href="https://www.aliyun.com/minisite/goods?taskPkg=amb618all&amp;pkgSid=356704&amp;recordId=3651197&amp;userCode=nbq6yopo">https://www.aliyun.com/minisite/goods?taskPkg=amb618all&amp;pkgSid=356704&amp;recordId=3651197&amp;userCode=nbq6yopo</a></span><span class="md-plain"> 选择购买ECS云服务器（</span><span class="md-pair-s"><strong><span class="md-plain">如果有备案域名需求，最好买包年包月的那种，三个月以上或者一年</span></strong></span><span class="md-plain">）</span></p>
<p class="md-end-block md-p md-focus"><span class="md-pair-s md-expand"><mark><span class="md-plain">我买的是ECS.S6 年费</span></mark></span></p>
<p class="md-end-block md-p"><img class="alignnone size-full wp-image-323" src="http://123.56.22.121/wp-content/uploads/2022/07/image-20220716193357831.png" alt="" width="1171" height="598" /></p>

<h3 class="md-end-block md-heading"><span class="md-plain">02 域名购买和解析</span></h3>
<p class="md-end-block md-p"><span class="md-plain">打开阿里云万网：</span><span class="md-link md-pair-s" spellcheck="false"><a href="https://wanwang.aliyun.com/?spm=5176.19720258.J_3207526240.53.18d976f400P8cg">https://wanwang.aliyun.com/?spm=5176.19720258.J_3207526240.53.18d976f400P8cg</a></span></p>
<p class="md-end-block md-p"><span class="md-plain">选择喜欢的域名进行购买。</span></p>
<p class="md-end-block md-p"><span class="md-pair-s"><strong><span class="md-plain">解析</span></strong></span><span class="md-plain">：打开你的域名控制台，解析时填入你的云服务器公网IP和你定义的域名</span></p>
<p class="md-end-block md-p"><img class="alignnone size-full wp-image-324" src="http://123.56.22.121/wp-content/uploads/2022/07/image-20220716194112893.png" alt="域名管理" width="1035" height="418" /></p>

<h3 class="md-end-block md-heading"><span class="md-plain">03 安装宝塔</span></h3>
<p class="md-end-block md-p"><span class="md-plain">配置秘钥对远程连接你的云服务器（记得保存好生成的秘钥文件）</span></p>
<p class="md-end-block md-p"><span class="md-image md-img-loaded" data-src="https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220716194621072.png"><img class="alignnone size-full wp-image-325" src="http://123.56.22.121/wp-content/uploads/2022/07/image-20220716194621072.png" alt="配置秘钥对" width="702" height="352" /></span></p>

<pre class="md-fences md-end-block md-fences-with-lineno ty-contain-cm modeLoaded" lang="bash" spellcheck="false"> <span role="presentation">yum install <span class="cm-attribute">-y</span> <span class="cm-builtin">wget</span> &amp;&amp; <span class="cm-builtin">wget</span> <span class="cm-attribute">-O</span> install.sh http://download.bt.cn/install/install_6.0.sh &amp;&amp; <span class="cm-builtin">sh</span> install.sh</span></pre>
<p class="md-end-block md-p"><span class="md-plain">【其他Linux操作系统的安装命令】</span><span class="md-link md-pair-s" spellcheck="false"><a href="https://www.bt.cn/btcode.html">https://www.bt.cn/btcode.html</a></span></p>

<h3 class="md-end-block md-heading"><span class="md-plain">04 宝塔安装套件</span></h3>
<p class="md-end-block md-p"><img class="alignnone size-full wp-image-326" src="http://123.56.22.121/wp-content/uploads/2022/07/image-20220716194801910.png" alt="宝塔安装套件" width="934" height="486" /></p>

<h3 class="md-end-block md-heading"><span class="md-plain">05 创建站点</span></h3>
<p class="md-end-block md-p"><span class="md-plain">打开宝塔面板并登入，找到左侧创建站点！</span></p>
<p class="md-end-block md-p"><span class="md-image md-img-loaded" data-src="https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220716195216362.png"><img class="alignnone size-full wp-image-327" src="http://123.56.22.121/wp-content/uploads/2022/07/image-20220716195216362.png" alt="创建宝塔站点" width="1059" height="959" /></span></p>

<h3 class="md-end-block md-heading"><span class="md-plain">06 安装wordpress</span></h3>
<p class="md-end-block md-p"><span class="md-plain">到官网下载wordpress </span><span class="md-link md-pair-s" spellcheck="false"><a href="https://cn.wordpress.org/download/">https://cn.wordpress.org/download/</a></span></p>
<p class="md-end-block md-p"><span class="md-plain">打开宝塔面板选择文件上传这个压缩包并解压：</span></p>
<img class="size-full wp-image-330 aligncenter" src="http://123.56.22.121/wp-content/uploads/2022/07/image-20220716195808901.png" alt="" width="217" height="679" />
<h3 class="md-end-block md-heading"><span class="md-plain">07 安装一些插件</span></h3>
<p class="md-end-block md-p"><span class="md-image md-img-loaded" data-src="https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220716195542407.png"><img class="alignnone size-full wp-image-328" src="http://123.56.22.121/wp-content/uploads/2022/07/image-20220716195542407.png" alt="" width="1585" height="678" /></span></p>

<h3 class="md-end-block md-heading"><span class="md-plain">08 网站备案</span></h3>
<p class="md-end-block md-p"><span class="md-plain">打开域名控制台选择ICP备案，之后按照流程去填！</span></p>
<img class="alignnone size-full wp-image-331" src="http://123.56.22.121/wp-content/uploads/2022/07/image-20220716200133011.png" alt="ICP备案" width="904" height="111" />

<img class="alignnone size-full wp-image-332" src="http://123.56.22.121/wp-content/uploads/2022/07/image-20220716200311562.png" alt="ICP备案流程" width="963" height="733" />
<h3 class="md-end-block md-heading"><span class="md-plain">09 GeneratePress网站主题底部添加备案号</span></h3>
<p class="md-end-block md-p"><span class="md-plain">打开WordPress后台，点击左侧导航栏 —— 外观 —— 主题编辑器 —— 在右侧的文件列表，找到functions.php，拉到最底部粘贴代码，点击更新文件保存。</span></p>
<p class="md-end-block md-p"><span class="md-image md-img-loaded" data-src="https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220716200618265.png"><img class="alignnone size-full wp-image-333" src="http://123.56.22.121/wp-content/uploads/2022/07/image-20220716200618265.png" alt="主页底部添加备案号信息" width="1900" height="667" /></span></p>