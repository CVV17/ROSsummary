[TOC]



#### ROS相关操作

###### 可视化工具

图形化显示话题通讯关系工具

```sh
$ rqt_graph
```

###### RVIZ

RVIZ数据可视化工具，对机器人状态实时观测的辅助工具，接收数据并显示。(观测传感器数据，机器人采集到的数据都可以图像化的形式显示在这个软件中)

```sh
$ rviz
```

（！rviz记得source一下工作空间环境参数）同样可以在launch文件中配置 

> 使用Rviz配置好文件之后，可以选定rviz文件夹另存为配置文件(lidar.rviz)，调用时直接填写参数，不用二次配置了。（新建文件夹是为方便管理，参见launch文件书写）



###### 查看运行ROS活跃话题

查看ROS系统中当前活跃着的话题

```sh
$ rostopic list
```

查看ROS系统中话题消息格式类型

```sh
$ rostopic type <话题名称>
```

查看指定话题里的消息包内容

```sh
$ rostopic echo <话题名称>
```

查看指定话题里的消息包数组内容 折叠

```sh
$ rostopic echo <话题名称> --noarr
```

查看消息包列表内容数据类型

```sh
$ rosmsg show <消息包的名称/消息类型>
```

查看话题发送频率

```sh
$ rostopic hz <话题名称>
```

`echo` 命令在 Unix 和类 Unix 系统中用于在终端输出一行文本。`-e` 选项允许解释一些转义字符

```sh
echo -e <消息内容>
```

###### **运行时调试修改参数**

```bash
$rosrun rqt_reconfigure rqt_reconfigure
```

具有动态调参的详细列表，可以立刻修改参数，可在Rviz中实时观察调整效果

###### 查看TF父子关系

订阅`/tf`话题，查看当前运行节点中有哪些坐标系，以及坐标系之间的**空间关系**。

`tf2_msgs/TFMessage.msg`里面的`geometry_msgs/TransformStampted[]`是一个数组，除了地图和机器人地盘坐标还有其他的坐标关系，可以用`tf_tree`指令查看获取空间坐标关系。

```sh
$ rosrun rqt_tf_tree rqt_tf_tree
```

跳出的窗口每一个椭圆都是坐标系，上方是下方椭圆的父级坐标系。

###### 查看下载的安装包

1.查询当前安装完成的所有包文件

```sh
$ rospack  list
```

在不知道要安装功能包的确切名字的情况下找到目标包

```sh
$ rospack  list  | grep rqt-
```

通过管道线 与`grep`命令， 输出与关键字`rqt-`相关的行



```sh
$ roscd <功能包名>
```

终端中进入指定软件包的文件地址



###### 运行功能包节点

```sh
$ rosrun <功能包名> <节点名>
```

###### ROS相关功能包下载指令

```sh
$ sudo apt install ros-[ros版本号]-Name[所要安装的功能包]
```

###### Github源码编译

进入下载好的源码文件夹(workspace/src)，先运行scripts里.sh 文件(安装编译所需依赖包的脚本文件)，再返回到workspace文件夹下，运行以下指令**编译**，对src文件夹里所有源代码工程进行**编译。**

```sh
$ catkin_make
```

source 配置文件.bash，将workspace的环境参数加载到终端程序里，否则会报错**找不到软件包**

最后lauch一下运行

```sh
$ roslaunch <功能包名> <节点名>
```

编译后source的原因

> `source` 指令在 Unix 和类 Unix 系统中是一个常用命令，用于执行指定文件中的命令。这个命令通常与 `.bashrc` 或 `.profile` 这类 shell 配置文件一起使用。以下是 `source` 指令的一些主要用途：
>
> 1. **执行配置文件**：当你修改了 `.bashrc` 或其他 shell 配置文件后，通常需要重新加载这些配置，使其立即生效。使用 `source` 命令可以重新加载并执行这些文件中的命令，而不需要开启新的终端会话。
> 2. **设置环境变量**：`source` 可以用来临时设置或修改环境变量。例如，你可以在 `.bashrc` 文件中设置 `PATH` 环境变量，然后使用 `source ~/.bashrc` 来应用更改。
> 3. **应用别名和函数**：如果你在配置文件中定义了别名或函数，使用 `source` 命令可以立即使用这些别名或函数。
> 4. **改变当前会话**：`source` 命令可以在当前的 shell 会话中改变环境设置，而不是创建一个新的 shell 会话。
> 5. **脚本中的自我执行**：在脚本中使用 `source` 脚本名称可以让脚本在当前 shell 环境中执行，而不是在子 shell 中执行。

**实操环节：**

github网站里面搜索wpr_simulation克隆到本地里`主文件夹/catkin_ws/src`

https://github.com/6-robot/wpr_simulation.git

执行`src/scripts/对应版本的.sh文件`目的(使用脚本安装编译需要的依赖库)

```sh
$ ./XXX_for_XXX.sh
```

退回到工作空间`主文件夹/catkin_ws/` 进行编译

```sh
$ catkin_make
```

这个时候会创建build devel等一系列文件夹

运行之前，将catkin_ws工作空间里的环境参数加载到终端程序里(使用source指令载入工作空间的环境设置)

```sh
$ source 工作空间/devel/setup.bash
```

最后运行`roslaunch`里面(会自动运行`roscore`)

```sh
$ roslaunch <功能包名> <节点名>
```

就成功运行啦！（下载源码并在本地编译运行）

> 可选项：将设置工作空间环境参数source指令添加到终端程序初始化.bashrc脚本之中
>
> 每次运行就不用source环境参数了
>
> ```sh
> $ gedit ~/.bashrc
> ```
>
> 在文件的末尾将source指令粘贴到文件底部
>
> ```sh
> $ source ~/工作空间/devel/setup.bash
> ```
>
> 完成！！！



##### 创建Package软件包并运行节点 ##报错处理##

创建在`~/catkin_ws/src`文件夹里

```sh
$ catkin_create_pkg <功能包名称> <依赖项列表>
```

使用catkin工具创建软件包。会生成`include`	` src` 两个文件夹和`CmakeLists.txt`<编译相关的配置文件>	`package.xml` <对软件包的描述>两个文件

依赖项放在`opt/ros/noetic/share/roscpp`文件夹下

##**`src`存放cpp源码文件**

相关节点源码就放在src中

####**`CMakeLists.txt`设置节点源码编译规则**  (重点)

`cmakelist.txt` 增加编译可执行文件executable的输入和输出  以及依赖项

在`~/catkin_ws/`下执行`catkin_make`   或 在VScode里编译

##**运行节点**

确认ROS核心处于运行状态

```sh
$ rosrun <包名> <节点文件名>
```

 ##报错处理##

- 若出现找不到相应软件包 可能是工作空间环境参数没有配置到终端source指令（类似于GitHub源码编译后运行之前需要source）

  或者新建的软件包（只填写launch文件）未进行编译未进入ros软件包列表

- 若找不到ros相关函数实现，就是在**编译规则**中添加函数实现的库文件specific_library

 ##报错处理##

##**ROS节点**

包含消息类型对应头文件

```c++
include <ros/ros.h>
include <std_megs/String.h>
```

初始化 

```c++
 ros::init(argc,argv,"节点名称");
```

循环

```c++
while(ros::ok()){}
```

**发布者：**确定**话题**名称和消息类型；通过**NodeHandler**泛化函数发布话题传入ros::publisher pub对**消息**包中的数据赋值；调用pub的publisher函数将消息包发送到话题当中。

**订阅者：**订阅话题中所发布的消息包内容。确定话题名称和消息类型；包含include <ros/ros.h>消息类型头文件。通过**NodeHandler**泛化函数订阅话题传入ros::subscriber sub，并设置回调函数对消息包进行处理；**ros::spinOnce()**接收到的新消息包后运行回调函数。



###### python语言编写节点

创建在`~/catkin_ws/src`文件夹里

```sh
$ catkin_create_pkg <功能包名称> <依赖项列表>
```

区别于c++功能包创建过程(编写好代码之后，编译，运行)，创建好工作空间之后，退会**主目录工作空间**直接进行编译操作，之后就不用再进行编译了(无论是修改之后)，可以直接运行(配置文件的权限)。//不需要修改查马克list.txt内容

> ##**编译目的**是为了让新建的功能包进入到ros软件包列表之中，在后续运行节点时，ros能够在软件包列表中找到新建的软件包。

##**在包中新建python节点文件**

在src目录中创建`scripts`文件夹存放python节点文件

对新建文件增加权限

```sh
$ chmod +X <节点文件名>
```

changemode +execute添加执行权限





##### **launch文件**

launch文件放在任意功能包下，实现一个launch文件实现启动多个节点的功能

```sh
$ roslaunch <功能包名> <launch文件名>
```

示例代码：(简要说明)描述多层嵌套的数据结构

```xml
<标记名称 属性名1=“属性值1”> 内容 </标记名称>
<标记名称 属性名2=“属性值2”/> //容器没有内容时的简约写法
```

```xml
<launch>

	<node pkg="pkg_name" type="type_name" name="name"  output="screen" launch-prefix="gnome-terminal -e"/ args="-d $(find slam_pkg)/rviz/slam.rviz"/> 
    <include file="${find <launch文件所处软件包名>/launch/<launch文件名>}"/>
</launch>
```

- `launch-prefix="gnome-terminal -e"`在单独运行在独立终端中。
- `output="screen" `将节点信息输出在终端。

- `args="-d $(find slam_pkg)/rviz/slam.rviz"`等同于`rosrun rivz rviz -d home/robot/catkin_ws/src/slam_pkg/rviz/slam.rviz `

  `args`填写<配置文件的完整地址>`$(find slam_pkg)/rviz/slam.rviz`

  加载RVIZ的**配置文件**，免去重新加载配置的过程。(-d -directory)

- `launch`文件包含另外一个`launch`文件

  `<include file="${find <launch文件所处软件包名>/launch/<launch文件名>}"/>`

  调用`find`函数后补全之后的相对路径

通过launch文件修改文件里的参数，参数通过`index.ros.org`官网进行查找功能包的`paramter`参数描述

###### 分组

```xml
<launch>
	<group>
        <node pkg="pkg_name" type="type_name" name="name"  output="screen" launch-prefix="gnome-terminal -e"/ args="-d $(find slam_pkg)/rviz/slam.rviz">
            <param name="<参数名称1>" value="数值大小" />
            <param name="<参数名称2>" value="数值大小" />
        </node>
        <include file="${find <launch文件所处软件包名>/launch/<launch文件名>}"/>
    </group>
    <--!-->可分为多个组，组间名字需要有所区别</--!-->
    <group>
        <node pkg="pkg_name" type="type_name" name="name"  output="screen" launch-prefix="gnome-terminal -e"/ args="-d $(find slam_pkg)/rviz/slam.rviz">
            <param name="<参数名称1>" value="数值大小" />
            <param name="<参数名称2>" value="数值大小" />
        </node>
        <include file="${find <launch文件所处软件包名>/launch/<launch文件名>}"/>
    </group>

</launch>
```

###### 参数设置

```xml
<launch>

  <!-- 载入 机器人 和 长走廊 的仿真场景 -->
  <include file="$(find wpr_simulation)/launch/wpb_stage_corridor.launch"/>

  <!-- Hector -->
  <node pkg="hector_mapping" type="hector_mapping" name="hector_mapping"/>

  <!-- Rviz -->
  <arg name="rvizconfig" default="$(find wpr_simulation)/rviz/corridor.rviz" />
  <node name="rviz" pkg="rviz" type="rviz" args="-d $(arg rvizconfig)" required="true" />

  <!-- 运动控制 -->
  <node pkg="rqt_robot_steering" type="rqt_robot_steering" name = "rqt_robot_steering"/>

</launch>
```

  `<arg name="rvizconfig" default="$(find wpr_simulation)/rviz/corridor.rviz" />`

配置文件单行书写，简化节点运行指令，`required`参数目标文件找到返回`ture`

```xml
<arg name="rvizconfig" default="$(find wpr_simulation)/rviz/corridor.rviz" />
  <node name="rviz" pkg="rviz" type="rviz" args="-d $(arg rvizconfig)" required="true" />
```



> `launch`文件的编写完后，须退回工作空间执行编译操作`catkin_make`编译工作，防止之后运行找不到文件软件包位置。

###### 

##### 创建自定义消息的步骤

和创建软件包相似

- 在工作空间/`src` 中创建软件包

  ```sh
  $ catkin_creat_pkg <消息包名称> <依赖包名称> message_generation message_runtime
  ```

  ##创建新的软件包，依赖项`message_generation`、`message_runtime` 分别是消息包生成和运行时需要的依赖项，消息包一般以`_msgs` 结尾。

- 消息包新建msg目录（与src平级），新建自定义消息类型文件(例如carry.msg)，以`.msg` 结尾并在文件里定义消息成员结构。

  ```c++
  string name
  int64 star
  string data
  //数据类型 变量名
  //数据类型与std_msg标准消息包基础数值类型一一对应
  //其他软件包定义的消息类型 嵌套使用
  ```

设置消息**编译规则**

- 在`CMakeList.txt`编译文件中，确认依赖项`message_generation`、`message_runtime` 已填入。

  ```c++
  find_package( catkin REQUIRED COMPONENTS
  	message_generation
  	message_runtime
  	roscpp
  	rospy
  	std_msgs
  )
  ```

  `.msg` 文件加入`add_message_files()` 

- ```
  add_message_files(
  	FILES
  	Carry.msg
  	#Message1.msg
  	#Message2.msg 改为新建的例如Carry.msg
    )
    
  generate_messages(
    	DEPENDENCIES
    	std_msgs
    	#表明新的消息类型需要依赖的其他消息列表  用到了其中的string 和int64类型
    )
  
  catkin_package(
  	#INCLUDE_DIRS include
  	#LIBRARIES qq_msgs
  	CATKIN_DEPENDS message_generation message_runtime rospy roscpp std_msgs
  	#取消注释确保message_runtime在其中，确保依赖新消息包的其他软件包能在运行时使用新定义的消息类型
  	#DEPENDS system_lib
  )
  
  ```

打开`package.xml`文件中的依赖项列表

确保均有依赖项`message_generation`、`message_runtime`

```
<build_depend>message_generation</build_depend>
<build_depend>message_runtime</build_depend>
<exec_depend>message_generation</exec_depend>
<exec_depend>message_runtime</exec_depend>
```

最后退回工作空间，运行指令编译消息包，生成新的自定义消息类型

```sh
$catkin_make
```

完成！！！可以查看新创建的消息类型是否进入ROS消息列表

```sh
$ rosmsg show <消息包的名称/消息类型>
```

调用**自定义消息包**时，需要注意的问题

1. 修改代码，再调用新消息包消息类型时需要注意依赖功能包，否者编译会出现未定义的消息类型 

   ```cmake
   find_package( catkin REQUIRED COMPONENTS
   	#message_generation
   	#message_runtime
   	roscpp
   	rospy
   	std_msgs
   	<新的消息包名称>
   )
   ```

2. ```cmake
   add_executable(chao_node src/chao_node.cpp)
   add_dependencies($(projectname)_node <消息包名>_generate_messages_cpp)
   //为节点编译提供依赖项,先编译好消息类型在编译节点
   ```

3. `package.xml`文件中的依赖项列表`<exec_depend>` `<build_depend>`添加所依赖**新的消息包**的名称

   再catkin_make重新编译



#### 框架结构

##### 工作空间目录结构

`主文件夹/catkin_ws/src`

scripts目录用于放置脚本文件<安`装依赖包映射端口等不经常用的文件>和python程序

##### ROS节点间的通讯机制

###### TOPIC话题和MESSAGE消息

发布者和订阅者极大的提供了设计灵活性，我们可以整合不同的功能模块实现的指定功能要求和提高节点的复用性

**Topic**

1. **话题topic**是节点间持续通讯的一种形式；通讯的**节点**通过**话题名称**建立话题**通讯**连接。
2. 通讯中传输的数据是**消息Message**。消息会按照一定频率持续不断发送确保**数据实时性**
3. 消息发送方是话题**发布者Publisher**，接受方是话题的**订阅者Subsciber**
4. 通常话题中只有一个发布者，防止指令相互干扰；但有多个发布者的话题需要控制发布信息先后顺序，确保数据被准确接收到。

**Message**

根据任务场景不同，发布者和订阅者之间通讯所发送的数据内容不同（距离数据，速度指令），所采用的message消息格式也会有所不同，满足传输要求。

一般消息类型是基于std_message (含有很多基础消息类型)上构建或使用的。

建立通讯



##### 激光雷达

`Gazebo`时模拟真实机器人发出的传感器数据的工具；`Rviz`是接收传感器数据并显示的工具(显示机器人实际探测到的实际状况)

**Rviz**配置流程：文件另存为后缀为`.rviz` 会保存显示条目、视角、界面配置

`Global Frame`选择`base_footpint`

`laserscan`激光雷达显示条目，激光话题名称`\scan`

**IMU**

ROS激光雷达数据格式对于障碍物轮廓点阵的具体描述。`sensor_msgs`

IMU数据信息包含了机器人的空间姿态信息，订阅`/imu/data`获取`imu`姿态信息获取。



##### TF关系//Gmapping算法 Hector算法对比

在`map`和`base_footprint`之间的`odometry`，是为了应对单一环境缺少特征（雷达点云和参照物配准实现定位功能）无法拼接局部地图，获取机器人相对原点的坐标加入的**里程计**(通过电机转速计算机器人位置)。

在里程计的帮助下，有效克服了激光雷达SLAM位移信息缺失、障碍物点云配准失效的问题。避免建图过程的中断，建图结果和实际场地基本保持了一致。

用不同形式的定位方法去克服某种单一SLAM算法的缺陷，**减小误差**或者**增加稳定性**。

`map->>-odometry->>-base_footprint`（`rviz`中默认`tf`树）

> 解释：里程计输出的时底盘`base_footpint`相对于`odometry`的相对位置。通过障碍物点云匹配修正`odometry->>-base_footprint`理论与实际之间存在的偏移(打滑)，就是`map->>-odometry`这一段。最终实现了底盘`base_footprint`相对于`map`的位置信息，得到相对准确的空间坐标。
>
> 在已有地图模型上，hector和campping算法效果类似。但在边运行边建图中，需要里程计加入实现对单一特征环境的定位功能，hector算法就不能够胜任了。

**gmapping**介绍： [gmapping - ROS Wiki.html](gmapping - ROS Wiki.html) 

##### MOVE_BASE节点

![](img\overview_tf.png)

全局规划器、全局/局部代价地图

###### AMCL定位算法

**AMCL**(adaptive Monte Carlo Localization) **自适应蒙特卡洛定位算法**，使用粒子滤波在已知地图中进行定位的算法，具有自动纠错功能，同时使用了里程计和激光雷达数据。

在推测机器人位置附近生成一系列粒子群，分别以各个粒子(具有不同的朝向)为原点，将得到的障碍物激光点云数据和实际障碍物边缘进行对比，得到匹配效果最好的粒子点（计算所得机器人位置最贴合实际情况的点），将原来的机器人本体更新到效果最好的点，作为新的机器人本体。

可以解决传感器（里程计）误差不断累积的问题，以此使机器人定位精度维持在一定的范围内，实现较为精确并具有自动纠错的定位效果。

采用不断更新粒子群目的：解决更新的粒子在之后的几次位移误差累积，依旧会出现障碍物特征匹配不上的情况。

> 自身定位是导航的关键基础，AMCL保证机器人即使在各种累计误差叠加下找到相对准确的位置
>
> 每次都使用粒子群而不是一个确切的点进行更新，更新过程中不断删减误差过大的粒子，最后粒子群大致围绕在机器人本体周边，克服里程计误差问题。粒子数目默认是100-5000个。
>
> 只要控制`map->>-odometry`的相对位置，跳跃突变来实现本体分身的切换。`odometry->>-bace_footprint`主要是里程计负责。

###### 代价地图参数设置

更多参考`cost_2d`里的lidar

```xml
<node pkg="move_base" type="move_base" respawn="false" name="move_base"  output="screen">
    <rosparam file="$(find wpb_home_tutorials)/nav_lidar/costmap_common_params.yaml" command="load" ns="global_costmap" />
    <rosparam file="$(find wpb_home_tutorials)/nav_lidar/costmap_common_params.yaml" command="load" ns="local_costmap" />
    <rosparam file="$(find wpb_home_tutorials)/nav_lidar/local_costmap_params.yaml" command="load" />
    <rosparam file="$(find wpb_home_tutorials)/nav_lidar/global_costmap_params.yaml" command="load" />
    <rosparam file="$(find wpb_home_tutorials)/nav_lidar/local_planner_params.yaml" command="load" />
    <param name="base_global_planner" value="global_planner/GlobalPlanner" /> 
    <param name="use_dijkstra" value="true"/>
    <param name="base_local_planner" value="wpbh_local_planner/WpbhLocalPlanner" />
    <param name= "controller_frequency" value="10" type="double"/>
  </node>
```

`<rosparam file="$(find wpb_home_tutorials)/nav_lidar/costmap_common_params.yaml" command="load" ns="local_costmap" />` 

`ns`是`namespace`命名空间，加载的参数将会被限定在这个命名空间内，你可以通过这个命名空间来访问或修改这些参数。`rosparam`参数服务器的配置，指示参数服务器加载指定的文件。`command`，`param`设置参数后面接着`value`。

`costmap_common_params`影响的是代价地图的形状，通用的参数通过一个文件用不同的命名空间调用，使参数一致。不同的参数就由`global_costmap_params`和`local_costmap_params`文件描述

链接： [move_base - ROS Wiki.html](move_base - ROS Wiki.html) 



###### 恢复行为Recovery Behavior

![](img\recovery_behaviors.png)

根据机器人的特性和需求重新设计恢复行为的机制

默认的恢复机制有`clear_costmap_recovery/ClearCostmapRecovery`    `rotate_recovery/RotateRecovery ` `move_slow_and_clear/MoveSlowAndClear`

更多参数搜索`rotate_recovery`和`clear_costmap_recover`具体赋值障碍层名称参考程序源码`obstacle_layer`。要么后续修改在`yaml`中代价地图中将`obstacle_layer`改为`obstacle`，

要么只在`global_costmap_params.yaml`中将障碍层名称改为`obstacle_layer`



###### 局部规划器

**全局规划器**`Global_planner`产生的导航路径，再由**局部规划器**(运动控制器)控制执行。`Local_Planner`根据实际路况以及机器人底盘的运动特性，操纵机器人设法沿着导航路线移动，完成导航任务。

根据优化效果，开发局部规划器。`trajetory_Planner`   `##DWA_Planner##`   `Eband_Planner`   `##TEB_Planner##`  

> 注：##name## 相对较优的算法



**DWA** 动态窗口方法(Dynamic Window Approach)

DWA就是生成一系列运动轨迹速度方案，选取最优驱动机器人运动。**窗口**是机器人运动控制中，对机器人线路轨迹和运动速度可选择的空间

根据全局规划器规划的导航路线、实际现场中障碍物的位置，生成机器人的运动轨迹，剔除掉与障碍物相交的路径，剩下的路径就是待选择的窗口（可能选择的轨迹和轨迹上可能的速度变化）。生成的路径多为弧线。

实现步骤（**生成轨迹**和**挑选轨迹**）

**生成轨迹**是以机器人当前速度为基础，预测机器人未来的运动状态和移动线路。

速度的取值考虑底盘加速度的限制、刹车距离、尽快到达目标点

**挑选标准**是规划轨迹与全局导航路线的贴合程度、轨迹末端（预测的规划轨迹）和目标点的距离、轨迹路线和障碍点之间的距离。（过程、目标、风险三个因素）挑选出最佳作为控制执行路线

可以运行`rosrun rqt_reconfigure rqt_reconfigure`在线调参

> 更多参数设置查看：DWA_local_planner



**TEB**规划器(Timed Elastic Band)

根据全局规划器生成的全局导航路线，TEB截取一段进行优化，由全局导航路线和障碍物产生排斥力场所生成的轨迹，就是TEB的局部路径，TEB根据机器人的速度和加速度运动性能，预测未来一段时间内机器人的位置 ，选取目标点最短路径。

同样可以运行`rosrun rqt_reconfigure rqt_reconfigure`在线调参，调整参数到合适的数值，最后保存到`yaml`参数文件中

> 参考teb_local_planner  提供了`cost_map_converter_plugin`插件接口获取更好的规划性能和避障效果默认不开启。需要阅读网页里提供的文献。



##### Navigation导航接口

action通讯方式，消息播啊传输是双向的，持续不断返回信息实时返回导航进程，完成时返回导航完成的消息（或者导航失败的消息）`Action`【一次请求就可以连续返回消息的方式】。`Server`  <<---->> `Client`

`Server`  <<---->> `Client`

`Movebase(服务端)`  <<---->> `导航节点(客户端)`

导航节点发送导航请求，导航目标点的坐标值和期望的姿态。Movebase接收请求后完成导航。



构建`action`客户端，向`move_base`发送导航目标值，等待导航结果返回数显。



##### 中文报错问题

英文环境下`“zh_CN.UTF-8”`，中文环境下添空引号就行。

```sh
setlocale(LC_ALL,"zh_CN.UTF-8")
```

#### 



#### 消息格式汇总

ROS消息包主要分为两类：`std_msgs`标准消息包和 `common_msgs`常用消息包

##### std_msgs

主要包含标准数据类型

基础类型：string >> 消息包

​					Bool ; Byte; Char; int8; Int16; int32;Int64; empty

数组类型：ByteMultiArray; Int8MultiArray; Int16MultiArray; Int32MultiArray; Int64MultiArray

结构体类型：ColorRGBA; Duration; Time; Header; MultiArrayDimension; MultiArrayLayout

##### common_msgs

消息类型定义更加明确与应用更加贴切

|_>>sensor_msgs(传感器消息包)

|_>> geometry_msgs(几何消息包)

|_>>nav_msgs（导航消息包）



速度话题名称一般是`/cmd_vel`

##### geometry_msgs

Twist  >>  描述机器人运动状态

Twist/Vector3 >> 输出底盘运动指令

雷达数据话题`/scan`

##### sensor_msgs

LaserScan >> 激光雷达消息包的格式

IMU数据话题`/imu/data`

**IMU**(inertial Measurement Unit)惯性测量单元

sensor_msgs/imu  >> 惯性测量单元消息包格式

imu/orientation >> 融合角速度和三轴加速度计算的到的四元数

imu/Vector3 >> 测量到数据

##### nav_msgs

nav_msgs::OccupancyGrid

机器人导航使用的地图数据，`map_server`节点在话题`/map`发布信息消息数据

栅格尺寸（栅格边长）体现地图的精细程度(**地图分辨率**)   默认数值为0.05米



##### move_base_msgs

move_base_msgs::MoveBaseAction



**actionlib**

actionlib/client/simple_action_client

action通讯实现客户端和move_base进行action通讯。简化快速实现导航任务端的调用





安装包(网址)：https://index.ros.org/  消息格式内容查看

同时可以查看节点**发布**和**订阅**的**话题**

搜索功能寻找指定的安装包（类似于SLAM、CV等等）均有详细资料和用法

重点关注**相关文档**学习、借鉴