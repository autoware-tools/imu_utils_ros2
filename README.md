# IMU_UTILS_ROS2

- ros2 humble
- ubuntu 22.04

## build

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

## run

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

