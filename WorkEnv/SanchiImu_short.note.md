# Sanchi(三驰) 惯导

虽然售后很垃圾, 但是配置环境还是相对简单

传感器型号是 `100D2 `

坐标系可以在淘宝官网找到, 可见链接

https://item.taobao.com/item.htm?spm=a1z10.1-c-s.w137712-18788709158.6.43921d8232xB9w&id=564544478977



## 驱动安装

已经安装在 `tergeo-indoor` 里了

具体目录在 `tongji/tergeo-indoor/ros-tergeo-indoor/src/sensors/imu/sanchi_amov/`

是一个完整的 `ros` 包



## 运行

使用对应的 `launch` 文件

或者 `rosrun sanchi_amov sanchi_amov _port:=/dev/ttyUSB0 _model:=100D2 _baud:=115200`