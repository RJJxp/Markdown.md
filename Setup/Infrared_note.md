# 红外光电开关传感器

参考网站

[淘宝信息](https://item.taobao.com/item.htm?spm=a230r.1.14.103.48dc6c91jfxiRq&id=527070300777&ns=1&abbucket=6#detail)  [环境配置参考01](https://www.sohu.com/a/216548608_796852)  [环境配置参考02](https://www.ncnynl.com/archives/201610/918.html) [代码参考](https://www.arduino.cn/thread-80395-1-1.html)

[TOC]



## 1.环境配置

### 1.1 硬件

淘宝链接可见 [link](https://item.taobao.com/item.htm?spm=a230r.1.14.103.48dc6c91jfxiRq&id=527070300777&ns=1&abbucket=6#detail) 以下数据仅供参考

红外漫反射式光电开关传感器E3F-DS30C4

电压 6~36V

感应范围 10cm - 30cm

建议安装高度: 离地面15cm左右

- 接线

  ![pic01](pic/infrared/pic01.png)

- 调整感应距离

  ![pic03](pic/infrared/pic03.png)

### 1.2 接线

褐色正极: 接正极

蓝色负极: 接负极

黑色信号线: 接数字信号0

### 1.3 软件

- 安装 `Arduino` IDE

  > sudo apt-get install arduino

  通过 `arduino` 语句打开IDE, 会自动在根目录添加目录 `sketchbook/`

- 安装 `Arduino` 开发库

  - 通过 `apt-get` 安装

    > sudo apt-get install ros-kinetic-rosserial-arduino
    > 
    > sudo apt-get install ros-kinetic-rosserial

  - 通过源码安装

    > cd <catkin_ws>/src
    >
    > git clone https://github.com/ros-drivers/rosserial.git
    >
    > cd <catkin_ws>
    >
    > catkin_make

- 关联 `ROS` 与 `Arduino` 库

  >  cd <sketchbook>/libraries
  >  
  >  rm -rf ros_lib   // 之前如果有可以删除
  >  
  > rosrun rosserial_arduino make_libraries.py .

  注意语句最后有一个 `.`

- 查看 `ros_lib` 库

  重启 arduino , 点击 `file` --- `ros_lib` 可以看到已经安装的库



## 2.程序说明

### 2.1 大致原理

`arduino` IDE将写好的程序通过 `upload` 写入 `arduino` 开发板的内存

使用 `rosrun rosserial_python serial_node.py _port:=<port_name>` 语句

调用 `ros` 包来启动开发板内存中的程序

### 2.2 注意端口权限

第一次进行 `upload` 操作时, 会弹出找不到端口的错误

这时需要给端口赋予权限

但同时会找不到存在的端口

需要进行如下操作

增加当前用户对串口的默认访问权限

> sudo usermod -a -G dialout 用户名

使UDEV配置生效

> sudo service udev reload
>
> sudo service udev restart

重启机器即可



### 2.3 源代码

```c++
#include <ros.h>
#include <ros/time.h>
#include <sensor_msgs/Range.h>

ros::NodeHandle  nh;
sensor_msgs::Range range_msg;
ros::Publisher pub_range( "infrared_data", &range_msg);

const int hongwai = 0;
int val = 0;

unsigned long range_timer;

int getRange(int pin_num){
    int sample;
    sample = analogRead(pin_num);
    return sample;
}

char frameid[] = "/infrared_frame";

void setup()
{
  nh.initNode();
  nh.advertise(pub_range);
  
  range_msg.radiation_type = sensor_msgs::Range::INFRARED;
  range_msg.header.frame_id =  frameid;
  range_msg.field_of_view = 0.01;
  range_msg.min_range = 0.01;
  range_msg.max_range = 10;
  
  pinMode(hongwai,INPUT);
  
}

void loop()
{
  val = digitalRead(hongwai);
  // publish the range value every 50 milliseconds
  //   since it takes that long for the sensor to stabilize
  if ( (millis()-range_timer) > 50){
    if (val==LOW){
      range_msg.range = val;
    } else {
      range_msg.range = val;
    }
//    range_msg.range = getRange(analog_pin);
    range_msg.header.stamp = nh.now();
    pub_range.publish(&range_msg);
    range_timer =  millis();
  }
  nh.spinOnce();
}
```



## 3.运行

将代码复制到IDE

点击 `upload` , 待成功后

依次运行如下命令

打开终端

> roscore

运行节点

> rosrun  rosserial_python serial_node.py _port:=<port_name>

接下来便可以使用`rostopic echo` 语句来查看数据



