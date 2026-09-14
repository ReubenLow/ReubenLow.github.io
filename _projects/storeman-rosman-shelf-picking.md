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

A shelf-picking system in which each arm operates independently under its own commands, coordinated by shelf-state tracking that assigns work between them. Integrated testing demonstrated concurrent two-order operation with both arms running together.

<div class="row justify-content-sm-center">
    <div class="col-sm-7 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/storeman-rosman-shelf-picking/shelf-picking.jpg" title="The humanoid picking canned drinks from a stocked shelf" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The humanoid facing a stocked shelf, with wrist-mounted cameras on both arms.
</div>

## Hand-Eye Calibration

I implemented both eye-to-hand and eye-in-hand calibration workflows. These established the transform chains that carry an object detection through to a feasible end-effector pose, which is what allows a pixel-space detection on the shelf to become a grasp the arm can actually execute.

## Inverse Kinematics

Tuning the IK solver with quaternion optimisation improved the straightness of linear trajectories and extended reachability for items sitting centrally on the shelf.

End-effector pose consistency between the different motion APIs caused persistent IK failures that took time to pin down. Some control interfaces used frame conventions that differed from others, so a solution could look correct in isolation and then fail during integration. I worked through the TF tree exhaustively, validated the end-effector calculations against the FK service, documented the frame conventions for each API, and refactored the code to handle frame transformations explicitly at the interface boundaries.

## Control Architecture

- Six predefined ready-position joint states
- REST endpoints exposing per-arm and dual-arm control
- Shelf-state tracking to coordinate arm assignments
- Mid-scan interruption, where one arm grabs a detected item while the other carries on scanning
- Command gating and state isolation to keep the two arms from racing each other

The command gating and state isolation removed a class of failure modes that had affected earlier demonstration attempts, and made the more involved manipulation sequences possible.

## Pose Accuracy Benchmarking

Systematic benchmarking quantified end-effector errors across post-restart behaviour, engaged motion and motor-specific offsets. Collecting data on commanded and achieved joint angles and end-effector positions gave a quantitative basis for refining the control parameters, and surfaced mechanical issues that needed hardware attention.

Joint error compensation was folded into the scan start poses. Batched command publishing synchronised the pose updates and removed the snapbacks that appeared during dual-arm scans.

## Safety

Safety mechanisms covered collision-avoidance trajectories for the upper shelves, hard-limit callbacks that prevent the arms from reaching dangerous configurations, and retry-grab endpoints with non-blocking delays so that a failed grasp can be attempted again without stalling the sequence.

## Perception

Dataset collection spanning both the body and wrist cameras trained a Stage 1 canned-drink detector, giving the system the detections it needed from the two viewpoints available to it.

<!-- Skills Deployed -->

## Skills Deployed & Responsibilities

- Manipulation
  - Eye-to-hand and eye-in-hand calibration
  - IK solver tuning with quaternion optimisation
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
  - TF tree inspection and FK service validation
  - Frame convention documentation
