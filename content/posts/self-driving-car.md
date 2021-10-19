---
title: "Self Driving Car"
date: 2021-09-03T22:44:41+08:00
weight: 1
draft: false
categories: AI
tags:
  - self driving car
  - computer vision
  - learning...
# bookComments: false
---

# Intro

## Reading

### SAE J3016: Autonomous Car Level

source [1]({{< relref "#ref" >}})

0. all human
1. adaptive cruise control, anti-lock braking system, electronic stabilty control
2. hazard-minimizing longitudinal/ lateral control, emergency braking
3. full autonomy under certain condition, but human take control when in danger
4. full autonomy under certain condition, can replace human at some cases
5. fully autonomous



### Decision making

- On-board sensors
  - LIDAR
  - camera
  - GPS/INS
  - odometry(angle)

- Hierachical Decision Making System
  ![this](/general/slc_pic1.png)

1. Route Planning

   - Dijkstra
   - A*

2. Behavioral Layer: local driving task under rules and planned route

   - Input
     - other traffic participants
     - road conditions
     - signals from infrastructure
   - driving manual -> finite sets of actions -> finite state machine
   - uncertainty of behaviour -> predictive planning/ probabilistic planning formalisms
     - Probabilistic planning formalism : Markov Decision Process(MDP)

3. Motion Planning

   - cruise-in-lane, change-lane, turn-right -> continous path through the environment
   - static and dynamic obstacles -> collision-free trajectory <- travel time + discomfort

   1. Path planning ==Table of Algos==
      - Variational methods
        - rapid convergence to local optimum
      - Graph-search methods
        - not easy to local minima
        - but only a finite set of optimal path
      - Incremental search methods
        - build tree with nodes growing
        - until reach the goal
   2. Trajectory planning
      - TP in dynamic env = generalized PP in static env
      - ...

4. Control system: correct errors of motion planning

# Reference {#ref}
[1] Paden, Brian, et al. "A survey of motion planning and control techniques for self-driving urban vehicles." _IEEE Transactions on intelligent vehicles_ 1.1 (2016): 33-55. 

[2]Urmson, Chris, et al. "Autonomous driving in urban environments: Boss and the urban challenge." _Journal of Field Robotics_ 25.8 (2008): 425-466.