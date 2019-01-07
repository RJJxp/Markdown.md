# 第三方库说明文件

包含库见下表:

|      库      |  版本  |
| :----------: | :----: |
|    Eigen     | 3.3.4  |
| Cere-solver  | 1.13.0 |
|   Protobuf   | 3.4.1  |
| Cartographer | 1.0.0  |
|  Realsense   | 2.16.1 |

`Cartographer` 依赖 `Ceres-solver` , `Protobuf`

`Ceres-sovler` 依赖 `Eigen` 

`Realsense` 可以独立安装



## 安装 3rdparty 依赖库

需要先 git clone 对应仓库 `tongji/tergeo-indoor-3rdparty-ubuntu16-slam` 

然后对仓库内各个库的依赖库以 `apt-get install` 的方式安装

参考链接

[Realsense Manual Installation](https://github.com/IntelRealSense/librealsense/blob/master/doc/installation.md) 注意 `Realsense` 的权限问题

[Realsense Linux Distribution](https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md) 

[Ceres-solver Installation](http://ceres-solver.org/installation.html)

[Protobuf Installation](https://github.com/protocolbuffers/protobuf/blob/master/src/README.md)

[Cartographer Installation](https://google-cartographer.readthedocs.io/en/latest/) 

```cmake
# realsense
sudo apt-get install libssl-dev libusb-1.0-0-dev libgtk-3-dev libglfw3-dev

# ceres-solver 
sudo apt-get install libgoogle-glog-dev	# google-glog + gflags
sudo apt-get install libatlas-base-dev	# BLAS & LAPACK
sudo apt-get install libeigen3-dev	# Eigen3
sudo apt-get install libsuitesparse-dev	# SuiteSparse

# protobuf 无

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

