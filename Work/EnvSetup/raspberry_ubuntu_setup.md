# 树莓派3b+配置Ubuntu mate 16.04

<font color=red>**树莓派3b+只有USB2.0, 该实验中道崩殂**</font>

所需硬件

1. 树莓派3b+ 开发板
2. 32g手机tf卡



[TOC]

## 1.系统安装

总共分为三大步骤: 下载安装包, 格式化tf卡, 烧盘

### 1.1 Windows下安装

- 下载安装包

  在[树莓派 Ubuntu mate官网](https://ubuntu-mate.org/raspberry-pi/)可以下载 `.img` 镜像

- 格式化tf卡

  相比于Ubuntu, windows在此步骤中是非常友好的

  在 `我的电脑` 右键, 选中 `管理` , 在磁盘分区里面, 把tf卡(U盘)的分区删除, 新建一个磁盘分区

  记住, 一定要重新插拔tf卡(U盘), 因为分区名称可能会变

  然后在对u盘进行一遍格式化

  如果自己手动不放心, 可以使用软件SD Formatter 4.0进行一键格式化

- 烧盘

  不同于Ubuntu系统, windows系统下, 烧盘需要借用软件Win32 Disk Manager

  选择需要烧制的 `.img` 和要烧制的磁盘

  等待烧盘完成即可

  成功后, 重新插拔tf卡(U盘), 会出现不能识别的一个磁盘和一个名为boot的磁盘

### 1.2 Ubuntu下安装

- 下载安装包

  在[树莓派 Ubuntu mate官网](https://ubuntu-mate.org/raspberry-pi/)可以下载 `.img` 镜像

- 格式化tf卡

  - 清理tf卡(U盘)分区

    参考链接 https://www.linuxidc.com/Linux/2013-05/85115.htm

    插入tf卡, 使用 `fdisk` 语句来进行格式化

    在终端使用 `sudo fdisk -l` 查看所有磁盘分区信息

    可以看到自己的tf卡(U盘)的分区, 一般为 `/dev/sdx`

    进行tf卡分区管理, 使用语句 `sudo fdisk <YourTfCard>`

    根据提示输入 `m` 可以看到各种命令, 主要使用以下指令:

    ```c++
    d // delete a partition
    n // add a new partition
    p // print the partition
    w // write table to disk and exit
    ```

  ​	先使用 `d` 将所有的分区删除干净

  ​	再使用 `n` 建立一个新的分区

  ​	最后使用 `w` 保存分区的更改

  ​	中间可以随时使用 `p` 查看分区信息

  - 格式化tf卡

    终端输入 `sudo mkfs.<Form> <YourTfCard> ` 语句进行格式化

    如将 `/dev/sdc` 格式化为 `ext4` 格式的语句为

    `sudo mkfs.ext4 /dev/sdc`

    注意, 参数是磁盘不是分区, 所以是 `/dev/sdc` 而不是 `/dev/sdc1`

    ubuntu一般的文件格式是ext4

    在格式化的时候无需担心磁盘格式

    不用刻意地格式化为ext4

    格式化为ntfs非常慢, 不推荐, 怎么快怎么来

    只需要一个空的tf卡就可以了

- 烧系统盘, 简称烧盘

  ```c++
  // 安装烧盘软件
  sudo apt-get install gddrescue xz-utils
  // 解压下载的.img.xz镜像
  // 这一步也可以用ubuntu自带的解压, 都可以
  unxz ubuntu-mate-16.04.2-desktop-armhf-raspberry-pi.img.xz
  // 开始烧盘, 第一个参数是.img文件, 第二个参数 /dev/sdx 是硬盘
  sudo ddrescue -D --force ubuntu-mate-16.04.2-desktop-armhf-raspberry-pi.img /dev/sdx
  ```

  烧盘如果成功, 插拔tf卡, 应该会变成两个分区, 其中一个是boot

### 1.3 常见问题

各种踩过的坑, 巨坑, 天坑

#### 1.3-1 彩虹屏

第一次在树莓派安装Ubuntu, 碰到这种问题

怎么搜也搜不到这种问题的解决方案, 偶尔看到了一篇彩虹屏的解决方案, 就点进去看了看

那就是我要找的解决方案

问题是, 谁会管这种方块屏叫做彩虹屏, 哪个人第一次遇到这种情况会用'彩虹屏'关键词进行搜索?

参考链接 https://blog.csdn.net/weixin_42429385/article/details/83537377

![他们所说的彩虹屏](pic/Raspberry/01.jpeg)

出现此现象的原因是由于Ubuntu mate的驱动不兼容

需要将树莓派原生系统里面 `/boot` 中

```cpp
bootcode.bin
config.txt
fixup.dat
start.elf
```

四个文件覆盖到烧好盘的Ubuntu mate系统的 `/boot` 中

启动即可正常

获取此四个文件, 需要先在tf卡上, 先把树莓派的原生系统烧制一遍

重新插拔tf卡, 把 `/boot` 里面的四个对应文件拷贝出来

然后按照 [1.2 Ubuntu下安装](#1.2 Ubuntu下安装) 重新安装一遍Ubuntu mate系统

#### 1.3-2 Wifi无法连接(未完成)

解决方法, 只适合于以下情况

在终端输入 `ifconfig -a` 命令

若只有 `lo` 一个网络选项, 说明网卡驱动有问题



参考链接 https://www.jianshu.com/p/9795cd0d7f60



其他情况的wifi无法连接的解决方案非常简单



## 2.ROS安装(未完成)