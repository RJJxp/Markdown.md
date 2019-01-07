# 说明文档

raccoon项目依赖库安装说明

[TOC]

## 1.安装3rdparty 依赖库

需要先git clone 对应仓库 `tongji/tergeo-indoor/3rdparty-ubuntu16-slam`

然后对该仓库下各个库的依赖库以 `apt-get install` 的方式安装

参考链接

[Realsense Manual Installation](https://github.com/IntelRealSense/librealsense/blob/master/doc/installation.md) 注意 `Realsense` 的权限问题

[Realsense Linux Distribution](https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md) 

[Ceres-solver Installation](http://ceres-solver.org/installation.html)

[Protobuf Installation](https://github.com/protocolbuffers/protobuf/blob/master/src/README.md)

[Cartographer Document Installation](https://google-cartographer.readthedocs.io/en/latest/) 

```cmake
# realsense
sudo apt-get install libssl-dev libusb-1.0-0-dev libgtk-3-dev libglfw3-dev

# ceres-solver 
sudo apt-get install libgoogle-glog-dev	# google-glog + gflags
sudo apt-get install libatlas-base-dev	# BLAS & LAPACK
sudo apt-get install libeigen3-dev	# Eigen3
sudo apt-get install libsuitesparse-dev	# SuiteSparse

# protobuf

# cartographer
sudo apt-get install -y \
    clang \
    git \
    google-mock \
    libboost-all-dev \
    libcairo2-dev \
    libcurl4-openssl-dev \
    libeigen3-dev \
    libgflags-dev \
    libgoogle-glog-dev \
    liblua5.2-dev \
    libsuitesparse-dev \
    ninja-build \
    python-sphinx
```



## 2.安装raccoon 项目依赖库

需要先git clone 对应库 `tongji/tergeo-indoor/ros-tergeo-indoor`

- 安装ros 

  参考链接 [官方 wiki](http://wiki.ros.org/cn/kinetic/Installation/Ubuntu)

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

- 安装驱动

  ```cmake
  sudo apt-get install ros-kinetic-lms1xx	
  sudo apt-get install ros-kinetic-navigation	
  sudo apt-get install ros-kinetic-pointcloud-to-laserscan
  sudo apt-get install ros-kinetic-range-sensor-layer
  ```



## 3.进行build

cd到ros-tergeo-indoor中

运行 `./build.sh`