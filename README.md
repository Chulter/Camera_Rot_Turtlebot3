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
 git clone https://github.com/ROBOTIS-GIT/dynamixel-workbench.git
 git clone https://github.com/ROBOTIS-GIT/dynamixel-workbench-msgs.git
  `
  
  
  
  
  
