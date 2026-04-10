# IMU_UTILS_ROS2

imu_utils_ros2 是面向 ROS 2 生态的 IMU 惯性测量单元标定与性能分析工具包，基于经典 imu_utils 移植优化，适配 ROS 2 Humble/Iron 等版本。它通过 Allan 方差分析，精准解算陀螺仪与加速度计的零偏、随机噪声、随机游走等关键参数，支持从 rosbag2 读取静态 IMU 数据，自动完成误差建模与参数拟合，输出 YAML 格式标定结果。项目采用 C++ 开发，兼容常见 IMU 传感器，可直接为 SLAM、导航、姿态解算等机器人应用提供高精度传感器先验参数，提升系统定位与姿态估计稳定性，是自动驾驶、无人机、移动机器人开发中必备的 IMU 校准工具。

## 环境要求

- ros2 humble
- ubuntu 22.04

## 编译项目

```bash

source /opt/ros/humble/setup.bash
source $HOME/autoware_tomato/setup.bash

# 注意:需要使用系统的 glog，默认与 autoware 的 glog 冲突
# sudo find /usr/lib -name glog-config.cmake
# x86 -Dglog_DIR="/usr/lib/x86_64-linux-gnu/cmake/glog"
# arm -Dglog_DIR="/usr/lib/aarch64-linux-gnu/cmake/glog"

colcon build \
--install-base $HOME/autoware_tomato \
--cmake-args -DCMAKE_BUILD_TYPE=Release \
-Dglog_DIR="/usr/lib/aarch64-linux-gnu/cmake/glog" \
--packages-select code_utils imu_utils

```

## 运行项目

```bash

source /opt/ros/humble/setup.bash
source $HOME/autoware_tomato/setup.bash

mkdir -p $HOME/tomato-ros/imu_utils/

rm -rf $HOME/.ros/log/*

# IMU 200Hz
# 根据你的情况适当修改 HI14R3232100.launch.xml 中的参数

ros2 launch imu_utils HI14R3232100.launch.xml \
    serial_port:=/dev/ttyUSB_IMU0 \
    baud_rate:=230400 \
    timer_period_ms:=5 \
    data_save_path:=$HOME/tomato-ros/imu_utils/ \
    max_cluster:=100 \
    max_time_min:=120 \
    launch_imu:=true \
    launch_imu_utils:=true

```

## 关于作者

公众号《番茄ROS机器人》作者，长期致力于 ROS2 与 Autoware 框架下的低速无人驾驶解决方案研发，专注技术实战与经验沉淀，分享低速无人系统开发中的技术难点、解决方案与行业思考，与开发者共同推动低速无人驾驶技术的落地与创新。

## 关注作者

### 微信号

- 微信号：**smartros**
- 二维码：

![img](image/smartros.jpg "添加《番茄ROS机器人》微信号好友")

### 公众号

![img](image/tomato-ros.png "关注公众号《番茄ROS机器人》")

### 视频号

![img](image/tomato-ros-video.png "关注视频号《番茄ROS特种机器人》")

