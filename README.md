# Four-Wheel Robot Gazebo Simulation

ROS/Gazebo simulation of a four-wheel mobile robot with LiDAR obstacle avoidance and waypoint navigation.

## Overview

This educational ROS 1 project contains a four-wheel mobile robot model and experiments with basic motion planning in simulation. The robot is described with URDF/Xacro, visualized in RViz, and configured for Gazebo with a differential-drive controller and a laser scanner.

The repository has two separate catkin workspaces: `simulation_ws` contains the robot model, worlds, and launch files; `catkin_ws` contains the `motion_plan` Python nodes.

## Features

- Four-wheel robot model with a simulated laser scanner.
- Gazebo world and launch-file examples.
- A waypoint node that reads odometry, accepts `des_pos_x` and `des_pos_y` parameters, and publishes velocity commands to `/cmd_vel`.
- A LiDAR obstacle-avoidance node that divides laser readings into regions and publishes velocity commands.
- A laser-reading node that logs ranges from the scan topic.

## Tech Stack

- ROS 1 and catkin
- Python with `rospy`
- Gazebo and RViz
- URDF/Xacro
- ROS messages including `geometry_msgs`, `sensor_msgs`, and `nav_msgs`

## Requirements

The original project documentation names ROS Noetic, Gazebo, and RViz. However, the motion-planning scripts contain Python 2 syntax, while ROS Noetic uses Python 3. Port the scripts to Python 3 or use a compatible ROS 1 environment before attempting to run them.

The catkin manifests and launch files also need review; installing ROS alone is not sufficient to guarantee that the project will build or launch.

## Repository Layout

```text
simulation_ws/
  src/
    m4wr_description/  # Robot model, Gazebo configuration, and RViz/spawn launches
    Playground_game/   # Gazebo worlds and example launch files
catkin_ws/
  src/
    motion_plan/       # Waypoint, obstacle-avoidance, and laser-reading nodes
```

## Build and Run

Build each catkin workspace from its own directory after installing the required ROS packages:

```bash
cd simulation_ws
catkin_make

cd ../catkin_ws
catkin_make
```

Source the appropriate workspace setup file in each terminal before launching its packages. The existing launch files do not currently provide a verified, end-to-end startup sequence; see the limitations below before attempting to run the simulation.

## Limitations

- `go_to_point.py` uses Python 2 print syntax and needs porting for ROS Noetic.
- `obstacle_avoidance.py` subscribes to `/m2wr/laser/scan`, but the robot model publishes scans on `/m4wr/laser/scan`; the node will not receive the model's scans without a topic change or remapping.
- `bug0.launch` refers to `m2wr_description` and `simulation_gazebo`, which do not match the packages present in this repository.
- `motion_plan/package.xml` omits dependencies imported by the scripts, including `nav_msgs`, `tf`, and `std_srvs`.
- The package metadata still contains a placeholder license, and no automated tests or verified simulation workflow are included.
- This is experimental simulation code, not validated software for controlling a physical robot.
