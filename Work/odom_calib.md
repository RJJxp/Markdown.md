# 里程计标定

https://blog.csdn.net/qq_38338228/article/details/85263796

https://blog.csdn.net/qq_29230261/article/details/83146147

出于<激光slam理论与实践>

采用拟合的方法

使用激光定位作为真值对里程计进行在线标定



基于最小二乘的直线拟合, 虽然精度不高, 但实现简单

基于模型的方法, 非线性拟合, 精度高, 实现复杂

因此使用直线拟合

![img](https://img-blog.csdnimg.cn/20181226160257416.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MzM4MjI4,size_16,color_FFFFFF,t_70)



激光定位数据与里程计数据帧率不同

对此有两种解决方案

1. 如果激光定位数据足够多, 那么直接使用最小二乘进行拟合
2. 如果激光定位数据不够多, 则先对其进行曲线拟合, 重采样增多激光定位数据点