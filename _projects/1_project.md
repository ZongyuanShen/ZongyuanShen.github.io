---
layout: page
title: Coverage Path Planning in Unknown Environments
description:
img: assets/img/Picture2.jpg
importance: 1
category: work
related_publications: 
---

## Introduction
Coverage path planning (CPP) has been utilized in a variety of real-world robotic applications, such as seabed mapping, arable farming, and floor cleaning. CPP methods are broadly classified as offline or online. Offline methods rely on prior information of the environments to design coverage paths, which can be highly effective in ideal conditions. However, their performance degrades if the prior information is incomplete and/or incorrect. Therefore, it is essential to develop online CPP methods, which adapt the coverage path in realtime based on the collected environmental information.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_2.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Hierarchical Coverage Path Planning
Coming soon...

## Coverage Path Planning of 3D Underwater Terrain

**Related Paper:**
- Z. Shen, J. Song, K. Mittal, and S. Gupta, “CT-CPP: Coverage path planning for 3D terrain reconstruction using dynamic coverage trees,” IEEE Robotics and Automation Letters, vol. 7, no. 1, pp. 135–142, Jan. 2022. [<b><a href="https://ieeexplore.ieee.org/abstract/document/9573264">Paper</a></b>]
- Z. Shen, J. Song, K. Mittal, and S. Gupta, “Autonomous 3-D mapping and safe-path planning for underwater terrain reconstruction using multi-level coverage trees,” in MTS/IEEE OCEANS, 2017, pp. 1–6. [<b><a href="https://ieeexplore.ieee.org/document/8232157">Paper</a></b>]

**Method Overview:**
Due to sensing limitations, the proposed CT-CPP algorithm performs layered scanning of the 3D region to collect terrain data guided by a coverage tree (CT). Once the robot reaches a node, it covers the corresponding subregion using a 2D CPP algorithm. During the coverage of each planar subregion, the robot collects data for the 3D terrain structures below. Based on the data, the robot projects and stores the information about obstacles intersecting the plane below by forming a 2D occupancy map, which is used to identify the disconnected subregions on that plane. i.e., it adds child nodes to the CT. The updated tree is then used to plan an optimal tree traversal sequence by solving a traveling salesman problem. The above process continues until no unvisited node available on the tree.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_4.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

**Results:**
The CT-CPP algorithm is validated on a high-fidelity underwater simulator called [UWSim](https://wiki.ros.org/uwsim). The comparative evaluation results of the CT-CPP with the terrain following CPP [(TF-CPP)](https://link.springer.com/article/10.1007/BF00141150) methods is presented in Figure 3. Overall, CT-CPP performs better in all five scenes.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_5.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
