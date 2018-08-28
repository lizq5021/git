# 			服务器环境搭建

## 1.安装前准备

需要安装的软件

> 1. jdk1.7 
> 2. tomcat
> 3. mysql
> 4. nginx

开放常用端口号 执行命令 按 **i** 编辑

~~~shell
vim /etc/sysconfig/iptables
~~~

![](https://s1.ax1x.com/2018/08/27/PqD6yV.png)

添加如下

~~~ shell
-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 81 -j ACCEPT
~~~

![](https://s1.ax1x.com/2018/08/27/PqD2eU.png)

   

按**ESC**退出编辑模式 在按**shift+ZZ**保存

重启防火墙

~~~shell
service iptables restart
~~~

![](https://s1.ax1x.com/2018/08/28/PL6YRJ.png)

上传需要的软件

[软件下载地址](http://file.suhuanzhen.cn/)

> 使用wget命令

~~~shell
wget http://file.suhuanzhen.cn/apache-tomcat-7.0.90.tar.gz
~~~

> 1. 
>    创建一个目录存放软件

~~~shell
mkdir -p /www/dev
~~~

> 1. 进入该目录

~~~shell
cd /www/dev
~~~

> 1. 使用rz上传文件 提示命令不存在安装

~~~shell
yum -y install lrzsz
~~~

