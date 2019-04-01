

# Git 本地仓库与 Github 远程仓库关联

> 如果你已经在本地创建了一个 Git 仓库，又想在 GitHub 创建一个 Git 仓库，并且让这两个仓库进行远程同步，那就需要用到 SSH Key，github 拿到了你的公钥就会知道内容是你推送的。

## 1.SSH Key 的配置

**1.Windows 下打开 Git Bash，创建 SSH Key，按提示输入密码，可以不填密码一路回车**

~~~ nginx
ssh-keygen -t rsa -C "注册邮箱"
~~~

然后用户主目录 C:/Users/lizq/.ssh **(我的电脑)** 下有两个文件，id_rsa 是私钥，id_rsa.pub 是公钥

***

**2.获取 key，打开. ssh 下的 id_rsa.pub 文件，里面的内容就是 key 的内容**

~~~ nginx
start ~/.ssh/id_rsa.pub
~~~



**3.登录 GitHub，打开 "SSH Keys" 页面**

快捷地址：<https://github.com/settings/keys>

![](https://images2015.cnblogs.com/blog/446475/201512/446475-20151207095523105-1244401158.jpg)

**4.测试 ssh key 是否成功**

使用命令 

~~~ nginx
ssh -T git@github.com
~~~

如果出现 You’ve successfully authenticated, but GitHub does not provide shell access 。这就表示已成功连上 github。

***



## 2.远程库与本地库之间的操作

**1.从远程克隆一份到本地可以通过 git clone**

> Git 支持 HTTPS 和 SSH 协议

~~~nginx
git clone https://github.com/lizq5021/git.git
~~~

**2.配置用户名和邮箱**

首先配置自己的身份，这样在提交代码的时候就能知道是谁提交的

~~~nginx
#设置用户名
git config --global user.name "用户名"
~~~

~~~nginx
#设置邮箱
git config --global user.email "邮箱地址"
~~~

查看

~~~nginx
#查看用户名
git config --global user.name
#查看邮箱
git config --global user.email
~~~

还可以使用git客户端配置

![](https://s1.ax1x.com/2018/08/24/P7zXLV.png)



**3.添加文件到版本库**

新建 a.txt

~~~ nginx
git pull origin master #拉取最新

git add a.txt  #添加文件到版本库（只是添加到缓存区），.代表添加文件夹下所有文件

git commit -m "first commit"  #把添加的文件提交到版本库，并填写提交备注

~~~

> 到目前为止，我们完成了代码库的初始化，但代码是在本地，还没有提交到远程服务器，所以关键的来了，要提交到就远程代码服务器，进行以下两步

~~~ nginx
git remote add origin https://github.com/lizq5021/git.git  #把本地库与远程库关联
git push -u origin master    #第一次推送时
git push origin master  #第一次推送后，直接使用该命令即可推送修改
~~~

把本地库的内容推送到远程。使用 git push 命令，实际上是把当前分支 master 推送到远程。

~~~nginx
git remote rm origin	#先删除远程 Git 仓库
git remote add origin https://github.com/lizq5021/download-demo.git
##===========================
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/lizq5021/download-demo.git
git push -u origin master
~~~

