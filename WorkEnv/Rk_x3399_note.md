# X3399 开发板环境配置

向X3399的emmc中烧制系统Ubuntu 1604 LTS

在Ubuntu1604的系统下进行

并对自身raccoon进行环境的配置

[TOC]

## 1.配置 Ubuntu 16.04 LTS

下载镜像, 熟悉烧盘软件的使用, 烧盘

烧盘选择向内部的存储器烧制, 而不是tf卡

### 1.1 镜像下载

使用x3399官网给的[百度网盘链接](https://pan.baidu.com/s/1dGsrPKH) 密码 ryim

目录`/DVD_X3399/Image/x3399-ubuntu-2017-06-19`

下载后解压即可

至此, 系统镜像已经准备好

### 1.2 进入刷机模式

在X3399中, `vol+` 为 `recover` 键 

拔了电源线和usb_typeC线

按住 `recover` 键的同时, 接上电源线和usb_typeC线

此时应该会自动弹出x3399文件夹窗口

表示进入刷机模式成功

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

之前架构是amd64, 现在构架是arm64

依赖库有以下

> Realsense:		librealsense-2.16.1
>
> Eigen:			eigen-3.3.4
>
> Ceres-solver:		ceres-1.13.0
>
> Protocolbuffers:	protobuf-3.4.1
>
> Cartographer:	cartographer-1.0

Realsense可以独立安装

安装Cartographer, 需要Ceres-solver, Protocolbuffers

安装Ceres-solver, 需要Eigen

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

<font color=red>**注意 Point-02**</font>

建议在编译安装每个库的的时候, 从其 `Github` 找对应版本的源码安装包

并在 `README.md` 中, 查看其推荐的网址

这类网址一般都有依赖库, 设备使用的条件等等关键信息

自己写的教程基本都是参照其官网对应的说明文件

若出现不明白的地方, 请自行查阅官网文档

<font color=red>**注意 Point-03**</font>

编译安装好的库需要在外部存贮器备份

以防编译机子崩溃, 循环启动, 无法开机

此种方法只能通过刷机解决, 刷机会格式化

如果在外部存储设置中出现文件格式不支持, 可以打包成 `.tar.gz` 再备份



### 2.1 Realsense编译安装

版本 2.16.1 [下载链接](https://github.com/IntelRealSense/librealsense/archive/v2.16.1.zip) 

参考 [Realsense Github Release](https://github.com/IntelRealSense/librealsense/releases)  [Realsense Github 安装说明](https://github.com/IntelRealSense/librealsense/blob/master/doc/installation.md)  [官网Q&A](https://github.com/IntelRealSense/librealsense/wiki/Troubleshooting-Q&A#q-i-connected-the-camera-to-the-usb-port-but-it-is-not-recognized) 

- 需要安装的核心库

  `sudo apt-get install git libssl-dev libusb-1.0-0-dev pkg-config libgtk-3-dev`

  `sudo apt-get install libglfw3-dev`

- 编译安装

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
  ```

- 编译安装

### 2.4 Protocolbuffers编译安装

版本 3.4.1 [下载链接](https://github.com/protocolbuffers/protobuf/archive/v3.4.1.zip)

参考 [Protobuf Github Release](https://github.com/protocolbuffers/protobuf/releases)  [Protobuf Installation](https://github.com/protocolbuffers/protobuf/blob/master/src/README.md)

- 依赖库安装

  ```
  sudo apt-get install autoconf automake libtool curl make g++ unzip
  ```

- 编译安装

  ```cmake
  # 如果命令不能自动补全
  # 使用 sudo chmod 777 <filename> 赋予权限
  
  # 进行此步是为了./configure可行
  ./autogen.sh 
  # 可在后面加入 --prefix 来设置安装路径
  ./configure --prefix=<YourInstallPath>
  make
  make check	# 时间紧迫就不要check了
  sudo make install
  sudo ldconfig	# refresh shared library cache.
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




### 2.5 Cartographer编译安装

版本 1.0.0 [下载链接](https://github.com/googlecartographer/cartographer/archive/1.0.0.zip)

参考 [Cartographer Github Release](https://github.com/googlecartographer/cartographer/releases)  [Cartographer Document Installation](https://google-cartographer.readthedocs.io/en/latest/) 

