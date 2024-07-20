---
layout: project
title: SLAM
description: C++, ROS 2, Rviz
image: assets/images/SLAM/Demo.gif
imagewidth: 0
order: 989
---

[GitHub Repository](https://github.com/ME495-Navigation/slam-project-Alves-Zach){:target="_blank"}

As part of my Master's program at Northwestern University I had 10 weeks to implement a **SLAM** (Simultaneous Localization And Mapping) algorithm from scratch. **SLAM** involves using sensors onboard a vehicle to detect landmarks to create a map of the surroundings to make traversing the environment easier.

This implementation was meant to simulate a [**turtlebot3 burger**](https://www.turtlebot.com/turtlebot3/) which is a differential drive robot with a rotating lidar sensor on the top of the robot.

{% details **<u>Table of Contents</u>** %}
- [Demo Video](#demo-video)
- [Odometry](#odometry)
- [Simulated LiDAR](#simulated-lidar)
- [Kalman Filter](#kalman-filter)
{% enddetails %}

****
## Demo Video
The video below shows a successful implementation of the **SLAM** algorithm using a **Kalman Filter**. There are three modeled turtlebots that represent the following:

Red: Simulated robot with collision and source of LiDAR measurements

Blue: Estimated location of the red robot using only odometry

Green: Estimated location of the red robot using both odometry and **SLAM**

{% include youtube.html video_id="q6i9i0DOIZM" width="50%" %}
{:refdef: style="text-align: center;"}
_Video demonstration of the functional simulated SLAM algorithm_
{: refdef}

This implementation is successful because the green SLAM estimation robot follows the red robot closely even when the blue odometry estimation drifts. The blue robot can drift from the red "real" robot for two reasons. Noise has been added to the simulation, where if a wheel is commanded to move 1 radian, it may only move 0.98 radians to more accurately simulate a real robot. Also, odometry cannot take into account collisions, whereas a real robot would collide with a wall and the wheels would slip along the floor, the blue robot would think that the robot kept moving forward through the obstacle.

## Odometry
The motors on the **turtlebot3 burger** have encoders that can tell how much each wheel has rotated; using that data and integrating the wheel rotation time an estimation of the location of the simulated robot can be calculated. The estimated location is then fed into the Kalman Filter. Below is a photo demonstrating that the blue robot can follow the red robot very closely if no obsticles are encountered, but drifts significantly if an obsticle is hit.

{:refdef: style="text-align: center;"}
![Demonstrating the odometry calculations](/assets/images/SLAM/Odometry.png){: width="50%"}
{: refdef}

The above image shows the robot's path in their respective colors, this shows the red robot colliding with obsticles and eventually driving around them, though the blue robot drives through them, as the blue robot is meant to estimate the location of the red robot using only the spinning of the wheels.

## Simulated LiDAR
The **turtlebot3 burger** has a rotating LiDAR sensor onboard that obtains measurements around the robot multiple times a second. Shown below, is a picture showing orbs on both the obstacles and walls in the arena, the orbs show the locations that the LiDAR beams hit the surrounding surfaces. These measurements are the second input into the Kalman Filter that localizes the robot in the space.

{:refdef: style="text-align: center;"}
![Demonstrating the simulated LiDAR](/assets/images/SLAM/LiDAR.png){: width="50%"}
{: refdef}

## Kalman Filter
Using the odometry data, the data from the LiDAR sensor and the previous estimation of the red robot's location, an estimate can be made at regular intervals. These estimates become more and more accurate as time goes on due to weights put on each source of data that change over time. Staring off, the odometry has a high weight, though its weight begins to lower as the weight for the LiDAR data rises with increasing measurements.