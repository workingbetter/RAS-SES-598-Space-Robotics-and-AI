# Cart-Pole Optimal Control Assignment

[Watch the demo video](https://drive.google.com/file/d/1UEo88tqG-vV_pkRSoBF_-FWAlsZOLoIb/view?usp=sharing)
![image](https://github.com/workingbetter/RAS-SES-598-Space-Robotics-and-AI/blob/7121eb081454743c36d2e1d7ab1cf0a09e7ccce0/assignments/cart_pole_optimal_control/images/Cart%20Pole%20tunned.png)

## Overview of Approach
For this assignment, I manually tuned the LQR controller by modifying the Q and R matrices in lqr_controller.py, rebuilt the package with colcon build, and relaunched the simulation using ros2 launch cart_pole_optimal_control cart_pole_rviz.launch.py. I observed the system's behavior in RViz and noted performance improvements with specific tunings.

- Q Matrix: A diagonal matrix that assigns weights to the states, penalizing deviations from the equilibrium (x = 0, θ = 0). The state vector is [x, x_dot, theta, theta_dot]:
- [0]: Weight on cart position (x, in meters)
- [1]: Weight on cart velocity (x_dot, in m/s)
- [2]: Weight on pendulum angle (theta, in radians)
- [3]: Weight on pendulum angular velocity (theta_dot, in rad/s)

Higher Q values make the controller prioritize minimizing that state, making it more responsive to deviations.

- R Matrix: A scalar (in this case) that penalizes the control input (u, force in Newtons).
- Higher R values make the controller more conservative, reducing control effort (force) at the expense of state regulation.
- Lower R values allow more aggressive control, applying larger forces to stabilize the system.

## Default Parameters
- Q = diag([1.0, 1.0, 10.0, 10.0])
Low weight on x (1.0) and x_dot (1.0), meaning less priority on keeping the cart near the center or damping its velocity.
Higher weight on theta (10.0) and theta_dot (10.0), emphasizing keeping the pendulum upright.
- R = [[0.1]]
Moderate penalty on control effort, limiting the force applied to the cart.

## Tuning Strategy

The default tuning prioritizes pendulum stability over cart position, which may allow the cart to drift toward its ±2.5m limits under the 15N earthquake disturbances. To improve performance:

- Increase Q[x]: To keep the cart closer to the center (x = 0).
- Adjust Q[theta]: To maintain pendulum stability, possibly increasing it further if needed.
- Decrease R: To allow more control effort to counteract disturbances, but not too low to avoid excessive forces.
- 
After testing, I found that Q = [50, 5, 20, 10] and R = [0.05] prevented the system from falling, indicating a successful tuning.

## Analysis of Existing Q and R Matrices

Default Tuning: Q = [1, 1, 10, 10], R = [0.1]
- Observation: When running the simulation with default parameters, the pendulum often fell, and the cart exceeded the ±2.5m limit. This suggests insufficient control under the 15N disturbances.
- Analysis:
- Low Q[0,0] = 1: Minimal effort to keep x near zero, allowing the cart to drift.
- Low Q[1,1] = 1: Little damping of cart velocity.
- Moderate Q[2,2] = 10 and Q[3,3] = 10: Focus on pendulum stability, but not enough to counter disturbances.
- R = 0.1: Limits control force, reducing the controller's ability to respond aggressively.
### Tuned Parameters: Q = [50, 5, 20, 10], R = [0.05]
- Observation: With Q = [50, 5, 20, 10] and R = [0.05], the system remained stable (no falling) for the entire 60-second simulation.
- Analysis:
- High Q[0,0] = 50: Strong emphasis on keeping the cart near the center, reducing displacement.
- Moderate Q[1,1] = 5: Improved velocity damping compared to default.
- High Q[2,2] = 20: Increased priority on pendulum angle stability.
- Q[3,3] = 10: Maintained focus on angular velocity.
- Low R = 0.05: Allows more control effort (higher forces) to counteract disturbances.
- Duration of Stable Operation: The cart stays stable, It didn't fall for at lieast 5 minutes.
### Trade-offs
- Default: Prioritizes low control effort and pendulum stability but sacrifices cart position control.
- Tuned: Balances cart position and pendulum stability, using more control effort. This tuning better handles disturbances but increases force usage.

# Tuned performance metrics
- Max Cart Displacement: 0.27 m
- Max Angle Deviation: 3.26 deg
- Max Control Force: 78.13 N
- Avg Control Force: 13.00 N


























## License
This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).
[![Creative Commons License](https://i.creativecommons.org/l/by/4.0/88x31.png)](http://creativecommons.org/licenses/by/4.0/) 
