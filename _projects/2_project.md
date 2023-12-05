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

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/RR_4.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 2: Illustration of the SMART algorithm.
</div>

## Related Paper
- Z. Shen, J. P. Wilson, S. Gupta, and R. Harvey, “SMART: Self-morphing adaptive replanning tree,” IEEE Robotics and Automation Letters, vol. 8, no. 11, pp. 7312–7319, Sep. 2023.
