---
layout: page
title: Motion Planning in Dynamic Environments
description:
img: assets/img/Picture1.jpg
importance: 1
category: work
related_publications: 
---

## Introduction
Typical path planning problems in static environments aim to optimize the path between the start and goal states by minimizing a user-specified cost-function. However, many real world applications consist of dynamic obstacles, such as self-driving car, multi-robot motion planning, and service robot. It is desired that these robots autonomously navigate in dynamic environments while replanning in real-time as needed to achieve high success rates and low travel times.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_2.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Reactive Motion Planning

**Related Paper**
- Z. Shen, J. P. Wilson, S. Gupta, and R. Harvey, “SMART: Self-morphing adaptive replanning tree,” IEEE Robotics and Automation Letters, vol. 8, no. 11, pp. 7312–7319, Sep. 2023. [<b><a href="https://ieeexplore.ieee.org/document/10250928">Paper</a></b>][<b><a href="https://github.com/ZongyuanShen/SMART">Code</a></b>]

**Overview:**
We proposed an algorithm, called Self-Morphing Adaptive ReplanningTree (SMART), that facilitates real-time reactive replanning in dynamic environments for uninterrupted navigation. SMART performs risk-based tree-pruning if the current path is obstructed by nearby moving obstacle(s), resulting in multiple disjoint subtrees. Then, for speedy recovery, it exploits these subtrees and performs informed tree-repair at hot-spots that lie at the intersection of subtrees to find a new path. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_4.svg" title="example image" class="img-fluid rounded z-depth-1"%}
    </div>
</div>

**Performance Comparison with State-of-the-art**
- Robot: holonomic model with constant speed of 4m/s.
- Dynamic obstacle: holonomic model with different speeds.
- Scenario: 66m × 38m factory environment with static obstacle layout and 10 dynamic obstacles.
- Metrics: 1) Replanning time: Time to replan a new path, 2) Success rate: Fraction of successful runs out of the total, and 3) Travel time: Time from start to goal without collision.
- Baseline algorithms: [D* Lite](https://cdn.aaai.org/AAAI/2002/AAAI02-072.pdf), [ERRT](https://ieeexplore.ieee.org/abstract/document/1041624), [DRRT](https://ieeexplore.ieee.org/document/1641879), [MPRRT](https://ieeexplore.ieee.org/document/4209317), [RRTX](https://journals.sagepub.com/doi/full/10.1177/0278364915594679), [HLRRT](https://link.springer.com/article/10.1007/s10514-019-09879-8)*, [EBGRRT](https://www.sciencedirect.com/science/article/abs/pii/S0921889020304358), [MODRRT*](https://ieeexplore.ieee.org/document/9115288).

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/SMART_comparative_result.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

**Demo 1: Navigation in Dynamic Environments**
- Robot: holonomic model with constant speed of 4m/s.
- Dynamic obstacle: holonomic model with constant speed of 4m/s.
- Scenario: 32m × 32m open space with 15 dynamic obstacles.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/SMARTvsbaselineAlg_dynEnv_15obs_scene1.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/SMARTvsbaselineAlg_dynEnv_15obs_scene5.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

**Demo 2: Real Experiment in Dynamic Lab Scenario**
- Robot: [ROSMASTER X3](https://category.yahboom.net/collections/ros-robotics/products/rosmaster-x3) equipped with 1) a RPLIDAR S2L lidar with a range of 8 m for obstacle detection, 2) MD520 motor with encoder for detection of rotation angle and linear displacement, and 3) MPU9250 IMU for detection of speed, acceleration, and orientation.
- Localization: An Extended Kalman Filter is used to fuse data from the IMU and motor encoder for localization.
- Scenario: 7m × 7m lab space with a static layout and three humans.

<div class="row">
     <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/SMART_experiment.gif" title="example image" class="img-fluid rounded z-depth-1"%}
    </div>
</div>

**Demo 3: Navigation in Unknown Environments**
- Robot: holonomic model with constant speed of 4m/s and a lidar with a range of 8m for static obstacle detection.
- Scenario 1: 62m × 32m indoor office environment.
- Scenario 2: 62m × 32m outdoor forest environment.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/SMART_unknownEnv_scene1.gif" title="example image" class="img-fluid rounded z-depth-1"%}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/SMART_unknownEnv_scene2.gif" title="example image" class="img-fluid rounded z-depth-1"%}
    </div>
</div>
