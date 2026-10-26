---
layout: page
title: Breathing Phantom Simulator
description: A Pneumatically Actuated Chest Phantom for Testing Robotic Ultrasound on a Moving Body
img: assets/img/projects/breathing-phantom.jpg
importance: 2
category: work
---

<!-- TODO(Dev): add team members, e.g. **Team:** Name, Name -->

**Organization:** Healthcare Technology Innovation Center (HTIC), IIT Madras Research Park, Chennai<br>
**Role:** Robotics Engineer · 2026

Real patients breathe. With every breath the chest and abdomen rise and fall, so the surface a [robotic ultrasound system]({{ '/projects/1_project/' | relative_url }}) presses against is always moving. A static phantom cannot show whether the robot holds steady probe contact through that motion; testing on volunteers is slow, hard to repeat and not where an unproven controller should be tuned.

I built a **breathing phantom simulator**: a compact, pneumatically actuated test bed whose soft surface rises and falls like a breathing chest. It gives the force-controlled robot a **repeatable, moving target** to test against before it ever touches a person.

## System Overview

The phantom is built from three subsystems:

1. **Soft Breathing Surface:** A flexible membrane stretched over a dome that inflates and deflates, giving a compliant, chest-like surface for the ultrasound probe to press on.
2. **Pneumatic Actuation:** A motor-driven air pump pushes air through tubing into the enclosure under the membrane, so the surface moves smoothly without any rigid parts that a probe could hit.
3. **Microcontroller Control:** A microcontroller drives the pump, setting the inflate–deflate cycle that defines the "breathing" pattern.

<!-- TODO(Dev): confirm/adjust: membrane material, pump/valve type, microcontroller, breathing rate (breaths/min) and surface displacement (mm) -->

## Design Evolution

### First Prototype: Proving the Concept

<div style="max-width: 760px; margin: 1rem auto 0;">
  <div style="position: relative; width: 100%; padding-top: 56.25%;">
    <iframe src="https://drive.google.com/file/d/1YfyGrRKOcMTxJvPWVLearGo1Ffls4TPq/preview" style="position: absolute; inset: 0; width: 100%; height: 100%; border: 0;" class="rounded z-depth-1" allow="autoplay; fullscreen" allowfullscreen loading="lazy" title="Breathing phantom first prototype"></iframe>
  </div>
</div>
<div class="caption">
    First prototype: the DC air pump inflates and deflates the dome on a sealed enclosure to simulate breathing.
</div>

The first version answered one question: can a small pump and a sealed enclosure produce a convincing, repeatable rise-and-fall motion? It used a small domed surface on top of a box enclosure, a DC motor-driven air pump connected by tubing, and breadboard electronics. A fiducial marker is fixed to the top of the enclosure. <!-- TODO(Dev): explain what the marker was used for (e.g. camera-based tracking of the phantom) -->

### Final Version: Built for Robot Testing

The final version scales the idea up to something the robot can actually scan:

- **Larger, softer breathing surface:** A wide membrane dome mounted in a rigid black housing gives the probe a big enough area to sweep across, closer to the size of a real chest.
- **Positioned in the robot's workspace:** The phantom sits under the 7-DOF manipulator next to the torso phantom, so the same setup can switch between static and breathing tests.
- **Tested with the robot in the loop:** The robot lowers the ultrasound probe onto the moving surface and has to keep contact while the surface breathes, a direct test of the [admittance controller]({{ '/projects/1_project/' | relative_url }}).

## Why It Matters

A breathing surface turns force control from a static problem into a **disturbance-rejection** problem. As the surface rises, contact force spikes unless the robot yields; as it falls, the probe lifts off unless the robot follows. The phantom lets us see both failure modes safely and tune the controller until the probe rides the motion with steady contact.

<!-- TODO(Dev): add results if available, e.g.

## Results

| Condition | Contact-force variation (with breathing) |
| :-- | :-- |
| Position control only | x N |
| Admittance control (ours) | **y N** |
-->

## Technical Stack

- **Mechanical:** Domed membrane surface, rigid housing, sealed enclosure
- **Actuation:** DC motor-driven air pump, pneumatic tubing
- **Electronics:** Microcontroller-based pump control
- **Use:** Validation test bed for the force-controlled tele-ultrasound robot

## Video Demonstration

<div style="max-width: 760px; margin: 1rem auto 0;">
  <div style="position: relative; width: 100%; padding-top: 56.25%;">
    <iframe src="https://drive.google.com/file/d/1rvI4KMi0EcbTD7PgNjImjTXuLQLPk0os/preview" style="position: absolute; inset: 0; width: 100%; height: 100%; border: 0;" class="rounded z-depth-1" allow="autoplay; fullscreen" allowfullscreen loading="lazy" title="Breathing phantom final version"></iframe>
  </div>
</div>
<div class="caption">
    The 7-DOF arm brings the ultrasound probe onto the breathing surface of the final phantom.
</div>
