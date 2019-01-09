#  EC-A3288C 嵌入式主机刷机

本教程是在 Ubuntu 16.04 LTS 系统下, 对 [EC-A3288C 嵌入式主机](http://www.t-firefly.com/product/eca3288c?theme=pc) 进行 Ubuntu 系统的烧制

并且 [EC-A3288C 嵌入式主机](http://www.t-firefly.com/product/eca3288c?theme=pc) 的系统是安卓的情况下

参考链接如下: 

 [EC-A3288C 嵌入式主机](http://www.t-firefly.com/product/eca3288c?theme=pc) 

 [EC-A3288C 嵌入式主机资料下载页面](http://www.t-firefly.com/doc/download/58.html) 

 [AIO-3288C 高性能主板](http://www.t-firefly.com/product/industry/aio_3288c.html?theme=pc) 

 [AIO-3288C 刷机教程](http://wiki.t-firefly.com/zh_CN/AIO-3288C/upgrade_firmware-preface.html) 



## 1.固件下载与刷机软件下载

对比发现, 从 [EC-A3288C 嵌入式主机资料下载页面](http://www.t-firefly.com/doc/download/58.html) 下载的 Ubuntu 系统是 GPT 格式

EC-A3288C 嵌入式主机并没有教程, 所以到其主板 AIO-3288C 处寻找教程

刷机软件, 官方教程给的是 upgrade_tool_v1.24 和 upgrade_tool_v1.34

但是软件可以下载的只有 upgrade_tool_v1.33 和 upgrade_tool_v1.34

所以 upgrade_tool_v1.24 需要自己下载

由于之前配置 x3399 主机时, 使用的就是 upgrade_tool_v1.24, 直接使用就好

upgrade_tool_v1.24 下载链接 [百度网盘链接](https://pan.baidu.com/s/1dGsrPKH) 密码 ryim



# 2.进入刷机模式

拔掉电源, 用两个 USB 公头连接自己的电脑和嵌入式主机

用针顶住 POWER 键左方小孔里面的 RECOVERY 键不要松开

插上电源, 2-3秒后松开 RECOVERY 键

即可进入刷机模式



## 3.烧制系统

官方建议烧制流程

```cmake
# 1.用upgrade_tool_v1.24工具，要进行擦出flash，则运行：
sudo upgrade_tool_v1.24 ef <your_img_path>
# 2.上述完成后，用upgrade_tool_v1.34工具，烧写Ubuntu16.04 GPT固件，运行以下：
sudo upgrade_tool_v1.34 uf <your_img_path>
```

