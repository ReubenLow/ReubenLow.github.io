---
layout: page
title: Hierarchical Long-Horizon Planning for Humanoid Robotics
description: A vision-language planning system that turns spoken goals into verified robot subtasks.
img: assets/img/capstone/robot-breakfast-scenario.jpg
importance: 1
category: work
---

Final year capstone project for the Robotics Systems Engineering programme at the Singapore Institute of Technology. Hardware access to the Kuavo V4 Pro humanoid came through my industry attachment at Ceredroid AI.

<!-- Objective of the capstone -->

## Objective

Humanoid robots trained through Vision-Language-Action fine-tuning can carry out single skills reliably, such as pouring cereal from a jar into a bowl. Chaining those skills into a multi-step task is a separate problem. This project set out to close that gap with a long-horizon planner built on the Plan-Act-Correct-Verify loop.

The planner takes a natural language goal such as "make breakfast cereal with milk", breaks it into an ordered sequence of subtasks, checks each one against the skills the robot has actually been trained on, dispatches them one at a time, confirms visually that each finished, and recovers when one fails.

## Outcome

A working closed-loop planner demonstrated on a physical Kuavo V4 Pro humanoid, built across two architectural iterations and evaluated against two vision-language model backends over 10,800 scored trials and a 50-task simulation benchmark.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/capstone/robot-breakfast-scenario.jpg" title="Kuavo V4 Pro humanoid with the breakfast cereal scenario" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The Kuavo V4 Pro humanoid set up for the breakfast cereal preparation scenario.
</div>

## Grounding Plans in Real Skills

A language model asked to plan a task will happily produce steps the robot has no way of performing. The planner therefore runs every generated subtask through a grounding step that matches it against a curated annotation library of the robot's trained VLA skills, using a sentence encoder. Anything with no match is filtered out before dispatch, so a hallucinated or inexecutable action never reaches the robot.

## Pipeline Architecture

The first iteration loads a single Cosmos-Reason1-7B instance at startup and drives it with four separate YAML prompt configurations, one for each phase: decomposition, verification, recovery and format checking. Every inference call is independent, and continuity between phases comes from a JSON-backed state manager holding the goal, the subtask list and each subtask's status.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/capstone/pipeline-architecture.png" title="Pipeline execution flow" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The single-turn pipeline architecture, running decompose, ground, dispatch, verify and recover as independent inference calls over a JSON memory bank. Click to enlarge.
</div>

The limitation of this design is that the model has no memory of its own reasoning. Each call sees only what the state manager recorded, which blocks iterative plan refinement, weakens recovery decisions, and forces the scene to be re-analysed from scratch every time.

## Multi-Turn Conversation Architecture

The second iteration replaces the JSON state machine with a persistent conversation session. The dialogue history itself becomes the state: the model sees its own prior plans, the verification images, the outcomes and any user clarifications at every turn. This was built first on Gemini ER 1.5 using its native chat sessions and function calling, then a custom module brought the same architecture to Cosmos-Reason1-7B.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/capstone/multi-turn-architecture.png" title="Multi-turn conversation data flow" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The multi-turn architecture, where accumulated dialogue history carries state across the dispatch, verify and advance loop. Click to enlarge.
</div>

Carrying dialogue context across phases improved the quality of both verification and recovery compared with the stateless design.

## Comparing Two Planning Backends

Two vision-language models were evaluated as planning backends: NVIDIA Cosmos-Reason1-7B, a 7-billion-parameter model running locally on GPU, and Google Gemini Robotics ER 1.5, a cloud model specialised for embodied reasoning.

The comparison covered 10,800 scored trials spanning three scene complexity tiers and six prompt categories, using a pilot-then-confirmatory holdout design with Bonferroni-corrected hypothesis testing.

| Metric               | Cosmos-Reason1-7B | Gemini Robotics ER 1.5 |
| -------------------- | ----------------- | ---------------------- |
| Overall              | 40.75%            | 97.17%                 |
| Simple scenes        | 39.25%            | 98.33%                 |
| Medium scenes        | 43.08%            | 99.17%                 |
| Complex scenes       | 39.92%            | 94.00%                 |
| Alternative phrasing | 33.50%            | 96.50%                 |

Success rates on the confirmatory holdout. The gap held across every condition tested.

Cosmos-Reason1 proved notably sensitive to how a goal was phrased, dropping to 33.50% on alternative phrasings of the same task. Gemini ER 1.5 stayed above 94% everywhere.

## Long-Horizon Benchmark

Static planning quality does not reveal how a planner behaves over an extended episode, so the system was evaluated on 50 household tasks from the BEHAVIOR-1K benchmark in the OmniGibson simulator, covering cooking, cleaning and furniture rearrangement. This measured the planner's monitoring capability against simulator ground truth.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/capstone/behavior1k-omnigibson.jpg" title="BEHAVIOR-1K benchmark environment in OmniGibson" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The BEHAVIOR-1K benchmark environment in OmniGibson, with the R1Pro mobile manipulator.
</div>

The results traced a clear capability hierarchy, ordered by how much each level demands of the model's visual grounding.

| Capability            | Result                                 |
| --------------------- | -------------------------------------- |
| Decomposition quality | 6.68 / 7 mean score, 100% success rate |
| Progress monitoring   | 2.01 / 3                               |
| Success prediction    | 62% accuracy, 76% false positive rate  |

Reasoning about a single image is close to solved. Tracking state changes across a sequence of images is moderate. Judging a final scene against precise goal conditions is the weakest of the three, and it fails in a specific direction: the planner is optimistic, calling tasks complete when they are not.

## Failure Modes

Five distinct failure modes were identified across the benchmark episodes. Object hallucination was the most pervasive, and the most dangerous, because it is silent. The planner reports confidently incorrect observations without flagging any uncertainty, so a closed-loop system driving execution from those assessments receives no signal that anything is wrong. The 33% of tasks scoring below 1.5 out of 3 on progress monitoring would produce unreliable control decisions in a live system.

Three mitigations came out of this analysis: supplying explicit goal criteria derived from the benchmark's own task definitions, comparing initial and final scenes with change-detection prompting, and supplementing image-based verification with simulator state metrics.

<!-- Skills Deployed -->

## Skills Deployed & Responsibilities

- Vision-Language Models
  - Local GPU deployment of Cosmos-Reason1-7B
  - Gemini Robotics ER 1.5 chat sessions and function calling
  - Prompt design across decomposition, verification and recovery
- System Architecture
  - Plan-Act-Correct-Verify closed loop
  - Stateless pipeline and multi-turn conversation designs
  - RESTful dispatch to the humanoid VLA server
- Robotics Integration
  - Skill grounding against a VLA annotation library
  - Visual verification and failure recovery
- Evaluation and Statistics
  - Pilot-then-confirmatory holdout design
  - Bonferroni-corrected hypothesis testing
  - LLM-as-judge automated scoring
- Simulation
  - BEHAVIOR-1K benchmark in OmniGibson
