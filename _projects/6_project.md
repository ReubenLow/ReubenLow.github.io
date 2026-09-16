---
layout: page
title: Lite-kit Project 1
description: A robotic platform that can be remotely controlled wirelessly via a GUI.
img: assets/img/litekit1.jpg
importance: 12
category: work
---

A first year module project.

<!-- Describe the objective of Lite-kit Project 1 -->

## Objective

To build a robotic platform that could be driven remotely from a graphical user interface on a laptop, with the commands carried over a wireless link so the robot could be operated untethered.

## Outcome

A wheeled robot platform driven from a desktop GUI. The interface presents directional controls laid out as a directional pad and displays the command currently being issued, so the operator can confirm what the robot has been told to do.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/litekit1.jpg" title="The control GUI driving the robot platform" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The control GUI issuing a turn command, with the robot platform in the background.
</div>

## Wireless Communication

Zigbee carried the commands from the laptop to the robot. Using a wireless link meant the platform could be driven around the room without a tether limiting where it could go.

## Skills Deployed & Responsibilities

- GUI Development
  - Directional control interface
  - Command state display
- Wireless Communication
  - Zigbee link between laptop and robot
- Embedded Systems
  - Robot platform control
