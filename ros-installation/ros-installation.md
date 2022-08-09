---
title: ROS Installation
date: 2022-06-30 21:10:00
updated: 2022-06-30 21:10:00
tags: [ROS,Ubuntu,VMware]
categories:
- ROS
comments: true 
---

## 前言

目前，ROS 1 的最终版 Noetic 已完成发布。推荐安装的版本为 [ROS Melodic Morenia](http://wiki.ros.org/melodic/Installation) 和 [ROS Noetic Ninjemys](http://wiki.ros.org/noetic/Installation) 。对应 ROS 版本及OS可参考 [Distributions - ROS Wiki](http://wiki.ros.org/Distributions)。

以下将给出 Kinetic 的安装教程，其他版本类似。

## ROS Kinetic

[原文博客(Ubuntu16安装ROS Kinetic Kame（内含Gazebo）)]([(69条消息) Ubuntu16安装ROS Kinetic Kame（内含Gazebo）_MonroeLiu的博客-CSDN博客](https://blog.csdn.net/m0_46216098/article/details/123764190?spm=1001.2014.3001.5502))

### 一、Kinetic

#### 1. 添加下载源

官方默认安装源:

~~~shell
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
~~~

国内清华的安装源

```shell
sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.tuna.tsinghua.edu.cn/ros/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list'
Copy
```

国内中科大的安装源

```shell
sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.ustc.edu.cn/ros/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list'
Copy
```

PS : 建议使用国内资源，安装速度更快。

#### 2. 设置密钥

~~~shell
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
~~~

#### 3. 更新源

```shell
sudo apt-get update
```

#### 4. 安装完整安装包（时间较长耐心等待…）

```shell
sudo apt-get install ros-kinetic-desktop-full -y
```

#### 错误及解决

##### 错误提示

```shell
E: Failed to fetch http://packages.ros.org/ros/ubuntu/pool/main/p/pyside2/python-pyside2.qthelp_2.0.0+dev1-1~202102130210~rev1858~pkg5~git131fdfd1~ubuntu16.04.1_amd64.deb  Connection failed [IP: 64.50.233.100 80]

E: Unable to fetch some archives, maybe run apt-get update or try with --fix-missing?
```

![image-20220625174707175](../images/posts/ros-installation.assets/image-20220625174707175.png)

##### 原因：网络原因

##### 解决方案

输入以下命令，回车（**如仍报出同样错误，重复此操作**）

```shell
# 更新源
sudo apt-get update --fix-missing
# 输入安装命令
sudo apt-get install ros-kinetic-desktop-full -y
```

#### 卸载kinetic

可以调用如下命令:

```shell
sudo apt remove ros-kinetic-*
```

### 二、环境配置、构建依赖和初始化

#### 1. 环境配置（修改~/.bashrc文件）

方便在任意终端中使用 ROS

```shell
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

#### 2. 安装构建依赖

```shell
sudo apt install python-rosinstall python-rosinstall-generator python-wstool build-essential -y
```

#### 3. 初始化rosdep

```shell
sudo rosdep init
rosdep update
```

![image-20220625194753062](../images/posts/ros-installation.assets/image-20220625194753062.png)

ps：这一步报错可能性很大，两者报错解决方法一样。

#### 错误及解决

##### 错误提示

![image-20220625180501059](../images/posts/ros-installation.assets/image-20220625180501059.png)

##### 原因：境外资源屏蔽，网络原因

##### 解决方案

需要修改**资源下载的地址**，将软件包下载到本地，修改相关源码

###### 1. 下载软件包

官方地址（github）：[https://github.com/ros/rosdistro](https://github.com/ros/rosdistro)

B站赵老师（gitee）：[https://gitee.com/zhao-xuzuo/rosdistro](https://gitee.com/zhao-xuzuo/rosdistro)

###### 2. 文件路径

下载完，将文件复制到加目录下，切换进下载的文件夹，打印当前路径`pwd`。通常下载下来的文件名为`rosdistro-master`

```shell
# 打印出来的路径大致为
# USERNAME为你当前的用户空间，一般和用户名一致
/home/USERNAME/rosdistro-master
```

###### 3. 需要修改**四个**文件：

1. /home/USERNAME/rosdistro-master/rosdep/sources.list.d/20-default.list
2. /usr/lib/python2.7/dist-packages/rosdep2/sources_list.py
3. /usr/lib/python2.7/dist-packages/rosdep2/rep3.py
4. /usr/lib/python2.7/dist-packages/rosdistro/\__init__.py

主要就是将

其中的

`https://raw.githubusercontent.com/ros/rosdistro/master`

替换为

`file:///home/USERNAME/rosdistro-master` **（你的文件路径）**

**注意**：python文件中url本地文件地址格式是：**file://**+**文件地址**

###### 文件1 20-default.list

```
sudo gedit /home/USERNAME/rosdistro-master/rosdep/sources.list.d/20-default.list
```

修改后样例

```
# os-specific listings first
yaml file:///home/USERNAME/rosdistro-master/rosdep/osx-homebrew.yaml osx

# generic
yaml file:///home/USERNAME/rosdistro-master/rosdep/base.yaml
yaml file:///home/USERNAME/rosdistro-master/rosdep/python.yaml
yaml file:///home/USERNAME/rosdistro-master/rosdep/ruby.yaml
gbpdistro file:///home/USERNAME/rosdistro-master/releases/fuerte.yaml fuerte

# newer distributions (Groovy, Hydro, ...) must not be listed anymore, they are being fetched from the rosdistro index.yaml instead

```

###### 文件2 sources_list.py

```shell
sudo gedit /usr/lib/python2.7/dist-packages/rosdep2/sources_list.py
```

修改后样例

```python
# default file to download with 'init' command in order to bootstrap
# rosdep
# DEFAULT_SOURCES_LIST_URL = 'https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/sources.list.d/20-default.list'
DEFAULT_SOURCES_LIST_URL = 'file:///home/USERNAME/rosdistro-master/rosdep/sources.list.d/20-default.list'
```

###### 文件3 rep3.py

```shell
sudo gedit /usr/lib/python2.7/dist-packages/rosdep2/rep3.py
```

修改后样例

```python
# location of targets file for processing gbpdistro files
# REP3_TARGETS_URL = 'https://raw.githubusercontent.com/ros/rosdistro/master/releases/targets.yaml'
REP3_TARGETS_URL = 'file:///home/USERNAME/rosdistro-master/releases/targets.yaml'
```

###### 文件4 \__init__.py

```shell
sudo gedit /usr/lib/python2.7/dist-packages/rosdistro/__init__.py
```

修改后样例

```python
# index information

# DEFAULT_INDEX_URL = 'https://raw.githubusercontent.com/ros/rosdistro/master/index-v4.yaml'
DEFAULT_INDEX_URL = 'file:///home/USERNAME/rosdistro-master/index-v4.yaml'
```

###### 重新运行并成功

![image-20220625194353830](../images/posts/ros-installation.assets/image-20220625194353830.png)

### 三、测试（小海龟）

打开三个命令窗口，分别输入

#### 启动roscore

```shell
roscore
```

![image-20220625202612031](../images/posts/ros-installation.assets/image-20220625202612031.png)

#### 启动海龟GUI

```shell
rosrun turtlesim turtlesim_node
#rosrun为ROS命令行工具： rosrun 节点包 节点名
```

![image-20220625202625251](../images/posts/ros-installation.assets/image-20220625202625251.png)

#### 启动键盘控制节点

```shell
rosrun turtlesim turtle_teleop_key
```

![image-20220625202658273](../images/posts/ros-installation.assets/image-20220625202658273.png)

#### 最终效果

![image-20220625202750594](../images/posts/ros-installation.assets/image-20220625202750594.png)

## 结语

通用安装完成，如果还需要更多操作，需要下载相关依赖和扩展，比如无人机还需要大疆SDK等。

## Reference

1. [ROS/Installation - ROS Wiki](http://wiki.ros.org/ROS/Installation)
2. [Distributions - ROS Wiki](http://wiki.ros.org/Distributions)
3. [Installation/Ubuntu - ROS Wiki](http://wiki.ros.org/Installation/Ubuntu)



