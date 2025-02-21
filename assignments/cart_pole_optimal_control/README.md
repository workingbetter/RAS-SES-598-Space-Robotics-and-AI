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
- Higher Q values make the controller prioritize minimizing that state, making it more responsive to deviations.

- R Matrix: A scalar (in this case) that penalizes the control input (u, force in Newtons).
- Higher R values make the controller more conservative, reducing control effort (force) at the expense of state regulation.
- Lower R values allow more aggressive control, applying larger forces to stabilize the system.

## Default Parameters
- Q = diag([1.0, 1.0, 10.0, 10.0])
Low weight on x (1.0) and x_dot (1.0), meaning less priority on keeping the cart near the center or damping its velocity.
Higher weight on theta (10.0) and theta_dot (10.0), emphasizing keeping the pendulum upright.
- R = [[0.1]]
Moderate penalty on control effort, limiting the force applied to the cart.

































## License
This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).
[![Creative Commons License](https://i.creativecommons.org/l/by/4.0/88x31.png)](http://creativecommons.org/licenses/by/4.0/) 
