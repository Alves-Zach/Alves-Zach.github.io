---
layout: project
title:  Pick and Place Robotic Manipulation
description: Python, Controls Engineering, CoppeliaSim
image: assets/images/MECH449FinalProject/main.gif
imagewidth: 0
order: 989
---

[GitHub Repository](https://github.com/Alves-Zach/NU-MECH449-Final){:target="_blank"}

For the final project of MECH 449 Robotic Manipulation we were tasked with writing a controller to plan both chassis and arm movements of a mobile manipulator robot. The control scheme aims to minimize error, where error is the distance from the current arm and chassis locations to the "perfect" arm and chassis locations.

Three types of control schemes were required for this project to demonstrate our understanding of control systems inputs and how they affect the error of a system over time.

{% include youtube.html video_id="7w1IAbZ1y34" width="50%" %}
{:refdef: style="text-align: center;"}
_The final trajectory simulated on the mobile manipulator robot_
{: refdef}

{% details **<u>Table of Contents</u>** %}
- [Best Scheme](#best-scheme)
- [Overshoot Scheme](#overshoot-scheme)
- [New Task Scheme](#new-task-scheme)
{% enddetails %}

****

## Best Scheme
The best control scheme is an attempt to minimize error without having any overshoot. Figure 1 below shows the error of the system over time.

{:refdef: style="text-align: center;"}
![Error over time with the best control scheme.](/assets/images/MECH449FinalProject/BestParams.png){: width="50%"}
{: refdef}
{:refdef: style="text-align: center;"}
_Figure 1: Error over time with the best control scheme._
{: refdef}

## Overshoot Scheme
The overshoot control scheme attempts to correct error too fast, so the robot overshoots the perfect path location for that specific time. This can be seen around t = 3 seconds where the error goes beyond 0 into the negative. This scheme would be best in cases where a minor amount of overshoot is acceptable. For example, if a user set their heating unit to 70 degrees and it overshoots to 70.1 for a brief period the user would likely neither mind nor notice.

{:refdef: style="text-align: center;"}
![Figure 2: Error over time with the control scheme with overshoot](/assets/images/MECH449FinalProject/Overshoot.png){: width="50%"}
{: refdef}
{:refdef: style="text-align: center;"}
_Figure 2: Error over time with the control scheme with overshoot._
{: refdef}

## New Task Scheme
The new task control scheme is trying to accomplish the goal as fast as possible but without any overshoot just like the best scheme, but this goal has different starting and ending locations for the cube. Different starting and ending locations require different tuning parameters. This demonstrates the understanding that different tasks even within the same system require different tuning parameters for efficient outcomes.

{:refdef: style="text-align: center;"}
![Figure 3: Error over time with the control scheme with a different task.](/assets/images/MECH449FinalProject/NewTask.png){: width="50%"}
{: refdef}
{:refdef: style="text-align: center;"}
_Figure 3: Error over time with the control scheme with a different task._
{: refdef}