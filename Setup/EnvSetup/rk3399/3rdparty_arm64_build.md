# arm64 架构第三方库构建

直接从 x3399 配置环境移植过来

仅做参考

## 1. 要点注意

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

也需要在系统内部安装, 要安装两遍

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



## 2. 编译安装

### 2.1 Realsense编译安装

版本 2.18.1 https://github.com/IntelRealSense/librealsense/releases

- 需要安装的核心库

  `sudo apt-get install git libssl-dev libusb-1.0-0-dev pkg-config libgtk-3-dev`

  `sudo apt-get install libglfw3-dev`

- 编译安装

- 注意权限问题的解决

  ```cmake
  sudo cp config/99-realsense-libusb.rules /etc/udev/rules.d/
  sudo udevadm control --reload-rules && udevadm trigger
  ```

### 2.3 Ceres-solver编译安装

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



## 3. `3rdparty/install/` 

对于 `Raccoon/3rdparty/tergeo/` 的内容, 需要编译安装 `tergeo/tergeo` 

提取对应的链接文件和头文件