---
layout: page
title: Multi-Robot Localization
description: Leveraging Fixed Infrastructure Cameras for Collaborative Multi-View Perception of Indoor Autonomous Mobile Robots
img: assets/img/projects/amr.jpg
importance: 4
category: work
---

**Team:** Dharini Raghavan, Raviteja Upadrashta, Prof. Bharadwaj Amrutur<br>
**Organization:** Robert Bosch Center for Cyber-Physical Systems (RBCCPS), Indian Institute of Science, Bengaluru<br>
**Role:** Robotics Intern · Jan – Jun 2024<br>

Indoor mobile robots usually localize with LiDAR, which struggles in sparse, feature-poor spaces, while on-board cameras only see what is directly in front of them. Most warehouses and labs already have **fixed security (CCTV) cameras** that see the whole floor. This project turns those cameras into a shared perception sensor for a fleet of **Volta autonomous mobile robots**, fusing their view with each robot's own LiDAR, IMU, wheel odometry and depth camera, with the heavy computation running on **Qualcomm RB5** processors at the edge and on board.

## System Overview

<div class="row mt-3" style="margin-left: 0; margin-right: 0;">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/amr-volta.jpg" title="Volta AMR" class="img-fluid rounded" %}
    <div class="caption">The Volta autonomous mobile robot.</div>
  </div>
  <div class="col-sm mt-3 mt-md-0">
    <p>Infrastructure cameras, placed by a coverage-and-accuracy optimizer, stream to an <strong>RB5 edge server</strong> that estimates each robot's <strong>6D pose and uncertainty</strong>, tracks every robot with a Kalman filter, and associates measurements with the right robot.</p>
    <p>Over a WiFi 6 / 5G link, the associated measurements reach an <strong>RB5 on each robot</strong>, which fuses them with on-board sensors in an EKF and runs SLAM, giving better localization, mapping and obstacle awareness.</p>
  </div>
</div>

## My Contributions

As a robotics intern on a four-person team, I worked mainly on the localization side of the stack: getting camera-based pose estimates into ROS, fusing them with the robots' own sensors, and proving through experiments that the fusion actually helped. The team's overall approach used a YOLOv8-pose keypoint model trained on synthetic Unity data, with PnP to recover each robot's 6D pose and an uncertainty-aware camera placement method.

### Integrating Camera-Based Pose Estimation into ROS

<div class="row mt-3" style="margin-left: 0; margin-right: 0;">
  <div class="col-sm mt-3 mt-md-0">
    <p>I integrated the perception modules into the robot software: running the <strong>YOLOv8-pose</strong> keypoint detector on the infrastructure camera feeds, solving <strong>PnP with RANSAC in OpenCV</strong> from the 2D–3D keypoint correspondences, and publishing each robot's <strong>6D pose with its covariance</strong> as ROS messages that the localization stack could consume.</p>
    <p>This meant working through the practical details: camera intrinsics, transforms between each camera, the world frame and the robot frames, and filtering out bad detections before they reached the filter.</p>
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/amr-keypoints.jpg" title="Keypoint detection" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Keypoint detection on the Volta.</div>
  </div>
</div>

### Building the Multi-Robot Localization Pipeline

I developed the **EKF-based sensor fusion** pipeline in ROS using **robot_localization**, combining infrastructure-camera poses with each robot's **IMU, wheel encoders, LiDAR and on-board depth camera**, with the fused estimate fed into **RTAB-Map** SLAM. I configured sensor covariances from datasheets and the camera's estimated uncertainty, and worked on associating camera measurements with the correct robot when several robots were in view (Kalman tracking with **Mahalanobis gating**).

### Tuning, Experiments and Validation

I ran the localization experiments that produced the results below, comparing every sensor on its own and in combination against ground truth along repeatable test trajectories. Systematic tuning of the filter (process and measurement noise) and the pose-estimation pipeline improved localization accuracy by about **39%**. I also tested the pipeline in simulation (**Gazebo, Isaac Sim**) before running it on the real robots, recorded the navigation demos, and documented the results for reproducibility.

## Results

**Pose estimation:** the keypoint-based method beats generalizable 6D pose estimators in accuracy and runs in 11 ms without RANSAC.

| Algorithm | ADD-0.1d (%) | Inference time (ms) |
| :-- | :-: | :-: |
| Gen6D | 45.49 | 634 |
| BundleTrack | 85.49 | 101 |
| BundleSDF | 82.21 | 96 |
| **Ours (RANSAC)** | **94.65** | 240 |
| **Ours (no RANSAC)** | 92.10 | **11** |

**Localization:** fusing infrastructure cameras (In) with the IMU (I), wheel encoders (W), LiDAR (L) and on-board camera (VO) gives the lowest error.

| Approach | MAE x (cm) | MAE y (cm) | MAE yaw (°) |
| :-- | :-: | :-: | :-: |
| Wheel encoders only | 2.288 | 1.094 | 1.260 |
| RP LiDAR only | 1.353 | 1.189 | 1.621 |
| On-board camera only | 1.111 | 1.483 | 1.901 |
| Infrastructure camera only | 0.932 | 0.863 | 1.301 |
| **I + W + L + VO + In** | **0.712** | **0.812** | **0.692** |

<div class="row mt-3" style="margin-left: 0; margin-right: 0;">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/amr-trajectory.png" title="Trajectory tracking" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/amr-mapping.png" title="Mapping improvement" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
    Left: estimated pose tracks ground truth along a test trajectory. Right: maps before and after fusing infrastructure cameras.
</div>

Overall, marker-less pose estimation improves localization by **39.9%** over the compared approaches, and fusing infrastructure cameras improves multi-robot SLAM performance by **35%**.

## Navigation Demo

Obstacles on the floor are hidden from the robot's on-board camera but visible to the infrastructure cameras.

<div class="row mt-3" style="margin-left: 0; margin-right: 0;">
  <div class="col-sm mt-3 mt-md-0">
    <video src="{{ '/assets/video/amr-nav-without-fusion.mp4' | relative_url }}" class="img-fluid rounded z-depth-1" controls muted playsinline preload="metadata"></video>
    <div class="caption">Without camera fusion: the robot hits the small obstacles.</div>
  </div>
  <div class="col-sm mt-3 mt-md-0">
    <video src="{{ '/assets/video/amr-nav-with-fusion.mp4' | relative_url }}" class="img-fluid rounded z-depth-1" controls muted playsinline preload="metadata"></video>
    <div class="caption">With camera fusion: the robot navigates around them cleanly.</div>
  </div>
</div>

## Technical Stack

- **Perception:** YOLOv8-pose keypoint detection, PnP + RANSAC, OpenCV, semantic segmentation, 6D object pose estimation
- **Estimation and fusion:** Kalman tracking, Mahalanobis gating, Hungarian algorithm, EKF (robot_localization), RTAB-Map SLAM
- **Compute and networking:** Qualcomm RB5 (edge and on-board), WiFi 6 / 5G
- **Data and simulation:** Unity 3D synthetic data, Fusion 360 CAD, Gazebo, Isaac Sim
- **Robotics:** ROS, Volta AMR, RP LiDAR, IMU, wheel encoders, depth camera, CCTV infrastructure cameras

<div class="caption mt-4">
    Supported by the AI &amp; Robotics Technology Park (ARTPARK) at IISc and Qualcomm India.
</div>
