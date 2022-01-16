<h1 align="center">
  <br>
 <img src="https://github.com/Vamshi2198/Go-Chase-it-/blob/main/src/images/Project_title.png">
  <br>
</h1>
  
<h2 align="center">A mobile robot that chases red colored balls</h2>
  
<p align="center">
  <a href="https://www.udacity.com/robotics">
     <img src="https://s3-us-west-1.amazonaws.com/udacity-robotics/Extra+Images/RoboND_flag.png">
  </a>
  <a href="https://husarion.com/manuals/rosbot/">
     <img src="https://github.com/Vamshi2198/Go-Chase-it-/blob/main/src/images/husarion.jpg" width = "100" height = "50" >
  </a>
  <a href="https://aws.amazon.com/robomaker/">
     <img src="https://github.com/Vamshi2198/Go-Chase-it-/blob/main/src/images/aws.png" width = "100" height = "50">
  </a>
</p>

<p align="center">
  <a href="#overview">Overview</a> •
  <a href="#prerequisites">Prerequisites</a> •
  <a href="#directory-structure">Directory Structure</a> •
  <a href="#how-to-launch">How To Launch</a> •
  <a href="#testing">Testing</a>
</p>

![](https://github.com/Vamshi2198/Go-Chase-it-/blob/main/Go-Chase-It-GIF.gif)
## Overview  
This project is a part of Udacity's Robotics Software Engineer Nanodegree Program. In this project, I used [ROSbot](https://github.com/husarion/rosbot_description) as a mobile robot and [aws-robomaker-bookstore-world](https://github.com/aws-robotics/aws-robomaker-bookstore-world) as a gazebo world to replicate realistic simulation. ROSbot had a hard time following white colored balls due to the brightness present inside the bookstore-world. So, I designed red colored ball in gazebo to enable ROSbot to chase it. There are two C++ nodes required to program ROSbot to follow red colored balls.

1. `drive_bot`:  
* This node provides a `ball_chaser/command_robot` service to drive the robot by controlling its linear x and angular z velocities. The service should publish to the wheel joints and return back the requested velocities.

2. `process_image`:
* This node reads the robot’s camera image, analyzes it to determine the presence and position of a red ball. If a red ball exists in the image, this node requests a service via a client to drive the robot towards it.  

## Prerequisites
* Gazebo >= 7.0.
* ROS Kinetic .
* Cmake >= 4.1(mac, linux), 3.81(Windows)
* gcc/g++ >= 5.4
* Git
* you can click [here](https://s3-us-west-1.amazonaws.com/udacity-robotics/Virtual+Machines/Lubuntu_071917/RoboVM_V2.1.0.zip) to download the virtual machine with all dependencies included. 
* you will need [7-zip](http://www.7-zip.org/download.html) to extract the image and [VMware](http://www.vmware.com/) to run the virtual machine.


## Directory Structure  
```
.Go-Chase-It                                   # Go Chase It Project
├── catkin_ws                                  # Catkin workspace
│   ├── src
│   │   ├── aws-robomaker-bookstore-world      # folder that contains bookstore world
│   │   ├── ball_chaser                        # ball_chaser package        
│   │   │   ├── launch                         # launch folder for launch files
│   │   │   │   ├── ball_chaser.launch
│   │   │   ├── src                            # source folder for C++ scripts
│   │   │   │   ├── drive_bot.cpp
│   │   │   │   ├── process_images.cpp
│   │   │   ├── srv                            # service folder for ROS services
│   │   │   │   ├── DriveToTarget.srv
│   │   │   ├── CMakeLists.txt                 # compiler instructions
│   │   │   ├── package.xml                    # package info
│   │   ├── images 
│   │   ├── my_ball/my_ball                    # Model files for ball colored ball 
│   │   │   ├── model.config
│   │   │   ├── model.sdf
│   │   ├── my_robot                           # my_robot package        
│   │   │   ├── launch                         # launch folder for launch files   
│   │   │   │   ├── robot_description.launch
│   │   │   │   ├── world.launch               # Launches bookstore world
│   │   │   │   ├── teleop.launch              # To drive the rosbot
│   │   │   ├── meshes                         # meshes folder for sensors
│   │   │   │   ├── astra.stl
│   │   │   │   ├── box.stl
│   │   │   │   ├── rplidar.stl
│   │   │   │   ├── upper.stl
│   │   │   │   ├── wheel.stl
│   │   │   ├── realsense2_camera              # folder that contains launch files for realsense camera
│   │   │   ├── realsense2_description         # folder that contains description files for realsense camera
│   │   │   ├── urdf                           # urdf folder for xarco files
│   │   │   │   ├── materials.xacro            #contains material properties used in rosbot
│   │   │   │   ├── my_robot.xacro             
│   │   │   │   ├── rosbot.gazebo              #contains plugins to interact with rosbot
│   │   │   ├── worlds                         # world folder for world files
│   │   │   │   ├── empty.world
│   │   │   ├── CMakeLists.txt                 # compiler instructions
│   │   │   ├── Go-chase-it.rviz               # rviz configuration
│   │   │   ├── package.xml                    # package info
```

## How To Launch

#### Clone the project in catkin_ws/src/ and source the environment
```sh
$ cd /home/workspace/catkin_ws/src/
$ git clone https://github.com/Vamshi2198/Go-Chase-it-
$ source /opt/ros/(ros-distro)/setup.bash
```
#### Note : The world file proivided is empy because it only contains the url of remote repository, for this purpose you need to clone the aws-bookstore-world and place it inside your src folder. Also, delete the folder named aws-robomaker-bookstore-world manually before cloning.
```sh
$ cd /home/workspace/catkin_ws/src/
$ git clone https://github.com/aws-robotics/aws-robomaker-bookstore-world
```
#### Build the `Go-Chase-It` project
```sh
$ cd /home/workspace/catkin_ws/ 
$ catkin_make
```
#### After building the package, source your workspace
```sh
$ cd /home/workspace/catkin_ws/
$ source devel/setup.bash
```
#### Launch my_robot in Gazebo
```sh
$ roslaunch my_robot world.launch
```

#### Launch ball_chaser and process_image nodes in another terminal
```sh
$ cd /home/workspace/catkin_ws/
source devel/setup.bash
roslaunch ball_chaser ball_chaser.launch
```
#### Visualize
```sh
cd /home/workspace/catkin_ws/
source devel/setup.bash
rosrun rqt_image_view rqt_image_view
```

## Testing
Move the ball using the translate mode in gazebo. you should see the ROSbot follow the ball if it is in the region of the camera.
The code was tested on the following specifications
- **Processor:** `Intel Core i7-10875H`
- **Graphics:** `Nvidia GeForce GTX 1650 Ti 4GB GDDR6`
- **OS:** ` Ubuntu 20.04.3 LTS`
- **Kernal:** `5.10.60.1-microsoft-standard-WSL2`
- **ROS:** `noetic`
