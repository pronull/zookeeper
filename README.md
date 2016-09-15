#zookeeper学习笔记


###zookeeper 基础

####持久节点和临时节点

#####持久的znode，如/path，只能通过调用delete来删除，临时znode与之相反，当创建该节点的客户端崩溃或关闭了与zookeeper的链接时，该节点就会被删除

####有序节点
#####znode节点还可以设置为有序（sequential）节点，一个有序的znode节点被分配唯一一个单调递增的整数。当创建有序节点是，一个序号会被追加到路径之后。

####总之，zn   ode一共有4中类型，持久的（persistent），临时的（ephemeral），持久有序的和临时有序的