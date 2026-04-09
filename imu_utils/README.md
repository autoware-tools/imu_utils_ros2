# IMU 工具（imu_utils）
一个用于分析IMU性能的ROS功能包工具。艾伦方差（Allan Variance）分析工具的C++版本。

绘图通过Matlab实现，相关脚本位于`scripts`目录下。

本工具的核心功能：仅对IMU数据进行**艾伦方差分析**。
> 数据采集要求：将IMU**保持静止**，持续采集**2小时**数据。

---

## 参考资料
技术参考文档：
[《艾伦方差：陀螺仪噪声分析》](http://cache.freescale.com/files/sensors/doc/app_note/AN5087.pdf "艾伦方差：陀螺仪噪声分析")、
[VectorNav陀螺仪手册](https://www.vectornav.com/support/library/gyroscope "VectorNav陀螺仪手册")、
[《惯性导航入门》](http://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-696.html "惯性导航入门")

```
Woodman, O.J., 2007. 惯性导航入门（技术报告编号：UCAM-CL-TR-696）. 剑桥大学计算机实验室.
```

Matlab参考代码：[GyroAllan](https://github.com/XinLiGitHub/GyroAllan "GyroAllan")

---

## IMU噪声参数
| 参数名称                | YAML配置项 | 符号                | 单位                                                                 |
| ----------------------- | ---------- | ------------------- | -------------------------------------------------------------------- |
| 陀螺仪白噪声            | `gyr_n`    | <img src="https://latex.codecogs.com/svg.latex?{%5Csigma_g}">         | <img src="https://latex.codecogs.com/svg.latex?{%5Cfrac%7Brad%7D%7Bs%7D%5Cfrac%7B1%7D%7B%5Csqrt%7BHz%7D%7D}"> |
| 加速度计白噪声          | `acc_n`    | <img src="https://latex.codecogs.com/svg.latex?{%5Csigma_a}">         | <img src="https://latex.codecogs.com/svg.latex?{%5Cfrac%7Bm%7D%7Bs^2%7D%5Cfrac%7B1%7D%7B%5Csqrt%7BHz%7D%7D}"> |
| 陀螺仪零偏不稳定性      | `gyr_w`    | <img src="https://latex.codecogs.com/svg.latex?{%5Csigma_b_g}">       | <img src="http://latex.codecogs.com/svg.latex?\frac{rad}{s}&space;\sqrt{Hz}" title="\frac{rad}{s} \sqrt{Hz}" /> |
| 加速度计零偏不稳定性    | `acc_w`    | <img src="https://latex.codecogs.com/svg.latex?{%5Csigma_b_a}">       | <img src="http://latex.codecogs.com/svg.latex?\frac{m}{s^2}&space;\sqrt{Hz}" title="\frac{m}{s^2} \sqrt{Hz}" /> |

* 白噪声取值对应：时间间隔 τ=1 时；
* 零偏不稳定性取值对应：艾伦方差曲线**最小值附近**；

（依据技术文档：[《艾伦方差：陀螺仪噪声分析》](http://cache.freescale.com/files/sensors/doc/app_note/AN5087.pdf "艾伦方差：陀螺仪噪声分析")）

---

## 测试样例效果图
<img src="figure/gyr.jpg">
<img src="figure/acc.jpg">

* 蓝色 ：Vi-Sensor, ADIS16448，`200Hz`
* 红色 ：3dm-Gx4，`500Hz`
* 绿色 ：大疆A3飞控，`400Hz`
* 黑色 ：大疆N3飞控，`400Hz`
* 圆圈 ：xsens-MTI-100，`100Hz`

---

## 编译与运行方法
### 编译步骤
```
sudo apt-get install libdw-dev
```
* 下载依赖功能包 [code_utils](https://github.com/gaowenliang/code_utils "code_utils")；
* 将ROS功能包 `imu_utils` 和 `code_utils` 放入你的工作空间（通常命名为 `catkin_ws`）；
* 进入工作空间目录，执行 `catkin_make` 编译。

### 运行步骤
* 将IMU**保持静止**，持续采集**2小时**数据；
* （或者）播放ROS数据包数据集；
```
 rosbag play -r 200 imu_A3.bag
```
* 启动ROS节点：
```
roslaunch imu_utils A3.launch
```

⚠️ 注意你的launch文件配置：
```xml
<launch>
    <node pkg="imu_utils" type="imu_an" name="imu_an" output="screen">
        <param name="imu_topic" type="string" value= "/djiros/imu"/>
        <param name="imu_name" type="string" value= "A3"/>
        <param name="data_save_path" type="string" value= "$(find imu_utils)/data/"/>
        <param name="max_time_min" type="int" value= "120"/>
        <param name="max_cluster" type="int" value= "100"/>
    </node>
</launch>
```

---

## 输出样例
```
类型: IMU
名称: A3
陀螺仪:
   单位: " rad/s"
   三轴平均值:
      gyr_n: 1.0351286977809465e-04
      gyr_w: 2.9438676109223402e-05
   X轴:
      gyr_n: 1.0312669892959053e-04
      gyr_w: 3.3765827874234673e-05
   Y轴:
      gyr_n: 1.0787155789128671e-04
      gyr_w: 3.1970693666470835e-05
   Z轴:
      gyr_n: 9.9540352513406743e-05
      gyr_w: 2.2579506786964707e-05
加速度计:
   单位: " m/s^2"
   三轴平均值:
      acc_n: 1.3985049290745563e-03
      acc_w: 6.3249251509920116e-04
   X轴:
      acc_n: 1.1687799474421937e-03
      acc_w: 5.3044554054317266e-04
   Y轴:
      acc_n: 1.2050535351630543e-03
      acc_w: 6.0281218607825414e-04
   Z轴:
      acc_n: 1.8216813046184213e-03
      acc_w: 7.6421981867617645e-04
```

---

## 数据集下载
大疆 A3：`400Hz`
下载链接：[百度网盘](https://pan.baidu.com/s/1jJYg8R0 "DJI A3")

大疆 A3：`400Hz`
下载链接：[百度网盘](https://pan.baidu.com/s/1pLXGqx1 "DJI N3")

ADIS16448：`200Hz`
下载链接：[百度网盘](https://pan.baidu.com/s/1dGd0mn3 "ADIS16448")

3dM-GX4：`500Hz`
下载链接：[百度网盘](https://pan.baidu.com/s/1ggcan9D "GX4")

xsens-MTI-100：`100Hz`
下载链接：[百度网盘](https://pan.baidu.com/s/1i64xkgP "MTI-100")