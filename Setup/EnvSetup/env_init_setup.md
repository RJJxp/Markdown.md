# Raccoon 项目环境配置

本文档旨在说明

在Ubuntu1604的系统下, 向<font color=red>**基于arm64架构的X3399**</font>的emmc中烧制系统Ubuntu 1604 LTS

并在X3399上对项目 `Raccoon` 的环境进行配置

对于amd64架构的机器, 大同小异

Ubuntu 1604 LTS极易安装, 只进行 [2.配置Raccoon工作环境](#2.配置Raccoon工作环境) 即可

[TOC]

## 1.配置 Ubuntu 16.04 LTS

<font color=red>**在基于arm64架构的机器X3399上配置Ubuntu 1604 LTS**</font>

下载镜像, 熟悉烧盘软件的使用, 烧盘

烧盘选择向内部的存储器烧制, 而不是tf卡

正常电脑架构是amd64, 安装 Ubuntu 1604 LTS 自行百度

### 1.1 镜像下载

使用x3399官网给的[百度网盘链接](https://pan.baidu.com/s/1dGsrPKH) 密码 ryim

目录`/DVD_X3399/Image/x3399-ubuntu-2017-06-19`

下载后解压即可

至此, 系统镜像已经准备好

### 1.2 进入刷机模式

在X3399中, `vol+` 为 `recover` 键 

拔了电源线和usb_typeC线

按住 `recover` 键的同时, 接上电源线和usb_typeC线

屏幕会黑掉, 表示进入刷机模式成功

### 1.3 烧制系统

使用x3399官网给的[百度网盘链接](https://pan.baidu.com/s/1dGsrPKH) 密码 ryim

目录 `DVD_X3399/tools/x3399烧写工具/linux` 

下载 `Linux_Upgrade_Tool_v1.24.zip` 并解压

这是我们要使用的烧制工具 `upgrade_tool`

终端直接 `cd` 到解压目录, 并运行 `./upgrade_tool` 

若失败, 可能是由于权限原因, 使用语句 `sudo chmod 777 ./upgrade_tool` 赋予权限

再次运行 `./upgrade_tool` 若有报错

`error while loading shared libraries: libstdc++.so.6: cannot open shared object file: No such file or direcory`

说明缺少库文件, 使用 `sudo apt-get install libstdc++6` 和 `sudo apt-get install lib32stdc++6` 安装库

再次使用 `./upgrade_tool` 应该可以成功使用烧制软件

运行 `uf <YourImgFile>` 后者为 `.img` 的路径

例如 `uf /home/rjp/tmp/upgrade_ubuntu.img` 等待烧制

成功后, 系统会重启, Ubuntu便成功烧制到x3399开发板 



## 2.配置Raccoon工作环境

<font color=red>**amd64架构和arm64架构的机器配置环境的流程一模一样**</font>

依赖库有以下

|      库      |  版本  |
| :----------: | :----: |
|    Eigen     | 3.3.4  |
| Cere-solver  | 1.13.0 |
|   Protobuf   | 3.4.1  |
| Cartographer | 1.0.0  |
|  Realsense   | 2.16.1 |

`Realsense` 可以独立安装

安装 `Cartographer` , 需要 `Ceres-solver` , `Protocolbuffers`

安装 `Ceres-solver` , 需要 `Eigen`

<font color=red>**注意 Point-01**</font> 

为了获得lib, bin, share, include文件夹, 不建议直接安装到系统中

在库的目录下

```
mkdir build
cd build
cmake .. -DCMAKE_INSTALL_PREFIX=<YourInstallPath>
make
sudo make install
```

安装到制定路径

编译 `Ceres-solver` , `Cartographer` 时, 会默认从系统查找依赖库

需要再把需要的依赖库安装一次到系统上即可

也就是 `Eigen` , `Ceres-solver` , `Protobuf` 需要在制定路径安装

也需要在系统内部安装, 要安装两边

```cmake
cd build
cmake .. -DCAME_INSTALL_PREFIX=/usr/local/
make
sudo make install
```

<font color=red>**注意 Point-02**</font>

建议在编译安装每个库的的时候, 从其 `Github` 找对应版本的源码安装包

并在 `README.md` 中, 查看其推荐的网址

这类网址一般都有依赖库, 设备使用的条件等等关键信息

自己写的教程基本都是参照其官网对应的说明文件

若出现不明白的地方, 请自行查阅官网文档

<font color=red>**注意 Point-03**</font>

编译安装好的库需要在外部存贮器备份

以防编译机子崩溃(内存太小), 循环启动, 无法开机

此种问题只能通过刷机解决, 而刷机会格式化, 数据清空, 意味着之前安装好的库没了

如果在外部存储设置中出现文件符号格式不支持, 先打包成 `.tar.gz` 再备份



### 2.1 Realsense编译安装

版本 2.16.1 [下载链接](https://github.com/IntelRealSense/librealsense/archive/v2.16.1.zip) 

参考 [Realsense Github Release](https://github.com/IntelRealSense/librealsense/releases) 

​        [Realsense Manual Installation](https://github.com/IntelRealSense/librealsense/blob/master/doc/installation.md)  [Realsense Linux Distribution](https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md) 

​        [官网Q&A](https://github.com/IntelRealSense/librealsense/wiki/Troubleshooting-Q&A#q-i-connected-the-camera-to-the-usb-port-but-it-is-not-recognized) 

- 需要安装的核心库

  `sudo apt-get install git libssl-dev libusb-1.0-0-dev pkg-config libgtk-3-dev`

  `sudo apt-get install libglfw3-dev`

- 编译安装

- 注意权限问题的解决

  ```cmake
  sudo cp config/99-realsense-libusb.rules /etc/udev/rules.d/
  sudo udevadm control --reload-rules && udevadm trigger
  ```

### 2.2 Eigen编译安装

版本 3.3.4 [下载链接](http://bitbucket.org/eigen/eigen/get/3.3.4.tar.bz2)

参考 [Eigen 主页](http://eigen.tuxfamily.org/index.php?title=Main_Page)

- 直接编译安装

###　2.3 Ceres-solver编译安装

版本 1.13.0 [下载链接](https://github.com/ceres-solver/ceres-solver/releases/tag/1.13.0)

参考 [Cers-solver Github Release](https://github.com/ceres-solver/ceres-solver/releases)  [Ceres-solver Installation](http://ceres-solver.org/installation.html)

- 依赖库安装

  ```cmake
  # CMake
  sudo apt-get install cmake
  # google-glog + gflags
  sudo apt-get install libgoogle-glog-dev
  # BLAS & LAPACK
  sudo apt-get install libatlas-base-dev
  # Eigen3
  sudo apt-get install libeigen3-dev
  # SuiteSparse and CXSparse (optional)
  # - If you want to build Ceres as a *static* library (the default)
  #   you can use the SuiteSparse package in the main Ubuntu package
  #   repository:
  sudo apt-get install libsuitesparse-dev
  # - However, if you want to build Ceres as a *shared* library, you must
  #   add the following PPA:
  sudo add-apt-repository ppa:bzindovic/suitesparse-bugfix-1319687
  sudo apt-get update
  sudo apt-get install libsuitesparse-dev
  ```

  其中可以看到将 `SuiteSparse` 和 `CXSparse` 作为可选项

  本意是, `SuiteSparse` 库必须安装, 至于安装为何种形式, 看你自己

  在此, 我们默认安装为静态

  如果不安装, 后面cartographer的编译安装会提示你缺少 `SuiteSparse` 库, 就很难受

- 编译安装

### 2.4 Protocolbuffers编译安装

版本 3.4.1 [下载链接](https://github.com/protocolbuffers/protobuf/archive/v3.4.1.zip)

参考 [Protobuf Github Release](https://github.com/protocolbuffers/protobuf/releases)  [Protobuf Installation](https://github.com/protocolbuffers/protobuf/blob/master/src/README.md)

- 依赖库安装

  ```
  sudo apt-get install autoconf automake libtool curl make unzip
  ```

- 编译安装

  ```cmake
  # 官网编译安装方式 rviz在使用的时候会出问题
  # 找到的原因是Protobuf的编译方式有问题
  # 可能是某些不必要的库影响了rviz的某功能
  
  # # 如果命令不能自动补全
  # # 使用 sudo chmod 777 <filename> 赋予权限
  
  # # 进行此步是为了./configure可行
  # ./autogen.sh 
  # # 可在后面加入 --prefix 来设置安装路径
  # ./configure --prefix=<YourInstallPath>
  # make
  # make check	# 时间紧迫就不要check了
  # sudo make install
  # sudo ldconfig	# refresh shared library cache.
  
  
  # 使用cmake更加可控
  # 在此编译方式更加适合项目
  mkdir install
  cmake ../cmake/ -DCMAKE_BUILD_TYPE=Release -Dprotobuf_BUILD_TESTS=OFF -DCMAKE_INSTALL_PREFIX=<YourInstallPath>
  make 
  make install
  ```

  所遇到问题 `make check` 步骤出现错误

  > fail: google/protobuf/compiler/zip_output_unittest.sh
  >
  > fail: google/protobuf/io/gzip_stream_unittest.sh

  不知道怎么解决

  根据 [csdn 网址](https://blog.csdn.net/ShaoDu/article/details/85256797) 博主指出可能是版本问题, 重新来一遍就没有问题

  19:43 - 20:14 make finished

  20:14 - 20:55 make check finished

  之前使用 `Protobuf-3.4.1` 失败

  现在使用 `Protobuf-cpp-3.4.1` 成功

  并不能解释原因, 猜想是因为安装包的问题所以成功

  第二遍安装到系统的时候, 会报错


### 2.5 Cartographer编译安装

版本 1.0.0 [下载链接](https://github.com/googlecartographer/cartographer/archive/1.0.0.zip)

参考 [Cartographer Github Release](https://github.com/googlecartographer/cartographer/releases)  [Cartographer Document Installation](https://google-cartographer.readthedocs.io/en/latest/) 

- 安装依赖库

  ```cmake
  sudo apt-get install -y \
      clang \
      g++ \
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

- 正常编译安装

### 2.6 安装ROS

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

