---
layout: project
title:  2D Physics Simulation
description: Python, Dynamics 
image: assets/images/MECH449FinalProject/main.gif
imagewidth: 0
order: 989
---

[GitHub Repository](https://github.com/Alves-Zach/NU-MECH449-Final){:target="_blank"}

For the final project of MECH 449 robotic manipulation we were tasked with writing a controller to plan both chassis and arm movements of a mobile manipulator movement. The control scheme aims to minimize error, where error is the distance from the current arm and chassis locations to the "perfect" arm and chassis locations.

Three types of control schemes were required for this project to demonstrate our understanding of control systems inputs and how they affect the error of a system over time.

{% include youtube.html video_id="7w1IAbZ1y34" width="50%" %}
{:refdef: style="text-align: center;"}
_The corresponding gazebo simulation using my hand and finger motions as input._
{: refdef}

{% details **<u>Table of Contents</u>** %}
- [Frames](#Frames)
- [Lagrangian Dynamics](#Lagrangian-Dynamics)
- [External forces](#External-forces)
{% enddetails %}

****

### Frames
For this simulation I kept track of the location of 4 frames on the die's corners, and 4 on the edges of the box the die was in. Using this frames I was able to check if the corners of the jack intersected the box at each time interval, meaning I should simulate a collision.

### Lagrangian Dynamics
The dynamics of the system were simulated using the Euler-Lagrange equations of motion shown below.

{:refdef: style="text-align: center;"}
![Euler Lagrange Equations for basic motion.](/assets/images/MECH314FinalProject/EulerLagrangeEquation.png){: width="50%"}
{: refdef}
{:refdef: style="text-align: center;"}
_Euler Lagrange Equations for basic motion._
{: refdef}

Once a collision was detected, the equations below were used to determine the location and speed of the colliding objects the time instance directly after the collision. 

{:refdef: style="text-align: center;"}
![Collision update equations.](/assets/images/MECH314FinalProject/CollisionEquations.png){: width="50%"}
{: refdef}
{:refdef: style="text-align: center;"}
_Equation to use when a collision is detected_
{: refdef}

With these sets of equations and the frame definitions from before, the simulation could be run with arbitrary starting points and velocities.

### External forces
The box that the die was contained in had its own mass and was affected by gravity, thus there was a need to apply an external upward force to keep the box at the same y level and make the simulation easier to demonstrate. 

An external torque was also applied to the bounding box containing the die to demonstrate collisions between multiple sides of both objects.