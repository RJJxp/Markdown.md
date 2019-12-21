# Sanchi(三驰) 惯导

传感器型号是 `100D2 `

[官方淘宝链接](https://item.taobao.com/item.htm?spm=a1z10.1-c-s.w137712-18788709158.6.43921d8232xB9w&id=564544478977) 资料比较齐全, 在此不赘述



## 驱动安装

已经安装在 `tergeo-indoor` 里了

具体目录在 `tongji/tergeo-indoor/ros-tergeo-indoor/src/sensing/drivers/imu` 中

是一个完整的 `ros` 包



## 运行

USB2.0 或是 USB3.0 连接, 即插即用

运行对应的 `launch` 文件

或是 `rosrun sanchi_amov sanchi_amov _port:=/dev/ttyUSB0 _model:=100D2 _baud:=115200` 从中也可以看到其波特率为 `115200` 

