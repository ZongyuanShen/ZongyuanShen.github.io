---
layout: page
title: Active Path Replanning
description:
img: assets/img/4.jpg
importance: 1
category: work
related_publications: 
---

## Motivation
With the rapid development of artificial intelligence, the application of robots is gradually shifting from structured environments to dynamic and shared environments. Consider for instance a self-driving car navigating through urban traffic, or a service robot planning a path while avoiding other agents such as pedestrians. In all these domains, it is essential that the robots can safely operate in dynamic environments while replanning in real-time as needed to achieve : 1) high success rates and 2) low travel times. A variety of replanning algorithms exist in literature. One strategy is to plan an initial path and invoke re-planning whenever its execution becomes infeasible based on the current information, which is known as reactive replanning strategy. This method is suitable to the crowded environments where the future states of dynamics obstacles are difficult to compute, associate and predict. However, it will suffer from time inefficiency (frequent replanning) and path inefficiency (oscillating movements and detours) due to the absence of predicted information of dynamic obstacles. Furthermore, re-planning strategies may fail in the presence of robot deadlocks. Therefore, active replanning strategy predicts trajectories of moving obstacles and generate a new plan which can actively avoid collision to the moving obstacles in the future.

## Method Overview
The related paper is in production.
