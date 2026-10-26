---
layout: about
title: about
permalink: /
subtitle: Robotics & Product Development Engineer · Medical Robotics · Bengaluru, India

profile:
  align: right
  image: prof_pic.jpg
  image_circular: false # crops the image to make it circular
  more_info: >
    <p>M.Eng Autonomy & Robotics, UIUC</p>
    <p>B.Tech Mechatronics, MIT Manipal</p>
    <p>Bengaluru, India</p>

selected_papers: false # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page

announcements:
  enabled: false # includes a list of news items

latest_posts:
  enabled: false
---

I design and build robots for surgery and medicine. Most recently, as a Robotics Engineer at the **Healthcare Technology Innovation Center, IITM Research Park**, I developed an admittance-based force control framework for a 7-DOF manipulator and a grounded haptic master interface, so a sonographer can teleoperate an ultrasound probe with stable, safe probe–tissue contact. Before that I architected a multi-DOF robotic surgical lighting platform at **Hourglass Medical**, and built multi-robot localization pipelines at the **Robert Bosch Center for Cyber-Physical Systems, IISc**.

I hold an M.Eng in Autonomy & Robotics from the **University of Illinois Urbana-Champaign** and a B.Tech in Mechatronics Engineering from **Manipal Institute of Technology**. What I enjoy most is the full loop: take a messy problem, sketch the possible solutions, then prototype and test until one holds up on real hardware, with the safety and reliability a medical device demands.

I'm currently looking for robotics, controls and medtech roles in **Bengaluru**, where I can own systems end to end and work on technology that reaches patients.

## Technical Skills

**Robotics, Controls and Autonomy**

- **Force control and haptics:** admittance and impedance control, haptic master interfaces, teleoperation, system dynamics modeling and controller tuning
- **Kinematics and planning:** multi-DOF kinematic modeling and optimization, ROS/ROS 2 perception and planning nodes
- **Perception and estimation:** EKF-based sensor fusion, multi-robot localization, OpenCV, YOLO, PnP-based 6D pose estimation
- **Simulation:** Gazebo, Isaac Sim, MATLAB/Simulink/Simscape with hardware-in-the-loop testing

**Mechatronics and Medical Devices**

- **Design:** SolidWorks, electromechanical system architecture, MBSE with SysML
- **Hardware integration:** sensors and actuators, CAN bus, PLC and HMI programming, industrial vision
- **Medical device practice:** ISO 14971:2019 risk management, designing for safety, reliability and manufacturability (Class III context)
- **Languages:** Python, C++, MATLAB

Find my complete CV [here]({{ '/cv/' | relative_url }}).

## Projects

<div class="projects">
  <div class="row row-cols-1 row-cols-md-3">
  {% assign sorted_projects = site.projects | sort: "importance" %}
  {% for project in sorted_projects %}
    {% include projects.liquid %}
  {% endfor %}
  </div>
</div>
