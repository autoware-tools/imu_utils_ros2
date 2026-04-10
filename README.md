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