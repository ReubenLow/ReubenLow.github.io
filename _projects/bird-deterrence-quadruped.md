---
layout: page
title: Bird Deterrence Quadruped
description: An autonomous bird deterrence system built on a Unitree Go2 quadruped.
img: assets/img/iwsp/bird-deterrence-quadruped/quadruped-deployed.jpg
importance: 3
category: work
---

Completed during my IWSP industry attachment at Ceredroid AI.

<!-- Objective of the bird deterrence system -->

## Objective

To develop an autonomous bird deterrence system for the Unitree Go2 quadruped, integrating vision, embedded control and mechanics that could survive field deployment.

## Outcome

A field-deployable system that detects birds using a depth camera, aims a laser pointer at them through a custom pan-tilt turret, and varies its laser patterns so that birds do not habituate to it. Custom mounting brackets and integration with the Go2 codebase completed the platform.

<div class="row">
    <div class="col-sm-5 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/bird-deterrence-quadruped/quadruped-deployed.jpg" title="The bird deterrence quadruped" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-7 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/bird-deterrence-quadruped/turret-prototype.jpg" title="Prototype of the bird deterrence system" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The bird deterrence quadruped (left), and an early prototype of the system on the bench (right).
</div>

## Detection

Detection runs on a RealSense depth camera using an 18 MB YOLO model. A 100 MB variant was evaluated during selection. The smaller model produced fewer false positives in cluttered environments, so it was the one taken forward.

## Turret and Tracking

A custom pan-tilt turret co-locates the camera and the laser pointer, so the aim follows the detection directly. PID-based tracking converts detections and depth measurements into pan and tilt angles, tuned to follow a moving target smoothly and hold the aim steady once acquired.

## Embedded Control

The first build used an Arduino Uno. Its single UART interface created communication conflicts between USB programming and servo driver communication, which caused command failures and servo jitter. Migrating to an Arduino Mega with three hardware serial ports gave each channel its own dedicated port and cleared the conflict.

The servo driver board then imposed its own limit, enforcing five degree movement increments that were too coarse for accurate aiming. I bypassed the driver and generated the PWM signals directly from the Arduino GPIO pins, which brought the turret down to one degree precision.

## Deterrence Patterns

The laser runs several movement patterns, including sweep, erratic and oval paths. Varying the pattern stops birds from growing accustomed to a predictable stimulus and keeps the deterrent effective over repeated exposure.

## Outdoor Calibration

Field testing exposed the limits of a solution developed indoors. The system showed large aiming errors caused by depth sensor noise from reflective surfaces and by ambient lighting variation. I addressed this with depth post-processing and distance-scaled offset functions on the technical side, and with baseline outdoor calibration routines and recommended lighting conditions for deployment on the operational side. Dataset collection in the target environment supported the tuning.

## Power and Safety

Power regulation was handled with a buck converter and voltage regulator sized for the added payload. Laser power was deliberately limited and safety interlocks were built in, so that a malfunction produces a clear stop.

<!-- Skills Deployed -->

## Skills Deployed & Responsibilities

- Computer Vision
  - YOLO model selection and evaluation
  - RealSense depth sensing and post-processing
- Embedded Systems
  - Arduino firmware and UART protocol implementation
  - Direct PWM generation from GPIO
  - Buck converter and voltage regulation
- Control Systems
  - PID-based pan-tilt tracking
- Mechanical Design
  - Custom pan-tilt turret
  - Mounting brackets for field deployment
- Field Testing
  - Outdoor calibration procedures
  - Dataset collection
