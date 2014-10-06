#Hadoop2.4安装、配置、使用指南#
AUTH：じòぴé→尐俽 , PHILO

鉴于Hadoop配置繁琐，网络上的资料鱼龙混杂，我们决定写一个经过验证可行的Hadoop安装、配置、使用指南，免除新手苦苦摸索的痛苦。本文会不断的完善，你可能需要前往[Hadoop2.4安装、配置、使用指南](https://github.com/yupengfei/HadoopSettings)获取最新版。有问题或者建议欢迎加QQ群(55958311)讨论。
##安装环境##

+ 桌面：[Ubuntu Desktop 14.04.1 64位](http://www.ubuntu.com/download/desktop/)
+ 服务器：[Ubuntu Server 14.04.1 64位](http://www.ubuntu.com/server)
+ [Hadoop 2.4.1](http://www.apache.org/dyn/closer.cgi/hadoop/common/)


##编译##

###为什么我们需要自己来编译Hadoop

在官网上只提供了32bit版本的hadoop，如果想更加接近真实的生产环境那么您需要自己编译整个hadoop系统。
好消息是，maven构建系统搞定了整个编译过程的所有解决方案。当然包括软件打包。第二个好消息是，如果您是新手
对编译的要求不是很高，并且您也没有大把时间来解决编译问题，我们这里提供了我已经编译好的**您可以绕过本步骤**
直接下载已经编译好的点击[这里](http://pan.baidu.com/s/1sjsdym5)(Hadoop 2.4.1 64bit)



###编译的整个过程

[代码下载地址](http://mirrors.aliyun.com/apache/hadoop/common/)
这里感谢阿里云。

Maven已经给我们做了大部分的工作编译流程一共两步

1. 解压代码进入代码目录
2. 执行命令 mvn package -Pdist,native -DskipTests -Dtar

当然接下来就是耐心等待了

大概过程描述如下：
1. maven会帮你下很多jar包。最好找个网速好点的地方编译，绝对不能离线编译。
2. 之后就是遥遥无期的编译过程了，停下来的时候看好了大概是什么错误，基本上都是包没有打全的问题。

我再编译2.5.1的时候没有遇到任何问题的情况下参考下面信息
[INFO] ------------------------------------------------------------------------

[INFO] BUILD SUCCESS

[INFO] ------------------------------------------------------------------------

[INFO] Total time: 32:38.094s

[INFO] Finished at: Mon Oct 06 19:59:49 CST 2014

[INFO] Final Memory: 138M/336M

[INFO] ------------------------------------------------------------------------


##单机运行##


##集群部署##
集群的构架为一个NameNode，一个Secondary NameNode，一个Resource Manager，三个Slaves节点。对应的IP分别为

| hostname           | IP             |
| ------------------ | -------------- |
| namenode           | 172.16.253.211 |
| secondarynamenode  | 172.16.253.212 |
| resourcemanager    | 172.16.253.213 |
| slave1             | 172.16.253.214 |
| slave2             | 172.16.253.215 |
| slave3             | 172.16.253.216 |


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

7. 安装openjdk 7

    sudo apt-get install openjdk-7-jdk

8. 使用FileZilla连接server，修改profile文件，在开头添加

    export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
    export HADOOP_HOME=/opt/hadoop-2.4.1
    export PATH=$PATH:$HADOOP_HOME/bin
    export PATH=$PATH:$HADOOP_HOME/sbin
    export HADOOP_MAPRED_HOME=${HADOOP_HOME}
    export HADOOP_COMMON_HOME=${HADOOP_HOME}
    export HADOOP_HDFS_HOME=${HADOOP_HOME}
    export YARN_HOME=${HADOOP_HOME}

9. 编译hadoop
  //TODO 应该是philo写的
  //mvn package -DskipTests -Pdist,native -Dtar
  //生成文件在hadoop-2.4.1-src/hadoop-dist/target/hadoop-2.4.1
  //

10. 修改/etc/hosts

添加
    
    172.16.253.211     namenode
    172.16.253.212     secondarynamenode
    172.16.253.213     resourcemanager
    172.16.253.214     slave1
    172.16.253.215     slave2
    172.16.253.216     slave3

11. 修改masters、slaves文件，hadoop配置文件

masters文件记录的实际上是secondary namenode的hostname，在hadoop-2.4.1/etc/hadoop下面新建masters文件，写入

    secondarynamenode
    
slaves文件默认存在，记录的是slaves节点的hostname，修改hadoop-2.4.1/etc/hadoop下面的slaves文件，改为

    slave1
    slave2
    slave3

修改etc/hadoop/hadoop-env.sh修改export JAVA_HOME=${JAVA_HOME}为

    export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64

修改core-site.xml文件，将configuration之间加入
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://namenode:9000</value>
        <final>true</final>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/opt/tmp/hadoop-${user.name}</value>
    </property>
    <property>
        <name>io.file.buffer.size</name>
        <value>131072</value>
    </property>
    <property> 
        <name>fs.checkpoint.period</name> 
        <value>3600</value> 
        <description>The number of seconds between two periodic checkpoints.</description> 
    </property> 
    <property> 
        <name>fs.checkpoint.size</name> 
        <value>67108864</value> 
     <description>The size of the current edit log (in bytes) that triggers a periodic checkpoint even if the fs.checkpoint.period hasn't expired.  </description> 
    </property> 
    <property> 
        <name>fs.checkpoint.dir</name> 
        <value>/home/dfs/namesecondary</value> 
        <description>Determines where on the local filesystem the DFS secondary namenode should store the temporary images to merge.If this is a comma-delimited list of directories then the image is replicated in all of the directories for redundancy.</description> 
    </property> 

修改hdfs-site.xml文件，在configuration之间加入
   <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:/home/dfs/name</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/home/dfs/data</value>
    </property>
    <property>
        <name>dfs.replication</name>
        <value>3</value>
    </property>
    <property>
        <name>dfs.secondary.http.address</name>
        <value>secondarynamenode:50090</value>
    </property>
    <property>
        <name>dfs.http.address</name>
        <value>namenode:50070</value>
    </property>
    <property>
        <name>dfs.block.size</name>
        <value>16777216</value>
        <description>The block size for new files.</description>
    </property>

修改yarn-site.xml文件，将configuration之间加入
    <property>
        <name>yarn.resourcemanager.address</name>
        <value>resourcemanager:9001</value>
        <description>The address of the applications manager interface in the RM.</description>
    </property>

    <property>
        <name>yarn.resourcemanager.scheduler.address</name>
        <value>resourcemanager:18030</value>
        <description>The address of the scheduler interface,in order for the RM to obtain the resource from scheduler</description>
    </property>

    <property>
        <name>yarn.resourcemanager.resource-tracker.address</name>
        <value>resourcemanager:18025</value>
        <description>The address of the resource tracker interface for the nodeManagers</description>
    </property>

    <property>
        <name>yarn.resourcemanager.admin.address</name>
        <value>resourcemanager:18035</value>
        <description>The address for admin manager</description>
    </property>

    <property>
        <name>yarn.resourcemanager.webapp.address</name>
        <value>resourcemanager:18088</value>
        <description>The address of the RM web application.</description>
    </property>

    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
12. 创建.ssh文件夹，为ssh免登陆做准备
    
    cd
    mkdir .ssh
13. 使用FileZilla连接server，将编译完成的Hadoop拷入/opt目录
    
    拷贝过去的hadoop可能没有执行权限，在hadoop目录下手工执行
    
    chmod +x bin/*
    chmod +x sbin/*
    chmod +x libexec/*

14. 关闭虚拟机，clone6个虚拟机

![](HadoopConfigure/Configure1.png)
然后，分别进入六个虚拟机，修改hostname为其/etc/hostname和IP然后重启


15. 设置ssh免登陆

登陆namenode，运行

ssh-keygen -t rsa，然后一直回车

在/root/.ssh/目录下生成了两个文件 id_rsa 和 id_rsa.pub

    cat id_rsa.pub > ./authorized_keys

登陆resourcemanager，同样生成authorized_keys，把两个authorized_keys的内容拷贝到一起，复制到所有节点。

scp authorized_keys root@namenode:/root/.ssh/

scp authorized_keys root@secondarynamenode:/root/.ssh/

scp authorized_keys root@resourcemanager:/root/.ssh/

scp authorized_keys root@slave1:/root/.ssh/

scp authorized_keys root@slave2:/root/.ssh/

scp authorized_keys root@slave3:/root/.ssh/

验证能否无密码ssh，在namenode服务器上执行操作：

ssh root@namenode

ssh root@secondarynamenode

ssh root@resourcemanager

ssh root@slave1

ssh root@slave2

ssh root@slave3

注意：第一次可能会提示输入yes or no，之后就可以直接ssh到其他主机上去了。

16. 运行

在master上执行：

hdfs namenode -format

/opt/hadoop-2.4.1/sbin/start-dfs.sh

在resourcemanager上执行：

/opt/hadoop-2.4.1/sbin/start-yarn.sh

若一切顺利，在各服务器上输入jps查看执行情况。

    

17.运行yarn示例

 ./bin/hadoop jar ./share/hadoop/yarn/hadoop-yarn-applications-distributedshell-2.4.1.jar \
org.apache.hadoop.yarn.applications.distributedshell.Client \
-jar ./share/hadoop/yarn/hadoop-yarn-applications-distributedshell-2.4.1.jar \
-shell_command date \
-num_containers 1 \
-container_memory 10

##文件上传、下载##

##NameNode失效测试##

##动态添加Slaves##

##Yarn编程模型##





