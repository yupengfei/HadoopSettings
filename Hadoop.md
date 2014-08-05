#Hadoop2.4安装配置指南#


##安装环境##

+ 桌面：[Ubuntu Desktop 14.04.1 64位](http://www.ubuntu.com/download/desktop/)
+ 服务器：[Ubuntu Server 14.04.1 64位](http://www.ubuntu.com/server)
+ [Hadoop 2.4.1](http://www.apache.org/dyn/closer.cgi/hadoop/common/)


##编译##

1. 下载Hadoop的源代码
2. 解压
//TODO 接下来交给伟大的philo

##单机运行##


##集群部署##
集群的构架为一个NameNode，一个Secondary NameNode，一个Resource Manager，三个Slaves节点。

首先，安装一个虚拟机，在上面部署完成基础的操作系统。我们使用VirtualBox，过程如下：

1. 建立一个新的虚拟机
![](VirtualBoxImage/VirtualBox1.png)
![](VirtualBoxImage/VirtualBox2.png)
![](VirtualBoxImage/VirtualBox3.png)
![](VirtualBoxImage/VirtualBox4.png)
![](VirtualBoxImage/VirtualBox5.png)
![](VirtualBoxImage/VirtualBox6.png)
![](VirtualBoxImage/VirtualBox7.png)
![](VirtualBoxImage/VirtualBox8.png)
![](VirtualBoxImage/VirtualBox9.png)

2. 下载Ubuntu Server，检验Md5

    md5sum ubuntu-14.04.1-server-amd64.iso

检验结果应该为ca2531b8cd79ea5b778ede3a524779b9。

3. 安装Ubuntu Server
  
![](ServerInstallImage/ServerInstall1.png) 
![](ServerInstallImage/ServerInstall2.png)
![](ServerInstallImage/ServerInstall3.png)
![](ServerInstallImage/ServerInstall4.png)
![](ServerInstallImage/ServerInstall5.png)
![](ServerInstallImage/ServerInstall6.png)
![](ServerInstallImage/ServerInstall7.png)
![](ServerInstallImage/ServerInstall8.png)
![](ServerInstallImage/ServerInstall9.png)
![](ServerInstallImage/ServerInstall10.png)
![](ServerInstallImage/ServerInstall11.png)
![](ServerInstallImage/ServerInstall12.png)
![](ServerInstallImage/ServerInstall13.png)
![](ServerInstallImage/ServerInstall14.png)
![](ServerInstallImage/ServerInstall15.png)
![](ServerInstallImage/ServerInstall16.png)
![](ServerInstallImage/ServerInstall17.png)
![](ServerInstallImage/ServerInstall18.png)
![](ServerInstallImage/ServerInstall19.png)
![](ServerInstallImage/ServerInstall20.png)
![](ServerInstallImage/ServerInstall21.png)
![](ServerInstallImage/ServerInstall22.png)
![](ServerInstallImage/ServerInstall23.png)
![](ServerInstallImage/ServerInstall24.png)
![](ServerInstallImage/ServerInstall25.png)
![](ServerInstallImage/ServerInstall26.png)
![](ServerInstallImage/ServerInstall27.png)
![](ServerInstallImage/ServerInstall28.png)
![](ServerInstallImage/ServerInstall29.png)
![](ServerInstallImage/ServerInstall30.png)

4. 设置网络连接，更新操作系统

    cd /etc/network
    sudo vi interfaces

//TODO[vi编辑器手册]()

输入类似于

    auto lo
    iface lo inet loopback
    auto eth0
    iface eth0 inet static
    address 172.16.253.216
    netmask 255.255.255.0
    gateway 172.16.253.241
    dns-nameservers 114.114.114.114

:wq

运行

   sudo ifup eth0
   ifconfig

![](ServerInstallImage/ServerInstall31.png)
    

5. 设置ssh，使用root远程登录

首先开启root账号

    sudo passwd root

然后修改配置文件

    sudo vi /etc/ssh/sshd_config

修改28行

    PermitRootLogin without-password
    
为

   PermitRootLogin yes

:wq

重启ssh服务

    sudo service ssh restart

此时可以使用客户端登陆服务器

![](ServerInstallImage/ServerInstall32.png)

6. 升级系统

    sudo apt-get update

    sudo apt-get upgrade

7. 使用FileZilla连接server，修改profile文件


8. 使用FileZilla连接server，将编译完成的Hadoop拷入，修改masters、slaves文件

9. clone6个虚拟机

然后，分别进入六个虚拟机，修改hostname、hosts

1. 

最后，设置ssh免登陆


开机，

##文件上传、下载##

##NameNode失效测试##

##动态添加Slaves##

##Yarn编程模型##





