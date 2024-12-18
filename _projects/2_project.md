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
Typical path planning problems in static environments aim to optimize the path between the start and goal states by minimizing a user-specified cost-function (e.g., travel time). However, many real world applications (e.g., airports, factories, malls, offices, hospitals and homes) consist of dynamic obstacles (e.g., pedestrians, robots, carts and wheelchairs). It is envisioned that these applications will be increasingly witnessing the role of robots in supporting humans for various tasks. It is desired that these robots autonomously navigate in dynamic environments while replanning in real-time as needed to achieve high success rates and low travel times. Replanning strategies are characterized as reactive or proactive. Reactive strategies replan the path based on the current observations, which is useful when the future trajectories of dynamic obstacles are difficult to compute, associate and predict. Proactive strategies predict the trajectories of dynamic obstacles and then integrate the predicted information into the replanning algorithm to generate new paths which can actively avoid collisions to the dynamic obstacles in the spatio-temporal domain.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
        <p align="center" style="font-size:0.9rem;">
            (a) Self-driving car.
        </p>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_2.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
        <p align="center" style="font-size:0.9rem;">
            (b) Multi-robot system.
        </p>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
        <p align="center" style="font-size:0.9rem;">
            (c) Service robot.
        </p>
    </div>
</div>
<p align="center" style="font-size:0.9rem;">
    Figure 1: Applications of dynamic motion planning algorithms.
</p>

## Reactive Motion Planning

We proposed an algorithm, called **Self-Morphing Adaptive ReplanningTree (SMART)**, that facilitates real-time reactive replanning in dynamic environments for uninterrupted navigation. It is initialized by constructing a RRT* tree rooted at the goal and an initial path is found using the tree. As the cobot moves, the path could be blocked by dynamic obstacles. A new path is replanned following three steps: 1) tree-pruning, 2) tree-repair, and 3) tree-optimization.

**Tree-Pruning:** To find a safe trajectory via replanning, it is important to identify and prune the risky nodes of the tree. Since: 1) dynamic obstacles far away from the cobot do not pose an immediate risk, and 2) it is computationally inefficient to do feasibility checking around all detected obstacles, we present a local tree-pruning (LTP) strategy. A local reaction zone (LRZ) is created at the cobot position given the reaction time horizon and robot speed. An obstacle hazard zone (OHZ) is defined for each dynamic obstacle. Critical pruning region (CPR) consisting of OHZs that are intersecting with the LRZ is used for feasibility checking. As shown in Figure 2(a), the cobot constantly checks the feasibility of its current path in LRZ. If the current path is invalid, then all tree portions that fall within the CPR are considered to be risky for replanning and are thus pruned. This breaks the current tree and forms (possibly) multiple disjoint subtrees.

**Tree-Repair:** For fast and efficient tree-repair, SMART exploits the previous exploration efforts by retaining all the disjoint subtrees and pruned nodes for possible future additions. The tree-repair process starts by searching for hot-spots, where the nodes of disjoint subtrees lie in a local neighborhood and the reconnection of these subtrees can be done immediately. The search begins in the local search region (LSR), which is initialized as the 3 by 3 local neighborhood of the cell containing pruned path node closest to the cobot (Figure 2(b)). The hot-spots are ranked using a utility map to direct repairing to the region that has the shortest cost-to-come and cost-to-go (Figures 2(b) and (f)). Then, the hot-spot with the highest utility is picked and the tree is repaired by connecting the nodes of the disjoint subtrees in the neighborhood of this hot-spot (Figures 2(c)-(e) and (g)) until no more hot-spots can be found in LSR (Figure 2(e)). In this case, the LSR is expanded for further tree reparing (Figure 2(e)) until the goal-rooted subtree is reachable from the cobot (Figure 2(g)).

**Tree-Optimization and Path Search:** During tree-repair, several subtrees were connected to ultimately merge with goal-rooted subtree. These subtrees are optimized using rewiring cascades starting from all subtree nodes connected to goal-rooted subtree (Figure 2(h)). Then, an updated trajectory is found (Figure 2(i)). After that, all pruned nodes and the roots of the remaining disjoint trees are reconnected with goal-rooted subtree and a single tree is formed.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_4.svg" title="example image" class="img-fluid rounded z-depth-1"%}
    </div>
</div>
<p align="center" style="font-size:0.9rem;">
    Figure 2: Illustration of the SMART algorithm: (a) tree-pruning and disjoint tree creation and (b)-(i) tree-repair and replanning.
</p>

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
  
**Related Paper**
- Z. Shen, J. P. Wilson, S. Gupta, and R. Harvey, “SMART: Self-morphing adaptive replanning tree,” IEEE Robotics and Automation Letters, vol. 8, no. 11, pp. 7312–7319, Sep. 2023. [<b><a href="https://ieeexplore.ieee.org/document/10250928">Paper</a></b>][<b><a href="https://github.com/ZongyuanShen/SMART">Code</a></b>][<b><a href="https://drive.google.com/file/d/1d_cqbyHNAHxAA4SC-DgQBfWWJfAHBIod/view?usp=drive_link">Slide</a></b>]

## Proactive Motion Planning
Coming soon...
