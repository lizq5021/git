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
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
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

> 使用wget命令在服务器下载

~~~shell
wget http://file.suhuanzhen.cn/apache-tomcat-7.0.90.tar.gz
~~~

> 1.创建一个目录存放软件

~~~shell
mkdir -p /www/dev
~~~

> 2.进入该目录

~~~shell
cd /www/dev
~~~

> 3.使用rz上传文件 提示命令不存在安装

~~~shell
yum -y install lrzsz
~~~

> 4.输入rz选择要上传的文件



## 2.查看系统是否安装过这些软件

### 2.1.检查是否安装过jdk

> 1.命令

~~~shell
rpm -qa | grep java
~~~

![](https://s1.ax1x.com/2018/08/28/PL7vvV.png)

> 2.卸载软件

~~~shell
rpm -e --nodeps java-1.7.0-openjdk-1.7.0.45-2.4.3.3.el6.x86_64
rpm -e --nodeps java-1.6.0-openjdk-1.6.0.0-1.66.1.13.0.el6.x86_64
~~~

#### 2.1.1.查询是否还有残余文件

> 命令

~~~shell
find / -name java
~~~

![](https://s1.ax1x.com/2018/08/28/PLHeKK.png)

> 删除

~~~shell
rm –rf #文件名
#终极命令
find / -name java | xargs rm -rf 
~~~

### 2.2.检查mysql安装包

> 命令

~~~shell
rpm -qa | grep mysql
~~~

![](https://s1.ax1x.com/2018/08/28/PLH1PA.png)

> 卸载

~~~shell
rpm –e --nodeps #文件名
~~~

#### 2.2.1查看残余文件

> 命令

~~~shell
find / -name mysql
~~~

![](https://s1.ax1x.com/2018/08/28/PLHsx0.png)

#### 2.2.2删除残余文件

> 命令

~~~shell
find / -name mysql | xargs rm -rf
~~~

### 2.3.nginx检查

> 命令

~~~shell
rpm -qa | grep nginx
~~~

![](https://s1.ax1x.com/2018/08/28/PLqlct.png)

> 卸载

~~~shell
rpm -e --nodeps #文件名
~~~

#### 2.3.1查看是否有残余文件

> 命令

~~~shell
find / -name nginx
~~~

![](https://s1.ax1x.com/2018/08/28/PLqHED.png)

#### 2.3.2删除残余文件

> 命令

~~~shell
find / -name nginx | xargs rm -rf
~~~

## 3.安装jdk

> 进入软件存放目录

~~~shell
cd /www/dev/
~~~

![](https://s1.ax1x.com/2018/08/28/PLLIRs.png)

> 解压jdk

~~~shell
tar -zxvf jdk1.7.0_80.tar -C /usr/
~~~

> 配置环境变量

~~~shell
vim /etc/profile
~~~

> 在文件顶端输入如下内容

~~~shell
export JAVA_HOME= /usr/jdk1.7.0_80  #jdk安装目录  
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export  PATH=${JAVA_HOME}/bin:$PATH
~~~

![](https://s1.ax1x.com/2018/08/28/PLONSs.png)

> 保存文件
>
> 执行 profile 文件

~~~shell
source /etc/profile
~~~

> 查看是否安装成功

~~~shell
java -version
~~~

![](https://s1.ax1x.com/2018/08/28/PLOyY4.png)

> 安装成功

## 4.安装tomcat

### 4.1安装

> 进入软件存放目录

~~~shell
cd /www/dev
~~~

> 解压

~~~shell
tar -zxvf apache-tomcat-7.0.90.tar.gz -C /usr/local/
~~~

> 进入解压后的目录

~~~shell
tar -zxvf apache-tomcat-7.0.90.tar.gz -C /usr/local/
~~~

> 启动tomcat

~~~shell
./startup.sh
~~~

> 访问 

http://172.23.140.32:8080/

![](https://s1.ax1x.com/2018/08/28/PLvVFU.png)

> 成功

> 关闭命令

~~~shell
./shutdown.sh
~~~

### 4.2开机启动

> 进入下面目录

~~~shell
cd /etc/init.d/
~~~

> 创建文件

~~~shell
touch tomcat
~~~

> 编辑

~~~shell
vim tomcat
~~~

>添加已下内容

~~~shell
#!/bin/bash  
#  
# tomcat startup script for the Tomcat server  
#  
# chkconfig: 345 80 20  
# description: start the tomcat deamon  
#  
# Source function library  
. /etc/rc.d/init.d/functions  
   
prog=tomcat
JAVA_HOME=/usr/jdk1.7.0_80  #jdk路径
export JAVA_HOME  
CATALANA_HOME=/usr/local/apache-tomcat-7.0.90  #tomcat路径
export CATALINA_HOME  
   
case "$1" in  
start)  
    echo "Starting Tomcat..."  
    $CATALANA_HOME/bin/startup.sh  
	echo "Starting Tomcat ok"
    ;;  
   
stop)  
    echo "Stopping Tomcat..."  
    $CATALANA_HOME/bin/shutdown.sh  
	echo "Stopping Tomcat ok"
    ;;  
   
restart)  
    echo "Stopping Tomcat..."  
    $CATALANA_HOME/bin/shutdown.sh  
    sleep 2  
    echo  
    echo "Starting Tomcat..."  
$CATALANA_HOME/bin/startup.sh  
echo "Starting Tomcat ok"
    ;;  
   
*)  
    echo "Usage: $prog {start|stop|restart}"  
    ;;  
esac  
exit 0
~~~

> 保存文件

> 给文件添加可执行权限

~~~shell
chmod 755 tomcat
~~~

> 把tomcat添加到系统服务

~~~shell
chkconfig --add tomcat
~~~

> 开启服务

~~~shell
chkconfig tomcat on
~~~

>重启系统查看配置是否成功

~~~shell
reboot
~~~

>执行命令看端口号

~~~shell
netstat -na | grep 8080
~~~

![](https://s1.ax1x.com/2018/08/28/PLxX8g.png)

> 或者执行此命令

~~~shell
ps -ef | grep java
~~~

![](https://s1.ax1x.com/2018/08/28/PLxvvj.png)

> 开机启动成功

### 4.3操作命令

~~~shell
#启动命令
service tomcat start
#重启命令
service tomcat restart
#停止命令
service tomcat stop
~~~

> tomcat安装完成

## 5.mysql安装

> 安装mysql客户端

~~~shell
yum -y install mysql-server
yum -y install mysql-devel
~~~

> 启动mysql服务

~~~shell
service mysqld start
~~~

> 设置开机启动

~~~shell
chkconfig --add mysqld
chkconfig mysqld on
~~~

> 登录mysql

~~~shell
mysql -u root –p
~~~

![](https://s1.ax1x.com/2018/08/28/PLzrRg.png)

>第一次登录不需要密码回车就行
>
>设置密码

~~~shell
use mysql;
#设置密码
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123456');
flush privileges;
#退出重新登录
exit
~~~

### 5.1开启远程访问

>登录mysql

~~~shell
mysql -u root -p
~~~

~~~shell
use mysql;
#进行授权操作
grant all privileges on *.*  to 'root'@'%' identified by '123456' with grant option;
#重载授权表
flush privileges;
#退出mysql
exit
~~~

### 5.2测试远程访问

> 如图

![](https://s1.ax1x.com/2018/08/28/PLzHy9.png)

![](https://s1.ax1x.com/2018/08/28/PLzvFK.png)

> 报错 解决办法

~~~shell
#打开文件
vim /etc/my.cnf
~~~

看看是否有绑定本地回环地址的配置，如果有，注释掉下面这段文字：（在文字之前加上 #号即可） 

bind-address = 127.0.0.1

![](https://s1.ax1x.com/2018/08/28/POSVFf.png)

然后找到 [mysqld] 部分的参数，在配置后面建立一个新行，添加下面这个参数：
skip-name-resolve

![](https://s1.ax1x.com/2018/08/28/POSYfU.png)

> 保存文件并重启 mysql

~~~shell
service mysqld restart
~~~

> 再次测试

![](https://s1.ax1x.com/2018/08/28/POS6fO.png)

> 连接成功

------

## 6.安装nginx

### 6.1.安装

> 在安装 nginx 前首先要确认系统中安装了 gcc、pcre-devel、zlib-devel、openssl-deve

~~~shell
#一键安装
yum -y install gcc pcre-devel zlib-devel openssl openssl-devel
~~~

> 进入软件目录

~~~shell
cd /www/dev
~~~

> 解压nginx到当前目录

~~~shell
tar -zxvf nginx-1.13.10.tar.gz
~~~

> 进入解压目录

~~~shell
cd /www/dev/nginx-1.13.10
~~~

> 编译

~~~shell
./configure --prefix=/usr/local/nginx
~~~

~~~shell
#执行make命令
make
#执行make install命令
make install
~~~

### 6.2测试是否安装成功

~~~shell
#进入nginx目录
cd /usr/local/nginx/sbin/
#启动nginx
./nginx
~~~

> 输入服务器ip访问，看见下面页面证明安装成功

![](https://s1.ax1x.com/2018/08/28/PO9kPf.png)

### 6.3配置开机启动

~~~shell
#进入目录
cd /etc/init.d
#新建文件
touch nginx
#编辑文件
vim nginx
~~~

> 添加如下内容

~~~shell
#!/bin/sh
#
# nginx - this script starts and stops the nginx daemon
#
# chkconfig:   - 85 15
# description:  NGINX is an HTTP(S) server, HTTP(S) reverse \
#               proxy and IMAP/POP3 proxy server
# processname: nginx
# config:      /etc/nginx/nginx.conf
# config:      /etc/sysconfig/nginx
# pidfile:     /var/run/nginx.pid
# Source function library.
. /etc/rc.d/init.d/functions
# Source networking configuration.
. /etc/sysconfig/network
# Check that networking is up.
[ "$NETWORKING" = "no" ] && exit 0
nginx="/usr/local/nginx/sbin/nginx"  #nginx执行路径
prog=$(basename $nginx)
NGINX_CONF_FILE="/usr/local/nginx/conf/nginx.conf"  #nginx配置文件路径
[ -f /etc/sysconfig/nginx ] && . /etc/sysconfig/nginx
lockfile=/var/lock/subsys/nginx
make_dirs() {
   # make required directories
   user=`$nginx -V 2>&1 | grep "configure arguments:" | sed 's/[^*]*--user=\([^ ]*\).*/\1/g' -`
   if [ -z "`grep $user /etc/passwd`" ]; then
       useradd -M -s /bin/nologin $user
   fi
   options=`$nginx -V 2>&1 | grep 'configure arguments:'`
   for opt in $options; do
       if [ `echo $opt | grep '.*-temp-path'` ]; then
           value=`echo $opt | cut -d "=" -f 2`
           if [ ! -d "$value" ]; then
               # echo "creating" $value
               mkdir -p $value && chown -R $user $value
           fi
       fi
   done
}
start() {
    [ -x $nginx ] || exit 5
    [ -f $NGINX_CONF_FILE ] || exit 6
    make_dirs
    echo -n $"Starting $prog: "
    daemon $nginx -c $NGINX_CONF_FILE
    retval=$?
    echo
    [ $retval -eq 0 ] && touch $lockfile
    return $retval
}
stop() {
    echo -n $"Stopping $prog: "
    killproc $prog -QUIT
    retval=$?
    echo
    [ $retval -eq 0 ] && rm -f $lockfile
    return $retval
}
restart() {
    configtest || return $?
    stop
    sleep 1
    start
}
reload() {
    configtest || return $?
    echo -n $"Reloading $prog: "
    killproc $nginx -HUP
    RETVAL=$?
    echo
}
force_reload() {
    restart
}
configtest() {
  $nginx -t -c $NGINX_CONF_FILE
}
rh_status() {
    status $prog
}
rh_status_q() {
    rh_status >/dev/null 2>&1
}
case "$1" in
    start)
        rh_status_q && exit 0
        $1
        ;;
    stop)
        rh_status_q || exit 0
        $1
        ;;
    restart|configtest)
        $1
        ;;
    reload)
        rh_status_q || exit 7
        $1
        ;;
    force-reload)
        force_reload
        ;;
    status)
        rh_status
        ;;
    condrestart|try-restart)
        rh_status_q || exit 0
            ;;
    *)
        echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}"
        exit 2
esac
~~~

> 保存文件

~~~shell
#为nginx添加执行权限
chmod 755 nginx
#添加到系统服务
chkconfig --add nginx
chkconfig nginx on
~~~

~~~shell
#测试脚本是否可用,启动nginx
service nginx start
~~~

![](https://s1.ax1x.com/2018/08/28/PO9dd1.png)

> 启动成功

~~~shell
#停止nginx
service nginx stop
~~~

![](https://s1.ax1x.com/2018/08/28/PO9DJK.png)

> 停止成功

~~~shell
#重启nginx
service nginx restart
~~~

![](https://s1.ax1x.com/2018/08/28/PO9gLd.png)

> 重启成功
>
> 完成安装