---
layout: page
title: State Estimation & Calibration
description: LiDAR–Camera Calibration and GPS-Free, Map-Based Localization for an Autonomous Vehicle · CS 588, UIUC
img: assets/img/projects/cs588.jpg
importance: 5
category: work
---

**Course:** CS 588 Autonomous Vehicle System Engineering, University of Illinois Urbana-Champaign · Spring 2025<br>
**Team:** Michal Juscinski (lead), Renjie Sun, Dev Sakaria<br>
**Platform:** GEM e2 autonomous electric vehicle (GEMstack)

The class builds one shared autonomy stack for a full-size electric research vehicle, with each team owning one part. Our team owned **sensor calibration and state estimation**. We had two goals: give the perception teams accurate LiDAR–camera calibrations, and let the car **localize without GPS**, for example inside the Highbay garage where there is no satellite signal.

## LiDAR–Camera Calibration

<div class="row mt-3" style="margin-left: 0; margin-right: 0;">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/cs588-checkerboard.jpg" title="LiDAR points projected onto a camera image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/cs588-tuning.jpg" title="Manual fine-tuning of the extrinsics" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
    LiDAR points projected onto a corner camera image (left), and the tool for fine-tuning the six extrinsic parameters (right).
</div>

We calibrated camera intrinsics offline and removed lens distortion. We then computed the LiDAR-to-camera extrinsics with **Perspective-n-Point** from checkerboard captures and fine-tuned them by hand. To measure quality objectively, we marked strong features such as building edges in both the LiDAR and the image, then scored their alignment with **Intersection over Union** and **Hausdorff distance**.

| Calibration round | IoU | Hausdorff distance |
| :-- | :-: | :-: |
| Preliminary | 68.9% | 38.35 px |
| First (rectified images) | 90.5% | 19.52 px |
| **Final** | **93.8%** | – |

The final calibration pipeline, covering the corner and front cameras, was submitted to the shared GEMstack codebase for the other teams to use.

## Map-Based Localization Without GPS

<div class="mt-3 mb-3">
  {% include figure.liquid loading="lazy" path="assets/img/projects/cs588-map.jpg" title="LiDAR map of Highbay" class="img-fluid rounded z-depth-1" %}
</div>
<div class="caption">
    Geotagged LiDAR map of the Highbay garage and part of the parking lot.
</div>

Using a geotagged LiDAR map of the garage and parking lot (built with the Automated Inspection team), the estimator aligns each live LiDAR scan to the map with **Iterative Closest Point (ICP)**. This recovers the car's global position even with no GNSS signal. Measured against the Septentrio GNSS receiver outdoors, the second version reduced the average error from 13 m to **11 m** (**2.9 m inside the garage**) and the update time from 2.96 s to **1.89 s**. We also designed a hybrid estimator to blend GNSS and map-based estimates when both are available.

## My Contributions

- **Designed and implemented the first version of the map-based state estimator**, the ICP scan-to-map localization that the team then iterated on.
- **Calculated the calibration metrics** (IoU and Hausdorff distance) used to evaluate and improve each round of calibration.
- **Collected data** on the vehicle and served as the **safety driver** during data collection and testing.

## Technical Stack

- **Calibration:** Camera intrinsics, Perspective-n-Point, LiDAR-to-camera extrinsics, IoU / Hausdorff evaluation, OpenCV
- **Localization:** LiDAR mapping, Iterative Closest Point, GNSS (Septentrio), coordinate conversion to a local metric frame
- **Software:** Python, ROS, GEMstack
- **Vehicle:** GEM e2 with LiDAR, corner and OAK cameras, GNSS
