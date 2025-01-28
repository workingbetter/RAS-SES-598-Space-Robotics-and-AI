
# First-Order Boustrophedon Navigator

ROS2 package for precise lawnmower pattern navigation in Turtlesim using PD control.

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

![Velocity Profiles](https://github.com/workingbetter/RAS-SES-598-Space-Robotics-and-AI/blob/0bdb14aedaa7ed18d95e0293cd53cdbbdbbd510e/assignments/first_order_boustrophedon_navigator/turtle%20poxe%20x%20and%20y.png)
### Real-Time Pose Tracking

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
   - 0.6 provided complete coverage without overlap and higher values doesn't cover the whole area 
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

## Installation & Reproduction
```bash
# Clone repository
cd ~/ros2_ws/src
git clone https://github.com/warkingbetter/first_order_boustrophedon_navigator.git
# Build and run
colcon build --packages-select first_order_boustrophedon_navigator
ros2 launch first_order_boustrophedon_navigator boustrophedon.launch.py
