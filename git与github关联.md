# Git 本地仓库与 Github 远程仓库关联

> 如果你已经在本地创建了一个 Git 仓库，又想在 GitHub 创建一个 Git 仓库，并且让这两个仓库进行远程同步，那就需要用到 SSH Key，github 拿到了你的公钥就会知道内容是你推送的。

## 1.SSH Key 的配置

**1.Windows 下打开 Git Bash，创建 SSH Key，按提示输入密码，可以不填密码一路回车**

~~~ 
ssh-keygen -t rsa -C "注册邮箱"
~~~

然后用户主目录 C:/Users/lizq/.ssh 下有两个文件，id_rsa 是私钥，id_rsa.pub 是公钥

***

**2.获取 key，打开. ssh 下的 id_rsa.pub 文件，里面的内容就是 key 的内容**



