# Cart-Pole Optimal Control Assignment

![video](https://drive.google.com/file/d/1v2NaOhOOva_NndIcqf6n8oDgZT_1k1qv/view?usp=sharing)  


![image](https://github.com/workingbetter/RAS-SES-598-Space-Robotics-and-AI/blob/616d92ea7ed6c7dd0dd2f17885594ab91077da1f/assignments/cart_pole_optimal_control/images/Cart%20Pole%20tunned.png)

## Overview of Approach
For this assignment, I manually tuned the Linear Quadratic Regulator (LQR) controller by adjusting the Q and R matrices in `lqr_controller.py`. After each tuning iteration, I rebuilt the package using `colcon build` and relaunched the simulation with `ros2 launch cart_pole_optimal_control cart_pole_rviz.launch.py`. I observed the system's behavior in RViz and refined the parameters based on performance.

### Q Matrix
- **Definition**: A diagonal matrix that assigns weights to the states, penalizing deviations from the equilibrium (`x = 0`, `θ = 0`).
- **State Vector**: `[x, x_dot, theta, theta_dot]`  
  - `[0]`: Weight on cart position (`x`, in meters)  
  - `[1]`: Weight on cart velocity (`x_dot`, in m/s)  
  - `[2]`: Weight on pendulum angle (`theta`, in radians)  
  - `[3]`: Weight on pendulum angular velocity (`theta_dot`, in rad/s)  
- **Effect**: Higher Q values increase the controller's focus on minimizing deviations in that state, improving responsiveness.

### R Matrix
- **Definition**: A scalar that penalizes the control input (`u`, force in Newtons).  
- **Effect**:  
  - Higher R values reduce control effort, making the controller more conservative.  
  - Lower R values allow more aggressive control to stabilize the system.

## Default Parameters
- **Q Matrix**: `diag([1.0, 1.0, 10.0, 10.0])`  
  - Low weight on `x` (1.0) and `x_dot` (1.0), prioritizing pendulum stability over cart position.  
  - Higher weight on `theta` (10.0) and `theta_dot` (10.0), focusing on keeping the pendulum upright.  
- **R Matrix**: `[[0.1]]`  
  - Moderate penalty on control effort, limiting the force applied.

## Tuning Strategy
The default tuning favored pendulum stability but allowed the cart to drift toward its ±2.5m limits under 15N earthquake disturbances. To address this, I used the following strategy:

- **Increase Q[x]**: To prioritize keeping the cart near the center (`x = 0`).  
- **Adjust Q[theta]**: To ensure pendulum stability, increasing it if necessary.  
- **Decrease R**: To allow more control effort to counter disturbances, while avoiding excessive forces.

After experimentation, I settled on **Q = [50, 5, 20, 10]** and **R = [0.05]**, which kept the system stable without falling.

## Analysis of Existing Q and R Matrices

### Default Tuning: Q = [1, 1, 10, 10], R = [0.1]
- **Observation**: The pendulum often fell, and the cart exceeded the ±2.5m limit under disturbances.  
- **Analysis**:  
  - `Q[0,0] = 1`: Minimal effort to keep `x` near zero, allowing drift.  
  - `Q[1,1] = 1`: Little damping of cart velocity.  
  - `Q[2,2] = 10` and `Q[3,3] = 10`: Reasonable focus on pendulum stability, but insufficient for disturbances.  
  - `R = 0.1`: Limited control force, reducing responsiveness.

### Tuned Parameters: Q = [50, 5, 20, 10], R = [0.05]
- **Observation**: The system remained stable for the full 60-second simulation.  
- **Analysis**:  
  - `Q[0,0] = 50`: Strong focus on keeping the cart centered.  
  - `Q[1,1] = 5`: Improved velocity damping.  
  - `Q[2,2] = 20`: Enhanced pendulum angle stability.  
  - `Q[3,3] = 10`: Maintained angular velocity control.  
  - `R = 0.05`: Allowed higher control effort to counter disturbances.  
- **Duration of Stable Operation**: Stable for at least 5 minutes without falling.

### Trade-offs
- **Default Tuning**:  
  - Emphasizes low control effort and pendulum stability but neglects cart position.  
- **Tuned Parameters**:  
  - Balances cart position and pendulum stability with increased control effort.  
  - Better disturbance rejection at the cost of higher forces.

### Data Extraction
To analyze the system's performance, I extracted data from the simulation logs. Specifically, I used the `ros2 topic echo` command to capture the joint state and control force data into text files. For example:

- Joint state data: ros2 topic echo /world/empty/model/cart_pole/joint_state > joint_state_data.txt
- Control force data: ros2 topic echo /model/cart_pole/joint/cart_to_base/cmd_force > control_force_data.txt


These commands recorded the necessary data points during the simulation, which I later used for analysis.

### Performance Analysis and Visualization
I used Python with the Matplotlib library to analyze the extracted data and visualize the system's performance. The analysis included calculating key metrics such as maximum cart displacement, maximum angle deviation, maximum control force, and average control force.

I wrote a Python script to parse the text files and compute these metrics. For example:

- Parsed the joint state data to extract cart positions and pendulum angles.  
- Parsed the control force data to extract the applied forces.  
- Computed the maximum and average values for the relevant metrics.

Additionally, I generated plots to visualize the cart position, pendulum angle, and control force over time. These plots helped me understand the system's behavior and verify the effectiveness of the tuned parameters.

Here's an partial code snippet used for plotting:

```python
import numpy as np
import matplotlib.pyplot as plt

# Load and parse data
cart_positions, pendulum_angles = parse_joint_state('joint_state_data.txt')
control_forces = parse_control_force('control_force_data.txt')

# Plot cart position
plt.plot(time, cart_positions, label='Cart Position')
plt.xlabel('Time (s)')
plt.ylabel('Position (m)')
plt.title('Cart Position over Time')
plt.legend()
plt.show()
```

## Tuned Performance Metrics
- **Max Cart Displacement**: 0.27 m  
- **Max Angle Deviation**: 3.26 deg  
- **Max Control Force**: 78.13 N  
- **Avg Control Force**: 13.00 N  

## Visualizations
![image](https://github.com/workingbetter/RAS-SES-598-Space-Robotics-and-AI/blob/ea05d7f8a863a174822d79d2da710ce505c8d646/assignments/cart_pole_optimal_control/images/metrics.png)

## Control Force Analysis
- **Default Tuning**:  
  - Control forces were inadequate against 15N disturbances over time.  
- **Tuned Parameters**:  
  - Forces peaked at 78 N, effectively stabilizing the system.  
  - The lower R value enabled sufficient effort to handle disturbances.

## License
This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).  
[![Creative Commons License](https://i.creativecommons.org/l/by/4.0/88x31.png)](http://creativecommons.org/licenses/by/4.0/)
