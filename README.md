# 部署教程

## 解压代码并编译
解压压缩包到~/目录下，解压后的文件夹名为ik_ws
```
cd ik_ws
colcon build
```

## 运行
```
source ~/ik_ws/install/setup.bash
ros2 run airship_ik ik_node
```

## Service接口定义与调用

Service的名称为`/grasp`，数据类型为`grasp_interface_pkg/srv/Grasp`, 其定义如下：
```
float64 x
float64 y
float64 z
int8 ret
int8 g
---
int8 ret
```
其中x, y, z为目标位置（夹爪中心位置）相对于base系的坐标，单位为米

1.遥操作模式：当g=1时，夹爪闭合，返回ret=0。g=0时，夹爪张开，返回ret=0

2.抓取模式：当g=-1时，先发送夹爪张开指令，等待1秒后，下发机械臂运动指令，等待运动到指定位置，再发送夹爪闭合指令，等待1秒后，返回ret=0

两种模式下，若x,y,z不在工作空间内，代表不可达，直接返回ret=1。

调用例子：
```
source ~/ik_ws/install/setup.bash
ros2 service call /grasp grasp_interface_pkg/srv/Grasp "x: 0.3 y: 0.0 z: 0.2 ret: 0 g: 1"
```