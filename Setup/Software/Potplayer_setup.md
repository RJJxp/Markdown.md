# 2018-12-17 14:57:10

第二次硬解potplayer

记录一下怎么捣鼓

如果在没有网络的情况下

貌似说的有点矛盾, 从 `github` 里面 `clone` 也是需要网络的

但无论如何, 还是记录一下

软件版本 `Potplayer 64bit 1.7.14804`



## 滤镜设置

- 滤镜的激活条件 "不使用"
  ![pic01](pic/potplayerSetup/pic01.png)

- 在 “滤镜” --- “内置OpenCodec” 中设置 “NVIDIA CUDA解码器” 为 “无条件使用”

  ![pic02](pic/potplayerSetup/pic02.png)

- 在 “滤镜” --- “视频解码器” --- "内置解码器设置"

  ![](pic/potplayerSetup/pic03.png)

  "使用硬件加速" 打勾, 其他保持默认, 如红框所示

  ![pic04](pic/potplayerSetup/pic04.png)



## 视频设置

视频设置很简单, 只需要把解码器设置一下就可以

"视频渲染器" 设置为 "EVR(Vista/.Net3)"

![pic05](pic/potplayerSetup/pic05.png)