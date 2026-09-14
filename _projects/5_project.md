---
layout: page
title: Lite-kit Project 2
description: A robotic platform that tracks coloured objects.
img: assets/img/litekit2.jpg
importance: 10
category: work
---

<!-- Describe the objective of Lite-kit Project 2 -->

## Objective

To build a robotic platform capable of detecting a coloured object and tracking it, combining vision with omnidirectional movement.

## Outcome

A mecanum-wheeled platform using a Pixy camera for colour tracking, with an ultrasonic sensor for distance measurement and an actuated light mounted above the chassis. The mecanum wheels give the platform omnidirectional movement, so it can reposition itself towards a target without needing to turn on the spot first. An emergency stop button sits on the chassis.

<div class="row justify-content-sm-center">
    <div class="col-sm-7 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/litekit2.jpg" title="The colour-tracking robot platform" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The platform, showing the Pixy camera, ultrasonic sensor, actuated light and mecanum wheels.
</div>

## Colour Tracking

Colour tracking is a built-in function of the Pixy camera. The colour to follow is selected by drawing a box over it in the camera's interface, and the Pixy then reports the position of anything matching that colour. The controller receives those positions directly and drives the platform towards the target.

## Skills Deployed & Responsibilities

- Computer Vision
  - Colour tracking using the Pixy camera
- Embedded Systems
  - Microcontroller wiring and sensor integration
  - Ultrasonic distance sensing
  - Motor control for mecanum drive
