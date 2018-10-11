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

