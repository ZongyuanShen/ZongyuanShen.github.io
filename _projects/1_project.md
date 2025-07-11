---
layout: page
title: Online Coverage Path Planning
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

## Topic 1: Hierarchical Coverage Path Planning

**Z. Shen**, B. Shirose, P. Sriganesh, and M.Travers, “CAP: A Connectivity-Aware Hierarchical Coverage Path Planning Algorithm for Unknown Environments using Coverage Guidance Graph,” in IEEE/RSJ International Conference on Intelligent Robots and Systems **(IROS)**, 2025. [<b><a href="http://arxiv.org/abs/2503.00647">Paper</a></b>][<b><a href="https://youtu.be/1pH-PkcRVZg">Video</a></b>]

**Overview:**
Existing CPP methods generate well-defined coverage patterns and ensure complete coverage in finite time, but prioritize immediate cost minimization, not considering a global perspective of the entire search area. These myopic decisions result in inefficient global coverage paths. In this regard , we introduce CAP, a connectivity-aware hierarchical coverage path planning algorithm for efficient coverage of unknown environments. CAP incrementally constructs a coverage guidance graph to capture the global topological structure of the environment. Based on the updated graph, the hierarchical planner determines an efficient path to maximize global coverage efficiency and minimize local coverage time.

<center>
<iframe width="825.6" height="464.4"
src="https://github.com/user-attachments/assets/4cfec916-dfb9-4a24-9ff1-0c33120e637c">
</iframe>
</center>

## Topic 2: 3D Underwater Terrain Mapping

**Z. Shen**, J. Song, K. Mittal, and S. Gupta, “CT-CPP: Coverage path planning for 3D terrain reconstruction using dynamic coverage trees,” **IEEE Robotics and Automation Letters**, vol. 7, no. 1, pp. 135–142, Jan. 2022. [<b><a href="https://ieeexplore.ieee.org/abstract/document/9573264">Paper</a></b>]

**Overview:**
2D CPP methods are rendered insufficient for applications involving 3D surfaces. In this regard, we introduce CT-CPP algorithm for 3D underwater terrain mapping. CT-CPP performs layered scanning of the 3D region to collect terrain data guided by a coverage tree. Once the robot reaches a node, it covers the corresponding subregion and collects data for the 3D terrain structures below it. Then, the robot projects the information about obstacles intersecting the plane below by forming a 2D occupancy map, which is used to identify the disconnected subregions on that plane. i.e., it adds child nodes to the tree. The updated tree is then used to plan an optimal tree traversal sequence.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_4.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_5.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
