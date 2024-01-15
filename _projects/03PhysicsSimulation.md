---
layout: project
title:  2D Physics Simulation
description: Python, Dynamics 
image: assets/images/MECH314FinalProject/main.gif
imagewidth: 0
order: 989
---

[GitHub Repository](https://github.com/Alves-Zach/NU-MECH314-Final){:target="_blank"}

Our final project for MECH 314 Machine Dynamics was to program a physics simulation with collision from scratch in Python. The simulation had to have two square bodies, visualized as die within a box, and collide with each other multiple times and on multiple sides to demonstrate the robustness of our simulator.

{% include youtube.html video_id="zbDKalFRUCY" width="50%" %}
{:refdef: style="text-align: center;"}
_The corresponding gazebo simulation using my hand and finger motions as input._
{: refdef}

{% details **<u>Table of Contents</u>** %}
- [Frames](#frames)
- [Lagrangian Dynamics](#lagrangian-dynamics)
- [External forces](#external-forces)
{% enddetails %}

****

### Frames
For this simulation I kept track of the location of 4 frames on the die's corners, and 4 on the edges of the box the die was in. At each time interval my program checked if any of the corners of the die were intersecting the box, if so, different equations of motion were used to simulate collision.

### Lagrangian Dynamics
The dynamics of the system were simulated using the Euler-Lagrange equations of motion shown below.

{:refdef: style="text-align: center;"}
![Euler Lagrange Equations for basic motion.](/assets/images/MECH314FinalProject/EulerLagrangeEquation.png)
{: refdef}
{:refdef: style="text-align: center;"}
_Euler Lagrange Equations for basic motion._
{: refdef}

Once a collision was detected, the equations below were used to determine the location and speed of the colliding objects the time instance directly after the collision. 

{:refdef: style="text-align: center;"}
![Collision update equations.](/assets/images/MECH314FinalProject/CollisionEquations.png)
{: refdef}
{:refdef: style="text-align: center;"}
_Equation to use when a collision is detected_
{: refdef}

With these sets of equations and the frame definitions from before, the simulation could be run with arbitrary starting positions, velocities, and accelerations.

### External forces
The box that the die was contained in had its own mass and was affected by gravity, thus there was a need to apply an external upward force to keep the box at the same y level and make the simulation easier to demonstrate. 

An external torque was also applied to the bounding box containing the die to demonstrate collisions between multiple sides of both objects.