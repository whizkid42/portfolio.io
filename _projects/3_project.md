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

In most operating rooms the surgical light is still moved by hand. When the surgeon needs light somewhere new, they either break concentration to reach for the sterile handle or ask an assistant to adjust it, and the light is rarely exactly where it is needed. Each adjustment interrupts the workflow, adds fatigue and is another chance to compromise the sterile field.

This capstone project designs a **robotic surgical lighting system the surgeon controls without using their hands**. The surgeon wears smart glasses with a jaw-clench sensor on the temple, a laser pointer, a camera and an IMU. They clench once and look at the area that needs light, which switches the laser on; a second clench confirms the spot. Three ceiling-mounted robotic light arms then automatically reposition and focus on that point, with no surgical assistant involved.

The project focused on **system architecture**: defining every subsystem, interface, behaviour and failure mode in SysML so the concept is ready for simulation and hardware prototyping.

## How It Works

<div class="mt-3 mb-3">
  <img src="{{ '/assets/img/projects/ot-lighting-workflow.svg' | relative_url }}" alt="Surgeon workflow: clench and look, confirm, localize and plan, light focuses, fine-tune" class="img-fluid rounded z-depth-1" style="width: 100%;">
</div>

<div class="row mt-3">
  <div class="col-sm-7">
    <p>The interaction is designed so the light only ever moves when the surgeon deliberately asks it to:</p>
    <ul>
      <li><strong>Start-up:</strong> Putting on the glasses puts the system in <em>Auto</em> mode and the lights are ready to move.</li>
      <li><strong>Target:</strong> Clench once and look at the target region to switch on the laser; clench again to confirm. The camera captures the laser dot and an LED flashes to acknowledge it.</li>
      <li><strong>Respond:</strong> The selected light arm moves above the target point and focuses on it.</li>
      <li><strong>Fine-tune the angle:</strong> Clench and hold, then tilt the head right or left to tilt the light by 15° in that direction. Releasing the clench locks the light in place.</li>
      <li><strong>Manual mode:</strong> A single tap on the right or left temple switches to <em>Manual</em> mode and releases the light hold state.</li>
    </ul>
  </div>
  <div class="col-sm-5">
    {% include figure.liquid loading="lazy" path="assets/img/projects/ot-swimlane.png" title="Surgeon interaction swimlane" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Swimlane of surgeon action, system response and lighting response.</div>
  </div>
</div>

## System Architecture

<div class="row mt-3">
  <div class="col-sm-6">
    {% include figure.liquid loading="lazy" path="assets/img/projects/ot-full-architecture.png" title="Full system architecture" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Full system architecture: hardware, ROS 2 software stack and communication layer.</div>
  </div>
  <div class="col-sm-6">
    <p>The design is split into three layers:</p>
    <p><strong>Hardware.</strong> The <em>surgeon glasses unit</em> carries a clench-detection sensor, a Class 1 red laser pointer, a wide-angle RGB camera, an IMU for orientation tracking and an Arduino Nano. The <em>lighting system</em> has three 4-DOF robotic arms, boom-mounted in a triangle formation, each carrying a focusable LED light head and driven by its own CAN-enabled ARM Cortex motor controller with closed-loop encoder feedback.</p>
    <p><strong>Software (ROS 2).</strong> A user-interface node publishes the clench signal and switches the laser; a laser-detection node finds the dot in the camera feed; transformation nodes convert the 2D pixel to a 3D point in the world frame; an obstacle-avoidance node builds planning maps with no-go zones from YOLO detections; the lighting-planner node selects the best arm and runs inverse kinematics; and actuation nodes convert joint targets into CAN messages with closed-loop PID tracking per joint.</p>
    <p><strong>Communication.</strong> ROS 2 carries the high-level logic, image processing, transformation and planning; the CAN bus provides low-latency control of the laser and lighting motors; a feedback and safety loop confirms that the light spot is aligned with the target and triggers a safe state or retry otherwise.</p>
  </div>
</div>

## SysML Model

<div class="mt-3 mb-3">
  {% include figure.liquid loading="lazy" path="assets/img/projects/ot-bdd.png" title="SysML Block Definition Diagram" class="img-fluid rounded z-depth-1" %}
</div>
<div class="caption">
    Block Definition Diagram of the Robotic Surgical Lighting System (RSLS), modeled in Modelio.
</div>

I formalized the system with **Model-Based Systems Engineering (MBSE)** in SysML using Modelio, following the **V-model** development approach. The top-level **RSLS** block (with Idle/Active/Error status and Manual/Auto modes) is composed of six subsystems, each defined with its attributes and operations in the **Block Definition Diagram (BDD)**:

1. **Glasses:** clench detector, camera, laser, IMU, Arduino and Bluetooth modules.
2. **Perception Unit:** camera interface, laser-point detector, IMU integrator, coordinate transformer, YOLO and a vision status reporter.
3. **Lighting Planner:** arm selector, IK solver, trajectory generator, obstacle-aware motion planner, command dispatcher and state monitor.
4. **Control System:** arm kinematics, joint controller and CAN interface.
5. **Robotic Lighting Arm:** three manipulator arms with motor and light controller units and short-range IR sensors.
6. **Safety System:** emergency stop, no-go zones, manual override and LED status feedback.

**Internal Block Diagrams (IBDs)** then define the ports, data flows and behaviour inside the key subsystems.

## Subsystem Design

### Perception: From a Laser Dot to a 3D Target

<div class="mt-3 mb-3">
  {% include figure.liquid loading="lazy" path="assets/img/projects/ot-ibd-perception.png" title="Perception Unit IBD" class="img-fluid rounded z-depth-1" %}
</div>
<div class="caption">
    Internal Block Diagram of the Perception Unit.
</div>

- **Laser detection:** Each frame is converted from RGB to LAB colour space, thresholded on the colour channels, cleaned with morphological operations, and the centroid of the best-matching blob gives the laser pixel (u, v).
- **Glasses pose:** An operating-room camera detects an ArUco marker and uses `solvePnP` to estimate the glasses' position; the IMU quaternion gives its orientation. Together they form the view transform.
- **3D projection:** The laser pixel is back-projected through the camera intrinsics into a ray, rotated into the world frame and intersected with the surgical-table plane to get the 3D target.
- **Obstacles and head tilt:** A YOLO detector finds surgeons and instruments to build a 3D occupancy map; a tilt checker turns clench-and-tilt gestures into left/right tilt commands.

### Lighting Planner: Choosing an Arm and a Safe Path

<div class="mt-3 mb-3">
  {% include figure.liquid loading="lazy" path="assets/img/projects/ot-ibd-planner.png" title="Lighting Planner IBD with algorithms" class="img-fluid rounded z-depth-1" %}
</div>
<div class="caption">
    Internal Block Diagram of the Lighting Planner, annotated with its core algorithms.
</div>

- **Reachability:** A target evaluator checks the distance from each arm's base to the target against that arm's reach, and a decision gate only forwards reachable targets.
- **Arm selection:** For each reachable arm, inverse kinematics is solved; the nearest arm with a valid IK solution is selected.
- **Safe trajectory:** A sampling-based planner searches for a collision-free joint trajectory against the obstacle map and a safety margin within a **300 ms** time budget, and falls back to holding position if none is found.

### Control: Executing the Motion over CAN

<div class="mt-3 mb-3">
  {% include figure.liquid loading="lazy" path="assets/img/projects/ot-ibd-control.png" title="Control System IBD" class="img-fluid rounded z-depth-1" %}
</div>
<div class="caption">
    Internal Block Diagram of the Control System.
</div>

A trajectory buffer feeds time-indexed joint targets to a **PID joint controller**, which sends commanded joint angles over a **CAN interface** to the active arm and closes the loop on encoder readings. The CAN layer retries failed transmissions, flags communication timeouts as faults, and a light-intensity manager sets the brightness of the active light head. An emergency-stop input halts motion and holds the joints in a safe position at every stage.

### Safety by Design (FMEA)

<div class="mt-3 mb-3">
  {% include figure.liquid loading="lazy" path="assets/img/projects/ot-safety-block.png" title="Safety System block" class="img-fluid rounded z-depth-1" %}
</div>
<div class="caption">
    Safety System block: attributes and operations.
</div>

A high-level **Failure Mode and Effects Analysis (FMEA)** identified where the system could fail and defined a fallback or safe state for each. This shaped the safety system: **emergency stop** that disables motion and power to the arms, **short-range IR sensors** that raise proximity alerts, predefined **no-go zones** and a **safety margin** around staff and obstacles, a **manual override**, LED status feedback, and a system-health monitor that aggregates every subsystem's status.

## Scope and Next Steps

Because of time and hardware constraints, the project stopped at **architecture design and simulation readiness**; no physical prototype or simulation was built. Recommended next steps:

- Complete the behaviour models and **simulate** the system, including occlusions and sensor noise
- Build a **hardware prototype** of the glasses and a light arm to validate the concept
- Add automated calibration, stronger failure detection and **human-factors testing** with surgeons
- Explore continuous target tracking and integration with augmented-reality displays

## Technical Stack

- **Systems engineering:** MBSE, SysML (BDDs, IBDs), Modelio, V-model, FMEA
- **Software architecture:** ROS 2 nodes and topics, CAN bus
- **Perception:** OpenCV (LAB thresholding, ArUco, solvePnP), YOLO, vision + IMU fusion, ray–plane projection
- **Planning and control:** Inverse kinematics, sampling-based collision-free planning, PID joint control
- **Hardware concept:** Smart glasses (clench sensor, laser, RGB camera, IMU, Arduino Nano), 3 × 4-DOF boom-mounted light arms, ARM Cortex motor controllers
