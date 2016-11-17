# Round 1 介绍

                                         一、Hadoop详细安装文档
注意
	安装需要配置，建议2G以上的内存 ，10G左右的磁盘空间
安装虚拟机
	这里使用VirtualBox下载地址在http://pan.baidu.com/s/1hqA5A64#dir/path=%2Fmr
vitualBox-4.3.4-91027-Win.1412846830.exe这个文件

安装过程略过

安装linux ，
在虚拟机中安装ubuntu
1.	新建虚拟机，名称选择master，类型linux,版本ubuntu。
 
内存根据机器配置选择，因为我们要建立3~4台虚拟机，如果是2G内存的机器，可以设置300~500M。

注意，这里可以选择虚拟系统保存的文件位置 ，大家根据自己磁盘的情况去选择，通过 这个来选择。选择创建，完成这个操作

2.	安装unbuntu。直接双击新建好的虚拟机
 
 
在弹出的窗口中选择下载好的ubuntu镜像开始安装系统。

3.	安装过程，所有的选择，都选择默认的即可。
注意：虚拟机与实际系统之间的操作切换，使用ctrl+alt退出当前虚拟机，鼠标可见，进入windows系统的操作。直接鼠标点入虚拟机，即可切入。这个操作贯穿整个后继的过程。
 

用户名：hadoop，密码：123456， 
在生成的虚拟机上，点右键选择设置，
 

将网络的网卡修改成桥接网卡，界面名称选项会自动变化，然后点确定。
安装securecrt
1.	下载
在http://pan.baidu.com/s/1hqA5A64#dir/path=%2Fmr
下载securecrt.zip与jdk-6u45-linux-i586.bin两个文件
将securecrt.zip解压到windows某一个目录下，
执行 
即可启动sercurecrt

2．	获取虚拟机ip
	启动虚拟机，进入界面
 

输入密码，123456，
进入后，点击 ，输入terminal

选择 

得到 

输入ifconfig,得到类同
 
找到中间的inet addr地址， 一般是192.168.x.xxx打头的，记下来。

3	安装openssh-client与lrzsz
保证互联网是联通的
在命令行里输入
 
一路yes ,确认
输入
 
一路确认

安装java环境
1.	下载软件
下载jdk-8u111-linux-x64.gz 文件

2.	上传文件
运行前面下载好的sercuecrt
选择
 

在窗口中填入在虚拟机查看到的ip，如192.168.x.xxx。出现
选择connent
 
正常的话，选择ok后，能进入

如不能进入，肯定是ip地址没有输入正确，或没有安装
安装openssh-client与lrzsz，请检查。
 
这样就可以直接通过securecrt联接虚拟机了。

输入rz，回车
 
应当会弹出
 
选择文件的窗口

找到前面下载好的jdk-8u111-linux-x64.gz 文件，选择add，即可上传文件，

 

完成后输入ll，即可看到jdk-8u111-linux-x64.gz文件上传成功
 
3.	安装java
给这个文件执行权限
输入chomd +x ./jdk-8u111-linux-x64.gz 

输入
./jdk-8u111-linux-x64.gz 

回车等待后，即可看到

Java安装完成。

4.	配置java环境
使用vi编辑。



在文件最后一行加入
export JAVA_HOME=/home/hadoop/jdk1.8.0_111/
export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JRE_HOME/lib
export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
保存退出

输入source ~/.bashrc
 
测试java配置是否成功
直接输入java -version
 
有相应的显示，即成功。

安装hadoop
1.下载hadoop
在ftp上下载hadoop-2.6.5.tar
这个文件

2.	上传hadoop安装包
如同前面安装java环境一样，使用securtCRT的rz功能上传hadoop-2.6.5.tar.gz这个文件到虚拟机的系统中。
同样在securtcrt中ll时，能得到
 
3.	安装hadoop
解压缩
 
使用cd进入相应的目录
 
新增tmp目录
mkdir  /home/hadoop/hadoop-2.6.5tmp
 
4.	配置hadoop
sbin目录下
使用vi修改master文件
 
将localhost修改成master
 
保存退出


使用vi修改slaves文件
注意，这里准备设置几台slave机器，就写几个，因为配置的是三个虚拟机，一台做master，两台做slave，所以这里写成了两个slave

1.使用vim修改core-site.xml
 <configuration>
	<property>
                <name>fs.defaultFS</name>
                <value>hdfs://vm10-0-0-2.ksc.com:9000</value>
        </property>
        <property>
                <name>hadoop.tmp.dir</name>
                <value>file:/home/hadoop/hadoop-2.6.5/tmp</value>
                <description>Abase for other temporary directories.</description>
        </property>
</configuration>
：q保存
2.使用vim修改hadoop-env.sh
# The java implementation to use.
export JAVA_HOME=/usr/java/jdk1.8.0_111
：q保存
3.使用vim修改hdfs-site
<configuration>
	<property>
                <name>dfs.namenode.secondary.http-address</name>
                <value>vm10-0-0-2.ksc.com:50090</value>
        </property>
        <property>
                <name>dfs.replication</name>
                <value>3</value>
        </property>
        <property>
                <name>dfs.namenode.name.dir</name>
                <value>file:/home/hadoop/hadoop-2.6.5/tmp/dfs/name</value>
        </property>
        <property>
                <name>dfs.datanode.data.dir</name>
                <value>file:/home/hadoop/hadoop-2.6.5/tmp/dfs/data</value>
        </property>
</configuration>
：q保存
4.使用vim修改mapred-site
<configuration>
        <property>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
        </property>
        <property>
                <name>mapreduce.jobhistory.address</name>
                <value>vm10-0-0-2.ksc.com:10020</value>
        </property>
        <property>
                <name>mapreduce.jobhistory.webapp.address</name>
                <value>vm10-0-0-2.ksc.com:19888</value>
        </property>
</configuration>
：q保存
5.使用vim修改yarn-env.sh
6.使用vim修改yarn-site
<configuration>

<!-- Site specific YARN configuration properties -->
        <property>
                <name>yarn.resourcemanager.hostname</name>
                <value>vm10-0-0-2.ksc.com</value>
        </property>
        <property>
                <name>yarn.nodemanager.aux-services</name>
                <value>mapreduce_shuffle</value>
        </property>
</configuration>
：q保存
其它都不用修改。

修改本地网络配置
使用vi编辑/etc/hosts文件
命令：sudo vim /etc/hosts
192.168.2.100 master
192.168.2.60 slave1
192.168.2.189 slave2
Ip地址进行修改。

关闭虚拟机
可在virtualbox可强制关闭，也可以在系统中关机


复制虚拟机
1.	复制
在虚拟机上右键选择复制

注意，要选择初始化所有网卡的mac地址
 

根据自己需求两台虚拟机作为slave。

同样，确认一下网络联接方式，是否为桥接方式。

2.	设置所有台机器的ip地址。
分别启动相关的虚拟机

修改机器的ip 地址
在虚拟机的图形界面里，
选择设置 ，单击打开
在出来的窗口里，选择
 
打开 

修改成如下的形式
选择ipv4 ,分配方式选择成manual

具体的ip地址，根据实际的情况来设置，因为培训教室里都是192.168.2.x的网段，所以我这里设置成了192.168.2.x

建立互信关系
1.	生成公私钥
在master机器的虚拟机命令行下输入ssh-keygen 
一路回车，全默认
 
2.	复制公钥
复制一份master的公钥文件
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
 

同样，在所有的slave机器上，也在命令行中输入
ssh-keygen 
一路回车，全默认

所有的salve机器上
从master机器上复制master的公钥文件
scp master:~/.ssh/authorized_keys /home/hadoop/.ssh/
 

3．	测试联接
在master机器上
分别向所有的slave机器发起联接请求
如：
ssh slave1
记得一旦联接上，所有的操作，就视同在对应的slave上操作，所以一定要记得使用exit退出联接。

启动hadoop
1.	初始化
在master机器上
进入/home/hadoop/hadoop-2.6.5/bin目录

cd /home/hadoop/hadoop-2.6.5/bin
 
运行
./hadoop namenode –format
初始化hadoop的文件系统。
 

2.	启动
sbin目录下执行./start-all.sh
中间过程 ，可能提示要判断是否,输入yes
 

输入jps，查看进程是否都正常启动。
master下正常显示：
3156 SecondaryNameNode
2965 NameNode
3595 Jps
3340 ResourceManager

slave下正常显示：
3016 NodeManager
3144 Jps
2892 DataNode
 
如果一切正常，应当有如上的一些进程存在。


3.	测试系统
输入./hadoop fs –ls /
 
能正常显示文件系统。

如此，hadoop系统搭建完成。否则，可以去
/home/hadoop/hadoop-2.6.5/logs
目录下，查看缺少的进程中，对应的出错日志。

二、 

1. 完成每天的UV统计
  将每天的用户访问数据通过mapreduce处理，通过正则表达式对数据进行切割得到用户ip，map中ip当做key，value为1。reduce中进行统计（类似于wordcount），最后输出的形式为key为ip，reduce为这个ip当天出现的次数。将其输出到一个指定的文件。
2. 完成每天访问量Top10的Show统计
  读取1中的文件，通过mapreduce对其数据进行统计，map输出中ip为key，之前统计的数量为value。reduce中将其放到一个map中，再通过对数量进行排序，取前十条数据。
3. 完成每天的次日留存统计
  将连续两天的数据均进行1中处理，将两天的数据分别输出到两个文件中。使用mapreduce处理这两个数据，通过文件名来区分这两个文件中读取的内容，在第一个后边+“！”，ip为key，value为1（或1？），reduce将key相同的数据放到set中，如果其中数据大于1条，那么就是留存的用户，输出这个用户的ip。
