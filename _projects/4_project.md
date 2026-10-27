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

<div class="row mt-3" style="margin-left: 0; margin-right: 0;">
  <div class="col-sm mt-3 mt-md-0">
    <p>As a robotics intern on a four-person team, I worked on the localization side of the stack: getting camera-based pose estimates into ROS, fusing them with the robots' own sensors, and proving through experiments that the fusion helped.</p>
    <ul>
      <li><strong>ROS integration:</strong> ran YOLOv8-pose and PnP + RANSAC on the camera feeds and published each robot's 6D pose with covariance as ROS messages.</li>
      <li><strong>Localization pipeline:</strong> built EKF fusion (robot_localization) of camera poses with IMU, wheel odometry, LiDAR and depth camera, feeding RTAB-Map SLAM.</li>
      <li><strong>Tuning:</strong> tuned filter noise and sensor covariances, improving localization accuracy by about <strong>39%</strong>.</li>
      <li><strong>Experiments and validation:</strong> tested in Gazebo and Isaac Sim, then on the robots against ground truth, and documented the results.</li>
    </ul>
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/amr-keypoints.jpg" title="Keypoint detection" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Keypoint detection on the Volta.</div>
  </div>
</div>

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
