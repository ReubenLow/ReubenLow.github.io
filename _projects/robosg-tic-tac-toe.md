---
layout: page
title: RoboSG Tic-Tac-Toe Humanoid
description: An interactive Kuavo V4 humanoid that plays tic-tac-toe against members of the public.
img: assets/img/iwsp/robosg-tic-tac-toe/tic-tac-toe-humanoid.jpg
importance: 3
category: work
---

Completed during my IWSP industry attachment at Ceredroid AI.

<!-- Objective of the RoboSG tic-tac-toe demonstration -->

## Objective

To build an interactive tic-tac-toe demonstration for the RoboSG 2025 event, integrating manipulation, perception and conversational interaction so that members of the public could play a full game against a Kuavo V4 humanoid.

## Outcome

A Kuavo V4 humanoid that played tic-tac-toe against visitors, identifying the pieces and the board state by computer vision, placing its own piece in any of the nine positions, and using head gestures to stay engaging between moves. The demonstration ran reliably in an uncontrolled public environment and proved popular with visitors.

<div class="row justify-content-sm-center">
    <div class="col-sm-7 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/robosg-tic-tac-toe/tic-tac-toe-humanoid.jpg" title="The humanoid placing a piece on the tic-tac-toe gameboard" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The humanoid mid-game, holding a piece above the gameboard.
</div>

## Gameboard Design

The gameboard was designed in CAD and optimised for computer vision. Ball-centering mechanisms in each cell settled every piece into a consistent position, which gave the detector a stable and predictable target and made board-state classification reliable.

## Manipulation and Perception

I programmed complete trajectory sets covering all nine board positions. Each move was segmented into two stages: the arm first aligns the camera over the target cell so the current state can be classified, then places the piece. Arm positioning and the classification server were synchronised over ROS so that each stage only began once the previous one had settled. The placement motion had to absorb variability in the gripper so that pieces landed cleanly in the intended cell.

Piece detection ran on a YOLO model trained on the game pieces, and I annotated part of the dataset it was trained on.

## State Management

Early versions let players send action requests faster than the robot could execute them, which caused movement conflicts and erroneous ball detections. I added a queuing system for movement commands together with blocking that rejected new commands while a motion was active. Splitting the trajectories into distinct grab-and-align and place stages, each synchronised through ROS topics, gave the determinism needed for consistent gameplay.

## Validation

Stress testing across motor restarts validated that the trajectories stayed repeatable, confirming the system could hold its accuracy over a long public event.

## At the Event

The demonstration ran at RoboSG, where it engaged ministers, industry professionals and members of the community. Visitors played against the robot directly, and the game format gave them an immediate and approachable way to see the manipulation and perception work in action.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/robosg-tic-tac-toe/robosg-demo-1.jpg" title="The demonstration at RoboSG" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/robosg-tic-tac-toe/robosg-demo-2.jpg" title="A visitor placing a piece on the gameboard" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Demonstration of the humanoid at RoboSG.
</div>

<!-- Skills Deployed -->

## Skills Deployed & Responsibilities

- Motion Planning
  - Trajectory programming across all nine board positions
  - Two-stage trajectory segmentation
  - Head gesture control
- Computer Vision
  - YOLO model training on the game pieces
  - Dataset annotation
  - Piece identification and board-state classification
- ROS
  - Synchronisation between arm positioning and classification server
  - State management, command queuing and blocking
- Mechanical Design
  - CAD gameboard prototyping
- Quality Assurance
  - Stress testing across motor restarts
