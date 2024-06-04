---
layout: page
title: Coverage Path Planning in Unknown Environments
description:
img: assets/img/CPP_1.jpg
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

## 3D Coverage Path Planning Under Sensing Limitations

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

**Related Papers:**
- Z. Shen, J. Song, K. Mittal, and S. Gupta, “CT-CPP: Coverage path planning for 3D terrain reconstruction using dynamic coverage trees,” IEEE Robotics and Automation Letters, vol. 7, no. 1, pp. 135–142, Jan. 2022. [<b><a href="https://ieeexplore.ieee.org/abstract/document/9573264">Paper</a></b>][<b><a href="https://drive.google.com/file/d/1XSsm64LvsbXlODqu2VYrISYYgJDlc3WC/view">Slide</a></b>]
- Z. Shen, J. Song, K. Mittal, and S. Gupta, “Autonomous 3-D mapping and safe-path planning for underwater terrain reconstruction using multi-level coverage trees,” in Proc. OCEANS'17 MTS/IEEE, Anchorage, AK, USA, Sep. 2017, pp. 1–6. [<b><a href="https://ieeexplore.ieee.org/document/8232157">Paper</a></b>]
- Z. Shen, J. Song, K. Mittal, and S. Gupta, “An autonomous integrated system for 3-d underwater terrain map reconstruction,” in Proc. OCEANS'16 MTS/IEEE, Monterey, CA, USA, Sep. 2016, pp. 1–6. [<b><a href="https://ieeexplore.ieee.org/abstract/document/7761067">Paper</a></b>]

## Coverage Path Planning using Neural Network

**Motivation:** CPP in known environments is an application of the traveling salesman problem (TSP), in that the optimal CPP trajectory is also an optimal TSP tour. Finding the optimal solution, however, is NP-Hard. While many heuristics exist, these methods are either: i) sub-optimal, or ii) not suitable for online computation. Many robot missions are dynamic in nature, requiring fast online coverage path replanning in response to changes in the environment, robot, or mission objectives. Thus, it is essential to develop a coverage path planner that can quickly find near-optimal tours under dynamic conditions.

**Method Overview:** we proposed a deep learning-based CPP algorithm, called Coverage Path Planning Network (CPPNet), which can quickly find the near-optimal coverage path in complex environments. CPPNet is built using a convolutional neural network (CNN), whose input is a graph-based representation of the occupancy grid map, and whose output is an edge probability heat graph, where the value of each edge is the probability of belonging to the optimal TSP tour. A greedy search can then be performed over this heat graph to rapidly produce a nearoptimal TSP tour in an environment of arbitrary complexity. CPPNet can be invoked on-demand to generate a near-optimal coverage path for the environment with the most up-to-date information. The advantages of CPPNet are as follows: it i) produces near-optimal coverage trajectories, ii) efficiently works in environments with complex shapes, and iii) is computationally efficient for real-time implementation.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_6.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<p align="center" style="font-size:0.9rem;">
    Figure 4: The training and online architecture of CPPNet.
</p>

**Results:** Figure 5 presents the box plots of the trajectory lengths and online inference times of CPPNet and [2-opt](https://en.wikipedia.org/wiki/2-opt) solutions over 1300 scenarios. In general, CPPNet is able to generate trajectories that are within 5-27% of the 2-opt solution; however, CPPNet requires several orders of magnitude less computation time, thus making it suitable for online coverage path planning in dynamic or partially unknown environments.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_7.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<p align="center" style="font-size:0.9rem;">
    Figure 5: Performance evaluation for CPPNet as compared to 2-opt.
</p>

**Related Paper:**
- Z. Shen, P. Agrawal, J. P. Wilson, R. Harvey, and S. Gupta, “CPPNet: A coverage path planning network,” in Proc. OCEANS'21 MTS/IEEE, San Diego, CA, USA, Sep. 2021, pp. 1–5. [<b><a href="https://ieeexplore.ieee.org/abstract/document/9705671">Paper</a></b>]

## Coverage Path Planning Under Energy Constraints

**Motivation:** In the real-life situations, autonoumous robots are energy-constrained due to the limited size of batteries; thus limiting their duration of operation. Especially to cover a large area, it may not be able to finish the task in one iteration. Therefore, while executing a coverage trajectory, the robot has to return to the charging station for a recharge before the battery runs out and then contiunous the work. While a variety of CPP methods are available in literature, only a limited body of work has focused on energy-constrained CPP problem.

**Method Overview:** We proposed a 2D CPP algorithm for complete coverage of unknown environments using energy-constrained vehicles. During coverage, the vehicle detects unknown obstacles for dynamic map-building, while continuously monitoring the remaining energy so that it has sufficient energy for returning to the charging station when its battery runs low. Upon battery depletion, the vehicle retreats back to the charging station and after recharging it advances to a nearby cell to restart the coverage of the remaining uncovered area. The advantages of the proposed algorithm are as follows: i) produces easy-to-follow and user-desired back-and-forth coverage trajectories, ii) chooses a nearby unexplored cell as the new start point after each recharging to avoid longer travel to the cell from where it retreated back, iii) works in unknown environments, and iv) computationally efficient for real-time implementation.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_8.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<p align="center" style="font-size:0.9rem;">
    Figure 6: An example illustrating the execution of the proposed algorithm in an environment containing unknown obstacles.
</p>

**Results:** The performance of the proposed algorithm is validated on a high-fidelity simulator called [Player/Stage](https://playerstage.sourceforge.net/), which is a software tool for visualization and simulation of the robotic tasks. In this work, the vehicle is considered to have the total energy capacity of 320 units. It is equipped with a sonar sensor with a maximum range of 5m for obstacle detection. Two complex scenarios of 50m × 50m search areas containing multiple obstacles are used for testing and validation. Each of these scenarios is partitioned into a 50 × 50 tiling structure, where each cell is of size 1m × 1m. Figure 7 shows the results of the proposed algorithm for two different scenarios. In both scenarios, the battery charging station is located at the bottom left corner. The two figures show the vehicle’s coverage trajectory at different iterations. Finally, complete coverage of the whole area is achieved, thus proving the effectiveness of the proposed algorithm.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_9.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_10.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<p align="center" style="font-size:0.9rem;">
    Figure 7: The vehicle trajectory showing coverage process of the search area.
</p>

**Related Paper:**
- Z. Shen, J. P. Wilson, and S. Gupta, “ε*+: An online coverage path planning algorithm for energy-constrained autonomous vehicles,” in Proc. OCEANS'20 MTS/IEEE, Gulf Coast, MS, USA, Oct. 2020, pp. 1–6. [<b><a href="https://ieeexplore.ieee.org/document/9389353">Paper</a></b>]

## 2D Coverage Path Planning Algorithm
Coming soon...
