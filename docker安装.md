### 腾讯云内和升级

~~~shell
#查看内核版本
uname -r
2.6.32-754.6.3.el6.x86_64 #2版本
~~~

**内核升级**

~~~~shell
#导入 public key
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
#为 RHEL-7，SL-7 或 CentOS-7 安装 ELRepo：
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
#为 RHEL-6，SL-6 或 CentOS-6 安装 ELRepo：
rpm -Uvh http://www.elrepo.org/elrepo-release-6-8.el6.elrepo.noarch.rpm
~~~~

> 这里需要注意的是~，在 ELRepo 中有两个内核选项，一个是 kernel-lt(长期支持版本)，一个是 kernel-ml(主线最新版本)，采用长期支持版本 (kernel-lt)，更稳定一些

~~~shell
# kernel-lt(稳定版)
yum --enablerepo=elrepo-kernel install kernel-lt -y 

# kernel-ml(最新版)
yum --enablerepo=elrepo-kernel install kernel-ml -y 
~~~

**安装完成，需要修改 grub**

~~~shell
vim /etc/grub.conf
~~~

> 根据安装好以后的内核位置，修改 default 的值，一般是修改为 0，因为 default 从 0 开始，一般新安装的内核在第一个位置，所以设置 default=0

**重启**

~~~shell
#重启
reboot
#查看内核
uname -r
#4.18.13-1.el6.elrepo.x86_64
#升级成功
~~~

### 安装docker

~~~shell
#安装
yum -y install docker-io
#启动docker
service docker start
#关闭docker
service docker stop
~~~

**docker操作**

~~~shell
#搜索镜像
docker search tomcat
#拉取镜像
docker pull tomcat
#根据镜像启动容器
docker run ‐‐name mytomcat ‐d tomcat:latest
#查看运行中的容器
docker ps 
#停止运行中的容器
docker stop  容器的id
#查看所有的容器
docker ps ‐a
#启动容器
docker start 容器id
#删除一个容器
 docker rm 容器id
#启动一个做了端口映射的tomcat
docker run ‐d ‐p 8888:8080 tomcat
# ‐d：后台运行
# ‐p: 将主机的端口映射到容器的一个端口    主机端口:容器内部的端口
~~~

**修改docker镜像地址**

~~~shell
#镜像加速直接运行命令
#该脚本可以将 –registry-mirror 加入到你的 Docker 配置文件 /etc/default/docker 中。适用于 #Ubuntu14.04、Debian、CentOS6 、CentOS7、Fedora、Arch Linux、openSUSE Leap 42.1，其他版本可#能有细微不同。 http://3272dd08.m.daocloud.io 为国内加速链接
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://3272dd08.m.daocloud.io
##############
#这个 json 文件不存在的，不需要担心，直接编辑
vim /etc/docker/daemon.json
#下面是内容
~~~

~~~json
{ 
	"registry-mirrors":["https://registry.docker-cn.com"] 
}
~~~

**下载oracle**

~~~shell
#首先下载镜像，镜像可以在阿里云上找
docker pull registry.cn-hangzhou.aliyuncs.com/qida/oracle-xe-11g
#下载下来后，开始 run
docker run -d -p 1521:1521 --name oracle 容器id
#查看运行容器
docker ps -a
~~~

**运行oracle容器**

~~~shell
docker run -d -p 49160:22 -p 49161:1521 -e ORACLE_ALLOW_REMOTE=true 

#运行，并开放 49160 和 49161 端口，分别对应 22 端口和 oracle 端口（SSH 和 oracle 数据库）

#hostname: localhost
#port: 49161
#sid: xe
#username: system
#password: oracle
#SYSTEM 和 SYS 的初始密码都为 oracle
#Container SSH 的 root 密码为admin。

~~~


