# 视觉SLAM

## Ch 1 Introduction

## Ch 2 初识SLAM

两大基本问题： 定位、建图

两类传感器：

+ 环境：二维码Marker，GPS， 导轨与磁条
+ 机器人本体：IMU，激光，相机

相机：

+ 单目：Monocular     没有深度，移动相机产生深度  
+ 双目：Stereo             通过视差（parallax）计算深度，计算量非常大
+ 深度：RGBD               通过物理方法测量，结构光ToF，功耗大，量程小，易受干扰
+ 其他：鱼眼等

视觉SLAM框架：

+ 前端： 视觉里程计 Visual Odometry  相邻图像估计相机运动，有漂移，特征点法/直接法
+ 后端：非线性优化 Optimization    带噪声数据优化轨迹和地图，状态估计，最大后验概率估计MAP，EKF，图优化
+ 回环检测：Loop Closing
+ 建图：Mapping

​	![image-20230804174753380](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230804174753380.png)

回环检测：

+ 检测是否回到早先位置
+ 识别过往场景
+ 计算图像间相似性

+ 词袋模型

建图：

+ 度量地图/拓扑地图
+ 稠密建图/稀疏地图

数学描述：

+ 离散时间，位置->随机变量服从概率分布
+ 运动方程

​	![image-20230804175737138](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230804175737138.png)

+ 观测方程

  ![image-20230804175820619](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230804175820619.png)

![image-20230804180016268](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230804180016268.png)



---

## Ch3 三维空间刚体运动

+ 运动描述：旋转矩阵，变换矩阵，四元数，欧拉角

+ Eigen库的矩阵，几何模块

### 3.1 点、向量和坐标系，旋转矩阵

旋转矩阵R:

+ 正交矩阵
+ 行列式为1
+ 特殊正交群： Special Orthogonal Group

齐次坐标，变换矩阵，特殊欧式群（Special Euclidean Group)

![image-20230804223442722](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230804223442722.png)

欧拉角：Euler Angles

+ yaw-pitch-roll 偏航-俯仰-滚转  Z-Y-X

万向锁：Gimbal Lock

+ ZYX，若pich为正负90°，则第三次和第一次旋转绕同一轴，丢失一个自由度

四元数（Quaternion）

---

## Ch4 李群与李代数 Lie Group and Lie Algebra

### 4.1 李群李代数基础

+ 三维旋转矩阵构成特殊正交群 SO(3)
+ 三维变换矩阵构成特殊欧式群SE(3)

群（Group） 集合加上运算的代数结构 （A，.）

+ 封闭性
+ 结合律
+ 幺元
+ 逆

![image-20230804232300285](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230804232300285.png)

李群（Lie Group）

+ 连续（光滑）性质
+ 流形
+ SO(3), SE(3)，没有加法，难以取极限，求导

李代数(Lie Algebra)

+ 与李群对应的一种结构，位于向量空间
+ 记作so(3), se(3)

![image-20230804235619138](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230804235619138.png)

### 4.2 指数映射和对数映射

### 4.3 李代数求导与扰动模型



---

## Ch 5 相机与图像

### 5.1 相机模型 

景深：在照片中清晰的那一部分，从最近的清晰处到最远的清晰处之间的范围。影响因素：光圈大小，焦距，拍摄物体与相机的距离

![image-20230805144828904](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805144828904.png)

![image-20230805145145902](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805145145902.png)

![image-20230805150438574](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805150438574.png)

畸变：镜头，边缘严重

+ 径向畸变：桶形失真，枕形失真

![image-20230805160958303](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805160958303.png)

+ 切向畸变

+ 数学模型

  ![image-20230805161039335](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805161039335.png)

**双目模型**

![image-20230805161421347](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805161421347.png)

**RGB-D相机：物理手段测量深度**

+ ToF或结构光两种主要原理
+ 通常能得到与RGB图对应的深度图

### 5.2 图像

### 5.3 图像处理

### 5.4 实践：点云拼接



---

## Ch 6 非线性优化 Non-linear Optimization

最小二乘法

Gauss-Newton, Levenburg-Marquadt下降策略

Ceres库，g2o库

### 6.1 状态估计问题

+ 简单情况：线性系统，高斯噪声
+ 复杂情况：非线性系统，非高斯噪声

![image-20230805164919430](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805164919430.png)

![image-20230805164927744](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805164927744.png)

![image-20230805171941782](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805171941782.png)

![image-20230805172133717](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805172133717.png)

![image-20230805172410779](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805172410779.png)

![image-20230805172844962](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805172844962.png)

### 6.2 非线性最小二乘

![image-20230805173327832](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805173327832.png)

**最速下降法（Steepest Method）- 保留一阶梯度**

![image-20230805173536852](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805173536852.png)

+ 会碰到zigzag问题（过于贪婪）

**牛顿法（）- 保留二阶梯度**

![image-20230805174436078](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805174436078.png)

+ 迭代次数少，但需要计算复杂的Hessian矩阵

+ 回避方法：近似处理

  + Gauss-Newton

  ![image-20230805174757095](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805174757095.png)

​		**不能保证H总是可逆**

   + Levenberg-Marquadt

     + G-N属于线搜索，先找到方向，再找到长度
     + L-M属于信赖区域方法（Trust Region），认为近似只在区域内可靠

     ![image-20230805175332090](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805175332090.png)

     ![image-20230805175412891](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805175412891.png)
     
     

---

## Ch 7 视觉里程计 Visual Odometry

+ 特征点，提取特征点，特征点匹配
+ 对极几何，约束，恢复图像间的摄像机的三维运动
+ PNP，利用三维结构与图像的对应关系，求解摄像机的三维运动
+ 理解ICP问题，利用点云的匹配关系，求解摄像机的三维运动
+ 通过三角化，获得二维图像上对应点的三维结构

### 7.1 特征点法

路标 Landmark

+ 数量充足
+ 较好区分性，以实现数据关联

特征点

+ 可重复性，可区别性，高效，本地
+ 关键点：位置，大小，方向，评分等
+ 特征点周围的图像信息-描述子（descripter）
+ SIFT\SURF\ORB

特征匹配

+ 暴力匹配
+ 快速最近邻：FLANN

### 7.2 实践：特征提取与匹配

### 7.3 2D-2D 对极几何

特征点间对应关系：

+ 对极几何：两个单目图像，得到2D-2D间的关系
+ PnP：帧和地图，得到3D-2D间的关系
+ ICP：RGB-D，得到3D-3D间的关系

![image-20230805222153624](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805222153624.png)

![image-20230805222650273](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805222650273.png)

![image-20230805223740662](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805223740662.png)

![image-20230805224054951](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805224054951.png)

![image-20230805225015747](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805225015747.png)

![image-20230805225235003](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805225235003.png)

八点法的讨论：

+ 用于单目SLAM的初始化
+ 尺度不确定性：归一化t或特征点的平均深度
+ 纯旋转问题：t=0时无法求解
+ 多于八点对时：最小二乘或RANSAC

从单应矩阵恢复R，t：

![image-20230805231129367](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805231129367.png)

![image-20230805231759869](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230805231759869.png)

### 7.6 实践：三角测量

### 7.7 PnP 3D-2D

已知3D点的空间位置和相机的投影点，求相机的旋转和平移（外参）

+ 代数解法

  + DLT

    ![image-20230806003757606](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230806003757606.png)

  + P3P

    利用三对点求相机外参

    缺点：

    + 对三个点以上的情况难以处理
    + 误匹配时算法失效

  + EPnP/UPnP

+ 优化解法

  + Bundle Adjustment
  
    + 最小化重投影误差（Minimizing a reprojection error)
    + 投影关系：
    + 定义重投影误差并取最小化
  
    ![image-20230807143720583](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230807143720583.png)
  
  ![image-20230807143858339](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230807143858339.png)
  
  

![image-20230807144637240](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230807144637240.png)

### 7.8 PNP实践

### 7.9 3D-3D ICP

两组配对好的3D点，求其旋转和平移，可用迭代最近点求解（ICP, Iterative Closest Point）

![image-20230807150305336](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230807150305336.png)

推导：

![image-20230807150357198](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230807150357198.png)

![image-20230807150709739](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230807150709739.png)

![image-20230807150852054](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230807150852054.png)

![image-20230807151018440](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230807151018440.png)

### 7.10 ICP实践



---

## Ch 8 视觉里程计（2）

### 8.1 直接法和光流

![image-20230807152346222](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230807152346222.png)

不提取特征：

+ 通过其他方式寻找配对点：光流
+ 无配对点：直接法

### 8.2 光流

稀疏光流： 以Lucas-Kanade LK光流为代表

稠密光流：以Horn-Schunck HS光流为代表

本质上是估计像素在不同时刻图像中的运动

![image-20230807154826857](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230807154826857.png)

灰度不变假设：![image-20230807155035274](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230807155035274.png)

实际由于高光/阴影/材质/曝光等不同可能不成立

![image-20230807155554551](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230807155554551.png)

![image-20230807155955407](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230807155955407.png)

![image-20230807160501923](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230807160501923.png)

### 8.3 实践：LK光流

### 8.4 直接法

光流缺点：

+ 仅估计了像素间的平移
+ 没有用到相机本身的几何结构
+ 没有考虑到相机的旋转和图像的缩放

直接法推导

+ 两个帧，运动未知，有初始估计R，t
+ 第一帧看到了点P，投影为p1
+ 按照初始估计，P在第二帧上投影为p2

![image-20230809135643082](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230809135643082.png)

![image-20230809141248266](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230809141248266.png)

![image-20230809143917232](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230809143917232.png)



+ 稀疏直接法：只处理稀疏角点或关键点
+ 稠密直接法：使用所有像素
+ 半稠密直接法：使用部分梯度明显像素

### 8.5 直接法实践

![image-20230809145827074](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230809145827074.png)

![image-20230809150045292](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230809150045292.png)

## Ch 10 后端

+ 理解以EFK为代表的滤波器后端原理
+ 理解非线性化的后端工作原理
+ 使用g2o和Ceres实际操作后端优化

### 10.1 EKF滤波器形式后端

后端（Backend)

+ 从带噪声的数据估计内在状态-状态估计问题
+ Estimated the inner state from noisy data

渐进式(Incremental)

+ 保持当前状态的估计，在加入新信息时，更新已有的估计（滤波）

批量式（Batch）-主流

+ 给定一定规模的数据，计算该数据下的最有估计（优化）

不确定性随时间累积->观测周围，将不确定性保持在一定范围

![image-20230810150948206](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230810150948206.png)

![image-20230810151348063](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230810151348063.png)



### 10.2 BA与图优化

**Bundle Adjustment** 

+ 属于批量式优化方法

+ 给定多个相机位姿和观测数据，计算最优的状态估计
+ 定义每个运动/观测方程的误差，从初始估计开始寻找梯度下降

![image-20230810153211011](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230810153211011.png)













# ROS

## LIDAR 激光雷达

**LIDAR： Light Detection and Ranging**

**分类：**

+ 单线雷达/多线雷达
+ 三角测距雷达/TOF雷达
+ 机械旋转雷达/固态雷达

**数据格式**

+ 激光雷达的数据通常表现为三维点云。每个点包括X、Y和Z坐标以及可能的其他信息，如强度（光的反射强度）。

+ 在ROS中，激光雷达的数据通常使用`sensor_msgs/PointCloud2`消息类型表示。此消息类型包括点云的组织结构（有序或无序）、尺寸、点的数据字段（如位置和强度）以及点数据本身。

**处理方式**

1. **预处理**：包括滤波、去噪、降采样等，以减少计算复杂性和去除噪声。
2. **分割与分类**：根据几何、反射特性等将点云分割成有意义的部分，例如，识别出建筑、植被、地面等。
3. **特征提取**：从点云中提取有用的特征以用于进一步的分析或识别。
4. **配准和建模**：使用点云数据构建三维模型或将多个点云源对齐。
5. **导航和定位**：在机器人领域，激光雷达经常用于导航和定位，例如使用SLAM（同时定位和映射）算法。

**数据格式**

![image-20230810175325076](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230810175325076.png)

**消息包**

![image-20230810181340742](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230810181340742.png)

实现步骤

+ 构建lidar_pkg软件包
+ 构建新节点 lidar_node
+ 在节点中，向ROS大管家NodeHandler申请订阅话题/scan，并设置回调函数为LidarCallback（）。
+ 构建回调函数LidarCallback（），用来接收和处理雷达数据
+ 调用ROS_INFO()显示雷达监测到的距离





## RViz

The Robot Visualization Tool

RViz是一个3D可视化工具，用于显示与机器人操作系统（ROS）一起使用的传感器数据、机器人状态、计划、导航等。

**主要功能**

- **机器人模型可视化**：可以显示URDF定义的机器人模型，并实时显示其配置和状态。
- **传感器数据显示**：支持许多常见的传感器数据类型，包括2D和3D激光扫描、点云、图像、深度图像等。
- **路径和导航可视化**：用于显示机器人的目标、路径和导航信息，支持导航栈的各个组件。
- **TF变换树**：可以可视化ROS中的TF变换树，用于理解和调试坐标变换。
- **交互式标记**：支持3D交互式标记，使开发人员能够与运行的ROS应用程序进行交互。





## IMU 惯性测量单元

**Inertial Measurement Unit**（惯性测量单元）

用于测量和报告物体的速度、方向和重力相关的加速度，通过这些信息可以估算物体的姿态。

IMU 的核心组成部分包括：

1. **加速度计**：测量三轴上的线性加速度。
2. **陀螺仪**：测量三轴上的角速度。
3. **（有时的）磁力计**：测量三轴上的磁场强度，可以用于获取方向信息，如指南针。

**误差累积**：由于IMU主要是基于积分的方法来估计速度和位置，所以误差会随着时间的推移累积。

**数据格式：**

![image-20230810232305774](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230810232305774.png)

![image-20230810232553905](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230810232553905.png)

**常用消息格式：2最常用**

![image-20230811003631738](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230811003631738.png)

**实现步骤：**

1. 构建软件包：imu_pkg
2. 构建新节点：imu_node
3. 申请订阅话题/imu/data，设置回调函数IMUCallback（）。
4. 构建回调函数IMUCallback（）
5. 使用TF工具将四元数转换为欧拉角
6. 调用ROS_INFO（）显示转换后的欧拉角数值

**航向锁定实现步骤：**

+ 发布速度控制话题/cmd_vel
+ 设定一个**目标朝向角**，当姿态信息与目标朝向角不一致时，控制机器人转向目标朝向角



## ROS消息包

![image-20230812211732886](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230812211732886.png)

![image-20230812212549889](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230812212549889.png)

![image-20230812213708425](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230812213708425.png)



![image-20230812223745011](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230812223745011.png)



## 地图数据格式

占栅格地图

+ 栅格边长-》地图分辨率   ROS默认0.05m
+ 三种状态 0 100 -1 （无障碍物 有障碍物 未知）
+ 行列拼接-》数组((0,0)开始，行优先)    nav_msgs::OccupancyGrid

地图node实现步骤

+ 构建软件包map_pkg，依赖项里增加nav_msgs
+ 创建节点map_pub_node
+ 发布话题/map，消息类型nav_msgs::OccupancyGrid
+ 构建一个nav_msgs::OccupancyGrid地图消息包，赋值，发送到话题/map
+ 编译运行
+ 启动RViz，订阅话题/map，显示地图



## SLAM

Simultaneous Localization And Mapping

**Hector_Mapping（ROS工具包）:**

![image-20230815014057435](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230815014057435.png)

![image-20230815014137028](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230815014137028.png)



# VSLAM
