# The cp15_auxiliary_files repository

## Description

This respository contains a couple of files:

1. Configuration files:
    This is the example of using this three controller
    
    joint_state_broadcaster:
      type: joint_state_broadcaster/JointStateBroadcaster

    joint_trajectory_controller:
          type: joint_trajectory_controller/JointTrajectoryController

    rb1_base_controller:
      type: diff_drive_controller/DiffDriveController

2. Urdf file
    This rb1_ros2_base.urdf.xacro is composed of the files in this folder.
    I modified the elevator.urdf.xacro joint. I add the friction to avoid elevator slide down when the elevator up.

## Install instruction 

```
git clone https://github.com/leokim0711092/Checkpoint15_ControlFramework.git
```
```
cd ~/ros2_ws && colcon build && source install/setup.bash
```

## Launch 
1. Description: 
    This launch file include 7 things:
    1) Open Gazebo
    2) Spawn robot
    3) Publish robot state
    4) Wait for service client node: Waiting the control manager appear
    5) Load joint_state_controller
    6) Load diff_drive_controller
    7) Load_joint_controller

2. Execute
```
ros2 launch rb1_ros2_description rb1_ros2_xacro.launch.py
```

## Operation

1. Moving:
```
ros2 topic pub --rate 10 /rb1_base_controller/cmd_vel_unstamped geometry_msgs/msg/Twist "{linear: {x: 0.0, y: 0, z: 0.0}, angular: {x: 0.0,y: 0.0, z: 0.2}}"
```
2. Lifting the object
```
 ros2 action send_goal /joint_trajectory_controller/follow_joint_trajectory control_msgs/action/FollowJointTrajectory "{trajectory: {joint_names: ['robot_elevator_platform_joint'], points: [{positions: [0.05], time_from_start: {sec: 1, nanosec: 0}}]}}"
```
3. Elevator down
```
 ros2 action send_goal /joint_trajectory_controller/follow_joint_trajectory control_msgs/action/FollowJointTrajectory "{trajectory: {joint_names: ['robot_elevator_platform_joint'], points: [{positions: [0.00], time_from_start: {sec: 1, nanosec: 0}}]}}"
```