# Camera_Rot_Turtlebot3

# 1. Bevezetés

A projekt cékja a turtlebot on lévő állo kaera mountot ami egy raspberry kaera modult tart ,kicserélni egy mount ra ami képes a kamerát forgatni.

[//]: # (Image References)

[image1]: ./assets/20230418_095734.jpg "Alap"

## Alap elhelyezés

 ![alt text][image1] 
## Tartalomjegyzek

 1. [Bevezetés](# 1.-Bevezetés)
 

   
 #  2. Csomagok letoltese 
 Elso sorban le kell toltsuk az alap csomagot amely a turtlebot3 , Ros ,Dynamixel lesz elsosorban
 
 Ros letoltese :
 
 http://wiki.ros.org/noetic/Installation
 
 Engedelyezni szeretnenk a ros csomagokat hogy letolthessuk 
 
 `sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`
 
 Erdemes feltenni a curl-t biztos ami biztos 
 
 `sudo apt install curl`
 
 Hozzaadjuk a kulcsot :
 
 `curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -`
 
 Ezek utan : 
 
 `sudo apt update`
 
 es 
 
 `sudo apt upgrade`
 
 Ha megvannak ezek el lehet kezdeni a ros feltelepiteset:(Erdemes a teljes deskotp packaget felrakni)

 `sudo apt install ros-noetic-desktop-full`
 
 (Ha a parancsra nem akarja feltenni a Ros t mivel nem "Trusted source" ,akkor ezt fel kell oldani a kernelen vagy ui on belul .
 Pelda video https://www.youtube.com/watch?v=ET2fXRExxco&t=170s&ab_channel=CyberZypher )
 
 
 Ha feletelepult a Ros erdemes letrehozni egy workspace es ennek helyet elhelyezni a bashrc faljban 
 
 `cd ~`  
`mkdir -p catkin_ws/src`  
`cd catkin_ws`  (Tetszoleges nev)
`catkin_make`  

Ezutan a bash be valo elhelyezes :

 `cd ~`  
 
`nano .bashrc/`

Ha megnyilt a legaljara a ket kommand :

 `source /opt/ros/noetic/setup.bash`  

 `source ~/catkin_ws/devel/setup.bash`  
 
 Ezutan ctrl +s es ctrl+x el elhagyjuk a bashrc fajlt
 
 Ezutan belepve a source folderbe:
 
 `cd ~/catkin_ws/src`
 
 Ezutan letolthaejuk a turtlebot csomagjait 
 
 `git clone https://github.com/ROBOTIS-GIT/turtlebot3_simulations
  git clone https://github.com/ROBOTIS-GIT/turtlebot3_msgs
  git clone https://github.com/MOGI-ROS/turtlebot3`
 
Majd a Dynamixel csomagjait is mainly az oldal lepeseit kell kovetni par kaviattal :
https://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_workbench/

`
 git clone https://github.com/ROBOTIS-GIT/dynamixel-workbench.git`
 `
 git clone https://github.com/ROBOTIS-GIT/dynamixel-workbench-msgs.git
  `
 `git clone https://github.com/ROBOTIS-GIT/DynamixelSDK.git`
  
 Ha a dependency-k felvannak teve elso feladat egy gazeboos kornyezetben megcsinalni a csukloinkat 
 
 ehhez hasznaljuk az alap packege et 
 
 Azon belul :
\catkin_ws\src\turtlebot3\turtlebot3_description\urdf\turtlebot3_burger.urdf.xacro
  
  Ehhez hozzadjuk a kamera modulunkat 
  
```html

     
<!-- fajdalom kezdete -->

  <joint name="camera_base" type="revolute">
    <limit upper="1.57" lower="-1.57" velocity="10" effort="1"/>
    <origin xyz="0.01 0 0.106" rpy="0 0 0"/>
    <child link="camera_link1"/>
    <parent link="base_footprint"/>
    <axis xyz="0 0 1" />
  </joint>   

  <link name='camera_link1'>
    <pose>0 0 0 0 0 0</pose>
    <inertial>
      <mass value="0.1"/>
      <origin xyz="0 0 0" rpy="0 0 0"/>
      <inertia
          ixx="1e-6" ixy="0" ixz="0"
          iyy="1e-6" iyz="0"
          izz="1e-6"
      />
    </inertial>

    <collision name='collision'>
      <origin xyz="0 0 0" rpy="0 0 0"/> 
      <geometry>
         <cylinder length="0.05" radius="0.010" />
      </geometry>
    </collision>

    <visual name='camera_link1_visual'>
      <origin xyz="0 0 0" rpy="0 0 0"/>
      <geometry>
          <cylinder length="0.002" radius="0.015" />
      </geometry>
    </visual>

  </link>


 <!-- Camera -->
  <joint type="fixed" name="camera_joint">
    <origin xyz="0.01 0 0.003" rpy="0 0 0"/>
    <child link="camera_link"/>
    <parent link="camera_link1"/>
    <axis xyz="0 1 0" />
  </joint>

  <link name='camera_link'>
    <pose>0 0 0 0 0 0</pose>
    <inertial>
      <mass value="0.1"/>
      <origin xyz="0 0 0" rpy="0 0 0"/>
      <inertia
          ixx="1e-6" ixy="0" ixz="0"
          iyy="1e-6" iyz="0"
          izz="1e-6"
      />
    </inertial>

    <collision name='collision'>
      <origin xyz="0 0 0" rpy="0 0 0"/> 
      <geometry>
        <box size=".01 .005 .005"/>
      </geometry>
    </collision>

    <visual name='camera_link_visual'>
      <origin xyz="0 0 0" rpy="0 0 0"/>
      <geometry>
        <box size=".0005 .01 .008"/>
      </geometry>
    </visual>

  </link>

 <joint type="fixed" name="camera_optical_joint">
    <origin xyz="0 0 0" rpy="-1.5707 0 -1.5707"/>
    <child link="camera_link_optical"/>
    <parent link="camera_link"/>
  </joint>

  <link name="camera_link_optical">
  </link>

  <gazebo reference="camera_link">
    <material>Gazebo/Red</material>


  </gazebo>


  <!--fajadalom vege--> 




 
  
  
  
