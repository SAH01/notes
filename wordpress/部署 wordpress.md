# 一、wordpress部署

1. 安装centos图形化界面命令

> yum groupinstall "GNOME Desktop" "Graphical Administration Tools"
>
> ln -sf /lib/systemd/system/runlevel5.target /etc/systemd/system/default.target
>
> reboot
>
> 然后使用VNC连接！

```csharp
# 修改mysql密码
set password for root@localhost = password('reliable'); 
```

------------



**centos安装宝塔**：yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh

【其他Linux操作系统的安装命令】[https://www.bt.cn/btcode.html](https://link.zhihu.com/?target=https%3A//www.bt.cn/btcode.html)

> 外网面板地址: http://123.56.22.121:9876/860eeb4b
>
> 内网面板地址: http://172.17.163.92:9876/860eeb4b
>
> username: reliable
>
> password: success01



```
添加站点
reliableyang.cn
www.reliableyang.cn
123.56.22.121
```



> wordpress 用户名：admin
>
> wordpress 密码：YNBWg9rXfyNdhkJi)GQYkJW$
>
> SMTP网易邮箱2：JEXIHCTFNHRKMYWS
>
> **MySQL**
>
> reliable_cn	reliable_cn	AFMwSHmJrSFyFGLR
>
> **FTP**
>
> reliable_cn   PtBtZyE5G47tFyXL
>
> **宝塔网址**
>
> http://123.56.22.121:9876/ftp



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



> **新主题评论通知网易密码**：WKAIIUGTMEZTYFZA
>
> **outlook配置密码：** LJOSGGEXHJSQVWLA



**301跳转：**

```
https://www.reliableyang.cn/register
【https://www.reliableyang.cn/】

留言 https://www.reliableyang.cn/%E7%BB%99%E6%88%91%E7%95%99%E8%A8%80
【https://www.reliableyang.cn/】

留言 https://www.reliableyang.cn/%E7%95%99%E8%A8%80
【https://www.reliableyang.cn/】

https://www.reliableyang.cn/logout
【https://www.reliableyang.cn/wp-login.php?action=logout&redirect_to=https%3A%2F%2Fwww.reliableyang.cn&_wpnonce=1c584761e3】

隐私政策 https://www.reliableyang.cn/%E9%9A%90%E7%A7%81%E6%94%BF%E7%AD%96【https://www.reliableyang.cn/yszc】

https://www.reliableyang.cn/yang-blog
【https://www.reliableyang.cn/】
```



