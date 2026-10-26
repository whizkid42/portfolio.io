---
layout: page
title: Tele-Ultrasound Robot
description: Haptic Teleoperation of a Force-Controlled 7-DOF Manipulator for Remote Ultrasonography
img: assets/img/projects/ultrasound.jpg
importance: 1
category: work
---

<!-- TODO(Dev): add team members, e.g. **Team:** Name, Name, Name -->

**Organization:** Healthcare Technology Innovation Center (HTIC), IIT Madras Research Park, Chennai<br>
**Role:** Robotics Engineer · Jan 2026 – Sep 2026

An ultrasound scan is only as good as the hands holding the probe, and trained sonographers are scarce outside large hospitals. This project builds a **teleoperated, robot-assisted ultrasound system** in which an expert can scan a patient from a remote console. The sonographer moves a grounded haptic device; a 7-DOF robotic manipulator holding the probe reproduces that motion at the patient's side, while the contact force between probe and tissue is regulated by the robot and rendered back to the operator's hand.

My work focused on the control stack that makes this safe and usable: keeping the robot **compliant and stable in contact with soft tissue**, and letting the operator **feel** what the probe is doing.

## System Overview

<div class="mt-3 mb-3">
  <img src="{{ '/assets/img/projects/ultrasound-architecture.svg' | relative_url }}" alt="Tele-ultrasound system architecture" class="img-fluid rounded z-depth-1" style="width: 100%;">
</div>
<div class="caption">
    System architecture: operator commands flow left to right; measured contact force flows back to the controller and the operator.
</div>

The system is split into three layers that run in a closed loop:

1. **Haptic Master Interface (operator side):** A grounded haptic device that captures the sonographer's hand motion and renders contact forces through impedance-based feedback.
2. **Real-Time Control Layer:** Maps master motion to probe pose commands in the robot's task space and runs the admittance controller that governs how the arm reacts to contact.
3. **Force-Controlled Manipulator (patient side):** A 7-DOF arm holding the ultrasound probe, which complies to the body's surface instead of rigidly forcing its way to a commanded position.

## Key Contributions

### Admittance-Based Force Control

Image quality depends on keeping the probe pressed against the skin with a steady, moderate force; too little loses acoustic coupling, too much is uncomfortable and unsafe. Pure position control cannot guarantee this because the body's surface and stiffness are never known exactly.

I developed an **admittance-based force control framework** for the 7-DOF manipulator. The measured interaction force is passed through virtual mass–damper–spring dynamics to generate motion:

$$
M\,\ddot{\tilde{x}} + D\,\dot{\tilde{x}} + K\,\tilde{x} = F_{ext} - F_{d}
$$

where $$\tilde{x}$$ is the deviation from the commanded probe pose, $$F_{ext}$$ the measured contact force and $$F_d$$ the desired contact force. The arm therefore yields when tissue pushes back and settles at the target force, giving **compliant interaction and stable probe–tissue contact**.

### Real-Time External Control Layer

The robot's built-in controller is general-purpose and not tuned for sustained contact with soft, moving tissue. I designed and implemented a **real-time external control layer** that runs on top of it and augments the internal controller for this task, improving **force tracking, responsiveness and task-specific performance**.

### Grounded Haptic Master with Impedance Feedback

I architected and prototyped the **grounded haptic master interface**. It works as an impedance device: the operator's motion goes in, and a force comes out. Contact forces measured at the probe are rendered at the stylus, so the sonographer can feel how hard they are pressing and when the probe touches or leaves the skin, making teleoperation **intuitive and safe**.

### Dynamics Modeling & Stability Tuning

A human, a robot and soft tissue in one loop can easily become unstable. I **modeled the system dynamics** and tuned the control parameters (virtual inertia, damping and stiffness, and the feedback gains on the master side) to keep the loop stable while preserving responsiveness, for reliable human–robot interaction in a clinical environment.

## Validation

The system was tested on an anthropomorphic torso phantom (shown in the video below) and on the [breathing phantom simulator]({{ '/projects/2_project/' | relative_url }}) I built to reproduce respiratory motion, which checks how well contact is held on a moving surface.

<!-- TODO(Dev): add quantitative results if you can share them, e.g.

### Force Tracking Performance

| Metric | Robot internal controller | With external admittance layer (ours) |
| :-- | :-- | :-- |
| Force tracking error (RMS) | x N | **y N** |
| Settling time | x s | **y s** |
| Peak contact-force overshoot | x N | **y N** |
-->

## Technical Stack

- **Control:** Admittance control, impedance-based haptic rendering, real-time control loops, system dynamics modeling
- **Hardware:** 7-DOF robotic manipulator, ultrasound probe, grounded haptic master device, force sensing at the probe
- **Validation:** Anthropomorphic torso phantom, breathing phantom simulator
<!-- TODO(Dev): add software (e.g. C++, Python, ROS 2) and the robot / haptic device models if you can name them -->

## Video Demonstration

<div style="max-width: 420px; margin: 1.5rem auto 0;">
  <div style="position: relative; width: 100%; padding-top: 177.78%;">
    <iframe src="https://drive.google.com/file/d/1CKYvC6o1VgbYKikvikO2uMMoFu3pPbBj/preview" style="position: absolute; inset: 0; width: 100%; height: 100%; border: 0;" class="rounded z-depth-1" allow="autoplay; fullscreen" allowfullscreen loading="lazy" title="Tele-ultrasound robot demo"></iframe>
  </div>
</div>
<div class="caption">
    Teleoperation demo: the operator drives the haptic master device (bottom) and the 7-DOF arm moves the ultrasound probe over the torso phantom.
</div>
