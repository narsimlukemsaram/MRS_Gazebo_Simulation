# MRS Gazebo Simulation
This is the repository for the Multi-Robot Systems (MRS) Gazebo Simulation: To run two UAVs in SITL, they can arm and start missions simultaneously using keyboard control.   

# System Requirements
Required OS is Ubuntu 18.04 LTS 64-bit (ROS Melodic). The repository are supposed to be compiled by catkin tools.

# Dependencies
External dependencies required.

Install the DarkNet ROS and its dependencies from:
https://github.com/leggedrobotics/darknet_ros

Install the XTDrone and its dependencies from:
https://www.yuque.com/xtdrone/manual_en/basic_config

# Installation
We will assume you have your ROS workspace in the ~/catkin_ws folder link and you have installed all the dependencies (DarkNet ROS and XTDrone). 

# Software
Clone the repository:
$ cd ~/catkin_ws/src
$ git clone https://github.com/narsimlukemsaram/mrs_gazebo_simulation.git

Compile the software:
$ catkin_make

After successful compilation, follow below steps to execute the simulation:

# Terminal 1:
Start roscore
$ roscore

# Terminal 2:
Start YOLO/DarkNet ROS on UAV1, at this point, load the network parameters first, and then wait for the image to arrive.
$ cd ~/catkin_ws
$ source devel/setup.bash
$ roslaunch launch_inference launch_inference_drone_1.launch

# Terminal 3:
Start YOLO/DarkNet ROS on UAV2, at this point, load the network parameters first, and then wait for the image to arrive.
$ cd ~/catkin_ws
$ source devel/setup.bash
$ roslaunch launch_inference launch_inference_drone_2.launch

# Terminal 4:
Then start PX4 simulation, at this time YOLO receives the image and starts other UAV detection.
$ cd ~/PX4_Firmware/launch
$ roslaunch px4 darknet_ros_iris_two_drone.launch

# Terminal 5:
Then establish UAV1 communication.
$ cd ~/XTDrone/communication
$ python multirotor_communication.py iris 0

# Terminal 6:
Then establish UAV2 communication.
$ cd ~/XTDrone/communication
$ python multirotor_communication.py iris 1

# Terminal 7:
Control UAV1 takeoff. For multiple UAVs, it is easy for Offboard mode to control it to take off with the expectation of a greater than 0.3 m/s speed in z axis, so you can not use Takeoff mode (v in the keyboard). Press I repeatedly to increase the expected z speed to more than 0.3m/s, then press B to switch to the offboard mode, then press T to unlock and take off. After flying to the appropriate height, press S to achieve hovering.
$ cd ~/XTDrone/control/keyboard
$ python modified_multirotor_keyboard_control.py iris 1 vel 0

# Terminal 8:
Control UAV2 takeoff. For multiple UAVs, it is easy for Offboard mode to control it to take off with the expectation of a greater than 0.3 m/s speed in z axis, so you can not use Takeoff mode (v in the keyboard). Press I repeatedly to increase the expected z speed to more than 0.3m/s, then press B to switch to the offboard mode, then press T to unlock and take off. After flying to the appropriate height, press S to achieve hovering.
$ cd ~/XTDrone/control/keyboard
$ python modified_multirotor_keyboard_control.py iris 1 vel 1
