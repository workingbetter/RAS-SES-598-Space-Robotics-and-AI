

## Assignment 3: Rocky Times Challenge - Search, Map, & Analyze

### 1. Introduction
This report outlines my work on Assignment 3 of the RAS-SES-598 Space Robotics and AI course, titled the "Rocky Times Challenge." The objective was to develop an autonomous drone controller using ROS2 and the PX4 SITL simulator to search for, map, and analyze two cylindrical rock formations (10m and 7m tall), estimate their dimensions, and land on the taller cylinder. Unfortunately, I encountered significant technical issues that prevented me from completing the assignment fully. These included compatibility problems with Ubuntu 24.04 and ROS2 Jazzy, followed by simulation crashes after updates and finally going to  Ubuntu 22.04 with ROS2 Humble. Despite these setbacks, this document details my efforts, the setup process, my intended approach, and the troubleshooting steps I took to address the problems.

### 2. Environment Setup
Setting up the simulation environment proved to be the most challenging aspect of this assignment. Below is a step-by-step account of my attempts, the issues I faced, and my efforts to resolve them:

- **Initial Attempt: Ubuntu 24.04 with ROS2 Jazzy**  
  - I started by forking the course repository to my GitHub account (`https://github.com/workingbetter/RAS-SES-598-Space-Robotics-and-AI`) and cloning it to my local machine running Ubuntu 24.04.
  - I installed ROS2 Jazzy and followed the instructions to set up the PX4 SITL simulator with Gazebo. However, Gazebo failed to launch correctly, displaying errors and immideatly closing (crushing) after i run the sim. I spent time trying to fix the inssues and reinstalling packages and searching online for solutions, but the simulator remained non-functional.
  - Suspecting issues between ROS2 Jazzy and the required Gazebo version, I decided to switch to the recommended setup: Ubuntu 22.04 with ROS2 Humble.

- **Switch to Ubuntu 22.04 with ROS2 Humble**  
  - I performed a clean installation of Ubuntu 22.04 on my system and installed ROS2 Humble, PX4-Autopilot (commit `9ac03f03eb`), RTAB-Map, OpenCV, and Python 3.10 as per the assignment guidelines.
  - I cloned my forked repository again into a ROS2 workspace (`~/ros2_ws/src`) and created a symlink to integrate it properly.
  - I executed the `deploy_px4_model.sh` script to copy custom model files into the PX4 directory.
  - After building the workspace with `colcon build`, I launched the simulation using the command:  
    ```bash
    ros2 launch terrain_mapping_drone_control cylinder_landing.launch.py
    ```
  - Initially, this worked successfully—the Gazebo simulation opened, and I could see the drone and the cylindrical rock formations in the environment.

- **Issues After System Update**  
  - To ensure my system was up-to-date, I ran the following commands in the terminal:  
    ```bash
    sudo apt update
    sudo apt upgrade
    cd ~/PX4-Autopilot
    bash ./Tools/setup/ubuntu.sh
    ```
  - After the update, the simulation started crashing upon launch. Gazebo would either freeze shortly after loading or fail to load the world entirely, with terminal errors indicating problems like missing model files or plugin failures.
  - I captured a screenshot of the crash (attached with this submission), which shows the error messages in the terminal and the partially loaded Gazebo window.
  - I attempted to fix this by reinstalling ROS2 Humble, PX4, and Gazebo, and even tried rolling back some updates using cached packages, but the simulation continued to crash. With the submission deadline approaching, I couldn’t resolve the issue in time.
![Screenshot 2025-03-25 201431](https://github.com/user-attachments/assets/c5972843-b877-4188-8792-da8d358be664)

### 3. Intended Approach
Although I couldn’t implement the drone controller due to the simulation issues, I had planned a clear strategy to complete the mission. Here’s how I intended to approach the assignment:

- **Search Strategy**  
  - The drone would follow a spiral trajectory starting from its initial position to systematically explore the unknown environment and locate the two cylinders. This would ensure efficient coverage of the area.

- **Cylinder Detection and Mapping**  
  - Using the drone’s RGBD camera and RTAB-Map, I would generate a 3D point cloud of the environment.
  - I planned to process this data with geometric algorithms (e.g., cylinder fitting) to identify the cylindrical shapes and estimate their heights and diameters.

- **Path Planning**  
  - After detecting both cylinders, I would calculate their positions in the world frame and plan a shortest-path trajectory to the taller (10m) cylinder for landing, optimizing for energy efficiency.

- **Landing**  
  - The drone would use depth data or visual servoing to align itself with the center of the 10m cylinder’s top surface and execute a precise landing.

### 4. Implementation Efforts
Due to the persistent simulation crashes, I couldn’t write or test the controller code. However, I examined the provided package files to understand their purpose and how I might modify them:

- **`cylinder_landing_node.py`**: This seemed to be the main node for managing the mission stages (takeoff, search, mapping, landing).
- **`geometry_tracker.py`**: Likely designed to process point cloud data and detect the cylinders’ geometry.
- **`spiral_trajectory.py`**: Probably used to generate the spiral search path.

My plan was to adapt these scripts to implement my spiral search, cylinder detection, and landing logic, but without a working simulation, I couldn’t proceed beyond this conceptual stage.

### 5. Results
Because the simulation is currently non-functional, I couldn’t execute the mission or collect results such as mission completion time, energy usage, or accuracy of the cylinder dimension estimates. The attached screenshot of the crash represents the final state of my project at submission. It shows the simulation failing to load properly, with error messages in the terminal.

### 6. Discussion
The key challenges I faced were rooted in the simulation environment’s instability:

- **Compatibility Issues**  
  - My initial attempt with Ubuntu 24.04 and ROS2 Jazzy failed due to Gazebo incompatibilities, teaching me the importance of sticking to the recommended software stack (Ubuntu 22.04 with ROS2 Humble).

- **Simulation Crashes Post-Update**  
  - The simulation worked briefly on Ubuntu 22.04 until the system update disrupted it. The crashes likely stemmed from updated libraries or altered file paths that broke the PX4-Gazebo integration. My attempts to revert or reinstall didn’t succeed in the available time.

- **Time Management**  
  - I spent a significant portion of my time troubleshooting the environment rather than developing the controller, which limited my progress.

**Lessons Learned**:  
- I should have avoided running `sudo apt upgrade` once the simulation was working to maintain a stable environment.
- Taking a snapshot or backup of the working setup could have saved time when issues arose.
- Seeking assistance earlier (e.g., after the Ubuntu 24.04 failure) might have helped me pivot to a working solution sooner.

### 7. Conclusion
Although I couldn’t complete the "Rocky Times Challenge" due to technical difficulties, this assignment provided valuable experience in setting up robotics simulations, managing dependencies, and troubleshooting complex issues. I documented my process thoroughly to demonstrate my effort and understanding of the requirements, even though the simulation crashes prevented implementation. I’m disappointed not to have a working solution but am motivated to learn from this and improve my skills for future projects.

---

## Submission Notes
- **GitHub Repository**: `https://github.com/workingbetter/RAS-SES-598-Space-Robotics-and-AI`  
- **Attached Files**: Screenshot of the simulation crash (terminal errors and Gazebo window).  
- **Final Status**: The simulation launches but crashes shortly after, halting further development.

---

This document captures everything you’ve described—your struggles with Ubuntu 24.04 and Jazzy, the brief success with Ubuntu 22.04 and Humble, the crash after updating, and your inability to fix it in time. It’s a complete submission that explains your situation clearly and professionally. Good luck!
