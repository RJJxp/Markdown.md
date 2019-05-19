## 1. 相机的使用

在文件 `realsense-2.2.0/realsense2_camera/launch/includes/nodelet.launch.xml` 修改其中参数

> <arg name="pointcloud_texture_stream" default="RS2_STREAM_COLOR"/>

使相机可以使用RGB彩色图像



相机支持的分辨率和帧数如下

|  分辨率  |       帧数        |
| :------: | :---------------: |
| 1280*720 | 6, 15, 30, 60, 90 |
| 840*480  | 6, 15, 30, 60, 90 |
| 640*480  | 6, 15, 30, 60, 90 |
| 640*360  | 6, 15, 30, 60, 90 |
| 480*270  | 6, 15, 30, 60, 90 |
| 424*240  | 6, 15, 30, 60, 90 |
| 320*240  | 6, 15, 30, 60, 90 |
| 320*280  | 6, 15, 30, 60, 90 |

注意: D435支持的最低配置, USB3.0 424 \* 240 \* 60fps

realsense 相机一致采用 USB 进行数据传输, 最好使用 USB3.0

根据自己需求修改相机配置文件, 运行节点

```cmake
roslaunch realsense2_camera rs_camera.launch
```

查看和保存图像文件

```cmake
rosrun image_view image_view image:=<image topic>
rosrun image_view image_saver image:=<image topic>
```



## 2. 相机的标定

http://wiki.ros.org/camera_calibration/Tutorials/MonocularCalibration 

http://wiki.ros.org/image_view 

使用 ROS 的 `camera_calibration` 包

```cmake
# --size mxn 中间的是小写字母ｘ
# --square 是用棋盘格标定时, 棋盘格边长
rosrun camera_calibration cameracalibrator.py --size 8x6 --square 0.108 image:=/my_camera/image camera:=/my_camera  --no-service-check
```



使用 matlab 中的视觉工具进行标定

