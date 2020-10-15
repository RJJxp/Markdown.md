# Tergeo 感知模块环境配置

由于本机拥有Nvidia显卡 GTX-1060

智驾系统 `Tergeo` 的 `tergeo-view` 工具无法在 `docker` 镜像中运行

出于调试需求, 必须在本机环境下(而非 `docker` 镜像中)配置 `Perception` 模块所需库环境



[toc]



## 0. Docker安装与环境配置

 [官网安装教程](https://docs.docker.com/engine/install/ubuntu/) **Ubuntu系统** 

官方提供了三种安装docker的途径

- 使用源安装

- 使用 `.deb` 安装(个人推荐)

   [安装包下载地址](https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/) **对于Ubuntu1804LTS, amd64的 `.deb` 的下载地址** 

  需要安装 `container.io` , `docker-ce-cli` , `docker-ce` 

   `docker-ce-cli` , `docker-ce` 两者版本需要保持一致

  推荐使用较新的版本 `19` , 对显卡的支持也比较好

  `container.io` 版本较新即可

- 使用一键脚本安装

安装之后需要对 docker 权限等环境进行配置 [官方配置教程](https://docs.docker.com/engine/install/linux-postinstall/) 

```bash
# 1. create the `docekr` group
sudo groupadd docker
# 2. add your user to the `docker` group
sudo usermod -aG docker $USER
# 3. Log out and log back in so that your group membership is re-evaluated.
#      If testing on a virtual machine, 
#      it may be necessary to restart the virtual machine for changes to take effect.
#      On a desktop Linux environment such as X Windows, 
#      log out of your session completely and then log back in.
#      On Linux, you can also run the following command to activate the changes to groups:
newgrp docker
# 4. Verify that you can run `docker` commands without `sudo`
docker run hello-world
```

在安装 `docker` 并完成配置环境后, 需要安装 `docker-compose` [官方安装教程](https://docs.docker.com/compose/install/) 

```bash
# 1.Run this command to download the current stable release of Docker Compose:
#     To install a different version of Compose, 
#      substitute 1.27.4 with the version of Compose you want to use.
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# 2. Apply executable permissions to the binary:
sudo chmod +x /usr/local/bin/docker-compose
# 3. Test the installation
docker-compose --version
```



## 1. Tergeo 镜像安装

在 `tergeo-release-hub` 中的制品库 `deb` 中

下载最新 `tergeo.deb` 和 `tergeo-base.deb` 并安装

同时, 为了之后的镜像管理, 将旧镜像全部删除

提取镜像内容至本机目录 `/tergeo` 下

```bash
# 进入image, 会生成一个实例的 container
docker run -it tergeo_amd64:release
# 启动container
docker start <container>
# 进入contrainer 确定环境的路径
docker exec -it <container> /bin/bash
# 将container目录 /tergeo 的内容拷贝至本机 /tergeo 目录下
docker cp <container>:/tergeo <tmp_dir>
cp -rf <tmp_dir> /tergeo
# 删除container
docker rm <container>
```



## 2. Tergeo Perception 模块依赖环境配置

### 2.1 Protobuf

TODO:

### 2.2 PCL

 [官网安装教程](https://pointclouds.org/downloads/#linux) 本机当前版本为 `1.8` 

```bash
sudo apt install libpcl-dev
```

### 2.3 Ceres 

 [官方安装教程](http://ceres-solver.org/installation.html) 本机当前版本为 `1.14.0` 

依赖环境安装

```bash
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

之后正常编译安装即可









