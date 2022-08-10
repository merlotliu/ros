# Understanding ROS Nodes

[ROS](http://wiki.ros.org/ROS)/ [Tutorials](http://wiki.ros.org/ROS/Tutorials)/ [UnderstandingNodes](http://wiki.ros.org/action/fullsearch/ROS/Tutorials/UnderstandingNodes?action=fullsearch&context=180&value=linkto%3A"ROS%2FTutorials%2FUnderstandingNodes")

**描述**：本文介绍ROS图形概念，并讨论了命令行工具 [roscore](http://wiki.ros.org/roscore), [rosnode](http://wiki.ros.org/rosnode), 和[rosrun](http://wiki.ros.org/rosrun) 的使用。

**下一节:** [Understanding ROS Topics](./ros-tutorials-beginner-6-understanding-topics.md)

## Prerequisites

安装`ros-tutorials`元功能包，使用以下命令：

```shell
$ sudo apt-get install ros-<distro>-ros-tutorials
```

Replace `<distro>` with the name of your ROS distribution (e.g. indigo, jade, kinetic, noetic)

## Quick Overview of Graph Concepts

[Nodes](http://wiki.ros.org/Nodes): 使用ROS与其他节点通信的可执行程序。

[Messages](http://wiki.ros.org/Messages): ROS中订阅或发布一个话题时使用的数据类型。

[Topics](http://wiki.ros.org/Topics): 节点通过话题发布数据（Messages），通过订阅话题接受数据（Messages）。

[Master](http://wiki.ros.org/Master): ROS的 Name 服务（帮助节点之间建立连接）。

[rosout](http://wiki.ros.org/rosout): 相当于stdout/stderr。

[roscore](http://wiki.ros.org/roscore): Master + rosout + parameter server (参数服务器)。

## Nodes

节点（Nodes） 不过是 ROS package 的可执行文件。ROS 节点使用 ROS 用户库（client library）与其他节点通信。节点能够发布或订阅话题（topics）。节点也能够提供或使用服务（service）。

## Client Libraries

ROS 用户库（client library）允许节点使用不同的编程语言实现通信：

roscpp = c++ client library

roscpp = python client library

## roscore

使用ROS的第一步就是运行`roscore`：

```shell
$ roscore
```

命令行将出现类似以下的信息：

```shell
... logging to /home/ml/.ros/log/1fbff624-fcd4-11ec-9814-000c2963efd7/roslaunch-ml-ros-kinetic-3041.log
Checking log directory for disk usage. This may take awhile.
Press Ctrl-C to interrupt
Done checking log file disk usage. Usage is <1GB.

started roslaunch server http://ml-ros-kinetic:34205/
ros_comm version 1.12.17


SUMMARY
========

PARAMETERS
 * /rosdistro: kinetic
 * /rosversion: 1.12.17

NODES

auto-starting new master
process[master]: started with pid [3052]
ROS_MASTER_URI=http://ml-ros-kinetic:11311/

setting /run_id to 1fbff624-fcd4-11ec-9814-000c2963efd7
process[rosout-1]: started with pid [3065]
started core service [/rosout]

```

## rosnode

打开一个**新终端**，并使用`rosnode`来查看`roscore`运行了哪些节点。请保证`roscore`运行终端的启动。

**Notes**：当打开新终端时，环境变量将会重置，`~/.bashrc`文件将会执行。如果运行类似`rosnode`的ROS命令出现错误，那么可能需要添加一些环境变量到你的`~/.bashrc`文件，或者手动`source`。

`rosnode`将展示当前运行的ROS节点的有关信息。输入`rosnode list`命令以获取当前所有活跃节点：

```shell
$ rosnode list
```

命令行输出

```shell
/rosout
```

表明当前仅有`rosout`一个节点运行。`rosout`的作用是记录节点的调试输出。

使用`rosnode info`命令可以获取特定节点的相关信息：

```shell
$ rosnode info /rosout
```

以下就是`rosout`的有关信息，例如发布了`/rosout_agg`...

```shell
--------------------------------------------------------------------------------
Node [/rosout]
Publications: 
 * /rosout_agg [rosgraph_msgs/Log]

Subscriptions: 
 * /rosout [unknown type]

Services: 
 * /rosout/get_loggers
 * /rosout/set_logger_level


contacting node http://ml-ros-kinetic:41947/ ...
Pid: 3065

```

## rosrun

使用`rosrun`可以启动ROS节点。`rosrun`使用`package name`和`node name`直接运行节点，而不需要`package`和`node`的完整路径：

```shell
$ rosrun [package_name] [node_name]
```

现在，我们使用该命令运行`turtlesim`package下的`turtlesim_node`节点。

打开**新终端**，输入：

```shell
$ rosrun turtlesim turtlesim_node
```

将弹出以下窗口：

![image-20220706105748105](.\ros_tutorials_understanding_nodes.assets\image-20220706105748105.png)

**Notes**：这只乌龟的样式可能与你的不同。这是因为ROS提供了许多[乌龟的样式](http://wiki.ros.org/Distributions#Current_Distribution_Releases)，可以说是官方的小彩蛋。

打开新终端，重新输入：

```shell
$ rosnode list
```

命令行输出：

```shell
/rosout
/turtlesim
```

ROS一个比较有用的特性就是，可以在命令行修改节点的名称。

关闭`turtlesim`终端以关闭节点（或者在该终端使用`ctrl+c`）。然后重新运行，并且需要在后面添加 [重映射参数（Remapping Argument）](http://wiki.ros.org/Remapping Arguments)：

```shell
$ rosrun turtlesim turtlesim_node __name:=my_turtle
```

重新输入`rosnode list`：

```shell
$ rosnode list
```

命令行输出：

```shell
/my_turtle
/rosout
```

**Notes**：如果输出仍然为`/turtlesim`，说明你用了`ctrl+c`而不是关闭窗口。此时，可以尝试先使用`rosnode cleanup`，然后在输入`rosnode list`。

当输出为`/my_turtle`时，可以使用`rosnode ping`测试节点是否启动成功：

```shell
$ rosnode ping my_turtle
```

```shell
rosnode: node is [/my_turtle]
pinging /my_turtle with a timeout of 3.0s
xmlrpc reply from http://ml-ros-kinetic:46881/	time=0.463009ms
xmlrpc reply from http://ml-ros-kinetic:46881/	time=1.638889ms
xmlrpc reply from http://ml-ros-kinetic:46881/	time=1.659870ms
xmlrpc reply from http://ml-ros-kinetic:46881/	time=1.644135ms

```

## Conclusion

涵盖的内容：

roscore = ros + core : master (为 ROS 提供 name service) + rosout (stdout / stderr) + parameter server (参数服务器)；

rosnode = ros + node : 获取节点信息的 ROS 工具；

rosrun = ros + run : 运行指定`package`下的指定`node`；

## Reference 

1. http://wiki.ros.org/ROS/Tutorials/UnderstandingNodes
