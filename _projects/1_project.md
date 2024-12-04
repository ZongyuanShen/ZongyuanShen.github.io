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
Typical operations of autonomous vehicles such as terrain mapping, arable farming, floor cleaning, structural inspection, lawn mowing, oil spill cleaning, humanitarian demining, usually require covering of all points in the search area, i.e., _Coverage Path Planning_ (CPP). Often, these operations are performed in either completely unknown or partially known environments where the information on the exact geometrical shapes and locations, of obstacles and map boundaries, may be incorrect or incomplete. Therefore, it is essential to utilize online (i.e., sensor-based) CPP methods that can dynamically unfold the environment via discovering unknown obstacles on the path; thus facilitating online trajectory adaptation, obstacle avoidance, mapping boundaries, and complete coverage.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
        <p align="center" style="font-size:0.9rem;">
            (a) Seabed mapping.
        </p>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_2.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
        <p align="center" style="font-size:0.9rem;">
            (b) Arable farming.
        </p>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
        <p align="center" style="font-size:0.9rem;">
            (c) Floor cleaning.
        </p>
    </div>
</div>
<p align="center" style="font-size:0.9rem;">
    Figure 1: Applications of coverage path planning algorithms.
</p>

## 3D Coverage Path Planning

**Motivation:**
Existing CPP methods are of two types: 2D and 3D. While 2D methods are applied for tasking on 2D surfaces (e.g., floor cleaning and lawn mowing), they are rendered insufficient for applications involving 3D surfaces. For example, a 2D CPP method can be applied for mapping a 3D underwater terrain by operating an autonomous underwater vehicle (AUV) at a fixed depth, such that the side-scan sonar can scan the seabed. However, this method will be unable to explore the regions above the AUV, thus generating an incomplete terrain map. On the other hand, if the AUV is operated at a higher level, then the sensors will be unable to scan the terrain due to their limited field of view. Therefore, a 3D CPP method is needed for 3D terrain mapping.

**Method Overview:**
We proposed a 3D CPP algorithm, called CT-CPP, using a coverage tree (CT), where the nodes (except the root node) of CT represent the disconnected subregions, as shown in Figure 2. CT-CPP incrementally builds a CT in a top-down manner, which is used to plan and track the progress of 3D coverage. 

Once the AUV reaches a node, it covers the corresponding subregion using a [2D CPP algorithm](https://ieeexplore.ieee.org/abstract/document/8286947). During the coverage of each planar subregion, the AUV uses the downward-facing multi-beam sonar sensor to collect data for the 3D terrain structures that are within the sensor’s range extended at least up to the plane below. Based on the data, the AUV projects and stores the information about obstacles intersecting the plane below by forming a 2D probabilistic occupancy map [(POM)](https://mitpress.mit.edu/9780262201629/probabilistic-robotics/). Since the underwater terrain may contain narrow regions which can be risky for the AUV to navigate and avoid collisions, an image morphological operator [‘closing’](https://link.springer.com/book/10.1007/978-3-662-05088-0) is applied on the POM to close the narrow areas. Subsequently, a symbolic map is obtained such that the cells whose occupancy probability is higher than the threat probability are marked as threat, while the others are marked as safe. This serves two purposes: i) the updated symbolic map is used to identify the disconnected subregions on that plane. i.e., it adds child nodes to the CT, and ii) it ensures the AUV’s safety when it navigates on that plane.

The updated tree is then used to plan the AUV trajectory by generating an optimized tree traversal sequence using a heuristics-based solution to the traveling salesman problem. The above process continues until the AUV completes the coverage of all nodes of the tree and no new nodes are created. Then, the data collected from all nodes are integrated offline for complete 3D terrain reconstruction using [α-Shape algorithm](https://en.wikipedia.org/wiki/Alpha_shape).

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_4.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<p align="center" style="font-size:0.9rem;">
    Figure 2: Incremental construction of the coverage tree.
</p>

**Results:**
The CT-CPP algorithm is validated on a high-fidelity underwater simulator called [UWSim](https://wiki.ros.org/uwsim) based on the [Robot Operating System](https://www.ros.org/). The AUV, called Girona 500, is simulated. Five scenes of size 450m × 450m × 400m are randomly generated for performance validation as shown in Figure 3(a). A comparative evaluation of the coverage trajectories generated by the CT-CPP and the terrain following CPP [(TF-CPP)](https://link.springer.com/article/10.1007/BF00141150) methods in scene 1 is presented in Figure 3(b) and Figure 3(c), respectively. Figure 3(f) shows the total trajectory lengths for all five scenes, which indicate that the CT-CPP method took the shorter path lengths. Figure 3(g) shows that the CT-CPP method consumed less energy in all five scenes as compared to the TF-CPP method due to its frequent vertical motions. Further, we numerically evaluated the reconstruction errors using different α, which is a positive parameter controlling the desired level of details. As seen in Figure 3(h), CT-CPP performs better in all five scenes. Thus, CT-CPP is an alternate method to TF-CPP and it is expected to outperform the TF-CPP method in a significant number of scenarios.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_5.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<p align="center" style="font-size:0.9rem;">
    Figure 3: Performance evaluation for CT-CPP as compared to TF-CPP.
</p>

**Related Paper:**
- Z. Shen, J. Song, K. Mittal, and S. Gupta, “CT-CPP: Coverage path planning for 3D terrain reconstruction using dynamic coverage trees,” IEEE Robotics and Automation Letters, vol. 7, no. 1, pp. 135–142, Jan. 2022. [<b><a href="https://ieeexplore.ieee.org/abstract/document/9573264">Paper</a></b>][<b><a href="https://drive.google.com/file/d/1XSsm64LvsbXlODqu2VYrISYYgJDlc3WC/view">Slide</a></b>]

## 2D Coverage Path Planning
Coming soon...
