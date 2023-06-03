# Camera_Rot_Turtlebot3

# 1. Bevezetés

A projekt cékja a turtlebot on lévő állo kaera mountot ami egy raspberry kaera modult tart ,kicserélni egy mount ra ami képes a kamerát forgatni.

[//]: # (Image References)

[image1]: ./assets/20230418_095734.jpg "Alap"
[image2]: ./assets/controller_service_call.png "CTRL"
[image5]: ./assets/Screenshot-2023-06-03-140649.jpg  "turtle mounted"
[image6]: ./assets/wizzard2.png "Dynamixel f"
[image7]: ./assets/wizz2.png "Dynamixel k"
[image8]: ./assets/fogaskerekek.png "Ket fogaskerek"
[image9]: ./assets/match_sik.png "Alap sik"
[image10]: ./assets/loft.png "Loft"
[image11]: ./assets/Kut.png "Merere vagas"
[image12]: ./assets/alap11.png "Alap csinalas"
[image13]: ./assets/ball_cutout.jpg "Golyok"
[image3]: ./assets/20230418_095817.jpg "Kamera kozel"
[image4]:  ./assets/20230509_101242.jpg "Kamera mounted"
[image14]:  ./assets/Usbpd.png "Usbpi"

## Alap elhelyezés

 ![alt text][image1] 
 
## Tartalomjegyzek

 1. [Bevezetés](#-1.-Bevezetés)
 

   
 #  2. Csomagok letöltese 
 Első sorban le kell töltsuk az alap csomagot amely a turtlebot3 , Ros ,Dynamixel lesz elsősorban
 
 Ros letöltese :
 
 http://wiki.ros.org/noetic/Installation
 
 Engedélyezni szeretnénk a ros csomagokat hogy letölthessuk 
 
 `sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`
 
 Érdemes feltenni a curl-t biztos ami biztos 
 
 `sudo apt install curl`
 
 Hozzáadjuk a kulcsot :
 
 `curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -`
 
 Ezek után : 
 
 `sudo apt update`
 
 és 
 
 `sudo apt upgrade`
 
 Ha megvannak ezek el lehet kezdeni a ros feltelepítését:(Érdemes a teljes deskotp packaget felrakni)

 `sudo apt install ros-noetic-desktop-full`
 
 (Ha a parancsra nem akarja feltenni a Ros t mivel nem "Trusted source" ,akkor ezt fel kell oldani a kernelen vagy ui on belül .
 
 Pelda video https://www.youtube.com/watch?v=ET2fXRExxco&t=170s&ab_channel=CyberZypher )
 
 Ha feletelepült a Ros érdemes létrehozni egy workspace és ennek helyet elhelyezni a bashrc faljban 
 
 `cd ~`  
`mkdir -p catkin_ws/src`  
`cd catkin_ws`  (Tetszoleges nev)
`catkin_make`  

Ezutan a bash be valo elhelyezés :

 `cd ~`  
 
`nano .bashrc/`

Ha megnyilt a legaljára a két kommand :

 `source /opt/ros/noetic/setup.bash`  

 `source ~/catkin_ws/devel/setup.bash`  
 
 Ezutan ctrl +s és ctrl+x el elhagyjuk a bashrc fajlt
 
 Ezutan belepve a source folderbe:
 
 `cd ~/catkin_ws/src`
 
 Ezutan letölthetjük a turtlebot csomagjait 
 
 `git clone https://github.com/ROBOTIS-GIT/turtlebot3_simulations
  git clone https://github.com/ROBOTIS-GIT/turtlebot3_msgs
  git clone https://github.com/MOGI-ROS/turtlebot3`
 
Majd a Dynamixel csomagjait is mainly az oldal lépéseit kell követni pár kaviáttal :
https://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_workbench/

`
 git clone https://github.com/ROBOTIS-GIT/dynamixel-workbench.git`
 `
 git clone https://github.com/ROBOTIS-GIT/dynamixel-workbench-msgs.git
  `
 `git clone https://github.com/ROBOTIS-GIT/DynamixelSDK.git`
  
  
  ## 3.Turtlebot3 as virtualis kamera setup
  
 Ha a dependency-k felvannak téve első feladat egy gazeboos környezetben megcsinálni a csuklóinkat 
 
 Be kell álítanunk a bashrc fájlba, hogy mi a burger el dolgozunk ezzel a kommand al :
 
 `echo "export TURTLEBOT3_MODEL=burger"`
 
 Használjuk az alap packege et 
 
 Azon belül :
\catkin_ws\src\turtlebot3\turtlebot3_description\urdf\turtlebot3_burger.urdf.xacro
  
  Ehhez hozzádjuk a kamera modulunkat 
  
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

A gazebo fájl ba hozzácsatolhatjuk a kamera plugint 
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

Ezutan rviz-en belül meg is tekinthetjük a kamera modúlt

## 4.Dynamixel setup 

A dynamixel trukkös lehet ezért elsősorban fontos leelenőriznünk, hogy felismeri-e a rendszerünk ehhez egy dynamixel wizzard programot használunk :

https://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_wizard2/

A telepittés után elsó dolgunk a beallitásokon belül  (Options) realisztikus baudrate et meghatározni majd ha ismerjuk a model ID címet lecsökkenteni realisztikus értékre nincs értelme  végigfutni minden ID-n  ha tudjuk azt .

 ![mi][image6]
 
 ![alt text][image7]

Ha a wizzard megkapta a dynamixelt akkor pozició kovetést el lehet végezni a görgo segítségevel ha a torque enegdélyezve van 

Ha megkapta  a gepunk akkor kezdhetjuk a workbench felepittését és egy egyszerű write-read protokolt akarunk lefutattni  

Elso lépés az SDK konyvtar inicializálasa

https://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_sdk/library_setup/cpp_linux/#cpp-linux

`gcc -v `

Ha gcc nagyobb mint 5.4.0 20160609 akkor haladhaunk tovább ha nem update elnünk kell

`sudo apt-get install gcc-5`

Ezután :

`sudo apt-get install build-essential`

Feltesszuk a multilib funkciót (Tobb nyelv összekötése)

`sudo apt-get install gcc-multilib g++-multilib`

Bemegyünk a make folderbe :

/catkin_ws/src/DynamixelSDK/c++/build/linux64

Ezen belül make elünk

`make`

Alaposan kell figyelni hogy ne legyen error ha van újra kell make elni elöbb türlunk egyet

`make clean`
 
majd make újra 

`make`
  
 Ha sikeresen lefutott akkor kirakjuk a fő asztalra 
 
 `sudo make install`
  
  Ez is bakiba eshet ekkor letöroljuk és újra make install 
  
 `sudo make reinstall`
  
  Ha feltettük akkor csinálunk egy beépített read-write protokolt
  
  /catkin_ws/src/DynamixelSDK/c++/example/protocol1.0/read_write/linux64
  
  ezen belül :
  
  `make`
  
  Error esetén törlés és újra make
  
Megnézzük a portjainkat :

`ls /dev/tty*`

és mi az USB0 keressük de máshogy is nevezhetik ha látjuk mehetünk tovább 
  
 
<details>
 
<summary>Ha wsl2-en  belűl dolgozunk akkor átdobhatjuk a wsl2-nek a portokat windowsról </summary>

 https://devblogs.microsoft.com/commandline/connecting-usb-devices-to-wsl/
 
 Feltelepítjük 
 
 https://github.com/dorssel/usbipd-win/releases
 
 Ubuntun belül 
 
 `sudo apt install linux-tools-5.4.0-77-generic hwdata`
 
`sudo update-alternatives --install /usr/local/bin/usbip usbip /usr/lib/linux-tools/5.4.0-77-generic/usbip 20`
 
 Ezután windowson belül pl Powershell vagy csak cmd 
 
 `usbipd wsl list`
 
  ![alt text][image14] 
 
 usbipd wsl attach --busid <busid>
 
 Itt láthatjuk a port okon levő eszközök bus id-ját és meg kell keressük a dynamixelet
 ha megvan akkor meg kell nyitni a wsl-t és nyitva kell tartani amíg fut a cmd is
 
 
 `usbipd wsl attach --busid <busid>`
 (<busid> belul a talált id rakjuk )
  Engedelyt kér a rendszert ezután ha megadtuk :
 
  Ubuntun belül :
 
 `lsusb `
 Ezután látnunk kéne a port ot 
 
 `ls /dev/tty*`
 -is
  
  
</details>
 Ha láltjuk a USB0 port ot akkor  visszamegyünk 
 
  /catkin_ws/src/DynamixelSDK/c++/example/protocol1.0/read_write/linux64
 
 és 
 
 `sudo chmod a+rw /dev/ttyUSB0`
 
 ezután ha lefutott akkor mehet a protokol 
 
 `./read_write`
 
 Ekkor fel kell ismerje a port-ot és oda vissza paszolja (ha ez errorba fut de az elöző lepések nem, lehet nem probléma lehet folytatni)
 
 ## 5.Kamera modúl mozgatás
 
 `rosrun dynamixel_workbench_controllers find_dynamixel /dev/ttyUSB0`
 
 Megkeressük a baudrate et és id ját a dynamixelünknek, ez ugyanazt kell adja mint a Wizzard 
 
 Használhatjuk a base yaml fájlokat a adynamixel worskpace ből
 
 joint_2_0.yaml :
 
 ```console pan:
  ID: 1
  Return_Delay_Time: 0
  Operating_Mode: 3
  Profile_Acceleration: 0
  Profile_Velocity: 0
tilt:
  ID: 2
  Return_Delay_Time: 0
  Operating_Mode: 3
  Profile_Acceleration: 0
  Profile_Velocity: 0```
 
 Nekünk csak egy mozgás kell most  szóval (Vigyázzunk, hogy az ID n sajátunk legyen )
 
  ```console pan:
  ID: 13
  Return_Delay_Time: 0
  Operating_Mode: 3
  Profile_Acceleration: 0
  Profile_Velocity: 0 ```
Bemegyunk a dynamixel_controller.launch ba:
  ```xml 
 <launch>
  <arg name="usb_port"                default="/dev/ttyUSB0"/>
  <arg name="dxl_baud_rate"           default="57600"/>
  <arg name="namespace"               default="dynamixel_workbench"/>

  <arg name="use_moveit"              default="false"/>
  <arg name="use_joint_state"         default="true"/>
  <arg name="use_cmd_vel"             default="false"/>

  <param name="dynamixel_info"          value="$(find dynamixel_workbench_controllers)/config/basic.yaml"/>

  <node name="$(arg namespace)" pkg="dynamixel_workbench_controllers" type="dynamixel_workbench_controllers"
        required="true" output="screen" args="$(arg usb_port) $(arg dxl_baud_rate)">
    <param name="use_moveit"              value="$(arg use_moveit)"/>
    <param name="use_joint_states_topic"  value="$(arg use_joint_state)"/>
    <param name="use_cmd_vel_topic"       value="$(arg use_cmd_vel)"/>
    <rosparam>
      publish_period: 0.010
      dxl_read_period: 0.010
      dxl_write_period: 0.010
      mobile_robot_config:                <!--this values will be set when 'use_cmd_vel' is true-->
        seperation_between_wheels: 0.160  <!--default value is set by reference of TB3-->
        radius_of_wheel: 0.033            <!--default value is set by reference of TB3-->
    </rosparam>
  </node>
</launch>
 ```
 Beállítjuk a baudrate ünket és a basic.yaml helyett a miénket rakjuk 
 
 ```xml 
 <launch>
  <arg name="usb_port"                default="/dev/ttyUSB0"/>
  <arg name="dxl_baud_rate"           default="57600"/>
  <arg name="namespace"               default="dynamixel_workbench"/>

  <arg name="use_moveit"              default="false"/>
  <arg name="use_joint_state"         default="true"/>
  <arg name="use_cmd_vel"             default="false"/>

  <param name="dynamixel_info"          value="$(find dynamixel_workbench_controllers)/config/joint_2_0.yaml"/>

  <node name="$(arg namespace)" pkg="dynamixel_workbench_controllers" type="dynamixel_workbench_controllers"
        required="true" output="screen" args="$(arg usb_port) $(arg dxl_baud_rate)">
    <param name="use_moveit"              value="$(arg use_moveit)"/>
    <param name="use_joint_states_topic"  value="$(arg use_joint_state)"/>
    <param name="use_cmd_vel_topic"       value="$(arg use_cmd_vel)"/>
    <rosparam>
      publish_period: 0.010
      dxl_read_period: 0.010
      dxl_write_period: 0.010
      mobile_robot_config:                <!--this values will be set when 'use_cmd_vel' is true-->
        seperation_between_wheels: 0.160  <!--default value is set by reference of TB3-->
        radius_of_wheel: 0.033            <!--default value is set by reference of TB3-->
    </rosparam>
  </node>
</launch>
 ```
 
 Ekkor ha belépunk a workspace-be és lefordítjuk a csomagokat 
 `
 cd ~/catkin_ws && catkin_make`
 `
 Elindítva  controll fájlunk 
 
 `roslaunch dynamixel_workbench_controllers dynamixel_controllers.launch`
 `
Ha megtalálta a dynamixelt id vel együtt akkor 
 
 Egy rqt - t nyitva parhuzamosan beállítjuk a paramétereket :
 
 ![alt text][image2] 

 Vagy alternatívan :
 
 ```console 
 rosservice call /dynamixel_workbench/dynamixel_command "command: ''
id: 13
addr_name: 'Goal_Position'
value: 2048"
 ```
 Ezzel meg kéne mozdúljon 
 
 ###Ezutan ismetlödo mozgás létrehozása :
 
 Használjuk a motion.yaml fájlt 
 
 ```console 
 joint:
  names: [pan, tilt]
motion:
  names: [right, zero, left]
  right:
    step: [-3.14, -3.14]  # radian
    time_from_start: 2.0  # sec
  zero:
    step: [0.0, 0.0]
    time_from_start: 3.0
  left:
    step: [3.14, 3.14]
    time_from_start: 6.0
 
 ```
 
 Átalakítva :
 
 ```console 
 joint:
  names: [pan]
motion:
  names: [right, zero, left]
  right:
    step: [-3.14]  # radian
    time_from_start: 2.0  # sec
  zero:
    step: [0.0]
    time_from_start: 3.0
  left:
    step: [3.14]
    time_from_start: 6.0
 ```
 
 Majd a joint_operator.launch ban :
 
 (Beálítjuk hogy ismétlődjön a motion kicserélve a false -t)
 
 ```xml 
  <arg name="is_loop"               default="true"/>
 ```
 Hogy elindítsuk :
 `
 cd ~/catkin_ws && catkin_make`
 `
roslaunch dynamixel_workbench_controllers dynamixel_controllers.launch`
 `
 roslaunch dynamixel_workbench_operators joint_operator.launch
 `

 Ezután ismetlödő periodikus mozgást kell végezzen a dynamixel 
 
 ## 6.Modell felepítése 
 
Sok Cad softwerben dolgozhatunk de egyszerübbnek tartottam a Fusion360ben dolgozni mert jól ismerem de máshogy is meglehet csinálni .A cél két kúpfgaskerék ami ráfer a dynamixelre és elég kompakt hogy elférjen a turtle-on 
 
A sima gear plugin-el létrehozunk2 egymásba mesh elő fogaskereket (A fogak számát mi határozzuk meg csak az m találjon )
 
![alt text][image8] 
 
 Ezután elforgatjuk őket a kívánt pitch angle szerint ez esetben 53.1 Fok
 
Létrehozunk egy match négyzetet ami szélességben a kúpfogaskerekek kivánt hub távólsága egymástól és hossza a két tengely metszete 
 
![alt text][image9]
 
Loft parancsal az egyik fogat ki extrudáljuk használva a match négyzetet mint végpont
 
![alt text][image10]

Ezeután egy körrel levágjuk a nemkellő csúcsdarabot 
 
![alt text][image11]
 
 Ha ez megvan adunk neki egy hub ot amit mi határozunk meg de úgy , hogy ne interferáljon a másik kerékkel (Értsd ne érintse )
 
![alt text][image12]
 
 Ha megvan ezt megismételni a másik kúpfogaskerékre is és match elni majd a fogakat circle function el klónozni .
 Ha ez megvan az én design-om a könyebb design és a kényelem miatt nem konvenciális csuklót tartalmaz hanem a vázba bevágas van 8 db bb golyo elhelyezésehez ami segít a gordulekeny forgás létrehozásaban
 
![alt text][image4]
 
Végső mounting kamera 
 
![alt text][image13]
 
És a turtleboton elhelyezve 
 
![alt text][image13]
 
 
 
 
