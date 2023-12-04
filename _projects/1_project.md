---
layout: page
title: Online Coverage Path Planning
description:
img: assets/img/12.jpg
importance: 1
category: work
related_publications: 
---

### Introduction
Typical operations of autonomous vehicles such as structural inspection, floor cleaning, lawn mowing, terrain mapping, oil spill cleaning, arable farming, usually require covering of all points in the search area, i.e., _Coverage Path Planning_ (CPP). Often, these operations are performed in either completely unknown or partially known environments where the information on the exact geometrical shapes and locations, of obstacles and map boundaries, may be incorrect or incomplete. Therefore, it is essential to utilize online (i.e., sensor-based) CPP methods that can dynamically unfold the environment via discovering unknown obstacles on the path; thus facilitating online trajectory adaptation, obstacle avoidance, mapping boundaries, and complete coverage.

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
<div class="caption">
    Figure 1: Applications of coverage path planning. On the left, underwater terrain mapping. Middle, arable farming. Right, floor cleaning.
</div>

Now, I am working on the online coverage path planning problem in a number of scenarios as listed below.
- Fundamental 2D coverage path planning algorithm which works directly in continuous space, adapts the path pattern _in situ_ based on sensor information, guarantees complete coverage, computation and memory efficiency. (In production)
- 2D coverage path planning algorithm for energy-constrained autonomous robot. (In production)
- Deep-learning based coverage path planning algorithm. (Done)
- 3D coverage path planning algorithm for complete coverage of unknown obstacle-rich environments using autonomous robot with limited sensing range. (Done)

### 3D Coverage Path Planning

**Motivation:** Existing CPP approaches are of two types: 2D and 3D. While 2D approaches are applied for tasking on 2D surfaces (e.g., floor cleaning and lawn mowing), they are rendered insufficient for applications involving 3D surfaces. For example, a 2D CPP method can be applied for mapping a 3D underwater terrain by operating an autonomous underwater vehicle (AUV) at a fixed depth, such that the side-scan sonar can scan the seabed. However, this approach will be unable to explore the regions above the AUV, thus generating an incomplete terrain map. On the other hand, if the AUV is operated at a higher level, then the sensors will be unable to scan the terrain due to their limited field of view (FOV). Therefore, a 3D CPP method is needed for 3D terrain mapping.

**Method Overview:** We proposed the 3D coverage strategy using multi-level coverage trees, as shown below. The algorithm guides the AUV to scan layer by layer in a top-down manner, while it dynamically constructs a coverage tree to record the search progress. Each node in the coverage tree refers to a sub-region on a horizontal surface; initially, the tree contains a single root which is the highest layer. When the AUV is searching within a node using the ε* algorithm, it uses its multi-beam sensor to scan the environment beneath it and identifies the "child nodes" and addes them to the existing coverage tree. As it completes searching the current node, it calls TSP-optimized node traversal strategy to compute for the next region to explore. The coverage is completed when no more nodes are unexplored in the tree. Then, the multi-beam sensor measurements, which constitute a 3D point cloud, are used to reconstruct the surface using the Alpha-Shape algorithm.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_4.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 2: Incremental construction of the coverage tree.
</div>

**Results:** We have validated the algorithm on the Robot Operating System (ROS) with Underwater Simulator (UWSim). The results are shown below.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_5.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 3: Performance evaluation for CT-CPP as compared to TF-CPP.
</div>

**Related Papers:**
- Z. Shen, J. Song, K. Mittal, and S. Gupta, “CT-CPP: Coverage path planning for 3D terrain reconstruction using dynamic coverage trees,” IEEE Robotics and Automation Letters, vol. 7, no. 1, pp. 135–142, Jan. 2022.
- Z. Shen, J. Song, K. Mittal, and S. Gupta, “Autonomous 3-D mapping and safe-path planning for underwater terrain reconstruction using multi-level coverage trees,” in Proc. OCEANS'17 MTS/IEEE, Anchorage, AK, USA, Sep. 2017, pp. 1–6.
- Z. Shen, J. Song, K. Mittal, and S. Gupta, “An autonomous integrated system for 3-d underwater terrain map reconstruction,” in Proc. OCEANS'16 MTS/IEEE, Monterey, CA, USA, Sep. 2016, pp. 1–6.

### Deep-learning based Coverage Path Planning:

**Motivation:** CPP in known environments is an application of the traveling salesman problem (TSP), in that the optimal CPP trajectory is also an optimal TSP tour. Finding the optimal solution, however, is NP-Hard. While many heuristics exist, these methods are either: i) sub-optimal, or ii) not suitable for online computation. Many robot missions are dynamic in nature, requiring fast online coverage path replanning in response to changes in the environment, robot, or mission objectives. As such, it is of critical importance to develop a coverage path planner that can quickly find near-optimal tours under dynamic conditions.

**Method Overview:** we developed a deep learning-based algorithm, called Coverage Path Planning Network (CPPNet), which can quickly find the near-optimal coverage path in complex environments. CPPNet is built using a convolutional neural network (CNN), whose input is a graph-based representation of the occupancy grid map, and whose output is an edge probability heat graph, where the value of each edge is the probability of belonging to the optimal TSP tour. A greedy search can then be performed over this heat graph to rapidly produce a nearoptimal TSP tour in an environment of arbitrary complexity. CPPNet can be invoked on-demand to generate a near-optimal coverage path for the environment with the most up-to-date information. The advantages of CPPNet are as follows: it i) produces near-optimal coverage trajectories, ii) efficiently works in environments with complex shapes, and iii) is computationally efficient for real-time implementation.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_6.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 4: The training and online architecture of CPPNet.
</div>

**Results:** Figure 5 presents the box plots of the trajectory lengths (left) and online inference times (right) of the 2-opt and CPPNet solutions over all testing scenarios. In general, CPPNet is able to generate trajectories that are within 5-27% of the 2-opt solution; however, CPPNet requires several orders of magnitude less computation time, thus making it suitable for online coverage path planning in dynamic or partially unknown environments.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_7.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 5: Performance evaluation for CPPNet as compared to 2-opt TSP solver.
</div>

**Related Paper:**
- Z. Shen, P. Agrawal, J. P. Wilson, R. Harvey, and S. Gupta, “CPPNet: A coverage path planning network,” in Proc. OCEANS'21 MTS/IEEE, San Diego, CA, USA, Sep. 2021, pp. 1–5.
