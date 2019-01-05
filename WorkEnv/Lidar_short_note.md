# Sick 雷达 & Furay 雷达

## 接线

电源线和数据线连接

数据线是一条网线, 注意电脑接口

具体可以参考

https://www.cnblogs.com/21207-iHome/p/8022512.html



## 驱动安装

将驱动装入`ros-kinetic`

使用语句 `sudo apt-get install ros-kinetic-lms1xx`



## IP设置

IP第三位相同才可以互相发现设备

如 `192.168.0.1` 和 `192.168.0.66` , 它们的第三位都是 `0` 

现有Furay雷达的默认IP为 `192.168.1.121`

Sick雷达默认IP为 `192.168.0.1`

为了能找到设备, 需要在使用雷达的时候, 配置好自己的IP

设置IP第三位与雷达的相同

<font color=red size=5>注意</font>

在使用主从机的时候, 一定要避免IP冲突

主从机的IP第三位不能和雷达的第三位冲突

否则无法获取雷达的数据



## 运行

通过语句 `rosrun lms1xx LMS1xx_node _host:=192.168.1.121` 运行

发布话题名称为 `/scan` , 可以通过 `rostopic echo` 查看数据

或是写好 `launch` 文件



<font color=red size=5>注意</font>

开机时, 需要等待雷达初始化完毕, 在运行节点

Furay雷达正常启动, 所有指示灯熄灭

Sick雷达正常启动, 只有ok指示灯发出黄绿光

## 