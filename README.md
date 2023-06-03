# Camera_Rot_Turtlebot3

# 1. Bevezetés

A projekt cékja a turtlebot on lévő állo kaera mountot ami egy raspberry kaera modult tart ,kicserélni egy mount ra ami képes a kamerát forgatni.

[//]: # (Image References)

[image1]: ./assets/20230418_095734.jpg "Alap"
[image2]: ./assets/controller_service_call.png "CTRL"
[image5]: ./assets/Screenshot-2023-06-03-140649.jpg  "turtle mounted"
[image6]: ./assets/Screenshot-2023-06-03-131029.png "Dynamixel f"
[image7]: ./assets/Screenshot-2023-06-03-131102.png "Dynamixel k"
[image8]: ./assets/Screenshot-2023-06-03-140226.png "Ket fogaskerek"
[image9]: ./assets/Screenshot-2023-06-03-140245.png "Alap sik"
[image10]: ./assets/Screenshot-2023-06-03-140408.png "Loft"
[image11]: ./assets/Screenshot-2023-06-03-140503.png "Merere vagas"
[image12]: ./assets/Screenshot-2023-06-03-140601.png "Alap csinalas"
[image13]: ./assets/Screenshot-2023-06-03-140649.jpg "Golyok"
[image3]: ./assets/20230418_095817.jpg "Kamera kozel"
[image4]:  ./assets/20230509_101242.jpg "Kamera mounted"

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
 
 Be kell alitanunk a bashrc fajlba hogy mi a burger el dolgozunk ezzel a kommand al :
 
 `echo "export TURTLEBOT3_MODEL=burger"`
 
 Hasznaljuk az alap packege et 
 
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

 ![mi][image6]
 
 ![alt text][image7]

Ha a wizzard megkapta a dynamixelt akkor pozicio kovetest el lehet vegezni a gorgo segitsegevel ha a torque enegdelyezve van 

Ha megkapta  a gepunk akkor kezdhetjuk a workbench felepitteset es egy egyszeru write-read protokolt akarunk lefutattni  

Elso lepes az SDK konyvtar inicializalasa

https://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_sdk/library_setup/cpp_linux/#cpp-linux

`gcc -v `

Ha gcc nagyobb mint 5.4.0 20160609 akkor haladhaunk tovabb ha nem update elnunk kell

`sudo apt-get install gcc-5`

`sudo apt-get install build-essential`

Feltesszuk a multilib funkciot (Tobb nyelv osszekotese)

`sudo apt-get install gcc-multilib g++-multilib`

Bemegyunk a make folderbe :

/catkin_ws/src/DynamixelSDK/c++/build/linux64

Ezen belul make elunk

`make`

Alaposan kell figyelni hogy ne legyen error ha van ujra kell make elni elobb torlunk egyet

`make clean`
 
majd make ujra 

`make`
  
 Ha sikeresen lefutott akkor kirakjuk a fo asztalra 
 
 `sudo make install`
  
  Ez is bakiba eshet ekkor letoroljuk es ujra make install 
  
 `sudo make reinstall`
  
  Ha feltettuk akkor csinalunk egy beepitett read-write protokolt
  
  /catkin_ws/src/DynamixelSDK/c++/example/protocol1.0/read_write/linux64
  
  ezen belul :
  
  `make`
  
  Error eseten torles es ujra make
Megnezzuk a portjainkat :

`ls /dev/tty*`

es mi az USB0 keressuk de mashogy is nevezhetik 
  
 
 <details>
<summary>Ha wsl2 belul dolgozunk akkor atdobhatjuk a wsl2 nek a portokat windowsrol </summary>

 https://devblogs.microsoft.com/commandline/connecting-usb-devices-to-wsl/
 
 Feltelepitjuk 
 
 https://github.com/dorssel/usbipd-win/releases
 
 Ubuntun belul 
 
 `sudo apt install linux-tools-5.4.0-77-generic hwdata`
 
`sudo update-alternatives --install /usr/local/bin/usbip usbip /usr/lib/linux-tools/5.4.0-77-generic/usbip 20`
 
 Ezutan windowson belul pl Powershell vagy csak cmd 
 
 `usbipd wsl list`
 
 usbipd wsl attach --busid <busid>
 
 Itt lathatjuk a port okon levo eszkozok bus id jat es meg kell keressuk a dynamixelet
 ha megvan akkor meg kell nyitni a wsl es nyitva kell tartani amig fut a cmd is
 
 
 `usbipd wsl attach --busid <busid>`
 (<busid> belul a talalt id rakjuk )
  Engedelyt ker a rendszert ezutan ha megadtuk 
 
  Ubuntun belul :
 
 `lsusb `
 Ezutan latnunk kene a port ot 
 `ls /dev/tty*`
 -is
  
  
</details>
 Ha altjuk a USB0 port ot akkor  visszamegyunk 
 
  /catkin_ws/src/DynamixelSDK/c++/example/protocol1.0/read_write/linux64
 
 es 
 
 `sudo chmod a+rw /dev/ttyUSB0`
 
 ezutan ha lefutott akkor mehet a protokol 
 
 `./read_write`
 
 Ekkor fel kell ismerje a port ot es oda vissza paszolja (ha ez errorba fut de az elozo lepesek nem lehet nem problema lehet folytatni)
 
 ## 5.Kamera modul mozgatas
 
 rosrun dynamixel_workbench_controllers find_dynamixel /dev/ttyUSB0
 
 Megkeressuk a baudrate etet es id jat a dynamixelunknek ez ugyanazt kell adja mint a Wizzard 
 
 Hasznalhatjuk a base yaml fajlokat 
 
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
 
 Nekunk csak egy mozgas kell most  szoval (Vigyazzunk hogy az ID n sajatunk legyen )
 
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
 Beallitjuk abaudrate unket es a basic.yaml helyett a mienket rakjuk 
 
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
 
 Ekkor ha belepunk a workspace be es leforditjuk a csomagokat 
 `
 cd ~/catkin_ws && catkin_make`
 `
 Elinditva  controll fajlunk 
 roslaunch dynamixel_workbench_controllers dynamixel_controllers.launch
 `
Ha megtalalta a dynamixelt id vel egyutt akkor 
 
 Egy rqt - t nyitva parhuzamosan beallitjuk a parametereket 
 
 
 ![alt text][image2] 

 Vagy alternativan 
 
 ```console 
 rosservice call /dynamixel_workbench/dynamixel_command "command: ''
id: 13
addr_name: 'Goal_Position'
value: 2048"
 ```
 Ezzel meg kene mozduljon 
 
 Ezutan ismetlodo mozgas letrehozasa :
 
 Hasznaljuk a motion.yaml fajlt 
 
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
 
 Atalakitva :
 
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
 
 (Bealitjuk hogy ismetlodjon a motion kicserelve a fasle-t)
 
 ```xml 
  <arg name="is_loop"               default="true"/>
 ```
 Hogy elinditsuk :
 `
 cd ~/catkin_ws && catkin_make`
 `
roslaunch dynamixel_workbench_controllers dynamixel_controllers.launch`
 `
 roslaunch dynamixel_workbench_operators joint_operator.launch
 `
 
 Ezutan ismetlodo periodikus mozgast kell vegezzen a dynamixel 
 
 ## 6.Modell felepitese 
 
 ###Sok Cad softwerben dolgozhatunk de egyszerubbnek tartottam a Fusion360 mert jol ismerem de mashogy is meglehet csinalni .A cel ket kupfgaskerek ami rafer a dynamixelre es eleg kompakt hogy elferjen a turtle on 
 
 ###A sima gear plugin al letrehozunk2 egymasba mesh elo fogaskereket (A fogak szamat mi hatarozzuk meg csak az m talaljon )
 
![alt text][image8] 
 
### Ezutan elforgatjuk oket a kivan pitch angle szerint ez esetben 53.1 Fok
 
###Letrehozunk egy match negyzetet ami szelessegben a kupfogaskerekek kivan hub tavolsaga egymastol es hossza a ket tengely metszete 
 
![alt text][image9]
 
###Loft parancsal az egyik fogat ki extrudaljuk hasznalv az match negyzetet mint vegont
 
![alt text][image10]

 ###Ezeutan egy korrel levagjuk a nemkello csucsdarabot 
 
![alt text][image11]
 
### Ha ez megvan adunk neki egy hub ot amit mi hatarozunk meg de ugy hogy ne interferaljon a masik kerekkel (Ertsd ne erintse )
 
![alt text][image12]
 
### Ha megvan ezt megismetelni a masik kupfogaskerekre is es match elni majd a fogakat circle function el klonozni .
 Ha ez megvan az en designom a konyebb design es a kenyelem miatt nem konvencialis csuklot tartalmaz hanem a vazba bevagas van 8 db bb golyo elhelyezesehez ami segit a gordulekeny forgas letrehozasaban 
 
![alt text][image4]
 
###Vegso mounting kamera 
 
![alt text][image13]
 
###Es a turtleboton elhelyezve 
 
![alt text][image13]
 
 
 
 
