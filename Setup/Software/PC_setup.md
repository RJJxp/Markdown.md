# 装机指南

重装了很多次系统了

记录一些软件之类的东西



[TOC]



## Windows10

以后写程序, 学习, 全部转战Ubuntu

windows就是用来娱乐

### 01.基础软件

office 全家桶

#### 音乐播放器

- Foobar 2000

#### 视频播放软件

- Potplayer

#### 图片查看软件

- Picasa

#### 编辑器

- Typora
- UltraEdit
- Vscode

#### 浏览器

- edge
- cent browser
- chrome



### 02.小工具

- KMS 破解工具

- Everything 搜索工具
- Paint.net 轻量级图片编辑器
- SSR 
- Pandownloader 网盘下载器

- winRAR
- Obs

- 硕鼠



### 03.大众软件

- QQ, 微信
- 
- 迅雷9, 百度网盘, pandownloader
- PDF Reader
- 网易邮箱



### 04.学习

- texlive2017 windows和ubuntu两个都一样, 不如安装在windows下

- Git



### 05.环境设置

- 关闭windows部分同步设置

- 显示系统默认图标: 右键个性化---主题---桌面图标设置

- 关闭操作中心: 右键个性化---任务栏---打开或关闭系统图标

- 删除弹出提示: 回收站右键属性, 可以找到该项设置
- 用户账户控制: 我的电脑 --- 属性 --- 安全和维护

#### 05.01 禁用 Windows Defender

`win + R` 输入 `gpedit.msc` 打开本地策略编辑器

计算机配置 --- 管理模板 --- windows组件

能看到 windows defender 的配置文件

关闭就好

#### 05.03 禁用小娜 

`win + R` 输入 `gpedit.msc` 打开本地策略编辑器

计算机配置 --- 管理模板 --- windows组件

 能看到小娜的配置文件

#### 05.04 禁用 Windows Update







## Ubuntu 16.04 LTS

主要是软件和环境

### 01.学习软件

- Pycharm
- Qt

- Vscode



### 02.学习环境

- Ros
- Ros-Cartographer
- 显卡, tensor flow



### 03.其他

- 网易云音乐



### 04.设置

wifi 驱动设置

 `/etc/rc.local` 文件

> echo "123"| sudo modprobe -r ideapad_laptop
>
> exit 0