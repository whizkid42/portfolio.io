---
layout: page
title: Robotic OT Lighting
description: Robotic Surgical Lighting with a Hands-Free, Jaw-Clench Input Modality
img: assets/img/projects/ot-lighting.jpg
importance: 3
category: work
---

**Organization:** Hourglass Medical, with the University of Illinois Urbana-Champaign (M.Eng capstone)<br>
**Role:** Robotics Systems Design Intern · May – Aug 2025<br>
**Industry Advisor:** Luke Morrison, Hourglass Medical

In most operating rooms the surgical light is still moved by hand. Every time the surgeon needs light somewhere new, they either reach for the sterile handle or ask an assistant, interrupting the workflow and risking the sterile field.

This capstone project designs a **robotic surgical lighting system the surgeon controls without using their hands**, through smart glasses fitted with a jaw-clench sensor, a laser pointer, a camera and an IMU.

## How It Works

1. **Clench & look:** Clench once and look at the area that needs light; the laser switches on.
2. **Confirm:** Clench again; the camera captures the laser dot and an LED flashes to confirm.
3. **Light follows:** The system locates the dot in 3D, and one of three ceiling-mounted robotic light arms moves above it and focuses, with no assistant involved.
4. **Fine-tune:** Clench and hold, then tilt the head to tilt the light by 15°; release to lock it in place. A tap on the temple switches to manual mode.

## System Architecture

<div class="mt-3 mb-3">
  {% include figure.liquid loading="lazy" path="assets/img/projects/ot-bdd.png" title="SysML Block Definition Diagram" class="img-fluid rounded z-depth-1" %}
</div>
<div class="caption">
    SysML Block Definition Diagram of the Robotic Surgical Lighting System (RSLS), modeled in Modelio.
</div>

I designed the end-to-end architecture using **Model-Based Systems Engineering (MBSE)** in SysML, following the V-model. The system is broken into six subsystems: the **Glasses** (input), a **Perception Unit** that finds the laser dot and converts it to a 3D target, a **Lighting Planner** that selects an arm and plans a collision-free path with inverse kinematics, a **Control System** that drives the joints over CAN with PID control, the **Robotic Lighting Arms**, and a **Safety System** (emergency stop, no-go zones, IR proximity sensing and manual override) shaped by a high-level **FMEA**.

## Scope and Next Steps

Because of time and hardware constraints, the project stopped at **architecture design and simulation readiness**; no physical prototype or simulation was built. Recommended next steps:

- Complete the behaviour models and **simulate** the system, including occlusions and sensor noise
- Build a **hardware prototype** of the glasses and a light arm to validate the concept
- Add automated calibration, stronger failure detection and **human-factors testing** with surgeons

## Technical Stack

- **Systems engineering:** MBSE, SysML (BDDs, IBDs), Modelio, V-model, FMEA
- **Software architecture:** ROS 2, CAN bus
- **Perception:** OpenCV, ArUco / solvePnP, YOLO, vision + IMU fusion
- **Planning and control:** Inverse kinematics, collision-free motion planning, PID joint control
- **Hardware concept:** Smart glasses (clench sensor, laser, RGB camera, IMU, Arduino Nano), 3 × 4-DOF boom-mounted light arms
