#zookeeper学习笔记


###zookeeper 基础

####持久节点和临时节点

#####持久的znode，如/path，只能通过调用delete来删除，临时znode与之相反，当创建该节点的客户端崩溃或关闭了与zookeeper的链接时，该节点就会被删除

####有序节点
#####znode节点还可以设置为有序（sequential）节点，一个有序的znode节点被分配唯一一个单调递增的整数。当创建有序节点是，一个序号会被追加到路径之后。

####总之，zn   ode一共有4中类型，持久的（persistent），临时的（ephemeral），持久有序的和临时有序的


##练习zookeeper

####下载zookeeper包，解压后进入文件夹，找到conf/zoo_sample.cfg 改名为zoo.cfg,然后打开该文件，修改dataDir为/home/qwe/zookeeper
####打开一个终端，启动服务器/bin/zkServer.sh start
####打开另一个终端，启动客户端/bin/zkCli.sh
####可以看到客户端链接服务其的信息，现在，在客户端输入ls / ，可以看到，zookeeper只有一个跟节点，创建一个znode，命令：create /workers ""  ,ls /
可以看到有一个新创建的节点workers，现在删除znode : delete /workers  ,退出客户端,quit，退出服务器：zkServer.sh stop

##zookeeper会话及生命周期
zookeeper 生命周期就是从创建到结束，无论是正常关闭还是因为超市而导致的过期。
zookeeper 的会话状态：connecting,connected,closed,not_connected.
一个会话从not_connected 开始，当zookeeper客户端初始化后转到connecting状态，与服务器端链接后转为connected状态。

`经过时间t后服务其接收不到会话的任何消息，服务器就会声明会话过期，而在客户端，经过t/3时间未接收到任何消息，客户端将会想服务器发送心跳消息，
在经过2t/3时间后，zookeeper将会开始寻找其他服务器，而此时他还有t/3的时间去寻找`

##zookeeper与仲裁模式
1. 首先配置一个环境变量，在终端输入：`export PATH_TO_ZK={zookeeper_path}`
2. 接下来创建一个文件夹test，在test里创建三个zookeeper的文件 z1,z2,z3
3. 在z1,z2,z3文件夹下分别创建3个data文件夹，data文件夹下创建文件名为myid的文件，内容分别为1,2,3，命令：echo 1 > z1/data/myid
4. 配置文件：在z1,z2,z3文件夹下分别创建配置文件z1.cfg,z2.cfg,z3.cfg，只要将几个参数分别改变下，<br>dataDir=./data ，
<br>clientPort=2181(对应文件的端口号分别修改，以免冲突),<br>server.1=127.0.0.1:2222:2223,<br>server.2=127.0.0.1:3333:3334,
<br>server.3=127.0.0.1:4444:4445
5. 启动zk服务器: cd z1 | $PATH_TO_ZK/bin/zkServer.sh start ./z1.cfg    cd z2 |  $PATH_TO_ZK/bin/zkServer.sh start ./z2.cfg
6. 启动zk客户端： $PATH_TO_ZK/bin/zkCli.sh -server 127.0.0.1:2181,127.0.0.1:2182,127.0.0.1:2183(哪怕没有启动server3也可以写上去)
7. 停止zk服务器: cd z1 | $PATH_TO_ZK/bin/zkServer.sh stop ./z1.cfg

##zookeeper主-从模式的实现

主从模式包括三个角色：<br>
-    主节点:主节点负责监视新的从节点和任务，分配任务给可用的从节点
-    从节点：从节点会通过系统注册自己，一琼宝主节点看到他们可以执行任务，然后开始监视新任务
-    客户端：客户端负责创建新任务并等待系统的响应