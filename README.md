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
  
  
  ## 3.Turtlebot3 as virtualis kamera setup
  
 Ha a dependency-k felvannak teve elso feladat egy gazeboos kornyezetben megcsinalni a csukloinkat 
 
 ehhez hasznaljuk az alap packege et 
 
 Azon belul :
\catkin_ws\src\turtlebot3\turtlebot3_description\urdf\turtlebot3_burger.urdf.xacro
  
  Ehhez hozzadjuk a kamera modulunkat 
  
```xml

     
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
```

A gazebo fajl ba hozzacsatolhatjuk a kamera plugint 
```xml
<!-- Camera -->
  <gazebo reference="camera_link">
    <sensor type="camera" name="camera1">
      <update_rate>30.0</update_rate>
      <visualize>false</visualize>
      <camera name="head">
        <horizontal_fov>1.3962634</horizontal_fov>
        <image>
          <width>640</width>
          <height>480</height>
          <format>R8G8B8</format>
        </image>
        <clip>
          <near>0.1</near>
          <far>25.0</far>
        </clip>
        <noise>
          <type>gaussian</type>
          <!-- Noise is sampled independently per pixel on each frame.
               That pixel's noise value is added to each of its color
               channels, which at that point lie in the range [0,1]. -->
          <mean>0.0</mean>
          <stddev>0.007</stddev>
        </noise>
      </camera>
      <plugin name="camera_controller" filename="libgazebo_ros_camera.so">
        <alwaysOn>true</alwaysOn>
        <updateRate>0.0</updateRate>
        <cameraName>head_camera</cameraName>
        <imageTopicName>image_raw</imageTopicName>
        <cameraInfoTopicName>camera_info</cameraInfoTopicName>
        <frameName>camera_link_optical</frameName>
        <hackBaseline>0.07</hackBaseline>
        <distortionK1>0.0</distortionK1>
        <distortionK2>0.0</distortionK2>
        <distortionK3>0.0</distortionK3>
        <distortionT1>0.0</distortionT1>
        <distortionT2>0.0</distortionT2>
      </plugin>
    </sensor>
  </gazebo>

```

Ezutan rviz en belul meg is tekinthetjuk a kamera modult

## 4.Dynamixel setup 

A dynamixel trukkos lehet ezert elsosorban fontos leelenoriznunk hogy felismeri e a rendszerunk ehhez egy dynamixel wizzard programot hasznalunk :

https://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_wizard2/

A telepittes utan elso dolgunk a beallitasokon belul  (Options) realisztikus baudrate et meghatarozni majd ha ismerjuk a model ID cimet lecsokkenteni realisztikus ertekre nincs ertelme  vegigutni minden ID ha tudjuk azt .
Ha a wizzard megkapta a dynamixelt akkor pozicio kovetest el lehet vegezni a gorgo segitsegevel ha a torque enegdelyezve van 

Ha megkapta  a gepunk akkor kezdhetjuk a workbench felepitteset es egy egyszeru write-read protokolt akarunk lefutattni  


 
  
  
  
