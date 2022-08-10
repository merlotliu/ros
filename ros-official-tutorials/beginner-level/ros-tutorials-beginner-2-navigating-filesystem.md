# Navigating the ROS Filesystem

[ROS](https://wiki.ros.org/ROS)/ [Tutorials](https://wiki.ros.org/ROS/Tutorials)/ [NavigatingTheFilesystem](https://wiki.ros.org/action/fullsearch/ROS/Tutorials/NavigatingTheFilesystem?action=fullsearch&context=180&value=linkto%3A"ROS%2FTutorials%2FNavigatingTheFilesystem")

**描述**：介紹ROS文件系統相關概念，包括使用`roscd`, `rosls`, `rospack` 命令行工具。

**下一节:** [Creating a ROS Package](./ros-tutorials-beginner-3-creating-package.md)

## 必要準備

爲了配合使用後面的ROS命令行工具，需要安裝`ros-tutorials`元功能包，使用以下命令：

```shell
$ sudo apt-get install ros-<distro>-ros-tutorials
```

'\<distro>' 替換成你安裝的ROS版本名稱（如：kinetic、lunar）。不過，通常完整安裝包安裝時，已安裝完成，這一步僅做檢查之用。

## 相關概念

**Packages**：ROS中的代碼組織單元，包含庫文件、可執行文件、脚本文件和其他手動加入的文件。

**Manifests(package.xml)：**當前功能包的描述文件，用於定義衆多包之間的依賴和獲取版本、維護者、許可證等信息。

## 文件系統工具

ROS代碼組織方式較爲分散，ROS命令行工具能夠幫助快速找到功能包的位置。

### rospack

**作用**：獲取`packages`相關信息；

`rospack find`可以獲取`package`的路徑信息。下面將簡單演示`rospack find`的使用。

用法：

```shell
$ rospack find [package]
```

示例：

```shell
$ rospack find roscpp
```

將會返回以下信息：

```shell
ROS_INSTALL_PATH/share/roscpp
```

如果是在`Ubuntu`下安裝的`ROS Kinetic`，將返回以下信息：

```shell
/opt/ros/kinetic/share/roscpp
```

### roscd

**作用**：直接切換到`ROS package`的路徑下；

#### ROS package 根目錄

用法：

```shell
$ roscd <package-or-stack>[/subdir]
```

切換到`roscpp`包下：

```shell
$ roscd roscpp
```

爲了驗證確實切換到了`roscpp`下，輸入`pwd`打印當前路徑：

```shell
$ pwd
```

可以發現，這一路徑與`rospack find`給出的路徑是一樣的：

```shell
ROS_INSTALL_PATH/share/roscpp
```

**Notes**：與其他`ROS Tools`一樣，`roscd`只會查找`ROS_PACKAGE_PATH`中列出的目录中的 `ROS package`。輸入以下命令可查看當前`ROS_PACKAGE_PATH`内容：

```shell
$ echo $ROS_PACKAGE_PATH
```

在`Ubuntu`下安裝的`ROS Kinetic`通常能夠收到以下信息：

```shell
/opt/ros/kinetic/share
```

與其他環境變量一樣的是，可以向`ROS_PACKAGE_PATH`添加其他目錄，不同路徑用 `:` 分割。

#### ROS package 子目錄

示例：

```shell
$ roscd roscpp/cmake
$ pwd
```

返回以下信息：

```shell
ROS_INSTALL_PATH/share/roscpp/cmake
```

#### roscd log

`roscd log`將切換到ROS日志文件的存放路徑。

**Notes**：如果在此之前尚未運行過任何ROS節點，將返回錯誤信息并表示路徑不存在。

```shell
$ roscd log
```

### rosls

**作用**：通過`ROS package`名稱即可列出其子目錄，而不需要完整的路徑；

用法:

```shell
$ rosls <package-or-stack>[/subdir]
```

示例：

```shell
$ rosls roscpp_tutorials
```

收到以下信息：

```shell
cmake launch package.xml  srv
```

### Tab 補全（Tab-completion）

輸入完整的`package`名稱是一件冗長乏味的事情。在前面的例子中，`roscpp_tutorials`無疑是一個相當長的名字了。所幸的是，一些ROS工具支持Tab補全。

可以輸入以下内容：

```shell
$ roscd roscpp_tut<<< now push the TAB key >>>
```

然後按下`TAB`鍵，命令行就會補全剩下的部分：

```shell
$ roscd roscpp_tutorials/
```

當然，這是因爲在在當前`ROS package`中，僅有`roscpp_tutorials`以`roscpp_tut`開頭。

現在我們嘗試輸入以下内容：

```shell
$ roscd tur<<< now push the TAB key >>>
```

按下`TAB`鍵后，命令行内容可能將補充至以下内容：

```shell
$ roscd turtle
```

然而，當前有多個`packages`以`turtle`開頭。再次按下`TAB`鍵，將會輸出所有以`turtle`開頭的`ROS package`。

```shell
turtle_actionlib/  turtlesim/ turtle_tf/
```

然后，命令行仍然顯示如下内容：

```shell
$ roscd turtle
```

如果想要定位到`turtlesim/ `，則需要至少再繼續在`turtle`後面輸入一個`s`，然後按下`TAB`：

```shell
$ roscd turtles<<< now push the TAB key >>>
```

當僅有一個`package`以`turtles`開頭時, 將出現以下内容：

```shell
$ roscd turtlesim/
```

如果想看到當前所有的以安裝`packages`的列表，可以在`rosls`后，連續點擊兩次`TAB`：

```shell
$ rosls <<< now push the TAB key twice >>>
```

### 幫助

當不明確`ROS Tools`使用規則時，可以在命令後鍵入`-h`獲得使用説明，如下：

```shell
$ rospack -h
```

命令行將顯示如下内容：

![image-20220703170334323](.\ros_tutorials_navigating_filesystem.assets\image-20220703170334323.png)

## 總結

可以注意到`ROS Tools`的命名格式：

rospack = ros + pack(age)

roscd = ros + cd

rosls = ros + ls

其實，這個命名格式在許多`ROS Tools`都適用。

## Reference 

1. http://wiki.ros.org/ROS/Tutorials/NavigatingTheFilesystem