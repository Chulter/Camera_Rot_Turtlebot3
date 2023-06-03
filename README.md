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
 
