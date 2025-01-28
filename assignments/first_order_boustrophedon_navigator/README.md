
# First-Order Boustrophedon Navigator
ROS2 package for precise lawnmower pattern navigation in Turtlesim using PD control.
This package enables autonomous coverage of a rectangular area in Turtlesim using a boustrophedon ("lawnmower") pattern, implemented with a PD (Proportional-Derivative) control system. Designed for efficiency and precision, it ensures full spatial coverage with minimal tracking error, making it ideal for applications requiring systematic exploration or resource distribution.

## Key Features
PD Control System: Optimized linear and angular velocity control for smooth trajectory tracking.

High-Density Coverage: Configurable spacing 0.6 units ensures no gaps or overlaps.

Real-Time Performance Monitoring: Integrated metrics for cross-track error and coverage completeness.

Parameter Tuning: For fast convergence and stability.

Visualization Tools: plots, error analysis, and live pose tracking via rqt_plot.

## Overview
Control Strategy
The robot follows waypoints generated in a boustrophedon pattern, alternating direction between rows to maximize efficiency. A PD controller adjusts linear and angular velocities to minimize tracking errors:

Linear Control: Aggressive proportional gain (Kp_linear=5.0) ensures rapid position correction, while a minimal derivative gain (Kd_linear=0.1) prevents overshoot.

Angular Control: High Kp_angular=8.0 enables instantaneous heading adjustments during turns, critical for maintaining the tight 180° reversals in the pattern.

Performance Validation
Precision: Achieves an average cross-track error of 0.102 units (under the 0.2 requirement) and 100% coverage completeness.

Responsiveness: Tuned derivative terms (Kd_linear=0.1, Kd_angular=0.1) balance damping and agility, enabling sharp transitions without oscillations.

## Development Insights
### Systematic Tuning Approach
Linear Optimization: Incrementally increased Kp_linear to eliminate sluggish motion while avoiding oscillations.

Angular Refinement: Boosted Kp_angular to ensure crisp 180° turns, critical for pattern integrity.

Pattern Validation: Tested spacing values to guarantee full coverage without redundant paths.

## Cross track Error Calculation
Data Collection
   - Saved the cross truck errors  to local directory using the code ros2 topic echo /cross_track_error > error_log.txt


After extracting the files I have used Google colab to create matplot lib visualization for the average and maximum cross track errors. Finally, I have uploaded to the github repository how i generated the visualisation using matplotlib and numpy.

### Challenges Overcome
Oscillation Mitigation: High Kp values initially caused overshoot; derivative terms were carefully calibrated to dampen oscillations while preserving speed.

Visualization Setup: Switched to Windows VSCode to resolve Matplotlib compatibility issues on VMs, enabling real-time error plotting.


## Final Tuned Parameters
| Parameter       | Value | Justification                              |
|-----------------|-------|--------------------------------------------|
| Kp_linear       | 5.0   | Fast position correction                   |
| Kd_linear       | 0.1   | Minimal damping for fast response          |
| Kp_angular      | 8.0   | Instantaneous turn response                |  
| Kd_angular      | 0.1   | Allow sharp turn transitions               |
| Spacing         | 0.6   | High-density coverage pattern              |

## Performance Metrics
| Metric                  | Value  | Requirement  | Status |
|-------------------------|--------|--------------|--------|
| Average Cross-Track Error | 0.102 | <0.2        | ✅     |
| Maximum Cross-Track Error | 0.266 | <0.5        | ✅     |
| Coverage Completeness    | 100%  | Full         | ✅     |

## Visualization
![Lawnmower Trajectory](https://github.com/workingbetter/RAS-SES-598-Space-Robotics-and-AI/blob/537238ab11d82693bff234bc724700ee66e2dbe8/assignments/first_order_boustrophedon_navigator/Boustrophedon_pattern.png) 

![Cross-Track Error Analysis](https://github.com/workingbetter/RAS-SES-598-Space-Robotics-and-AI/blob/1a6b1b65a4762461bbb70e29e9f341a83ff69d48/assignments/first_order_boustrophedon_navigator/error_analysis_complete1.png)

### Real-Time Pose Tracking
   ros2 run rqt_plot rqt_plot
![Velocity Profiles](https://github.com/workingbetter/RAS-SES-598-Space-Robotics-and-AI/blob/0bdb14aedaa7ed18d95e0293cd53cdbbdbbd510e/assignments/first_order_boustrophedon_navigator/turtle%20poxe%20x%20and%20y.png)

## Installation & Reproduction
```bash
# Clone repository
cd ~/ros2_ws/src
git clone https://github.com/workingbetter/first_order_boustrophedon_navigator.git
# Build and run
colcon build --packages-select first_order_boustrophedon_navigator
ros2 launch first_order_boustrophedon_navigator boustrophedon.launch.py
```
## Tuning Methodology

### Phase 1: Linear Motion Optimization
1. Started with default Kp_linear=1.0, observed sluggish response
2. Increased Kp_linear incrementally:
   - 2.0: Reduced sluggish responce
   - 5.0: Eliminated overshoot (10.0 caused oscillations near waypoints)
3. Tested Kd_linear values (0.1-0.5):
   - I selected 0.1 for fastest response  when I increase it to 0.5 it becomes slow at the end

### Phase 2: Angular Control Tuning
1. Initial Kp_angular=2.0 caused not boustrophedon pattern completion
2. Final Kp_angular=8.0 achieved:
   - Around 180° turn completion insted of the inital angular 2.0 which looks circular pattern
3. Kd_angular kept minimal(0.1) to avoid turn hesitation because it causes random pattern when increased 

### Phase 3: Pattern Optimization
1. Tested spacing values (0.5-1.5):
   - 0.6 provided complete coverage without overlap and higher values doesn't cover the whole area and smaller overlaps
   - Validated through trajectory visualization

### Challenges & Solutions
| Challenge                        | Solution                                  | Impact Avoided              |
|----------------------------------|-------------------------------------------|-----------------------------|
| Matplotlib installation on VM    | Used Windows VSCode                       | Enabled error visualization |

## Parameter comparision
| Parameter | Wnen Increased         | When Decreased           |
|-----------|------------------------|--------------------------|
| Kp_linear | Faster but oscillatory | Increased tracking error |
| Kp_angular| Crisper turns          | Slower heading response  |
| Spacing   | Quicker, Few coverage  | Overlaps                 |


