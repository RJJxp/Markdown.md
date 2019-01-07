# Realsense 

参考链接

 [Realsense Manual Installation](https://github.com/IntelRealSense/librealsense/blob/master/doc/installation.md) 

 [Realsense Linux Distribution](https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md) 

 [官网Q&A](https://github.com/IntelRealSense/librealsense/wiki/Troubleshooting-Q&A#q-i-connected-the-camera-to-the-usb-port-but-it-is-not-recognized) 



## 驱动安装

系统内通过 `apt-get install` 安装三个依赖库

> sudo apt-get install libgtk-3-dev libglfw3-dev libusb-1.0-0-dev

amd64架构的机器需要 `dpkg -i` 语句安装三个 `.deb` 包, arm64架构的机器则不需要

> librealsense2_2.16.5-0-realsense0.208_amd64.deb
> librealsense2-udev-rules_2.16.5-0-realsense0.208_amd64.deb
> librealsense2-dev_2.16.5-0-realsense0.208_amd64.deb

三个包的位置在 

`tongji/tergeo-indoor/3rdparty-ubuntu16-slam/sensor/drivers/librealsense-2.16.1` 



## 接线

直接 USB 接线

根据  [官网Q&A](https://github.com/IntelRealSense/librealsense/wiki/Troubleshooting-Q&A#q-i-connected-the-camera-to-the-usb-port-but-it-is-not-recognized) , 只有 USB3.0 才可以使用 `Realsense` , 只有 USB2.0 的设备直接可以放弃

要注意权限的问题 [Realsense Manual Installation](https://github.com/IntelRealSense/librealsense/blob/master/doc/installation.md) 

```cmake
sudo cp config/99-realsense-libusb.rules /etc/udev/rules.d/ 
sudo udevadm control --reload-rules && udevadm trigger
```

