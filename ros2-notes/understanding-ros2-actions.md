## ROS2 Actions 介绍

### 背景

Actions 是 ROS2 的一种通信类型，被用在长时间运行的任务中。

Actions 由目标（goal）、反馈（feedback）和结果（result）组成。

Actions 基于 topics 和 services 实现。除了 actions 任务可以取消以外，其他的功能都类似 services。同时，与 services 仅返回单个结果不同的是，actions 会在 result 达成之前持续稳定的发送 feedback。

Actions 使用 C/S 模式，类似发布订阅。大体流程是：action 客户端发送 goal 给 action 服务器，服务器接收到 goal 之后，验证其合法性，在正确的情况下，持续不断的返回 feedback，直到 result 返回。

![../../../_images/Action-SingleActionClient.gif](.gitbook/understanding-ros2-actions/Action-SingleActionClient.gif)

![](/ros2-notes/.gitbook/understanding-ros2-actions/Action-SingleActionClient.gif)

## Prerequisites


