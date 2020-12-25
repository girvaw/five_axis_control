# Some catkin notes
* Ensure that the catkin package is created with the following dependencies:

**catkin_create_pkg five_axis_robot controller_manager joint_state_controller robot_state_publisher**

* cd src
* git clone git@github.com:RobotArticulation/five_axis_control.git
* cd ..
* catkin_make

** when running the real robot, one may want to run dynamixel_control_hw within the same catkin workspace
* cd src
* git clone git@github.com:RobotArticulation/dynamixel_control_hw.git 
* note - dynamixel_control_hw depends on RobotArticulation/libdynamixel
* catkin_make

## Simulation ##

### Gazebo ###
* To run Gazebo:
    1) Open terminal window in catkin workspace
    2) source devel/setup.bash 
    3) roslaunch five_axis_robot gazebo_five_axis_kinect.launch

* This launch file does the following:
    1) Launches Gazebo
    2) Injects the five_axis_robot with a kinect_v2 head. 
    3) Starts a hardware controller to publish joint topics and services

* To **send** the controllers a target position command:
    rostopic pub -1 /five_axis_robot/joint1_position_controller/command std_msgs/Float64 "data: 3.142"

### ============= End of the Simulation ============= ###


## The real 5th axis ##
* To run roscore:
    1) Open terminal window in catkin workspace
    2) source devel/setup.bash 
    3) roscore

### Load the model then start the controller_manager and robot_state_publisher ###

* To launch the controller_manager and robot_state_publisher:
    1) Open terminal window in catkin workspace
    2) source devel/setup.bash
    3) roslaunch five_axis_robot five_axis.launch

* To **load** the controllers:
    rosservice call /five_axis_robot/controller_manager/load_controller "name: 'joint1_position_controller'"
    
* To **start** the controllers:
    rosservice call /five_axis_robot/controller_manager/switch_controller "{start_controllers: ['joint1_position_controller'],   stop_controllers: [], strictness: 2}"
    
* To **stop** the controllers:
    rosservice call /five_axis_robot/controller_manager/switch_controller "{start_controllers: [],   stop_controllers: ['joint1_position_controller'], strictness: 2}"
    
* To **send** the controllers a target position command:
    rostopic pub -1 /five_axis_robot/joint1_position_controller/command std_msgs/Float64 "data: 3.142"

### Rviz ###

* To launch the real five axis and rviz:
    1) Open terminal window in catkin workspace
    2) source devel/setup.bash 
    3) roslaunch five_axis_robot rviz_five_axis.launch

* To launch the standalone model with joint_state_publisher_gui in rviz:
    1) Open terminal window in catkin workspace
    2) source devel/setup.bash 
    3) roslaunch five_axis_robot rviz_five_axis_standalone.launch


