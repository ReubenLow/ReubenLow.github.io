---
layout: page
title: Systems Engineering Project 1 (SEP1)
description: A robotic platform (LIMO) capable of motion planning in a mapped area.
img: assets/img/sep1.jpg
importance: 3
category: work
---

<!-- Describe the objective of SEP1 -->
## Objective
The objective of this project was to create a autonomous navigation system for a robotic platform using ROS 1. The robot, referred to as LIMO, was required to traverse a 3x3 arena map representing various zones in Sentosa. Additionally, this project aimed to introduce the concept of a throwaway prototype, focusing on testing and verifying the key functionalities of the system. The project was conducted as part of the Systems Engineering module.

## Outcome
The project successfully demonstrated the robot's ability to autonomously navigate to specified goals within the arena. The navigation system implemented with the ROS 1 framework, allowed LIMO to accurately move to various zones of the arena.

<div class="videorow">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="assets/video/sep1/sepdemo.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
</div>
<div class="caption">
    LIMO Navigation Demo.
</div>

## Arena Layout
The arena was divided into nine zones, each representing a landmark in Sentosa, Singapore. The coordinates of each zone are as follows:

Zone 1: Fort Siloso
Zone 2: Universal Studios
Zone 3: Wings of Time
Zone 4: Sentosa Entrance
Zone 5: SEA Aquarium
Zone 6: Adventure Cove Waterpark
Zone 7: IFly 
Zone 8: Sentosa Merlion Park
Home Position (Centre)

## ROS1

<div class="videorow">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="assets/video/sep1/sep1video1.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
</div>
<div class="caption">
    Simulation of Navigation Functionalities in Gazebo
</div>

The navigation script implemented in Python, utilizes the ROS 1 framework. 
Key components and functionalities implemented in the script:

- Initialization: The script initializes a ROS node and sets up a simple action client to communicate with the move_base server, responsible for handling navigation goals.

- Goal Setting: Goals are defined as instances of MoveBaseGoal, each containing a target position and orientation. The script allows setting and saving these goals to ensure persistence between runs.

- Navigation Control: The robot's journey towards each goal is monitored using callbacks that provide feedback on the current status and location. These callbacks handle different navigation states, including goal processing, success, and failures.

- Graphical User Interface (GUI): A PyQt-based GUI was developed to provide a user-friendly interface for setting goals and monitoring the robot's navigation. The GUI includes buttons for each zone, an emergency stop (E-STOP) button, and options to save and load poses.

- Emergency Stop (E-STOP): The E-STOP functionality allows immediate cancellation of all goals and stops the robot's movement, ensuring safety during operation.

- Persistence: The script includes functions to save and load the robot's poses from a text file, ensuring that goal coordinates are retained across different sessions of navigation.

<div class="videorow">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="assets/video/sep1/sep1video2.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
</div>
<div class="caption">
    LIMO Navigation GUI.
</div>

## Python script snippet

A snippet showcasing the initialization and goal setting functions.

``` python
#!/usr/bin/env python
import rospy
import json
import actionlib
from move_base_msgs.msg import MoveBaseAction, MoveBaseGoal
from geometry_msgs.msg import PoseWithCovarianceStamped
from actionlib_msgs.msg import GoalID

rospy.init_node('goal_pose', anonymous=True)
sentosa_client = actionlib.SimpleActionClient('move_base', MoveBaseAction)
sentosa_client.wait_for_server()

def send_goal(goal):
    sentosa_client.send_goal(goal, goal_callback, moving_callback, location_callback)

def save_pose():
    global goal_list
    with open('limo_pose.txt', mode='w') as f:
        for goal in goal_list:
            pose = goal.target_pose.pose
            pose_data = {
                'position': {'x': pose.position.x, 'y': pose.position.y, 'z': pose.position.z},
                'orientation': {'x': pose.orientation.x, 'y': pose.orientation.y, 'z': pose.orientation.z, 'w': pose.orientation.w}
            }
            f.write(json.dumps(pose_data) + '\n')
    print("Poses have been saved.")

def load_pose():
    global goal_list
    index = 0
    with open('limo_pose.txt', mode='r') as f:
        lines = f.readlines()
        for line in lines:
            if index == 10:
                print("Invalid number of goals found in the text file!")
                break
            pose_data = json.loads(line)
            goal_list[index].target_pose.header.stamp = rospy.Time.now()
            goal_list[index].target_pose.pose.position.x = pose_data['position']['x']
            goal_list[index].target_pose.pose.position.y = pose_data['position']['y']
            goal_list[index].target_pose.pose.position.z = pose_data['position']['z']
            goal_list[index].target_pose.pose.orientation.x = pose_data['orientation']['x']
            goal_list[index].target_pose.pose.orientation.y = pose_data['orientation']['y']
            goal_list[index].target_pose.pose.orientation.z = pose_data['orientation']['z']
            goal_list[index].target_pose.pose.orientation.w = pose_data['orientation']['w']
            index += 1
    print("Goal coordinates have been loaded.")

```

{% raw %}

{% endraw %}
