---
layout: page
title: Tele-Ultrasound Robot
description: Teleoperated robot-assisted ultrasound with haptic feedback · HTIC, IIT Madras
img: assets/img/projects/ultrasound.jpg
importance: 1
category: work
---

**Healthcare Technology Innovation Centre (HTIC), IIT Madras Research Park · Jan 2026 – Sep 2026**

A teleoperated, robot-assisted ultrasound system that lets a sonographer scan a patient remotely. The operator moves a haptic master device; a force-controlled 7-DOF manipulator holding the ultrasound probe follows that motion at the patient's side, and contact forces are fed back to the operator's hand.

### What I worked on

- **Admittance-based force control** for a 7-DOF robotic manipulator, giving compliant interaction and stable probe–tissue contact during scanning.
- **A real-time external control layer** on top of the robot's internal controller, improving force tracking, responsiveness and task-specific performance.
- **A grounded haptic master interface** with impedance-based feedback, so the operator feels the contact and can teleoperate intuitively and safely.
- **Dynamics modeling and controller tuning** for stability and reliable human–robot interaction in a clinical setting.
- **Testing on phantoms**, including the [breathing phantom]({{ '/projects/2_project/' | relative_url }}) I built for this.

<div style="max-width: 420px; margin: 1.5rem auto 0;">
  <div style="position: relative; width: 100%; padding-top: 177.78%;">
    <iframe src="https://drive.google.com/file/d/1CKYvC6o1VgbYKikvikO2uMMoFu3pPbBj/preview" style="position: absolute; inset: 0; width: 100%; height: 100%; border: 0;" class="rounded z-depth-1" allow="autoplay; fullscreen" allowfullscreen loading="lazy" title="Tele-ultrasound robot demo"></iframe>
  </div>
</div>
<div class="caption">
    Teleoperation demo: the operator drives the haptic master device (bottom) and the 7-DOF arm moves the ultrasound probe over the patient phantom while holding steady contact force.
</div>
