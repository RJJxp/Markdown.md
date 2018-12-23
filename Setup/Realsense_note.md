# Realsense 



## 驱动安装

系统内通过 `apt-get install` 安装三个依赖库

> sudo apt-get install libgtk-3-dev libglfw3-dev libusb-1.0-0-dev

和 `dpkg -i` 语句安装三个 `deb` 包

> librealsense2_2.16.5-0-realsense0.208_amd64.deb
> librealsense2-udev-rules_2.16.5-0-realsense0.208_amd64.deb
> librealsense2-dev_2.16.5-0-realsense0.208_amd64.deb

三个包的位置在 https://xiaolvyun.baidu.com/tongji/icode/repo/tongji%2Ftergeo-indoor%2F3rdparty-ubuntu16-slam/files/master/tree/sensor-drivers/ 可以看到

也就是在项目在编译的时候需要下载 `3rdparty-ubuntu16-slam` 文件夹下的 `sensor-drivers` 可以找到三个安装包



## 接线

直接USB接线