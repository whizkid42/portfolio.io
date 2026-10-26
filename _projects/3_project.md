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

This capstone project designs a **robotic surgical lighting system the surgeon controls without using their hands**. The surgeon wears a pair of smart glasses with a jaw-clench sensor on the temple and a laser pointer. They clench to activate the laser, point it at the spot that needs light, and clench again to confirm. The ceiling-mounted robotic light arms then automatically reposition and focus on that point, with no surgical assistant involved.

The project focused on **system architecture**: defining every subsystem, interface and failure mode so the concept is ready for simulation and hardware prototyping.

## How It Works

<div class="mt-3 mb-3">
  <img src="{{ '/assets/img/projects/ot-lighting-workflow.svg' | relative_url }}" alt="Surgeon workflow: clench, point, confirm, localize and plan, light follows" class="img-fluid rounded z-depth-1" style="width: 100%;">
</div>
<div class="caption">
    Surgeon workflow: a clench–point–clench interaction, after which the system localizes the target and moves the light on its own.
</div>

<!-- TODO(Dev): confirm the interaction: does a clench switch the laser on and a second clench confirm, or does jaw movement itself steer the laser? -->

## System Architecture

<div class="mt-3 mb-3">
  <img src="{{ '/assets/img/projects/ot-lighting-architecture.svg' | relative_url }}" alt="Six-subsystem architecture of the robotic surgical lighting system" class="img-fluid rounded z-depth-1" style="width: 100%;">
</div>
<div class="caption">
    The six subsystems and their data flow. The safety system monitors and can override every stage.
</div>

I designed an end-to-end, modular architecture made of six core subsystems:

1. **Smart Glasses (input interface):** A jaw-clench sensor on the temple, a laser pointer and an IMU. Clenches are deliberate commands, so the light only moves when the surgeon intends it to.
2. **Perception Unit (3D localization):** Detects the laser dot on the surgical field with image processing and fuses camera data with IMU data, transforming the target into a 3D coordinate in the room frame.
3. **Lighting Planner (decision and planning):** Evaluates the target, selects which light arm should serve it, checks for obstacles using real-time vision, and plans a safe trajectory using inverse kinematics.
4. **Control System (real-time actuation):** Executes the planned motion with PID joint control, commanding the actuators over a CAN bus.
5. **Robotic Lighting Arms (mechanical actuators):** Ceiling-mounted, multi-DOF arms that aim and focus the surgical lights.
6. **Safety System:** Emergency stops, no-go zones and alignment verification, plus fallback and safe states for every stage of the pipeline.

## Key Contributions

### Model-Based Systems Engineering in SysML

I formalized the system using **Model-Based Systems Engineering (MBSE)** in SysML with Modelio. **Block Definition Diagrams (BDDs)** define each subsystem and its components; **Internal Block Diagrams (IBDs)** define the interfaces and data flow between them, from a clench at the glasses to a joint command at the light arm. The work followed the **V-model** development approach, so each design element traces to requirements and to how it will later be verified.

### Perception and Planning Design

I modeled the full chain from a laser dot to a light pose: how the projected dot is detected in the camera images, how vision and inertial data are fused to place the target in 3D, and how the planner picks an arm and an **obstacle-aware, inverse-kinematics-based trajectory** so the light never swings through the surgical team or the sterile field.

### Safety Analysis (FMEA)

An operating room is a safety-critical environment, so I ran a high-level **Failure Mode and Effects Analysis (FMEA)** to identify where the system could fail and to define a fallback or safe state for each failure. This drove the safety system's design: **emergency stops**, **no-go zones** for the arms, and **alignment verification** to confirm the light has reached the requested point.

## Scope and Next Steps

Because of time and hardware constraints, the project stopped at **architecture design and simulation readiness**; no physical prototype or simulation was built. Recommended next steps:

- Complete the internal block and behaviour models and **simulate** the system, including occlusions and sensor noise
- Build a **hardware prototype** of the glasses and a light arm to validate the concept
- Add automated calibration, stronger failure detection and **human-factors testing** with surgeons
- Explore continuous target tracking and integration with augmented-reality displays

## Technical Stack

- **Systems engineering:** MBSE, SysML (BDDs, IBDs), Modelio, V-model, FMEA
- **Robotics design:** Multi-DOF arm kinematics, inverse kinematics trajectory planning, obstacle avoidance, PID control
- **Perception design:** Laser-spot detection with image processing, vision + IMU sensor fusion, 3D localization
- **Embedded interfaces:** CAN bus actuator communication
- **Human–machine interface:** Jaw-clench input on smart glasses, wearable laser pointer

<!-- TODO(Dev): add slides or screenshots of your SysML BDD/IBD diagrams from the final presentation -->
