# [ROS](http://wiki.ros.org/ROS)/ [Concepts](http://wiki.ros.org/action/fullsearch/ROS/Concepts?action=fullsearch&context=180&value=linkto%3A"ROS%2FConcepts")

ROS有三个层次的概念：文件系统层次、计算图层次和社区层次。下面总结了这些级别和概念，后面的部分将更详细地介绍这些级别和概念。

除了三个层次的概念，ROS 还定义了两种类型的名称——Package Resource Names 和 Graph Resource Names——这将在下面讨论。

## ROS Filesystem Level

文件系统层次的概念主要涵盖了您在磁盘上遇到的 ROS 资源，例如：

**功能包（packages）**：在ROS中，功能包是组织软件的主要单元。一个功能包可能包含 ROS 运行时进程（节点）、一个依赖于 ROS 的库、数据集、配置文件或任何其他有用地组织在一起的东西。功能包是 ROS 中最基本的构建项和发布项。这意味着构建和发布的最细粒度的东西是一个包。

**元功能包（metapackages）**：元功能包的作用是将一些相关的包组织在一起，里面没有任何源码实现。

**功能包清单文件（packages manifests）**： 提供有关包的元数据，包括其名称、版本、描述、许可证信息、依赖项和其他元信息，如导出的包。

**存储库（repositories）**：共享公共 VCS 系统的软件包集合。共享一个 VCS 的包共享相同的版本，可以使用 `catkin` 发布自动化工具`bloom`一起发布。通常这些存储库将映射到转换后的 `rosbuild Stacks`。存储库也可以只包含一个包。

**消息类型（messages types ）**：消息描述，存储在 `my_package/msg/My Message Type.msg` 中，定义了在 ROS 中发送的消息的数据结构。

**服务类型（service types）**：服务描述，存储在 `my_package/srv/My Service Type.srv` 中，定义了 ROS 中服务的请求和响应数据结构。

## ROS Computation Graph Level

Computation Graph 是处理数据的 ROS P2P 网络进程。在ROS中，基本 Computation Graph 概念是节点、Master、Parameter Server、消息、服务、主题和包，所有这些都以不同的方式向 Graph 提供数据。

这些概念在 `ros_comm` 存储库中实现。

**节点（Nodes）**：节点是执行计算的进程。ROS被设计成细粒度的模块化；机器人控制系统通常包含许多节点。例如，一个节点控制激光测距仪，一个节点控制车轮马达，一个节点执行定位，一个节点执行路径规划，一个节点提供系统的图形视图，等等。ROS 节点是使用 `ROS client library` 编写的，例如 `roscpp` 或 `rospy`。

**Master**： ROS Master 提供名称注册和对计算图其余部分的查找。如果没有 Master，节点将无法找到彼此、交换消息或调用服务。

**参数服务器（Parameter Server）**：参数服务器允许通过密钥将数据存储在中央位置。它目前是 Master 的一部分。



## ROS Community Level



## Names

### Graph Resource Names

## Reference

1. [ROS/Concepts - ROS Wiki](http://wiki.ros.org/ROS/Concepts)