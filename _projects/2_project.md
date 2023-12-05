---
layout: page
title: Reactive Replanning
description:
img: assets/img/3.jpg
importance: 1
category: work
related_publications: 
---

## Introduction
Typical path planning problems in a static environment aim to optimize the path between the start and goal states by minimizing a user-specified cost-function (e.g., travel time). However, many real world applications (e.g., airports, factories, malls, offices, hospitals and homes) consist of moving obstacles (e.g., humans, robots, carts and wheelchairs). It is envisioned that these applications will be increasingly witnessing the role of cobots in supporting humans for various tasks. It is desired that these cobots autonomously navigate in dynamic environments while replanning in real-time as needed to achieve: 1) high success rates and 2) low travel times. Replanning strategies are characterized as active or reactive. The active strategies predict the future trajectories of moving obstacles to replan the robot’s path; however, their performance degrades in crowded environments where these trajectories are difficult to compute, associate and predict. Therefore, the reactive strategies replan the robot’s path based on the current information.

A variety of reactive replanning algorithms exist in literature. However, these algorithms either utilize inefficient tree pruning and repair strategies, which could lead to wasteful and slow tree growth; or search new path on an underlying graph, which could make the robot get stuck when new feasible path can not be found on the graph; or incrementally generate a single action to approach the goal, which can easily get trapped in local minima especially when the goal is far away and the environment is cluttered.

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

**Initialization:** First, a tiling is created on the space for searching hot spots later. Then, SMART is initialized by constructing a RRT* tree rooted at the goal, where each node maintains a data structure including the information about its position, parent, children, tree index, cell index and node status as active or pruned. Initially, all nodes are marked active. Then an initial path is found using the tree. As the cobot moves, the path could be blocked by dynamic obstacles. Thus, the paper presents an informed replanning strategy consisting of two-steps: 1) tree-pruning and 2) tree-repair.

**Local Tree-Pruning:** To find a safe trajectory via replanning, it is important to identify and prune the risky nodes of the tree. Since: 1) dynamic obstacles far away from the cobot do not pose an immediate risk, and 2) it is computationally inefficient to do feasibility checking around all detected obstacles, we present a local tree-pruning (LTP) strategy. A local reaction zone (LRZ) is created at the cobot position. An obstacle hazard zone (OHZ) is defined for each dynamic obstacle. Critical pruning region (CPR) consists of OHZs that are intersecting with the LRZ. As shown in Figure 1(a), the cobot constantly checks the validity of its current path by checking the path nodes and edges in LRZ starting from its current position. According to the LTP strategy, if the current path is invalid, then all tree portions that fall within the CPR are considered to be risky for replanning and are thus pruned. If a node is invalid, it is pruned along with its edges. If an edge is invalid but its two end nodes are safe, then only this edge is pruned. The parent and child information of all affected nodes is updated. Tree-pruning could break the current tree into disjoint subtrees.

**Tree-Repairing:** The tree-pruning step is followed by the tree-repair step, which is done to find an updated path to the goal. Figures 2(b)-(i) show an example of the tree-repair process. For fast and efficient tree-repair, SMART exploits the previous exploration efforts by retaining all the disjoint subtrees and pruned nodes for possible future additions. The tree-repair process starts by searching for hot-spots, where the nodes of disjoint subtrees lie in a local neighborhood and the reconnection of these subtrees can be done immediately. The search begins in the local search region (LSR), which is initialized as the 3 by 3 local neighborhood of the cell containing pruned path node closest to the cobot (Figure 2(b)). The hot-spots are ranked using a utility map to direct repairing to the region that has the shortest cost-to-come and cost-to-go (Figures 2(b) and (f)). Then, the hot-spot with the highest utility is picked and the tree is repaired by connecting the nodes of the disjoint subtrees in the neighborhood of this hot-spot (Figures 2(c)-(e) and (g)) until no more hot-spots can be found in LSR (Figure 2(e)) or the goal-rooted subtree is reachable from the robot (Figure 2(g)).

**Tree-Optimization and Path Search:** During tree-repair, several subtrees were connected to ultimately merge with goal-rooted subtree. These subtrees are optimized using rewiring cascades starting from all subtree nodes connected to goal-rooted subtree (Figure 2(h)). Then, an updated trajectory is found (Figure 2(i)). After that, all pruned nodes and the roots of the remaining disjoint trees are reconnected with goal-rooted subtree and a single tree is formed.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_4.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 2: Illustration of the SMART algorithm: (a) tree-pruning and disjoint tree creation and (b)-(i) tree-repair and replanning.
</div>

## Results
**Motivation:**:aa

## Related Paper
- Z. Shen, J. P. Wilson, S. Gupta, and R. Harvey, “SMART: Self-morphing adaptive replanning tree,” IEEE Robotics and Automation Letters, vol. 8, no. 11, pp. 7312–7319, Sep. 2023.
