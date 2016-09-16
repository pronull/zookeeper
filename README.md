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