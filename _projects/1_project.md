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

Now, I am working on the online CPP problem in a number of scenarios as listed below.
- Fundamental 2D CPP algorithm which works directly in continuous space, adapts the path pattern _in situ_ based on sensor information, guarantees complete coverage, computation and memory efficiency. (In production)
- Extend existing 2D CPP method to curvature constrained robot, energy-constrained robot, and machine learning based framework. (Done)
- 3D coverage path planning problem for complete coverage of unknown obstacle-rich environments using autonomous robot with limited sensing range. (Done)

### 3D Coverage Path Planning
**Background:** Existing CPP approaches are of two types: 2D and 3D. While 2D approaches are applied for tasking on 2D surfaces (e.g., floor cleaning and lawn mowing), they are rendered insufficient for applications involving 3D surfaces. For example, a 2D CPP method can be applied for mapping a 3D underwater terrain by operating an autonomous underwater vehicle (AUV) at a fixed depth, such that the side-scan sonar can scan the seabed. However, this approach will be unable to explore the regions above the AUV, thus generating an incomplete terrain map. On the other hand, if the AUV is operated at a higher level, then the sensors will be unable to scan the terrain due to their limited field of view (FOV). Therefore, a 3D CPP method is needed for 3D terrain mapping.

**Method Overview:** We proposed the 3D coverage strategy using multi-level coverage trees, as shown below. The algorithm guides the AUV to scan layer by layer in a top-down manner, while it dynamically constructs a coverage tree to record the search progress. Each node in the coverage tree refers to a sub-region on a horizontal surface; initially, the tree contains a single root which is the highest layer. When the AUV is searching within a node using the ε* algorithm, it uses its multi-beam sensor to scan the environment beneath it and identifies the "child nodes" and addes them to the existing coverage tree. As it completes searching the current node, it calls TSP-optimized node traversal strategy to compute for the next region to explore. The coverage is completed when no more nodes are unexplored in the tree. Then, the multi-beam sensor measurements, which constitute a 3D point cloud, are used to reconstruct the surface using the Alpha-Shape algorithm.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_4.svg" title="example image" class="img-fluid" %}
    </div>
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/CPP_6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 2: Incremental construction of the coverage tree.
</div>
