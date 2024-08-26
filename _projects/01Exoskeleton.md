---
layout: project
title: Exoskeleton control
description: Python, C++, ROS Noetic, IMU, EMG
image: assets/images/exoskeleton/demo.gif
imagewidth: 0
order: 989
---

[GitHub Repository](https://github.com/Alves-Zach/imu_exo_control){:target="_blank"}

## Intro

For my capstone project during the Master's of robotics program at Northwestern University I was given the opportunity to work at Shirley Ryan Ability Lab (SRALab) on a project to implement a method to control a lower body exoskeleton using a set of IMU/EMG sensors. My goal was to create an effective and easy to use control scheme that physical therapists at SRALab could use in a rehabilitation therapy session.

The current method of providing assistance to a stroke patient learning to walk again involves having a patient partially suspended on an elevated treadmill and the physical therapist grabbing the patient's legs one at a time to assist the patient to a more highly desired gait pattern. An improvement on this method would be to use a lower body exoskeleton pictured below that would be able to control each joint on the patient individually.

## Sensors
The sensors used for this project are the trigno combination IMU and EMG sensors. They are placed on the legs as shown below, the color coding shows the purpose each sensor has.

{:refdef: style="text-align: center;"}
![Trigno sensor placement](/assets/images/exoskeleton/IMUPlacementDiagram.png){: width="50%"}
{: refdef}

- The blue sensors measure the muscle activation of the gluteus maximus which are responsible for moving the hip joint backwards
- The blue and red sensors provide both IMU and EMG readings to set the desired angle for the exoskeleton hip joints and the muscle activation for the front of the thigh muscles
- The red sensors provide both IMU and EMG readings to set the desired angle for the exoskeleton hip joints and the muscle activation for the backof the thigh muscles 
- The green sensors only provide IMU data to set the desired angle for the knee joints on the exoskeleton as the shin muscles only control the ankles

## IMU controller
The first step in this project was getting IMU sensors on the different segments on a person's legs to control the exoskeleton using a position controller.

{% include youtube.html video_id="RF24Zl4vB4U " width="50%" %}
{:refdef: style="text-align: center;"}
_Video demonstration of the functional simulated SLAM algorithm_
{: refdef}

This demo demonstrates the physical therapist on the right using IMU sensors attached to her legs controlling the lower body exoskeleton I am wearing on the left. The quaternions that the IMUs generate are converted into joint angles in the sagittal plane of the therapist's body. Those joint angles are then fed to a position controller that commands motor torques based on the difference between the joint angles of the therapist and the joint angles of the exoskeleton.

This method allows the physical therapist to easily command a desired gait pattern to a patient while enabling the therapist to easily put on and take off the sensors to attend to other tasks during a therapy session.

## EMG stiffness augmentation
An important part of lower body rehabilitation using this setup is being able to vary controller stiffness to encourage the patient to tend towards more desirable gait patterns as the physical therapist sees fit. During test using only the IMUs controlling the desired position, an operator would control the proportional gain on the controller, changing it when the therapist said to do so. This method is functional but is more cumbersome and is less likely to find the correct stiffness. Also, different phases of a patient's gait pattern could require different levels of stiffness. This is why I aimed to augment the controller's proportional stiffness based on the muscle activation of the user wearing the IMU/EMG sensors.

TODO: Add flowchart showing basic overview of the EMG -> stiffness pipeline

The raw EMG data for each muscle comes in from the trigno_capture node, that gets converted into muscle activation data for each of the six muscles measured, those values then get converted into stiffness values for each of the 4 joints the exoskeleton controls. The method to convert muscle activation data to joint stiffness can be chosen from any listed below. All methods are available and can be chosen based on user preference

&emsp; - High-Low method: 

&emsp;&emsp;&emsp; ![High Low equation](/assets/images/exoskeleton/HighLow.png "High Low equation")

&emsp;&emsp;&emsp; Where σ is an array containing maximum voluntary contraction (MVC) readings for both muscles that control that joint

&emsp; - Maximum method: 

&emsp;&emsp;&emsp; Takes the maximum of the two MVC readings and converts that directly to the stiffness multiplier for the controller.

&emsp; - Minimum method:

&emsp;&emsp;&emsp; Takes the minimum of the two MVC readings and converts that directly to the stiffness multiplier for the controller.

TODO: Have a video demonstrating the different stiffness methods while walking on a treadmill, it will be one video demonstrating them all one after another

The final flowchart showing the used ROS nodes and data is as follows:

{:refdef: style="text-align: center;"}
![Final layout of ROS nodes and data flow](/assets/images/exoskeleton/ROSflow.png){: width="50%"}
{: refdef}

## Future work

This project has the opportunity to be improved on in the future, as the goal was to create an intuitive and easily controllable system for rehabilitation therapy, other methods for calculating stiffness could be more intuitive.

### Forearm contraction method
One method that I did not have the time to experiment with was using the contraction of another part of the body, such as the forearm, to augment joint stiffness. During my experiments I found it can be difficult to flex parts of the leg enough to significantly change the perceived muscle activation without significantly changing the kinematics of my gait. Using the muscle activation of another muscle not on the leg would be a creative way to avoid this issue.

### Percent of maximum output torque
The current method of figuring out the amount that the user is contracting their muscles does not take into account the strength of each muscle relative to the other antagonist muscles. Using some calibration method that takes into account each muscle's strength would make a more accurate controller as relative stengths would be able to be used when calculating stiffness.