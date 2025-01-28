
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

## Visual Evidence
![Lawnmower Trajectory](https://github.com/workingbetter/RAS-SES-598-Space-Robotics-and-AI/blob/537238ab11d82693bff234bc724700ee66e2dbe8/assignments/first_order_boustrophedon_navigator/Boustrophedon_pattern.png) 

![Cross-Track Error Analysis](https://github.com/workingbetter/RAS-SES-598-Space-Robotics-and-AI/blob/3ec2acfab9f24356aaf6edb7c5be33f27d3a3c26/assignments/first_order_boustrophedon_navigator/error_analysis_complete.png)

![Velocity Profiles]()

## Tuning Methodology

### Phase 1: Linear Motion Optimization
1. Started with default Kp_linear=1.0, observed sluggish response
2. Increased Kp_linear incrementally:
   - 2.0: Reduced steady-state error by 40%
   - 5.0: Eliminated residual position errors
3. Tested Kd_linear values (0.1-0.5):
   - Chose 0.1 for fastest response despite minor oscillations

### Phase 2: Angular Control Tuning
1. Initial Kp_angular=2.0 caused slow turn completion
2. Final Kp_angular=8.0 achieved:
   - 180° turn completion in <0.5s
   - Maximum turn overshoot: 0.15 units
3. Kd_angular kept minimal to avoid turn hesitation

### Phase 3: Pattern Optimization
1. Tested spacing values (0.5-1.5):
   - 0.6 provided complete coverage without overlap
   - Validated through trajectory visualization
2. Verified 12 complete passes in Turtlesim's 11x11 grid

## Challenges & Solutions
1. **Oscillations at High Speed**
   - Symptom: Wobbling during straight segments at Kp_linear=6.0
   - Solution: Reduced to 5.0 with Kd_linear=0.1 compromise

2. **Turn Overshoot**
   - Symptom: Turtle overshooting 180° turns
   - Solution: Increased Kp_angular to 8.0 for faster correction

3. **Edge Coverage**
   - Symptom: Incomplete coverage at world boundaries
   - Solution: Adjusted waypoint generation logic

## Parameter Sensitivity Analysis
| Parameter | +10% Effect            | -10% Effect              |
|-----------|------------------------|--------------------------|
| Kp_linear | Faster but oscillatory | Increased tracking error |
| Kp_angular| Crisper turns          | Slower heading response  |
| Spacing   | Efficiency gain        | Coverage gaps            |

## Installation & Reproduction
```bash
# Clone repository
cd ~/ros2_ws/src
git clone https://github.com/warkingbetter/first_order_boustrophedon_navigator.git
# Build and run
colcon build --packages-select first_order_boustrophedon_navigator
ros2 launch first_order_boustrophedon_navigator boustrophedon.launch.py
