# Tergeo Mapping

## 01. 修改参数

在本地 `/tergeo/conf/modules/mapping2/` 中

- `surveyor_mapping.conf` 中, 有**标定参数**和**激光雷达类型** 
- `ros_service_mapping.conf` 中, 存放建图结果的路径



## 02. 数据预处理

通过设置 ROS 参数提取关键帧

```bash
roscore
# new terminal
/tergeo/bin/run.sh tergeo_server bash
# or `/tergeo/bin/exec.sh tergeo_server` to get the image 
# use param `1` to start receiving data and pre-process, take the key-frame
rosservice call 1
# play bag
rosbag play <bag>
# when bag is finished, call param `2` to stop receiving the data
rosservice call 2
# start mapping
rosservice call 3
```



## 03. 开始建图

```bash
/tergeo/bin/run.sh tergeo_server bash
# 或 /tergeo/bin/exec.sh tergeo_server 进入镜像
cd /tergeo/bash/start_modules/
./start_lidar_mapping.sh
```

当出现 `Mapper: Map3dDEMProducerfinish` 

建图结果保存在所设置的路径 `/data/mapping` 中