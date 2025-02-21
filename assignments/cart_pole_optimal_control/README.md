# Cart-Pole Optimal Control Assignment

[Watch the demo video](https://drive.google.com/file/d/1UEo88tqG-vV_pkRSoBF_-FWAlsZOLoIb/view?usp=sharing)
![image](https://github.com/workingbetter/RAS-SES-598-Space-Robotics-and-AI/blob/7121eb081454743c36d2e1d7ab1cf0a09e7ccce0/assignments/cart_pole_optimal_control/images/Cart%20Pole%20tunned.png)

## Overview of Approach
For this assignment, I manually tuned the LQR controller by modifying the Q and R matrices in lqr_controller.py, rebuilt the package with colcon build, and relaunched the simulation using ros2 launch cart_pole_optimal_control cart_pole_rviz.launch.py. I observed the system's behavior in RViz and noted performance improvements with specific tunings.

- Q Matrix: A diagonal matrix that assigns weights to the states, penalizing deviations from the equilibrium (x = 0, θ = 0). The state vector is [x, x_dot, theta, theta_dot]:







































## License
This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).
[![Creative Commons License](https://i.creativecommons.org/l/by/4.0/88x31.png)](http://creativecommons.org/licenses/by/4.0/) 
