# git版本控制常用命令

## 1、配置身份信息

![image-20220426195122605](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220426195122605.png)

```bash
git config --global user.name "kuangshen"  #名称
git config --global user.email 24736743@qq.com   #邮箱
```

## 2、提交

```csharp

#查看指定文件状态
git status [filename]

#查看所有文件状态
git status

# git add .                  添加所有文件到暂存区
# git commit -m "消息内容"    提交暂存区中的内容到本地仓库 -m 提交信息
    
```

## 3、设置公钥

![image-20220426195356327](https://gitee.com/yang-chuanwei/typora-img/raw/master/img/image-20220426195356327.png)

```bash
# 进入 C:\Users\Administrator\.ssh 目录# 生成公钥ssh-keygen
git clone [远程仓库链接]	#复制生成的文件到本地项目 之后使用IDE操作push到远程仓库
git push	# 
```

## 4、版本切换

```bash
git log
#以只显示一行的方式来输出提交历史
git log --oneline
#查看所有版本历史
git reflog
#基于索引值切换版本
git reflog
git reset --hard [索引]

```



