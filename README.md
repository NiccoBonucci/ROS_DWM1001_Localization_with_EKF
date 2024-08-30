# Introduction to the project
This repository contains all the files needed for:
- Pose estimation of a robot using DWM1001 Ultra-WideBand Localization devices in combination with the UWB Android application
- Application of an Extended Kalman Filter to improve the pose estimation accuracy
- Comparison with ground truth values taken from the Optitrack localization system
The whole system has been tried and works on Ubuntu 20.04 with the Noetic Ninjemys version of ROS. The robot model used for this project is a unicycle, the simulation in the real environment is done with Turtlebot 2. To obtain the best result possible, make sure to follow the steps and the explanation. If you have a Optitrack system, it is a good pratice to set the UWB initiator anchor exactly on the Optitrack system origin, so that it's easier to compare the datas obtained from the different systems.

# Setup of the work environment
To properly setup your work environment to be able to use the localization system, you need to setup a catkin worksapce, then you need to clone this repository into the "src" folder.
Here's a brief explanation of the packages:
1) *ekf_unicycle_package* : package that contains the code to apply the Extended Kalman Filter to generate an estimation of the robot's pose, using the datas from the robot's odometry and the position generated by the UWB devices.
   
2) *ros-dwm1001-uwb-localization-master* : package that contains all the files related to the UltraWideBand localization system. The position of the tag/tags is obtained by combining the information of fixed anchors. The maximum number of anchors used for a single tag for the position estimation is 4 (the system chooses the 4 anchors closest to the tag's position). A serial connection from the tag to the computer is needed. At least 3 anchors are needed to estimate the in a proper way, better if it's 4 or more.
 The two main files to run are:
- dwm1001_active_1tag: this is the code to estimate the position of a robot. A single tag is needed; its 2D or 3D position is calculated by combining the information received by the fixed anchors. 
- dwm1001_active_2tag: this is the code to estimate the pose of a robot. 2 tags are needed. The robot's 2D or 3D position is obtained by combining the two tags' positions. The orientation is obtained by using the atan2 with respect to the estimated x and y positions of the tags.

3) *vrpn_client_ros* : package that contains the files to initialize the Optitrack localization system on ROS. For this matter, the optitrack is used as the ground truth information for the calculation of the error between the UWB estimated trajectory, the trajectory after the EKF filtering action and the real trajectory (knowing that the Optitrack has a very high accuracy).

# How to estimate the pose of a robot 
Here's the steps you have to follow to obtain a pose estimation from the UWB localization system:
1) Setup the anchors in the space you have and the tags on the robot making sure to save the positions and define the devices' type on the UWB app, then confirm the setup.
2) Setup the Optitrack's sensors on the robot so that the point used for the position estimation coincides with the UWB robot position estimation obtained by combining the two tags' datas.
3) Connect the tags to the USB ports of the computer and specify the ports' names in the params.yaml file of the UWB package
4) Enable the use of the serial ports (if the permission is denied)
5) Make sure you have an established connection to the Turtelbot 2 and the /odom topic is being published. If you are running the turtlebot and the EKF pose estimation code on two different computers, connect to the ROS Master of the Turtlebot to be able to subscribe correctly to the /odom topic
6) Run the following scripts:
   - 



