---
layout: page
title: Biomechanics of Taekwondo Punches
description: Motion Capture, Inverse Kinematics and Inverse Dynamics of Dominant vs Non-Dominant Rear-Hand Punches · ME 481, UIUC
img: assets/img/projects/me481.jpg
importance: 8
category: work
---

**Course:** ME 481 Whole Body Musculoskeletal Biomechanics, University of Illinois Urbana-Champaign · Spring 2025<br>
**Team:** AJ Bernardo, Helen Chang, Michael Moy, Dev Sakaria

Martial arts training usually favours the dominant side, but how differently does the body actually move when the same technique is thrown with the other hand? We asked: **how do the leg kinematics and ground reaction forces of a Taekwondo rear-hand punch differ between the dominant and non-dominant hand?** The answer can point instructors towards targeted training that reduces left–right imbalances.

## Experimental Setup

<div class="row mt-3" style="margin-left: 0; margin-right: 0;">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/me481-setup.jpg" title="Experimental setup" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Motion capture markers, force plate and the punch.</div>
  </div>
  <div class="col-sm mt-3 mt-md-0">
    <p>Each subject wore reflective markers and stepped forward into a rear-hand punch, landing the rear foot on a <strong>force plate</strong> while <strong>motion capture cameras</strong> recorded the movement. The trial was then repeated on the opposite side for the non-dominant hand.</p>
    <p>The marker data was scaled to an <strong>OpenSim</strong> musculoskeletal model, and we ran:</p>
    <ul>
      <li><strong>Inverse kinematics</strong> for ankle, knee and hip flexion angles</li>
      <li><strong>Inverse dynamics</strong> for ankle, knee and hip joint moments</li>
    </ul>
    <p>We also extracted the peak ground reaction force, stride length and stance width from the force-plate data, normalized every trial to a full gait cycle, and compared conditions with paired t-tests.</p>
  </div>
</div>

<div class="mt-3 mb-3">
  {% include figure.liquid loading="lazy" path="assets/img/projects/me481-opensim.jpg" title="OpenSim model" class="img-fluid rounded z-depth-1" %}
</div>
<div class="caption">
    Motion capture markers (left) and the scaled OpenSim model at 50% of the loading phase (right).
</div>

## Results

<div class="mt-3 mb-3">
  {% include figure.liquid loading="lazy" path="assets/img/projects/me481-angles.jpg" title="Joint angles" class="img-fluid rounded z-depth-1" %}
</div>
<div class="caption">
    Hip, knee and ankle angles over the movement cycle (mean ± SD), dominant (blue) vs non-dominant (red).
</div>

<div class="mt-3 mb-3">
  {% include figure.liquid loading="lazy" path="assets/img/projects/me481-moments.jpg" title="Joint moments" class="img-fluid rounded z-depth-1" %}
</div>
<div class="caption">
    Hip, knee and ankle moments from inverse dynamics, dominant vs non-dominant.
</div>

- **Less stable on the non-dominant side:** non-dominant punches showed a larger standard deviation and more pronounced peaks and troughs, especially at the hip while the foot was on the ground.
- **Earlier leg straightening:** around 25% of the cycle, the non-dominant leg straightened before the dominant one, which likely reduces the force transferred into the punch.
- **Ankle stays consistent:** ankle angles were similar between sides; the differences showed up mainly at the hip and knee.
- **Joint moments:** peak hip, knee and ankle moments were not significantly different between sides (paired t-tests, p = 0.29, 0.56 and 0.61), so the asymmetry is in **how** the movement is coordinated rather than in peak joint loading.
- **Ground reaction force:** peak GRF ranged from about **730 N to 1,080 N** across trials.

**Limitations and future work:** subjects had different experience levels, and the analysis looked only at the sagittal plane. A follow-up should include upper-body kinematics, other planes of motion and other techniques.

## Videos

<div class="row mt-3" style="margin-left: 0; margin-right: 0;">
  <div class="col-sm mt-3 mt-md-0">
    <div style="position: relative; width: 100%; padding-top: 56.25%;">
      <iframe src="https://drive.google.com/file/d/11l0uh-I3wlyCi3Utnv7KtgFbUoeCklAP/preview" style="position: absolute; inset: 0; width: 100%; height: 100%; border: 0;" class="rounded z-depth-1" allow="autoplay; fullscreen" allowfullscreen loading="lazy" title="Motion capture markers"></iframe>
    </div>
    <div class="caption">Motion capture marker data.</div>
  </div>
  <div class="col-sm mt-3 mt-md-0">
    <div style="position: relative; width: 100%; padding-top: 56.25%;">
      <iframe src="https://drive.google.com/file/d/1R4zOLi4CE9q4NF3fi1aFGezUsWsFuH-d/preview" style="position: absolute; inset: 0; width: 100%; height: 100%; border: 0;" class="rounded z-depth-1" allow="autoplay; fullscreen" allowfullscreen loading="lazy" title="OpenSim simulation"></iframe>
    </div>
    <div class="caption">OpenSim inverse kinematics of the punch.</div>
  </div>
</div>

## My Contributions

- **Data collection:** helped set up the motion capture and force plate, and served as one of the test subjects.
- **Data cleaning and processing:** cleaned and gap-filled the marker data, identified heel strikes and toe-offs to segment each trial, and prepared the files for OpenSim scaling, inverse kinematics and inverse dynamics.
- **Analysis and reporting:** contributed to plotting the joint angles and moments, the statistical comparison, and writing the proposal and final poster.

## Technical Stack

- **Motion capture and force plates:** marker-based motion capture, ground reaction force measurement
- **Musculoskeletal modelling:** OpenSim (scaling, inverse kinematics, inverse dynamics)
- **Analysis:** Python, gait-cycle normalization, paired t-tests
