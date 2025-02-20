# Cart-Pole Optimal Control Assignment

[Watch the demo video](https://drive.google.com/file/d/1UEo88tqG-vV_pkRSoBF_-FWAlsZOLoIb/view?usp=sharing)
![image](https://github.com/workingbetter/RAS-SES-598-Space-Robotics-and-AI/blob/7121eb081454743c36d2e1d7ab1cf0a09e7ccce0/assignments/cart_pole_optimal_control/images/Cart%20Pole%20tunned.png)

## System Description
- Inverted pendulum mounted on a cart
- Cart traversal range: ±2.5m (total range: 5m)
- Pole length: 1m
- Cart mass: 1.0 kg
- Pole mass: 1.0 kg
- Base amplitude: 15.0N (disturbance setting)
- Frequency range: 0.5-4.0 Hz 
- Additional Gaussian noise


## Existing Q/R Matrix Analysis
Default Parameters:
```python
# State cost matrix Q (default values)
Q = np.diag([1.0, 1.0, 10.0, 10.0])  # [x, x_dot, theta, theta_dot]

# Control cost R (default value)
R = np.array([[0.1]])  # Control effort cost
```
### Analysis:
Q matrix priorities:
- θ (pole angle) and θ_dot have 10x higher weight than position states
- Focuses on angular stability over cart position
R value of 0.1 allows moderate control effort
- Trade-off: Good for pole stabilization but risks cart position drift under disturbances












































## License
This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).
[![Creative Commons License](https://i.creativecommons.org/l/by/4.0/88x31.png)](http://creativecommons.org/licenses/by/4.0/) 
