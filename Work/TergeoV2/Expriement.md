# 调车

[toc]

## 01. 连接驾驶脑

通过有线或无线的方式, 连接清扫车或洗地机中的路由器

密码多为 `tonglu` 或是 `12345678` 

完成路由器连接后, 确保本机和驾驶脑在同一局域网下

之后进行使用本机ssh连接驾驶脑

清扫车IP多为 `192.168.1.102` 密码 `12345678` 

洗地机IP多为 `192.168.1.125` 密码 `12345678` 

```bash
ssh tonglu@192.168.1.102
```

修改 `~/.bashrc` 

```bash
# 设置ROS主从机
export ROS_MASTER_URI=http://192.168.1.102:11311
```

修改 `/etc/hosts` 

```bash
# 加入不同车车型的ip, 如sw210-024
192.168.1.102    sw210-024
```

确保 `rostopic echo <topic>` 有数据



## 02. 模块开启与ROS连接

```bash
# 开启某一服务, 如感知模块
docker start tergeo_perception
# 停止某一服务, 如感知模块
docker stop tergeo_perception
#############################
cd /tergeo/bin
# 开启所有服务
./start.sh
# 停止所有服务
./stop.sh
# 进入镜像
./exec.sh tergeo_server
./run.sh tergeo_server bash
```



## 03. TergeoView 开启

``` bash
# 进入镜像
# 如果rostopic echo 失败, 修改镜像内的~/.bashrc文件
roslaunch tergeo_view tergeo_view.launch
```



## 04. 测试代码

以感知模块中的 `perception_plugins` 为例

```bash
# 变量以 <var> 表示
scp <perception_plugins> tonglu@192.168.1.102:<path>
scp <cmake> tonglu@192.168.1.102:<path>
scp <config> tonglu@192.168.1.102:<path>
# 进入驾驶脑
ssh tonglu@192.168.1.102
# 关闭正在运行的感知模块
docker stop tergeo_perception
# 进入镜像
cd /tergeo/bin/
./exec.sh tergeo_server
# 编译代码
cd <path>/<perception_plugins>
./build.sh
# 将生成.so替换镜像内部同名.so
./install.sh
# 修改感知的conf参数, 确保参数正确
...
# 启动感知模块
cd /tergeo/bash/start_modules
./start_perception.sh
```

测试完成后, 删除代码确保代码安全



## 05. 记录

LidarReceiveNode 有控制激光雷达输出线束的参数

如果路沿检测有关于 Height 报错, 可能是配置文件参数问题.