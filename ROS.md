# ROS

##  一. 核心概念

### 1.1 节点与节点管理器

####  节点（Node） 执行单元

+ 可使用不同编程语言
+ 在系统中名称唯一

####  节点管理器 （ROS Master） 控制中心

+ 为节点提供注册，命名
+ 跟踪记录话题，服务通信；辅助相互查找，建立连接
+ 提供参数服务器



### 1.2 话题通信

**topic message(.msg) publish subscriber**

#### 话题（Topic） 异步通信机制

+ 节点间数据传输 总线
+ 发布/订阅模型，订阅者或发布者可不唯一
+ Publisher， Subscriber

#### 消息（Message） 话题数据

+ ROS提供或用户自定义， 类型， 数据结构
+ 多用于数据传输
+ .msg 定义，编译过程中生成对应代码文件

### 1.3 服务通信

**service .src  request response**

#### 服务（Service） 同步通信

+ 客户端/服务器模型 （C/S） 一对多
+ 带有反馈的同步通信
+ 多用于逻辑处理
+ .srv定义



### 1.4 参数

+ Parameter 全局共享字典
+ 服务器存储、检索运行时的参数
+ 适合静态、非二进制的配置参数



### 1.5 文件系统

#### 功能包（Package）

+ 基本单元，含节点源码、配置文件、数据定义

#### 功能包清单（Package manifes）

+ 记录功能包基本信息，包含作者信息、许可信息、依赖选项、编译标志等

#### 元功能包（Meta Packages）

+ 组织多个用于同一目的的功能包



---



## 二. 命令行工具

 ### 2.1 常用命令

+ rqt_graph

  程序结构

+ rostopic

  + rostopic **pub** -r 10 /turtle1/cmd_vel geometry_msgs/Twist "linera:

    x:1.0

    y:0.0

    z:0.0

    angular

    x:0.0

    y:0.0

    z:1.0"

    发布话题消息

+ rosservice

  + rosservice **call** /spawn"x:5.0

    y:5.0

    theta:0.0

    name:'turtle2'"

    发布服务请求

+ rosnode

  + rosnode list 查看节点列表

+ rosparam

+ rosmsg

+ rossrv

+ rosbag 记录实验数据，复现

  + rosbag record-a -O cmd_record 话题记录（-a全部topics  -O指定输出文件名称）
  + rosbag play cmd_record.bag 话题复现





---

## 三. 工作空间与功能包

### 3.1 工作空间(Workspace)

+ src      代码空间  Source Space
+ build  编译空间  Buid Space
+ devel  开发空间  Development Space
+ install  安装空间  Install Space

![](C:\Users\18217\Desktop\notes\workspace.png)

### 3.2 创建工作空间

创建

+ mkdir -p ~/**catkin_ws**/src      **-p** 创建整个路径中缺失的目录  **~/**  表示当前用户的主目录（home directory）

+ cd ~/catkin_ws/src

+ **catkin_init_workspace**

编译

+ cd ~/catkin_ws/

+ **catkin_make**

+ catkin_make install

设置环境变量

+ source devel/setup.bash

检查环境变量

+ echo $ROS_PACKAGE_PATH

### 3.3 创建功能包

**同一工作空间下不允许同名功能包**

创建

+ cd ~/catkin_wd/src   **在src目录下创建**
+ catkin_create_pkg test_pkg std_msgs rospy roscpp
+ **catkin_create_pkg <package_name>  [dependency1] [dependency2] [dependency3]**  **原型**

编译 

+ cd ~catkin_ws
+ **catkin_make**
+ source ~/catkin_ws/devel/setup.bash



---



## 四. Publisher的编程实现

![Publisher](C:\Users\18217\Desktop\notes\ROS\pic\Publisher.png)

### 4.1 cpp实现

```c++
#include <ros/ros.h>
#include <geometry_msgs/Twist.h>

int main(int argc, char **argv)
{
	// ROS节点初始化
	ros::init(argc, argv, "velocity_publisher");

	// 创建节点句柄
	ros::NodeHandle n;

	// 创建一个Publisher，发布名为/turtle1/cmd_vel的topic，消息类型为geometry_msgs::Twist，队列长度10
	ros::Publisher turtle_vel_pub = n.advertise<geometry_msgs::Twist>("/turtle1/cmd_vel", 10);

	// 设置循环的频率
	ros::Rate loop_rate(10);

	int count = 0;
	while (ros::ok())
	{
	    // 初始化geometry_msgs::Twist类型的消息
		geometry_msgs::Twist vel_msg;
		vel_msg.linear.x = 0.5;
		vel_msg.angular.z = 0.2;

	    // 发布消息
		turtle_vel_pub.publish(vel_msg);
		ROS_INFO("Publsh turtle velocity command[%0.2f m/s, %0.2f rad/s]", 
				vel_msg.linear.x, vel_msg.angular.z);

	    // 按照循环频率延时
	    loop_rate.sleep();
	}

	return 0;
}

```

1. include <ros/ros.h>等头文件
2. ros节点初始化
3. 创建节点句柄
4. 用句柄创建一个Publisher
5. 设置循环频率
6. 循环发送，按频率延时



初始化ROS节点

+ **ros::init(argc, argv, "velocity_publisher");**

向ROS Master注册节点信息，含话题名、消息类型

+ **ros::NodeHandle n;**
+ **ros::Publisher turtle_vel_pub = n.advertise\<geometry_msgs::Twist\> ("/turtle1/cmd_vel", 10);**

创建消息数据，按一定频率发布

+ **ros::Rate loop_rate(10);**

  int count = 0;

  while**(ros::ok()**){

  ​		**geometry_msgs::Twist** **vel_msg**;

  ​		vel.msg.linear.x=0.5;

  ​		vel.msg.angular.z=0.2;

  ​		

  ​		turtle_vel_pub**.publish**(vel_msg);

  ​		

  ​		**loop_rate.sleep();**

  }

#### 配置CMakeLists.txt编译规则

+ **add_executable**(velocity_publisher src/velocity_publisher.cpp)

  把cpp编译成可执行文件

+ **target_link_libraries**(velocity_publisher ${catkin_LIBRARIES})

  把可执行文件与ROS库做链接

#### 编译并运行

+ cd ~/catkin_ws  在主工作空间中编译
+ catkin_make
+ **source devel/setup.bash**
+ **rosrun** learnig_topic velocity_publisher、

  **rosrun package_name node_name [args]**

#### 添加至环境变量

ctrl+h 显示隐藏文件 找到.bashrc，末尾增加

+ source /home/thx/catkin_ws/devel/setup.bash

### 4.2 py实现

```python
import rospy
from geometry_msgs.msg import Twist

def velocity_publisher():
	# ROS节点初始化
    rospy.init_node('velocity_publisher', anonymous=True)

	# 创建一个Publisher，发布名为/turtle1/cmd_vel的topic，消息类型为geometry_msgs::Twist，队列长度10
    turtle_vel_pub = rospy.Publisher('/turtle1/cmd_vel', Twist, queue_size=10)

	#设置循环的频率
    rate = rospy.Rate(10) 

    while not rospy.is_shutdown():
		# 初始化geometry_msgs::Twist类型的消息
        vel_msg = Twist()
        vel_msg.linear.x = 0.5
        vel_msg.angular.z = 0.2

		# 发布消息
        turtle_vel_pub.publish(vel_msg)
    	rospy.loginfo("Publsh turtle velocity command[%0.2f m/s, %0.2f rad/s]", 
				vel_msg.linear.x, vel_msg.angular.z)

		# 按照循环频率延时
        rate.sleep()

if __name__ == '__main__':
    try:
        velocity_publisher()
    except rospy.ROSInterruptException:
        pass


```

1. 导入依赖

   import rospy

   from geometry.msgs import Twist

2. 定义功能函数

3. 节点初始化

   rospy.init_node

4. 创建Publisher

   turtle_vel_pub = **rospy.Publisher**('/turtle1/cmd_vel', Twist, queue_size=10)

5. 设置循环频率

   rate = **rospy.Rate**(10) 

6. 循环

   **while not rospy.is_shutdown():**

7. 创建消息

   vel_msg = **Twist()**

8. 发送消息

    turtle_vel_pub.**publish**(vel_msg)

9. 频率延迟

   **rate.sleep()**
   
10. try except
    **except rospy.ROSInterruptException:**



---

## 五.Subscribe的编程实现

### 5.1 cpp实现

```s
#include <ros/ros.h>
#include "turtlesim/Pose.h"

// 接收到订阅的消息后，会进入消息回调函数
void poseCallback(const turtlesim::Pose::ConstPtr& msg)
{
    // 将接收到的消息打印出来
    ROS_INFO("Turtle pose: x:%0.6f, y:%0.6f", msg->x, msg->y);
}

int main(int argc, char **argv)
{
    // 初始化ROS节点
    ros::init(argc, argv, "pose_subscriber");

    // 创建节点句柄
    ros::NodeHandle n;

    // 创建一个Subscriber，订阅名为/turtle1/pose的topic，注册回调函数poseCallback
    ros::Subscriber pose_sub = n.subscribe("/turtle1/pose", 10, poseCallback);

    // 循环等待回调函数
    ros::spin();

    return 0;
}

```

1. 初始化ROS节点 **ros::init**

2. 创建节点句柄 **ros::NodeHandle**

3. 创建Subscriber

   **ros::Subscriber pose_sub = n.subscribe("/turtle1/pose", 10, poseCallback);**

4. 循环等待回调函数 ros::spin();

5. 回调函数

   **// 接收到订阅的消息后，会进入消息回调函数**
   **void poseCallback(const turtlesim::Pose::ConstPtr& msg)**
   **{**
       **// 将接收到的消息打印出来**
       **ROS_INFO("Turtle pose: x:%0.6f, y:%0.6f", msg->x, msg->y);**
   **}**



### 5.2 py实现

```s
import rospy
from turtlesim.msg import Pose

def poseCallback(msg):
    rospy.loginfo("Turtle pose: x:%0.6f, y:%0.6f", msg.x, msg.y)

def pose_subscriber():
	# ROS节点初始化
    rospy.init_node('pose_subscriber', anonymous=True)

	# 创建一个Subscriber，订阅名为/turtle1/pose的topic，注册回调函数poseCallback
    rospy.Subscriber("/turtle1/pose", Pose, poseCallback)

	# 循环等待回调函数
    rospy.spin()

if __name__ == '__main__':
    pose_subscriber()

```

1. 定义回调函数

   **def poseCallback(msg):**

2. ros节点初始化 

   **rospy.init_node**

3. 创建Subscriber 

   **rospy.Subscriber("/turtle1/pose", Pose, poseCallback)**

4. 循环等待回调函数

   **rospy.spin()**



---

## 六. 话题消息的定义与使用

### 6.1 定义msg文件

![image-20230707133724252](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230707133724252.png)



### 6.2 在package.xml中添加功能包依赖

```xml
  <build_depend>message_generation</build_depend>
  <exec_depend>message_runtime</exec_depend>
```



### 6.3 CMakeLists.txt添加编译选项

+ find_package(catkin REQUIRED COMPONENTS
    	geometry_msgs

    ​	message_generation
  )

+ add_message_files(FILES Person.msg)
  generate_messages(DEPENDENCIES std_msgs)

+ CATKIN_DEPENDS geometry_msgs roscpp rospy std_msgs turtlesim message_runtime



### 6.4 编译生成语言相关文件



---

## 七. 客户端Client的编程实现

![image-20230707142950629](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230707142950629.png)

### 7.1 创建功能包

在src目录下，catkin_create_pkg learning_service roscpp rospy std_msgs geometry_msgs turtlesim

### 7.2 实现客户端

```s
#include <ros/ros.h>
#include <turtlesim/Spawn.h>

int main(int argc, char** argv)
{
    // 初始化ROS节点
	ros::init(argc, argv, "turtle_spawn");

    // 创建节点句柄
	ros::NodeHandle node;

    // 发现/spawn服务后，创建一个服务客户端，连接名为/spawn的service
	ros::service::waitForService("/spawn");
	ros::ServiceClient add_turtle = node.serviceClient<turtlesim::Spawn>("/spawn");

    // 初始化turtlesim::Spawn的请求数据
	turtlesim::Spawn srv;
	srv.request.x = 2.0;
	srv.request.y = 2.0;
	srv.request.name = "turtle2";

    // 请求服务调用
	ROS_INFO("Call service to spwan turtle[x:%0.6f, y:%0.6f, name:%s]", 
			 srv.request.x, srv.request.y, srv.request.name.c_str());

	add_turtle.call(srv);

	// 显示服务调用结果
	ROS_INFO("Spwan turtle successfully [name:%s]", srv.response.name.c_str());

	return 0;
};

```

+ 初始化节点

+ 创建句柄

+ 循环等待服务，若服务存在，则继续

  aa发现/spawn服务后，创建一个服务客户端，连接名为/spawn的service

  ```c++
  ros::service::waitForService("/spawn");
  ros::ServiceClient add_turtle = node.serviceClient<turtlesim::Spawn>("/spawn");
  ```

+ 初始化请求数据

  ```c++
  turtlesim::Spawn srv;
  srv.request.x = 2.0;
  srv.request.y = 2.0;
  srv.request.name = "turtle2";
  ```

+ 请求服务调用

      add_turtle.call(srv);

+ 显示服务调用结果

	```s
ROS_INFO("Spwan turtle successfully [name:%s]", srv.response.name.c_str());

### 7.3 编译

### 7.4 py实现

```python
import sys
import rospy
from turtlesim.srv import Spawn

def turtle_spawn():
	# ROS节点初始化
    rospy.init_node('turtle_spawn')

	# 发现/spawn服务后，创建一个服务客户端，连接名为/spawn的service
    rospy.wait_for_service('/spawn')
    try:
        add_turtle = rospy.ServiceProxy('/spawn', Spawn)

		# 请求服务调用，输入请求数据
        response = add_turtle(2.0, 2.0, 0.0, "turtle2")
        return response.name
    except rospy.ServiceException, e:
        print "Service call failed: %s"%e

if __name__ == "__main__":
	#服务调用并显示调用结果
    print "Spwan turtle successfully [name:%s]" %(turtle_spawn())

```





---

## 八.服务端Server的编程实现

### 8.1 cpp实现

```c++
#include <ros/ros.h>
#include <geometry_msgs/Twist.h>
#include <std_srvs/Trigger.h>

ros::Publisher turtle_vel_pub;
bool pubCommand = false;

// service回调函数，输入参数req，输出参数res
bool commandCallback(std_srvs::Trigger::Request  &req,
         			std_srvs::Trigger::Response &res)
{
	pubCommand = !pubCommand;

    // 显示请求数据
    ROS_INFO("Publish turtle velocity command [%s]", pubCommand==true?"Yes":"No");

	// 设置反馈数据
	res.success = true;
	res.message = "Change turtle command state!"

    return true;
}

int main(int argc, char **argv)
{
    // ROS节点初始化
    ros::init(argc, argv, "turtle_command_server");

    // 创建节点句柄
    ros::NodeHandle n;

    // 创建一个名为/turtle_command的server，注册回调函数commandCallback
    ros::ServiceServer command_service = n.advertiseService("/turtle_command", commandCallback);

	// 创建一个Publisher，发布名为/turtle1/cmd_vel的topic，消息类型为geometry_msgs::Twist，队列长度10
	turtle_vel_pub = n.advertise<geometry_msgs::Twist>("/turtle1/cmd_vel", 10);

    // 循环等待回调函数
    ROS_INFO("Ready to receive turtle command.");

	// 设置循环的频率
	ros::Rate loop_rate(10);

	while(ros::ok())
	{
		// 查看一次回调函数队列 
    	ros::spinOnce();
		
		// 如果标志为true，则发布速度指令
		if(pubCommand)
		{
			geometry_msgs::Twist vel_msg;
			vel_msg.linear.x = 0.5;
			vel_msg.angular.z = 0.2;
			turtle_vel_pub.publish(vel_msg);
		}

		//按照循环频率延时
	    loop_rate.sleep();
	}

    return 0;
}

```

### 8.2 py实现

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import rospy
import thread,time
from geometry_msgs.msg import Twist
from std_srvs.srv import Trigger, TriggerResponse

pubCommand = False;
turtle_vel_pub = rospy.Publisher('/turtle1/cmd_vel', Twist, queue_size=10)

def command_thread():	
	while True:
		if pubCommand:
			vel_msg = Twist()
			vel_msg.linear.x = 0.5
			vel_msg.angular.z = 0.2
			turtle_vel_pub.publish(vel_msg)
			
		time.sleep(0.1)

def commandCallback(req):
	global pubCommand
	pubCommand = bool(1-pubCommand)

	# 显示请求数据
	rospy.loginfo("Publish turtle velocity command![%d]", pubCommand)

	# 反馈数据
	return TriggerResponse(1, "Change turtle command state!")

def turtle_command_server():
	# ROS节点初始化
    rospy.init_node('turtle_command_server')

	# 创建一个名为/turtle_command的server，注册回调函数commandCallback
    s = rospy.Service('/turtle_command', Trigger, commandCallback)

	# 循环等待回调函数
    print "Ready to receive turtle command."

    thread.start_new_thread(command_thread, ())
    rospy.spin()

if __name__ == "__main__":
    turtle_command_server()

```



---

##  九. 服务数据的定义与使用

### 9.1 srv文件

+  定义文件，---分割request和response

+ package.xml 添加功能包

  ```xml
  <build_depend>message_generation</build_depend>
  <exec_depend>message_runtime</exec_depend>
  ```

+ CMakeLists.txt 添加编译选项

  ```s
  find_package( ... message_generation)
  
  add_service_files(FILES Person.srv)
  generate_messages(DEPENDENCIES std_msgs)
  
  catkin_package( ... m  esage_runtime)
  ```



---

## 十.  参数的使用与编程方法

![image-20230711155247607](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230711155247607.png)

### 10.1 常用命令行

+ rosparam list
+ rosparam get param_key
+ rosparam set param_key param_value
+ rosparam dump file_name
+ rosparam load fire_name
+ rosparam delete param_key

### 10.2 数据格式

![image-20230711155804588](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230711155804588.png)

### 10.3 程序实现

**cpp实现**

```c++
#include <string>
#include <ros/ros.h>
#include <std_srvs/Empty.h>

int main(int argc, char **argv)
{
	int red, green, blue;

    // ROS节点初始化
    ros::init(argc, argv, "parameter_config");

    // 创建节点句柄
    ros::NodeHandle node;

    // 读取背景颜色参数
	ros::param::get("/background_r", red);
	ros::param::get("/background_g", green);
	ros::param::get("/background_b", blue);

	ROS_INFO("Get Backgroud Color[%d, %d, %d]", red, green, blue);

	// 设置背景颜色参数
	ros::param::set("/background_r", 255);
	ros::param::set("/background_g", 255);
	ros::param::set("/background_b", 255);

	ROS_INFO("Set Backgroud Color[255, 255, 255]");

    // 读取背景颜色参数
	ros::param::get("/background_r", red);
	ros::param::get("/background_g", green);
	ros::param::get("/background_b", blue);

	ROS_INFO("Re-get Backgroud Color[%d, %d, %d]", red, green, blue);

	// 调用服务，刷新背景颜色
	ros::service::waitForService("/clear");
	ros::ServiceClient clear_background = node.serviceClient<std_srvs::Empty>("/clear");
	std_srvs::Empty srv;
	clear_background.call(srv);
	
	sleep(1);

    return 0;
}

```

**py实现**

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

########################################################################
####          Copyright 2020 GuYueHome (www.guyuehome.com).          ###
########################################################################

# 该例程设置/读取海龟例程中的参数

import sys
import rospy
from std_srvs.srv import Empty

def parameter_config():
	# ROS节点初始化
    rospy.init_node('parameter_config', anonymous=True)

	# 读取背景颜色参数
    red   = rospy.get_param('/background_r')
    green = rospy.get_param('/background_g')
    blue  = rospy.get_param('/background_b')

    rospy.loginfo("Get Backgroud Color[%d, %d, %d]", red, green, blue)

	# 设置背景颜色参数
    rospy.set_param("/background_r", 255);
    rospy.set_param("/background_g", 255);
    rospy.set_param("/background_b", 255);

    rospy.loginfo("Set Backgroud Color[255, 255, 255]");

	# 读取背景颜色参数
    red   = rospy.get_param('/background_r')
    green = rospy.get_param('/background_g')
    blue  = rospy.get_param('/background_b')

    rospy.loginfo("Get Backgroud Color[%d, %d, %d]", red, green, blue)

	# 发现/spawn服务后，创建一个服务客户端，连接名为/spawn的service
    rospy.wait_for_service('/clear')
    try:
        clear_background = rospy.ServiceProxy('/clear', Empty)

		# 请求服务调用，输入请求数据
        response = clear_background()
        return response
    except rospy.ServiceException, e:
        print "Service call failed: %s"%e

if __name__ == "__main__":
    parameter_config()

```





 
