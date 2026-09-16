---
layout: page
title: Interactive Quadruped for Public Outreach
description: An Android-controlled Unitree Go2 built for community robotics demonstrations.
img: assets/img/iwsp/outreach-quadruped/bidadari-outreach.jpg
importance: 6
category: work
---

Completed during my IWSP industry attachment at Ceredroid AI.

<!-- Objective of the public outreach quadruped -->

## Objective

To develop an interactive control system for the Unitree Go2 that would let members of the public engage with the robot directly at community robotics demonstrations, with an interface simple enough for a first-time user to pick up in seconds.

## Outcome

A quadruped controlled from an Android app over MQTT, with movement, gestures, object detection and a soundboard all driven from the phone. It was deployed at two public outreach events, Bidadari Park and SIT Punggol Coast, where it engaged a wide range of community members and held up under real-world conditions.

<div class="row justify-content-sm-center">
    <div class="col-sm-7 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/outreach-quadruped/bidadari-outreach.jpg" title="Outreach session at Bidadari Park" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Outreach session at Bidadari Park.
</div>

## Android Teleoperation App

The app integrates MQTT communication with the Unitree SportClient SDK, exposing sit and stand commands, gait selection, directional movement, ramp climbing and jumping. Threading and quality-of-service configuration brought down command latency and jitter, so the robot responds promptly to a button press. Dynamic MQTT broker IP configuration lets the same app be pointed at a new broker on site, which made deployment across different event locations straightforward.

REST endpoint integration extended the app to object detection, gesture recognition, target setting and audio actions, all reachable from the same mobile interface.

## Designing for Public Use

The simplified interface was designed for school students. The scenario it was built to serve is an obstacle course that students would drive the quadruped through using the app, giving them a set of controls they could pick up straight away without having to learn the original controller.

<div class="row justify-content-sm-center">
    <div class="col-sm-9 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/outreach-quadruped/teleop-ui.png" title="Simplified teleoperation UI designed for the obstacle course" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The simplified teleoperation UI, designed for the obstacle course scenario.
</div>

Other details were added with those users in mind. E-stop latching allows for a way to halt the robot. Post-action input lockout prevents command conflicts when someone presses several buttons in quick succession. Haptic feedback confirms that a command has registered, which matters when the robot takes a moment to begin moving. A scrollable interface with icon-based selections fits the full set of functions into a layout a first-time user can navigate.

## Hardware Integration

I designed and fabricated mounting brackets for the Jetson AGX, the speaker system and the portable power supply. The front-mounted webcam bracket went through several design iterations with on-device testing between them. After field deployment, the AGX bracket was redesigned to be more compact and more durable, informed by how the first version held up at an event.

<div class="row justify-content-sm-center">
    <div class="col-sm-7 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/outreach-quadruped/early-iteration.jpg" title="An early iteration of the public outreach quadruped" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    An early iteration of the public outreach quadruped, wearing the fabricated lion head.
</div>

## Public Engagement Features

A soundboard system is triggered through endpoints on the Jetson AGX, with an Android menu that accepts user-generated dialogue so the robot's interactions can be personalised on the spot. A lion-themed costume was fabricated for the robot, including spray-painted head components.

<div class="row justify-content-sm-center">
    <div class="col-sm-9 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/outreach-quadruped/soundboard-page.png" title="Robot actions and soundboard page of the app" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Robot actions and soundboard page of the app.
</div>

## Field Deployment

The two outreach events validated the system's reliability outside the lab and produced useful feedback on interface usability and entertainment value. The project showed how robotics capability can be packaged for public accessibility, using an intuitive interface to spark interest in robotics among a broader audience.

<!-- Skills Deployed -->

## Skills Deployed & Responsibilities

- Android Development
  - Teleoperation app with MQTT communication
  - Unitree SportClient SDK integration
- User Experience Design
  - Simplified control interface for school students
  - E-stop latching and input lockout
  - Haptic feedback and icon-based navigation
- Systems Integration
  - REST endpoints on the Jetson AGX
  - Soundboard and audio actions
- Mechanical Design
  - Brackets for Jetson AGX, speaker and portable power
  - Iterative webcam bracket design
  - Lion costume fabrication
- Field Deployment
  - Public outreach at Bidadari Park and SIT Punggol Coast
