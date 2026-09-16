---
layout: page
title: Storeman-Rosman Autonomous Shelf Picking
description: Dual-arm concurrent shelf picking on the Kuavo humanoid.
img: assets/img/iwsp/storeman-rosman-shelf-picking/shelf-picking.jpg
importance: 4
category: work
---

Completed during my IWSP industry attachment at Ceredroid AI.

<!-- Objective of the Storeman-Rosman project -->

## Objective

To develop autonomous shelf-picking capabilities for the Kuavo humanoid, with dual-arm concurrent control so that both arms could work at the same time.

## Outcome

A shelf-picking system in which each arm operates independently under its own commands, coordinated by shelf-state tracking that assigns canned drink orders between them. Integrated testing demonstrated concurrent two-order operation with both arms running together.

<div class="row justify-content-sm-center">
    <div class="col-sm-7 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/storeman-rosman-shelf-picking/shelf-picking.jpg" title="The humanoid picking canned drinks from a stocked shelf" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The humanoid facing a stocked shelf, with wrist-mounted cameras on both arms.
</div>

## Control Architecture

- Six predefined ready-position joint states
- REST endpoints exposing per-arm and dual-arm control
- Shelf-state tracking to coordinate arm assignments
- Mid-scan interruption, where one arm grabs a detected item while the other carries on scanning
- Command gating and state isolation to keep the two arms from racing each other

The command gating and state isolation removed a class of failure modes that had affected earlier demonstration attempts, and made the more involved manipulation sequences possible.

## Pose Accuracy Benchmarking

Systematic benchmarking quantified end-effector errors across post-motor restart behaviour. Collecting data on commanded and achieved joint angles and end-effector positions gave a quantitative basis for refining the control parameters.

Joint error compensation was added into the scan start poses. Batched command publishing synchronised the pose updates and removed the snapbacks that appeared during dual-arm scanning.

## Safety

Safety mechanisms covered collision-avoidance trajectories for the upper shelves, hard-limit callbacks that prevent the arms from reaching configurations that caused collisions, and retry-grab endpoints with non-blocking delays so that a failed grasp can be attempted again without stalling or restarting the sequence.

## Perception

Dataset collection spanning both the body and wrist cameras trains the canned-drink detector, giving the system the detections it needed from the two viewpoints available to it. Each detection carries its own product class and a confidence score, so the system can single out the item an order actually calls for.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/storeman-rosman-shelf-picking/product-detection.jpg" title="Per-product detections on the shelf" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The detector separating products on the shelf, labelling two cans of Coke Less Sugar and a Sencha green tea as the gripper closes in.
</div>

Detections are then resolved to a position in three dimensions, which gives the arm a coordinate to reach for.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/storeman-rosman-shelf-picking/detection-coordinates.jpg" title="Detections logged with their 3D coordinates" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Detections logged with their x, y and z coordinates on the Kuavo head AGX, alongside the live RealSense colour stream.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="assets/video/storeman-rosman-shelf-picking/wrist-camera-detection.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true loop=true muted=true %}
    </div>
</div>
<div class="caption">
    The canned-drink detector running on both wrist cameras at once during a dual-arm scan, left feed and right feed side by side, with each detection labelled by confidence.
</div>

<!-- Skills Deployed -->

## Skills Deployed & Responsibilities

- Manipulation
  - Collision-avoidance trajectories
- Control Architecture
  - Dual-arm concurrent control
  - Command gating and state isolation
  - REST endpoints for per-arm and dual-arm operation
  - Shelf-state tracking
- Benchmarking
  - Pose-accuracy characterisation
  - Joint error compensation
- Computer Vision
  - Dataset collection across body and wrist cameras
  - Canned-drink detector training
- Debugging
  - FK service validation
