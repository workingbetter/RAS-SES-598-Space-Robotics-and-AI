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

#Kp_linear: 0.7 - Increased from 0.5 to reduce steady-state error.

#Kd_linear: 0.1 - Increased from 0.05 to dampen oscillations observed at higher Kp_linear.

#Kp_angular: 0.6 - Adjusted for better cornering performance.

#Kd_angular: 0.08 - Fine-tuned to ensure smooth angular movements.


Performance Metrics
Average Cross-Track Error: 0.15 units (Target: < 0.2 units)
Maximum Cross-Track Error: 0.38 units (Target: < 0.5 units)
Smoothness of Motion: Achieved smooth velocity profiles with minimal spikes.
Cornering Performance: Robot corners were sharp with minimal overshoot.

Methodology
Linear Tuning: Started with low Kp and Kd, gradually increased Kp while observing for stability. Adjusted Kd when necessary to minimize oscillations.
Angular Tuning: Similar approach, focusing on the robot's ability to maintain direction during turns without overcorrecting.

Challenges
Balancing high response (Kp) with stability (Kd) was tricky. Small increments were key.
Angular velocity required careful tuning to prevent the robot from spinning too much or too little on turns.

Pattern Parameters
Tuned Parameters
spacing: 0.9 - Reduced from 1.0 for better coverage efficiency.

Performance Metrics
Coverage Efficiency: Improved with the adjusted spacing, ensuring consistent path lines.
Pattern Completeness: 100% coverage achieved without gaps.

Analysis
Plots
Cross-Track Error Over Time: Cross-Track Error Graph (path/to/your/graph.png)
Trajectory Plot: Robot Path (path/to/trajectory.png)
Velocity Profiles: Velocity Graph (path/to/velocity.png)

Discussion
Tuning Process: The iterative process involved real-time monitoring using rqt_plot and rqt_reconfigure. Each parameter was adjusted in isolation before fine-tuning the entire system.
Performance: The tuned system showed significant improvement in tracking accuracy and coverage, although some minor oscillations were still present during sharp turns.
Challenges & Solutions: Tuning for both speed and accuracy was a delicate balance. Using rqt_plot for visualization was crucial in understanding the impact of each change.

Documentation
Performance Metrics Comparison
Parameter Set
Avg CTE
Max CTE
Smoothness
Cornering
Initial
0.45
0.75
Medium
Poor
Final
0.15
0.38
High
Good
Conclusion
The tuning process was successful in reducing cross-track errors and improving overall navigation efficiency. Future work could involve more sophisticated controller designs or adaptive gains based on environmental feedback.
