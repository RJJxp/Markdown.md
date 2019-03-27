# 初始化并运行建图模块

大体分为两个步骤

一是相关模块的配置, 二是运行建图的命令

现阶段是使用 USB2CAN 的硬件

后期要改成 CAN2RS232, 所以本文档只是长时间的临时文档



## 1. 环境配置

之前的 `Raccoon/3rdparty/tergeo/lib` 都是基于 `amd64` 架构

在 `arm64` 架构的 RK3399 下需要对之前 `amd64` 架构的链接和可执行文件进行重新编译

否则无法进行底盘的通信

### 1.1 安装 `ros_msg` 

需要将 `ros_msg` 安装进入系统, 是整个项目运行的基础

如果不安装, 会在其他模块编译构建时报错

- 克隆 `tergeo/ros_msg` , 切换分支到 `raccoon` , 并 `git pull` 拉取最新版本

- 确认架构

  在 `./DEBIAN/control` 中根据自己系统修改 `Architecture` 参数

   `Architecture: amd64` 或者是 `Architecture: arm64`  

- 构建并安装

  ```cmake
  ./build.sh
  sudo ./install.sh
  sudo dpkg -i xxx.deb
  ```

### 1.2 配置 `3rdparty/tergeo`  

- 编译 `tergeo/tergeo/` 

  切换 `raccoon` 分支, 并 `git pull` 拉取最新版本

  运行 `./build.sh` 直接编译构建

- 拷贝复制相应文件

### 1.3 配置 `install/` 

原则是, 在 `tegeo/` 中的模块切换到 `raccoon` 分支, 编译安装

 `Raccoon` 中的模块, 直接编译安装

注意编译安装, 确定自己是最新版本

每个部分编译安装时, 查看脚本 `build.sh` 和 `install.sh` 内容

了解具体安装路径, 并将安装文件整理到 `Raccoon/install` 模块下

安装结果中的 `conf` 不用管, 只需要更新对应的链接库和可执行文件

- 编译 `tergeo/tergeo/` 
- 编译 `tergeo/can_adapter` 
- 编译 `tergeo/guardian` 
- 编译 `Raccoon/vehicle_plugin` 

### 1.4 设置 usb 权限

在 `/etc/udev/rules.d/` 中新建 `99-myusb.rules` 文件

文件名一定以 `99` 开头, 文件内容为:

```cmake
##
ACTION=="add",SUBSYSTEMS=="usb", ATTRS{idVendor}=="04d8", ATTRS{idProduct}=="0053", GROUP="users", MODE="0777"
```

插拔 USBCAN 或是重启电脑即可不使用 sudo 命令运行权限程序



## 2. 运行命令

请保证环境配置所有步骤都成功进行

### 2.1 设置临时环境变量

根据自己库的地址, 终端输入命令

```cmake
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:'/home/rjp/tongji/Raccoon/install/lib':'/home/rjp/tongji/Raccoon/install/plugin/canbus'
```

设置临时环境变量, 即每次新开终端都需要输入以上命令

### 2.2 实现底盘远程操控

确保底盘上电, 雷达正常工作, USBCAN接入

- 连接底盘

  `roscore` 

  `Raccoon/install/` 中 `./bash/start_vehicle.sh` 连接底盘

  `Raccoon/install/` 中 `./bash/start_guardian.sh` 启动 `guardian` 通信

- 远程操控

  进入 `Raccoon/driver` 终端 `roslaunch raccoon_teleop keyboard_teleop.launch`   

### 2.3 建图

- 连接雷达

  修改自己 ip 地址

  使用 sick 雷达, 修改地址为 `192.168.0.5` 

  使用 furay 雷达, 修改地址为 `192.168.1.5` 

  进入 `Raccoon/driver` 终端 `roslaunch sick sick.launch ` 

- 运行建图

  进入 `Raccoon/mapping` 终端 `roslaunch raccoon mapping.launch` 

  修改坐标框架为 `laser` 

  同时切换到远程操控终端, 远程控制洗地机同时建图定位