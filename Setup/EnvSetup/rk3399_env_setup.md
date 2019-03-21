# RK3399 开发板环境配置

[TOC]

参考链接

官网资料 http://wiki.t-firefly.com/zh_CN/EC-A3399C/started.html

烧制系统指南 http://wiki.t-firefly.com/AIO-3399C/started.html

资源下载 http://www.t-firefly.com/doc/download/page/id/54.html



## 1. 烧制系统

在 ubuntu 1604 LTS 系统中, 向 <font color=red>**arm架构64位**</font> 的 RK3399 开发板烧制 ubuntu 系统

开发板烧制系统: Ubuntu 1604 GPT

### 1.1 系统与烧制工具下载

在 [资源下载](http://www.t-firefly.com/doc/download/page/id/54.html) 页面可以找到 ubuntu 的下载链接

PS: 由于网盘资料一直在变, 酌情选择合适系统

下载的 `tar.gz` 文件解压后, 可见自带 `upgrade_tool` 工具

尽管 [烧制系统指南](http://wiki.t-firefly.com/AIO-3399C/started.html) 指出, 需要先使用特定版本的 `upgrade_tool` 擦除系统

再使用另一个特定版本的 `upgrade_tool` 烧制系统

但在实际烧制过程中, 可以直接使用 `tar.gz` 自带的 `upgrade_tool` 工具直接进行擦除与烧制

### 1.2 烧制目标系统

- 进入 `loader` 模式

  用 usb-typeC 接线连接笔记本拔下电源, 长按 `reset` 键, 接通电源, 1-2s 后松开 `reset` 键即可进入 `loader` 模式

- 烧制系统

  使用语句 `sudo chmod 777 upgrade_tool` , 先给 `upgrade_tool` 赋予权限

  使用命令 `sudo ./upgrade_tool` 启动烧制工具

  1. 进入 `loader` 模式成功后, 通常输入 `1` 选择开发板

     PS: 如果进入 `loader` 模式失败, 则没有目标设备可以选择

  2. 使用命令 `ef <img_path>` 擦除现有系统

  3. 使用命令 `uf <img_path>` 烧制目标系统

  烧制系统成功后会自动重启

- 2019-03-14 遇到问题

  在第三步结束后, 有 `download firmware failed` 的提示

  客服反馈, 如果开机直接进入系统, 可以忽略这个错误

  开机重启, 发现系统可以成功进入



## 2. 驱动检查

- 有线网驱动检查

  雷达数据通过网线传输, 作为至关重要的一个传感器, 必须保证数据的可接收

  终端 `ifconfig` 命令, 查看是否有 `eth` 开头的网络驱动信息

  <font color=red>如果没有, 直接放弃这一块板子</font>

- 无线网卡驱动检查

  通过 ROS 主从机实现远程控制, 需要主机和从机在同一 wifi 下

  开发板需要有开启移动热点的功能

  终端 `sudo apt-get install iw` 之后, 使用 `iw list` 命令

  查看在 `supported interface modes` 中, 是否有包含 `AP` 的关键字

  有, 则表示可以开启移动热点

  无, 则需要多拿一套路由器

  参考 https://blog.csdn.net/ac_dao_di/article/details/71908444 配置移动热点



## 3. 系统环境搭建

鉴于之前完成了 arm 端第三方库的源码编译

现在所需完成工作有: ROS 系统安装, 相机驱动安装, 其他依赖库的安装

### 3.1 安装 ROS

参考 [官方 wiki](http://wiki.ros.org/cn/kinetic/Installation/Ubuntu) 

```cmake
# 添加 sources.list
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

# 添加 keys
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116

# 安装
sudo apt-get update
sudo apt-get install ros-kinetic-desktop-full

# 初始化 rosdep
sudo rosdep init
rosdep update

# 环境配置
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc

# 构建工厂依赖
sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential
```

### 3.2 相机驱动安装

所使用的是 Realsense 相机, 通过源码编译的方式安装 librealsense 2.18.1

Github 网址: https://github.com/IntelRealSense/librealsense/releases 

下载源码, 进入目录, 如下操作安装即可

```cmake
# 安装 realsense 依赖库
sudo apt-get install libssl-dev libusb-1.0-0-dev libgtk-3-dev libglfw3-dev

# 源码编译安装
mkdir build
cd build 
cmake ..
make
install
```

### 3.3 其他依赖库安装

```cmake
sudo apt-get install ros-kinetic-lms1xx	
sudo apt-get install ros-kinetic-navigation	
sudo apt-get install ros-kinetic-pointcloud-to-laserscan
sudo apt-get install ros-kinetic-range-sensor-layer
```

