---
layout: page
title: Lane Following & Sign Detection
description: Camera-Based Lane Following with Pure Pursuit and YOLO Stop/Slow Sign Detection on the GEM e4 · ECE 484, UIUC
img: assets/img/projects/ece484.jpg
importance: 6
category: work
---

**Course:** ECE 484 Principles of Safe Autonomy, University of Illinois Urbana-Champaign · Fall 2024<br>
**Team:** GEM 04 (Team Combat): Dev Sanjay Sakaria, Jason Mei, Pallaw Kumar<br>
**Platform:** GEM e4 autonomous electric vehicle, ROS

The goal was a real-time autonomous driving system for the **GEM e4** research vehicle that could **follow lanes** around the Highbay track using only a camera, and **respond to STOP and SLOW signs**, with safety as the first priority in a low-speed environment.

## System Architecture

<div class="mt-3 mb-3">
  {% include figure.liquid loading="lazy" path="assets/img/projects/ece484-architecture.jpg" title="System architecture" class="img-fluid rounded z-depth-1" %}
</div>
<div class="caption">
    ROS architecture: the ZED2 camera feeds both lane detection and YOLO sign detection, whose outputs drive the vehicle controller through PACMod.
</div>

The **ZED2 camera** image is processed by two parallel pipelines. **Lane detection** publishes the lane curvature and curve direction, and **YOLO detection** publishes any detected signs. The **vehicle controller** turns these into a desired steering angle and speed, which are sent to the car's drive-by-wire system through **PACMod**.

## Lane Detection

<div class="row mt-3" style="margin-left: 0; margin-right: 0;">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/ece484-binary.jpg" title="Binary lane image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/ece484-lanefit.jpg" title="Lane fit" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
    Binary image after colour and gradient filtering (left), and the polynomial lane fit in the bird's-eye view (right).
</div>

Each frame goes through **HLS colour filtering** and **gradient filtering** to produce a binary image of the lane markings. A **perspective transform** then creates a bird's-eye view, and a second-order polynomial is fitted to each lane line. The fit gives the lane's **curvature** and whether the road curves **left or right**.

## Control and Sign Detection

- **Pure Pursuit steering:** the controller picks a look-ahead point on the detected lane and steers the vehicle towards it, giving smooth tracking around curves.
- **YOLO sign detection:** a real-time **YOLO** single-stage detector recognizes STOP and SLOW signs (including flagger signs) in the camera feed, so the controller can stop or slow the vehicle.

## Outcome

Lane detection with curvature estimation and YOLO sign detection were completed and tested on recorded and live data from the car. Tuning of the controller and full integration on the GEM e4 were still in progress at the end of the semester.

## My Contributions

I worked on the **lane detection pipeline**, the **Pure Pursuit controller** and their ROS integration, and shared vehicle testing with the team.

- **Lane detection:** built the OpenCV pipeline. That meant tuning the HLS colour and gradient thresholds so the white and yellow lines stayed visible under Highbay lighting and outdoor glare, then calibrating the perspective transform for the bird's-eye view and implementing the sliding-window polynomial lane fit.
- **Curvature and direction estimation:** implemented the radius-of-curvature calculation and left/right curve detection, and published both as ROS topics (`lane_detection/curvature`, `lane_detection/curve_direction`) for the controller.
- **Pure Pursuit controller:** implemented the lateral controller that converts the detected lane into a look-ahead target and a steering command ($$\delta = \arctan(L/R)$$), tuned the look-ahead distance and speed for smooth tracking on curves, and linked it to the sign-detection output so the car slows or stops for SLOW and STOP signs. Commands go to the vehicle through PACMod.
- **ROS integration and debugging:** subscribed to the ZED2 image stream, visualized the lane fit and detections in RViz, and debugged the pipeline on recorded rosbags before running it on the car.
- **Testing on the GEM e4:** took part in on-vehicle test sessions, re-tuning thresholds as lighting changed and tuning the controller gains and checking the steering response against the detected lane.

## Videos

<div class="row mt-3" style="margin-left: 0; margin-right: 0;">
  <div class="col-sm mt-3 mt-md-0">
    <div style="position: relative; width: 100%; padding-top: 56.25%;">
      <iframe src="https://drive.google.com/file/d/1TOwfJjhc0EB-UiodbHO5K5hnGZ4JIGC3/preview" style="position: absolute; inset: 0; width: 100%; height: 100%; border: 0;" class="rounded z-depth-1" allow="autoplay; fullscreen" allowfullscreen loading="lazy" title="Lane detection"></iframe>
    </div>
    <div class="caption">Lane detection.</div>
  </div>
  <div class="col-sm mt-3 mt-md-0">
    <div style="position: relative; width: 100%; padding-top: 56.25%;">
      <iframe src="https://drive.google.com/file/d/1SAy9d9A18S7yIplXt48vNIsV5xc3vcvp/preview" style="position: absolute; inset: 0; width: 100%; height: 100%; border: 0;" class="rounded z-depth-1" allow="autoplay; fullscreen" allowfullscreen loading="lazy" title="Final result"></iframe>
    </div>
    <div class="caption">Final result: live lane fit and curve direction in ROS/RViz.</div>
  </div>
</div>

## Technical Stack

- **Perception:** OpenCV (HLS colour and gradient thresholding, perspective transform, polynomial lane fitting), YOLO
- **Control:** Pure Pursuit lateral control, speed control
- **Software:** Python, ROS, RViz
- **Vehicle:** GEM e4, ZED2 stereo camera, PACMod drive-by-wire
