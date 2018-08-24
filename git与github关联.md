# Git 本地仓库与 Github 远程仓库关联

> 如果你已经在本地创建了一个 Git 仓库，又想在 GitHub 创建一个 Git 仓库，并且让这两个仓库进行远程同步，那就需要用到 SSH Key，github 拿到了你的公钥就会知道内容是你推送的。

## 1.SSH Key 的配置

**1.Windows 下打开 Git Bash，创建 SSH Key，按提示输入密码，可以不填密码一路回车**

~~~ 
ssh-keygen -t rsa -C "注册邮箱"
~~~

然后用户主目录 C:/Users/lizq/.ssh **(我的电脑)** 下有两个文件，id_rsa 是私钥，id_rsa.pub 是公钥

***

**2.获取 key，打开. ssh 下的 id_rsa.pub 文件，里面的内容就是 key 的内容**

~~~ 
start ~/.ssh/id_rsa.pub
~~~



**3.登录 GitHub，打开 "SSH Keys" 页面**

快捷地址：[https://github.com/settings/keys]()

![](https://images2015.cnblogs.com/blog/446475/201512/446475-20151207095523105-1244401158.jpg)

**4.测试 ssh key 是否成功**

使用命令 

~~~ ssh -T git@github.com
ssh -T git@github.com
~~~

如果出现 You’ve successfully authenticated, but GitHub does not provide shell access 。这就表示已成功连上 github。

***



## 2.远程库与本地库之间的操作

**1.从远程克隆一份到本地可以通过 git clone**

> Git 支持 HTTPS 和 SSH 协议

~~~
git clone https://github.com/lizq5021/git.git
~~~

**2.配置用户名和邮箱**

