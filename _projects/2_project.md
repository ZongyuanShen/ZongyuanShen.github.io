---
layout: page
title: Reactive Path Replanning
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
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_2.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 1: Applications of reactive plannning including self-driving cars, multi-robot system, and service robot.
</div>

## Method Overview
We proposed an algorithm, called **Self-Morphing Adaptive ReplanningTree (SMART)**, that facilitates real-time reactive replanning in dynamic environments for uninterrupted navigation. It is initialized by constructing a RRT* tree rooted at the goal and an initial path is found using the tree. As the cobot moves, the path could be blocked by dynamic obstacles. A new path is replanned via an informed replanning strategy consisting of three steps: 1) tree-pruning, 2) tree-repair, and 3) tree-optimization.

**Tree-Pruning:** To find a safe trajectory via replanning, it is important to identify and prune the risky nodes of the tree. Since: 1) dynamic obstacles far away from the cobot do not pose an immediate risk, and 2) it is computationally inefficient to do feasibility checking around all detected obstacles, we present a local tree-pruning (LTP) strategy. A local reaction zone (LRZ) is created at the cobot position given the reaction time horizon and robot speed. An obstacle hazard zone (OHZ) is defined for each dynamic obstacle. Critical pruning region (CPR) consisting of OHZs that are intersecting with the LRZ is used for feasibility checking. As shown in Figure 1(a), the cobot constantly checks the feasibility of its current path in LRZ. If the current path is invalid, then all tree portions that fall within the CPR are considered to be risky for replanning and are thus pruned. This breaks the current tree and forms (possibly) multiple disjoint subtrees.

**Tree-Repair:** For fast and efficient tree-repair, SMART exploits the previous exploration efforts by retaining all the disjoint subtrees and pruned nodes for possible future additions. The tree-repair process starts by searching for hot-spots, where the nodes of disjoint subtrees lie in a local neighborhood and the reconnection of these subtrees can be done immediately. The search begins in the local search region (LSR), which is initialized as the 3 by 3 local neighborhood of the cell containing pruned path node closest to the cobot (Figure 2(b)). The hot-spots are ranked using a utility map to direct repairing to the region that has the shortest cost-to-come and cost-to-go (Figures 2(b) and (f)). Then, the hot-spot with the highest utility is picked and the tree is repaired by connecting the nodes of the disjoint subtrees in the neighborhood of this hot-spot (Figures 2(c)-(e) and (g)) until no more hot-spots can be found in LSR (Figure 2(e)). In this case, the LSR is expanded for further tree reparing (Figure 2(e)) until the goal-rooted subtree is reachable from the cobot (Figure 2(g)).

**Tree-Optimization and Path Search:** During tree-repair, several subtrees were connected to ultimately merge with goal-rooted subtree. These subtrees are optimized using rewiring cascades starting from all subtree nodes connected to goal-rooted subtree (Figure 2(h)). Then, an updated trajectory is found (Figure 2(i)). After that, all pruned nodes and the roots of the remaining disjoint trees are reconnected with goal-rooted subtree and a single tree is formed.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_4.svg" title="example image" class="img-fluid rounded z-depth-1"%}
    </div>
</div>
<div class="caption">
    Figure 2: Illustration of the SMART algorithm: (a) tree-pruning and disjoint tree creation and (b)-(i) tree-repair and replanning.
</div>

## Simulation in the Environments with Moving Obstacles
**Set-Up:** 
- Holonomic robot: moves at a speed of 4m/s.
- Dynamic obstacle: moves along a random heading for a random distance.
- Scenario 1: 32m × 32m space populated with only dynamic obstacles.
- Scenario 2: 66m × 38m space with a static obstacle layout and 10 dynamic obstacles.
- Baseline algorithms: [D* Lite](https://cdn.aaai.org/AAAI/2002/AAAI02-072.pdf), [ERRT](https://ieeexplore.ieee.org/abstract/document/1041624), [DRRT](https://ieeexplore.ieee.org/document/1641879), [MPRRT](https://ieeexplore.ieee.org/document/4209317), [RRTX](https://journals.sagepub.com/doi/full/10.1177/0278364915594679), [HLRRT](https://link.springer.com/article/10.1007/s10514-019-09879-8)*, [EBGRRT](https://www.sciencedirect.com/science/article/abs/pii/S0921889020304358), [MODRRT*](https://ieeexplore.ieee.org/document/9115288).

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Table I: Comparison of key features of SMART with other tree-based reactive replanning algorithms.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 3: Simulation testing scenarios.
</div>

**Results:** Figure 4 shows the comparative evaluation results on Scenario 1. Overall, SMART achieves significant improvements over other algorithms in success rate and replanning time in all case studies. This follows from the facts that i) tree-pruning not only reduces collision checking to nearby obstacles but also produces fewer disjoint trees for repairing, and ii) tree-repair exploits the disjoint subtrees and facilitates repairing at hot-spots for speedy recovery. Figure 5 shows that SMART achieves the lowest travel times because of i) low replanning times and ii) infrequent replanning. Furthermore, to investigate the value of the tree-repair step, we present an ablation study, where LRZ is removed, thus pruning all risky nodes. Figures 4 and 5 show that SMART w/o LRZ still performs significantly better than all other algorithms. Figure 5 shows the same trend in Scenario 2. As seen, SMART outperforms all other methods in terms of replanning time, success rate, and the total travel time.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_7.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 4: Comparative evaluation results of success rate and average replanning time for Scenario 1 with (a) 10 and (b) 15 moving obstacles.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_8.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 5: Comparative evaluation results of travel time of successful trials for Scenario 1 with (a) 10 and (b) 15 moving obstacles. The plots show the median and the 25th and 75th percentile values.
</div>

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_9.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_9.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Figure 6: Comparative evaluation results for Scenario 2.
</div>

**Demo:**

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/SMART_simulation.gif" title="example image" class="img-fluid rounded z-depth-1" width="600"%}
    </div>
</div>
<div class="caption">
</div>

## Real Experiment in the Environments with Moving Obstacles

- Robot: [ROSMASTER X3](https://category.yahboom.net/collections/ros-robotics/products/rosmaster-x3) equipped with 1) a RPLIDAR S2L lidar with a range of 8 m for obstacle detection, 2) MD520 motor with encoder for detection of rotation angle and linear displacement, and 3) MPU9250 IMU for detection of speed, acceleration, and orientation.
- Localization: An Extended Kalman Filter is used to fuse data from the IMU and motor encoder for localization.
- Scenario: 7m × 7m lab space with both static and three moving humans.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/SMART_experiment.gif" title="example image" class="img-fluid rounded z-depth-1" width="600"%}
    </div>
</div>
<div class="caption">
</div>

## Simulation in the Environments with Unknown Static Obstacles
  
## Related Paper
- Z. Shen, J. P. Wilson, S. Gupta, and R. Harvey, “SMART: Self-morphing adaptive replanning tree,” IEEE Robotics and Automation Letters, vol. 8, no. 11, pp. 7312–7319, Sep. 2023.
