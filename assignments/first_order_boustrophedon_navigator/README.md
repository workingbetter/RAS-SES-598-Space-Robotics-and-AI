# First-Order Boustrophedon Navigator 
## Michael
## Overview
This project involves tuning a PD controller and optimizing pattern parameters for a boustrophedon navigation system to achieve effective area coverage with a robot.
## Installation
- Clone the repository and create a symlink:
  ```bash
  cd ~/
  git clone https://github.com/workingbetter/RAS-SES-598-Space-Robotics-and-AI.git
  cd ~/ros2_ws/src
  ln -s ~/RAS-SES-598-Space-Robotics-and-AI/assignments/first_order_boustrophedon_navigator .

## Build the package
cd ~/ros2_ws
colcon build --packages-select first_order_boustrophedon_navigator
source install/setup.bash

## Launch the demo
ros2 launch first_order_boustrophedon_navigator boustrophedon.launch.py

##Controller Tuning
###Tuned Parameters
*Kp_linear: 0.7 - Increased from 0.5 to reduce steady-state error.
*Kd_linear: 0.1 - Increased from 0.05 to dampen oscillations observed at higher Kp_linear.
*Kp_angular: 0.6 - Adjusted for better cornering performance.
*Kd_angular: 0.08 - Fine-tuned to ensure smooth angular movements.
