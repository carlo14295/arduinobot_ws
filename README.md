# Arduinobot — ROS2 Robot Arm

A 4-DOF robotic arm controlled via ROS2, with full Gazebo simulation, MoveIt2 motion planning, custom Arduino firmware, and Alexa voice control.

> **Note:** This project was built by following the Udemy course [Robotics and ROS 2 — Learn by Doing! Manipulators](https://www.udemy.com/course/robotics-and-ros-2-learn-by-doing-manipulators/). All credit for the original design and teaching goes to the course author.

## Overview

Arduinobot is a complete robotics project that bridges hardware and software:

- **Hardware**: Arduino-based servo arm (base, shoulder, elbow, gripper)
- **Firmware**: Custom Arduino sketches for smooth servo control via serial protocol
- **ROS2 Control**: Custom `ros2_control` hardware interface communicating over serial
- **Motion Planning**: MoveIt2 integration with Pilz and KDL planners
- **Simulation**: Gazebo/Ignition with full physics and ros2_control support
- **Remote Control**: Alexa skill integration via ROS2 action server

## Packages

| Package | Description |
|---|---|
| `arduinobot_description` | URDF/Xacro robot model with STL meshes |
| `arduinobot_controller` | `ros2_control` hardware interface (serial) + slider GUI |
| `arduinobot_firmware` | Arduino sketches and Python serial nodes |
| `arduinobot_moveit` | MoveIt2 config (SRDF, kinematics, controllers) |
| `arduinobot_bringup` | Top-level launch files for real and simulated robot |
| `arduinobot_msgs` | Custom actions, services, and message types |
| `arduinobot_remote` | Alexa interface and ROS2 action task server |
| `arduinobot_utils` | Angle conversion utilities (Euler ↔ Quaternion) |
| `arduinobot_py_examples` | Python ROS2 examples (pub/sub, services, actions, MoveIt) |
| `arduinobot_cpp_examples` | C++ ROS2 examples (pub/sub, services, actions, MoveIt) |

## Requirements

- ROS2 Humble or Iron
- Gazebo / Ignition Fortress
- MoveIt2
- `ros2_control`, `ros2_controllers`
- `libserial-dev` (for real hardware)
- Arduino IDE (for firmware upload)

## Build

```bash
cd arduinobot_ws
rosdep install --from-paths src --ignore-src -r -y
colcon build
source install/setup.bash
```

## Run

### Simulation (Gazebo + MoveIt2 + Alexa interface)

```bash
ros2 launch arduinobot_bringup simulated_robot.launch.py
```

### Real Robot

```bash
ros2 launch arduinobot_bringup real_robot.launch.py
```

### Visualize URDF only

```bash
ros2 launch arduinobot_description display.launch.py
```

## Firmware

Upload the Arduino sketch to your board before running the real robot:

```
src/arduinobot_firmware/firmware/robot_control/robot_control.ino
```

The firmware communicates over serial at 115200 baud. Commands follow the format `b<deg>,s<deg>,e<deg>,g<deg>,` for base, shoulder, elbow, and gripper joints.

## Architecture

```
Alexa / Python API
       |
  ROS2 Action Server (arduinobot_remote)
       |
  MoveIt2 (arduinobot_moveit)
       |
  ros2_control (arduinobot_controller)
       |
  Serial (115200 baud)
       |
  Arduino (arduinobot_firmware)
       |
  Servo Motors (base, shoulder, elbow, gripper)
```

## License

MIT
