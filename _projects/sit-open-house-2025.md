---
layout: page
title: SIT Open House 2025 Humanoid Demonstration
description: Kuavo V4 humanoid manipulation driven by verbal interaction and computer vision.
img: assets/img/iwsp/sit-open-house-2025/humanoid-setup.jpg
importance: 3
category: work
---

Completed during my IWSP industry attachment at Ceredroid AI.

<!-- Objective of the SIT Open House demonstration -->

## Objective

To showcase humanoid manipulation driven by verbal interaction and computer vision at the SIT Open House 2025, using the Kuavo V4 platform. The demonstration had to run in a public setting where visitors interact with the robot directly, and where there is limited opportunity for operator intervention once the event is underway.

## Outcome

A Kuavo V4 humanoid that engaged visitors verbally, located souvenirs and canned drinks using computer vision, and carried out pick-and-place handovers with human-like gestures. The system maintained consistent performance over extended operating periods with diverse public interaction.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/sit-open-house-2025/humanoid-setup.jpg" title="Humanoid set-up at the SIT Open House 2025" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The humanoid set-up at the SIT Open House 2025, with the chatbot interface and souvenir dispenser on the demonstration table.
</div>

## Quality Assurance

I established the QA procedures for the demonstration, defining quantified metrics for each behaviour under test. The framework distinguished between operations that succeeded unaided and those that succeeded only after human intervention. This gave a quantifiable measurement of readiness, was subsequently adopted as the standard for later projects, and fed directly into demonstration planning.

## Workspace Layout and Reachability

VR teleoperation was used to validate reachability and optimise the layout of the demonstration workspace, confirming that every souvenir and canned drink position could be reached before the fixtures' position were finalised.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/iwsp/sit-open-house-2025/vr-teleop-reachability.jpg" title="Conducting reachability tests via VR teleoperation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Conducting reachability tests via VR teleoperation.
</div>

## Interaction and Manipulation

The technical scope covered fixture design for the souvenirs, trajectory programming for the pick-and-place operations, computer vision integration, and gesture programming so the robot's movements read as human-like to visitors. I also tuned the chatbot system prompts so its responses suited the context of the Open-house event.

<!-- Skills Deployed -->

## Skills Deployed & Responsibilities

- Quality Assurance
  - Test procedures and quantified success metrics
- VR Teleoperation
  - Reachability validation
- Motion Planning
  - Pick-and-place trajectory programming
  - Gesture programming
- Computer Vision Integration
- Mechanical Design
  - Souvenir fixtures
- Prompt Engineering
