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
We proposed a 3D CPP algorithm, called CT-CPP, using a coverage tree (CT), where the nodes (except the root node) of CT represent the disconnected subregions, as shown in Figure 2. CT-CPP incrementally builds a CT in a top-down manner, which is used to plan and track the progress of 3D coverage. 

Once the AUV reaches a node, it covers the corresponding subregion using a [2D CPP algorithm](https://ieeexplore.ieee.org/abstract/document/8286947). During the coverage of each planar subregion, the AUV uses the downward-facing multi-beam sonar sensor to collect data for the 3D terrain structures that are within the sensor’s range extended at least up to the plane below. Based on the data, the AUV projects and stores the information about obstacles intersecting the plane below by forming a 2D probabilistic occupancy map [(POM)](https://mitpress.mit.edu/9780262201629/probabilistic-robotics/). Since the underwater terrain may contain narrow regions which can be risky for the AUV to navigate and avoid collisions, an image morphological operator [‘closing’](https://link.springer.com/book/10.1007/978-3-662-05088-0) is applied on the POM to close the narrow areas. Subsequently, a symbolic map is obtained such that the cells whose occupancy probability is higher than the threat probability are marked as threat, while the others are marked as safe. This serves two purposes: i) the updated symbolic map is used to identify the disconnected subregions on that plane. i.e., it adds child nodes to the CT, and ii) it ensures the AUV’s safety when it navigates on that plane.

The updated tree is then used to plan the AUV trajectory by generating an optimized tree traversal sequence using a heuristics-based solution to the traveling salesman problem. The above process continues until the AUV completes the coverage of all nodes of the tree and no new nodes are created. Then, the data collected from all nodes are integrated offline for complete 3D terrain reconstruction using [α-Shape algorithm](https://en.wikipedia.org/wiki/Alpha_shape).

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_4.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 2: Incremental construction of the coverage tree.
</div>

## Results
The CT-CPP algorithm is validated on a high-fidelity underwater simulator called [UWSim](https://wiki.ros.org/uwsim) based on the [Robot Operating System](https://www.ros.org/). The AUV, called Girona 500, is simulated. Five scenes of size 450m × 450m × 400m are randomly generated for performance validation as shown in Figure 3(a). A comparative evaluation of the coverage trajectories generated by the CT-CPP and the terrain following CPP [(TF-CPP)](https://link.springer.com/article/10.1007/BF00141150) methods in scene 1 is presented in Figure 3(b) and Figure 3(c), respectively. Figure 3(f) shows the total trajectory lengths for all five scenes, which indicate that the CT-CPP method took the shorter path lengths. Figure 3(g) shows that the CT-CPP method consumed less energy in all five scenes as compared to the TF-CPP method due to its frequent vertical motions. Further, we numerically evaluated the reconstruction errors using different α, which is a positive parameter controlling the desired level of details. As seen in Figure 3(h), CT-CPP performs better in all five scenes. Thus, CT-CPP is an alternate method to TF-CPP and it is expected to outperform the TF-CPP method in a significant number of scenarios.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_5.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 3: Performance evaluation for CT-CPP as compared to TF-CPP.
</div>

**Related Paper:**
- Z. Shen, J. P. Wilson, S. Gupta, and R. Harvey, “SMART: Self-morphing adaptive replanning tree,” IEEE Robotics and Automation Letters, vol. 8, no. 11, pp. 7312–7319, Sep. 2023.
