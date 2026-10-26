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

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/ultrasound.jpg" title="Tele-ultrasound robot" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The tele-ultrasound setup.
</div>
