# 仿真说明

 `prius` 控制大致流程:

1. 预设PID控制参数
2. 改变 `brakepedalPrecent` , `gasPedalPercent` , `handwheelcmd` 参数, 如键盘等
3. 上述参数改变会影响四个车轮的受力, 通过 `setForce()` 将力赋给相应 joint
4. 实现车辆控制

---

考虑到与 `Tergeo` 智驾系统的通讯通过 速度和角速度

将速度, 角速度转换成力, 涉及硬件, 难度较大

因此 `prius` 这套流程不适合 `Tergeo` 仿真

---

继续使用 `gazebo_ros_diff_drive` 插件

接收 `Tergeo` 发送的车的速度和角速度, 将其转换成两个轮子的速度

实现差分控制.

---

因此调整车辆模型, 如质量, 位置等参数.

要求:

- 直行10米, 偏差在正负20cm之类
- 带速度的转弯, 转弯半径比较小
- 无速度, 自己绕自己转弯时, 几乎可以实现原地转弯
- 在算力大的机器上(谢老师的新机器),  不要出现严重卡顿或是车身摔倒

---

现阶段:

- 使用 `gazebo_ros` 的 `spwan_model` 载入 `.xacro` 或是 `.urdf` 模型时, 明显卡顿比 `.sdf` 减轻很多, 做的时候可以在 `.launch` 文件中更改载入模型类型做试验. 

- 是否存在可以直接计算出模型转动惯量的软件(建模软件)



## 代码

 `README.md` 说明比较详细

建议参考具体使用的脚本

 `tergeo_simulation` 分支 `master` 或是 `rjp_branch` 

 `tergeo_simulation` 分支 `master` , 需要本地安装 `ros_msgs` 

