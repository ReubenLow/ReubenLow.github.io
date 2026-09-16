---
layout: page
title: Bird Deterrence Quadruped
description: An autonomous bird deterrence system built on a Unitree Go2 quadruped.
img: assets/img/iwsp/bird-deterrence-quadruped/quadruped-deployed.jpg
importance: 4
category: work
---

Completed during my IWSP industry attachment at Ceredroid AI.

<!-- Objective of the bird deterrence system -->

## Objective

To develop an autonomous bird deterrence system for the Unitree Go2 quadruped, integrating vision, embedded control, electronics and custom mechanics.

## Outcome

A system that detects birds using a depth camera, aims a laser pointer at them through a custom pan-tilt turret, and varies its laser patterns so that birds do not habituate to it. Custom mounts and integration with the Go2 codebase completed the platform.

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

Detection runs on a RealSense depth camera using an 18 MB YOLO model. A 100 MB variant was evaluated during selection. The smaller model produced fewer false positives in cluttered environments, so it was the one taken forward. Each detection comes back as a bounding box and a confidence score, which the tracking stage turns into a target for the turret.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/bird-deterrence-quadruped/pig1.jpg" title="Pigeon detected at 0.84 confidence" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/bird-deterrence-quadruped/pig2.jpg" title="Pigeon detected at 0.88 confidence" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/bird-deterrence-quadruped/pig3.jpg" title="Pigeon detected at 0.91 confidence" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The model picking out pigeons in a cluttered hawker centre, at confidence scores of 0.84, 0.88 and 0.91. Click any image to enlarge.
</div>

## Turret and Tracking

A custom pan-tilt turret co-locates the camera and the laser pointer, so the aim follows the detection directly. PID-based tracking converts detections and depth measurements into pan and tilt angles, tuned to follow a moving target smoothly and hold the aim steady once acquired.

I designed the mechanical fixtures and mounts that carry the turret and the depth camera on the quadruped, holding the two components in a fixed position relative to each other so the aim of the laser stays calibrated against what the camera sees.

## Embedded Control

The first build used an Arduino Uno. Its single UART interface created communication conflicts between USB programming and servo driver communication, which caused command failures and servo jitter. Migrating to an Arduino Mega with three hardware serial ports gave each channel its own dedicated port and cleared the conflict.

The servo driver board then imposed its own limit, enforcing five degree movement increments that were too coarse for accurate aiming. I bypassed the driver and generated the PWM signals directly from the Arduino GPIO pins, which brought the turret down to one degree precision.

## Electronics

I drew up the electronics schematics for the system and sourced the electronic components that went into it, covering the controller, the servos and the laser.

## Deterrence Patterns

The laser runs several movement patterns, including sweep, erratic and oval paths. Varying the pattern stops birds from growing accustomed to a predictable stimulus and keeps the deterrent effective over repeated exposure.

## Depth Accuracy

Depth sensor noise from reflective surfaces, and variation in ambient lighting, produced large aiming errors. I addressed this with depth post-processing and distance-scaled offset functions, so that the pan and tilt angles stayed accurate across the working range.

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
- Electronics
  - Schematic design
  - Component sourcing
  - Buck converter and voltage regulation
- Control Systems
  - PID-based pan-tilt tracking
- Mechanical Design
  - Custom pan-tilt turret
  - Fixtures and mounts for the turret and depth camera
