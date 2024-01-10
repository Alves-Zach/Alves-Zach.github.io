---
layout: project
title: Shadowhands
description: Python, ROS 2, Gazebo
image: assets/images/shadowhands/main.gif
imagewidth: 0
order: 989
---

[GitHub Repository](https://github.com/ME495-EmbeddedSystems/final-project-teleop){:target="_blank"}

As our final project for my embedded systems in robotics class at Northwestern University my team decided to utilize the **Shadow Hand** robot hands attached to ABB seven-DOF robot arms to manipulate objects detected via computer vision.

Alongside the ABB robots, we created a user-end virtual simulation of the environment utilizing **RVIZ** and **Gazebo** where the user could wear a **Haptics** glove and have a third ABB arm attached to their wrist.

{:refdef: style="text-align: center;"}
![The transforms of each ring shown in RVIZ](/assets/images/shadowhands/BlockDiagram.png){: width="50%"}
{: refdef}
{:refdef: style="text-align: center;"}
_The transforms of each ring shown in RVIZ._
{: refdef}

{% details **<u>Table of Contents</u>** %}
- [Manipulator End](#manipulator-end)
- [User End](#user-end)
- [Future Work](#future-work)
{% enddetails %}

****

## Manipulator End
The manipulator end has both a computer vision and robotic manipulator portion to it. The computer vision side detects the locations of the toy rings on the desk and sends those positions to the motion planner that creates a series of actions for the robotic arm to execute to pick up the rings in order.

The computer vision on this end of the project utilizes color detection to detect which pixels corrispond to which ring. Then using other CV methods such as contouring, more accurately determines the edges of each rings, then the center of each ring based on the average location of pixels of the same color as the rings.

{:refdef: style="text-align: center;"}
![The transforms of each ring shown in RVIZ](/assets/images/shadowhands/cv.png){: width="50%"}
{: refdef}
{:refdef: style="text-align: center;"}
_The transforms of each ring shown in RVIZ._
{: refdef}

Once the ring locations are known relative to the camera, known positions and rotations from the camera to the robot base are used to compute the locations of the rings relative to the robot. These locations are then sent to the motion planner to create a series of actions that will pick up each ring in order and stack them on the post in the center of the desk.

## User End
The user end consisted of a haptics feedback glove capable of providing a gripping feedback to the user, a franka seven-DOF arm attached to the user's wrist to track position in 3D space and provide weight feedback, and a virtual simulation in Gazebo.

Because this project was intended to be a platform for delayed tele-operation a locally run physics simulator was needed to provide fast and accurate feedback to the user as in an actual use case the user would be much further away from the manipulator end thus incuring much more delay in feedback. Gazebo was the simulator our team decided on because of its integration with ROS2. 

Though because Gazebo is a physics simulator it does not allow users to program robot positions directly into the simulation, so our simulated hand had to take commands from a controller where the current real-world positions of the user's hand and fingers were the goal positions the controller was aiming for.

{% include youtube.html video_id="qrdn6vsrmfs" width="50%" %}
{:refdef: style="text-align: center;"}
_Me using the haptics glove to demo the user feedback._
{: refdef}

The franka robot that is attached to my wrist is constantly using kinematic equations to determine the location and rotation of my wrist relative to the base of the robot. That data is then sent to the Gazebo controller and used as desired positions.

{% include youtube.html video_id="QWVU5sAlT2Q" width="50%" %}
{:refdef: style="text-align: center;"}
_The corisponding gazebo simulation using my hand and finger motions as input._
{: refdef}

## Future Work

{% details **1. Improving controller tuning.** %}

While the physics simulator is very good at simulating freely moving objects such as the rings, the controller that moves the hand around the scene would need tuning before the project could actually be used in an actual scenario. Though tuning the controller was out of scope for a project this size, ROS contains tools to tune controllers and gazebo has the capability to make the hand model more accurate so that the random flipping wouldn't occur.

{% enddetails %}

{% details **2. Saving user input motion for future playback** %}

Future iterations of this project would almost need to include trajectory storing so that it could be run later on the manipulator end of the project. For example, if the user makes a mistake in their execution, that motion data could simply be deleted and not sent to the manipulator end, where as a directly controlled manipulator would be more prone to user errors.

{% enddetails %}

****