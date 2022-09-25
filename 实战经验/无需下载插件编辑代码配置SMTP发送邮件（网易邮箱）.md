### 无需下载插件编辑代码配置SMTP发送邮件（网易邮箱）

（解决不能进入wordpress后台，忘记密码，找回密码时提示站点邮件配置失败的问题，配置好之后即可通过邮件重置wordpress密码）

---------

使用ssh工具远程连接自己的服务器（Xshell或者服务器自带的工具均可），cd切换到安装wordpress的目录，找到主题的目录。

比如：

![image-20220728230532036](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220728230532036.png)

**找到functions.php并编辑保存**

在文件最后一行添加如下内容【把里面相关的内容均改为你自己的】

```php
// 配置邮件
add_action('phpmailer_init', 'mail_smtp');
function mail_smtp( $phpmailer ) {
    $phpmailer->FromName = '靠谱杨'; // 发件人昵称
    $phpmailer->Host = 'smtp.163.com'; // 邮箱SMTP服务器
    $phpmailer->Port = 465; // SMTP端口，不需要改
    $phpmailer->Username = 'bitter_7@163.com'; // 邮箱账户
    $phpmailer->Password = 'JEXIHCTFNHRKMYWS'; // 此处填写邮箱生成的授权码，不是邮箱登录密码
    $phpmailer->From = 'bitter_7@163.com'; // 邮箱账户同上
    $phpmailer->SMTPAuth = true;
    $phpmailer->SMTPSecure = 'ssl'; // 端口25时 留空，465时 ssl，不需要改
    $phpmailer->IsSMTP();
}
```

