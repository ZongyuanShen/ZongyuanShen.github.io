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
Typical path planning problems in a static environment aim to optimize the path between the start and goal states by minimizing a user-specified cost-function (e.g., travel time). However, many real world applications (e.g., airports, factories, malls, offices, hospitals and homes) consist of moving obstacles (e.g., humans, robots, carts and wheelchairs). It is envisioned that these applications will be increasingly witnessing the role of robots in supporting humans for various tasks. It is desired that these robots autonomously navigate in dynamic environments while replanning in real-time as needed to achieve: 1) high success rates and 2) low travel times. Replanning strategies are characterized as active or reactive. The active strategies predict the future trajectories of moving obstacles to replan the robot’s path; however, their performance degrades in crowded environments where these trajectories are difficult to compute, associate and predict. Therefore, the reactive strategies replan the robot’s path based on the current information.

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
We proposed an algorithm, called Self-Morphing Adaptive Replanning Tree (SMART), that facilitates real-time reactive replanning in dynamic environments for uninterrupted navigation.To initialize, SMART constructs a search-tree using the RRT* algorithm considering only the static obstacles and finds the initial path. Subsequently, while navigating, the robot constantly validates its current path for obstructions by nearby dynamic obstacles. If the path is infeasible, SMART performs quick informed replanning that consists of two-steps: 1) tree-pruning and 2) tree-repair. In the tree-pruning step, all risky nodes near the cobot are pruned. This breaks the current tree and forms (possibly) multiple disjoint subtrees. Next, the informed tree-repair step searches for hot-spots that lie at the intersection of different subtrees and provide avenues for real-time tree-repair. Then, the utilities of these hot-spots are computed using the shortest-path heuristics. Finally, these hot-spots are incrementally selected according to their utility for merging disjoint subtrees until a new path is found.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_4.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 2: Illustration of the SMART algorithm.
</div>

## Results
**Motivation:**:aa

## Related Paper
- Z. Shen, J. P. Wilson, S. Gupta, and R. Harvey, “SMART: Self-morphing adaptive replanning tree,” IEEE Robotics and Automation Letters, vol. 8, no. 11, pp. 7312–7319, Sep. 2023.
