# Turtlebot底盘通信方式

参考网址

https://blog.csdn.net/myL_Fm/article/details/48863945

https://blog.csdn.net/a472725641/article/details/52302540



---





启动 `turtlebot_bringup` 包(pkg)中的  `minimal.launch` 之后, 启动 `Kobuki_nodelet` 节点.

当 `Kobuki_nodelet` 被运行时，自动跳转到 `onInit()` 函数，函数中建立 `KobukiRos` 类的新对象，并调用其 `init()` 函数，在 `KobukiRos` 类的 `init()` 函数中，定义了若干个用于接收ROS话题的subscriber，若干个用于发布底盘传感器数据的publisher。并且启动了Kobuki这个驱动类的初始化函数。

最后启动KobukiRos对象的update()函数，用于检测底盘的各种状态，比如底盘的连接状态，电池状态，轮子是否接触地面等等。


通信类中定义了用于接收ROS命令话题的接收者，接收如/cmd_vel等话题。接收者接收到话题消息后，通过回调函数，将ROS通用格式的数据转换为底盘可用格式的数据，并调用驱动类的函数接口，通过串口给底盘单片机发送数据，实现对底盘的操控

`KobukiRos` 用于底盘和ROS通信的类---<font color=red>桥梁</font>

`Kobuki` 用于底盘驱动的类---<font color=red>心脏</font>

通信类中还定义了用于发布底盘状态话题的发布者，如发送/sensors/imu_data的话题。通信类通过驱动类的函数接口获得原始传感器数据，将其转换为ROS通用数据格式，由发布者发送给ROS上层，来实现反馈功能



最后，Kobuki_nodelet启动了一套完整的监测机制，用于监测底盘的各种异常状态