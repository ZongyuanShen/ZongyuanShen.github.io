---
layout: page
title: Reactive Motion Replanning
description:
img: assets/img/3.jpg
importance: 1
category: work
related_publications: 
---

## Motivation
Typical path planning problems in a static environment aim to optimize the path between the start and goal states by minimizing a user-specified cost-function (e.g., travel time). However, many real world applications (e.g., airports, factories, malls, offices, hospitals and homes) consist of moving obstacles (e.g., humans, robots, carts and wheelchairs). It is envisioned that these applications will be increasingly witnessing the role of cobots in supporting humans for various tasks. It is desired that these cobots autonomously navigate in dynamic environments while replanning in real-time as needed to achieve: 1) high success rates and 2) low travel times. Replanning strategies are characterized as active or reactive. The active strategies predict the future trajectories of moving obstacles to replan the robot’s path; however, their performance degrades in crowded environments where these trajectories are difficult to compute, associate and predict. Therefore, the reactive strategies replan the robot’s path based on the current information.

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
    Figure 1: Applications of reactive path replanning.
</p>

## Method Overview
We proposed an algorithm, called **Self-Morphing Adaptive ReplanningTree (SMART)**, that facilitates real-time reactive replanning in dynamic environments for uninterrupted navigation. It is initialized by constructing a RRT* tree rooted at the goal and an initial path is found using the tree. As the cobot moves, the path could be blocked by dynamic obstacles. A new path is replanned following three steps: 1) tree-pruning, 2) tree-repair, and 3) tree-optimization.

**Tree-Pruning:** To find a safe trajectory via replanning, it is important to identify and prune the risky nodes of the tree. Since: 1) dynamic obstacles far away from the cobot do not pose an immediate risk, and 2) it is computationally inefficient to do feasibility checking around all detected obstacles, we present a local tree-pruning (LTP) strategy. A local reaction zone (LRZ) is created at the cobot position given the reaction time horizon and robot speed. An obstacle hazard zone (OHZ) is defined for each dynamic obstacle. Critical pruning region (CPR) consisting of OHZs that are intersecting with the LRZ is used for feasibility checking. As shown in Figure 1(a), the cobot constantly checks the feasibility of its current path in LRZ. If the current path is invalid, then all tree portions that fall within the CPR are considered to be risky for replanning and are thus pruned. This breaks the current tree and forms (possibly) multiple disjoint subtrees.

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

## Simulation in the Dynamic Environments
- Robot: holonomic model with constant speed of 4m/s.
- Moving obstacle: moves along a random heading for a random distance.
- Scenario: 32m × 32m open space populated with only moving obstacles.
- Case studies: Two cases are conducted including 10 and 15 obstacles. Each obstacle moves at the same speed selected from the set {1,2,3,4}m/s, resulting in 8 different combinations of the number of obstacles and speeds. For each combination, 30 different obstacle trajectories are generated, resulting in a total of 240 case studies.
- Metrics: 1) Replanning time: Time to replan a new path, 2) Success rate: Fraction of successful runs out of the total, and 3) Travel time: Time from start to goal without collision.
- Baseline algorithms: [D* Lite](https://cdn.aaai.org/AAAI/2002/AAAI02-072.pdf), [ERRT](https://ieeexplore.ieee.org/abstract/document/1041624), [DRRT](https://ieeexplore.ieee.org/document/1641879), [MPRRT](https://ieeexplore.ieee.org/document/4209317), [RRTX](https://journals.sagepub.com/doi/full/10.1177/0278364915594679), [HLRRT](https://link.springer.com/article/10.1007/s10514-019-09879-8)*, [EBGRRT](https://www.sciencedirect.com/science/article/abs/pii/S0921889020304358), [MODRRT*](https://ieeexplore.ieee.org/document/9115288).

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
        <p align="center" style="font-size:0.9rem;">
            Table 1: Comparison of key features of SMART with other tree-based reactive replanning algorithms.
        </p>
    </div>
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/SMART_comparative_result_scene1_part1.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<p align="center" style="font-size:0.9rem;">
    Figure 3: Comparison of success rate and replanning time for the scenario with 10 obstacles.
</p>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/SMART_comparative_result_scene1_part2.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<p align="center" style="font-size:0.9rem;">
    Figure 4: Comparison of success rate and replanning time for the scenario with 15 obstacles.
</p>
        
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/SMART_comparative_result_scene1_part3.svg" title="example image" class="img-fluid rounded z-depth-1" %}
        <p align="center" style="font-size:0.9rem;">
            (a) Comparison of travel time for the scenario with 10 obstacles.
        </p>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/SMART_comparative_result_scene1_part4.svg" title="example image" class="img-fluid rounded z-depth-1" %}
        <p align="center" style="font-size:0.9rem;">
            (b) Comparison of travel time for the scenario with 15 obstacles.
        </p>
    </div>
</div>
<p align="center" style="font-size:0.9rem;">
    Figure 5: Comparison of travel time of successful trials. The plots show the median and the 25th and 75th percentile values.
</p>

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
<p align="center" style="font-size:0.9rem;">
    Video 1: Demos for the scenario with 15 moving obstacles. Both robot and obstacles move at a speed of 4m/s.
</p>

## Real Experiment in the Dynamic Environments
- Robot: [ROSMASTER X3](https://category.yahboom.net/collections/ros-robotics/products/rosmaster-x3) equipped with 1) a RPLIDAR S2L lidar with a range of 8 m for obstacle detection, 2) MD520 motor with encoder for detection of rotation angle and linear displacement, and 3) MPU9250 IMU for detection of speed, acceleration, and orientation.
- Localization: An Extended Kalman Filter is used to fuse data from the IMU and motor encoder for localization.
- Scenario: 7m × 7m lab space with a static layout and three humans.

<div class="row">
     <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/SMART_experiment.gif" title="example image" class="img-fluid rounded z-depth-1"%}
    </div>
</div>
<p align="center" style="font-size:0.9rem;">
    Video 2: Real experiment of SMART in a lab space.
</p>

## Simulation in the Unknown Static Environments
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
<p align="center" style="font-size:0.9rem;">
    Video 3: Validation of SMART in two unknown static scenarios.
</p>
  
## Related Paper
- Z. Shen, J. P. Wilson, S. Gupta, and R. Harvey, “SMART: Self-morphing adaptive replanning tree,” IEEE Robotics and Automation Letters, vol. 8, no. 11, pp. 7312–7319, Sep. 2023. [<b><a href="https://ieeexplore.ieee.org/document/10250928">Paper</a></b>][<b><a href="https://github.com/ZongyuanShen/SMART">Code</a></b>][<b><a href="https://drive.google.com/file/d/1d_cqbyHNAHxAA4SC-DgQBfWWJfAHBIod/view?usp=drive_link">Slide</a></b>]
- A follow-up journal paper is in production.
