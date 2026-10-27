---
layout: page
title: Soft Robotic Toe Separator
description: A Pneumatically Actuated Silicone Toe Separator with Air Chambers Formed by Dissolvable Sugar Inserts · BIOE 598, UIUC
img: assets/img/projects/toe-separator.jpg
importance: 7
category: work
---

**Course:** BIOE 498/598 Soft Robotics, University of Illinois Urbana-Champaign · Fall 2024<br>
**Team:** Nidish Koppula, Dev Sakaria, David Wilcoski<br>
**Award:** 🏆 **Best Design Process Award**, BIOE 498/598 Soft Robotics

After bunion or toe-fracture surgery, the big toe has to be held in the right position while it heals. Standard toe spacers are passive, one-size foam or gel blocks: they can't be adjusted as healing progresses, and they're often uncomfortable when walking. We designed a **soft, inflatable toe separator** whose silicone air chambers can be pumped up or let down, so the support can be **tuned to each patient** and adjusted over the course of recovery.

## Design

<div class="row mt-3" style="margin-left: 0; margin-right: 0;">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/toe-separator-chambers.jpg" title="Cast silicone separator" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/toe-separator-mold.jpg" title="3D-printed casting mold" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
    The cast silicone separator (left) and the 3D-printed mold, shaped to sit between the toes (right).
</div>

The separator is a three-layer **PneuNet** actuator:

- **Top and bottom layers in Dragon Skin 30:** a stiffer silicone that stops the chambers ballooning vertically and holds the pump connector in place.
- **Middle actuating layer in Ecoflex 00-30:** a very soft silicone containing the air chambers, which expand sideways to push the big toe back into alignment.

The angled geometry fits comfortably between the toes, and the chambers run along the length of the device so pressure is spread evenly over the contact area. The shape went through **three design iterations**.

## Fabrication: Sugar as a Dissolvable Core

<div class="row mt-3" style="margin-left: 0; margin-right: 0;">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/toe-separator-inserts.jpg" title="Sugar inserts in the mold" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Sugar inserts placed in the mold.</div>
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="lazy" path="assets/img/projects/toe-separator.jpg" title="Dissolving the sugar inserts" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Injecting water to dissolve the sugar and open up the air chambers.</div>
  </div>
</div>

Forming sealed internal chambers in cast silicone is usually the hard part. Our key idea was to use **dissolvable sugar inserts** as the chamber cores. Sugar dissolves in minutes, unlike PVA, which takes hours in heated water and needs a large outlet port. A small port is much easier to make airtight.

1. **Sugar inserts:** granulated sugar mixed with a little water, packed into 3D-printed moulds and dried for at least 6 hours.
2. **Layered casting:** pour the Dragon Skin 30 base and part-cure it at 60 °C; place the sugar inserts and pour the Ecoflex 00-30 layer; then pour the Dragon Skin 30 top layer and cure for 10 hours.
3. **Open the chambers:** inject water with a syringe to dissolve the sugar, then draw the solution out, leaving clean, precisely shaped hollow chambers.
4. **Assembly:** add small inlet ports and connect a hand pump through a bidirectional valve to inflate and deflate the separator.

## Testing and Results

- **Inflation tests:** the chambers survived **repeated inflation and deflation cycles at high pressure** with no material failure or leaks.
- **Functional tests:** worn on the foot, the inflated separator **moved the big toe outward into alignment**, and support was tested in simulated walking.
- The results confirmed our hypothesis: sugar inserts are a **fast, simple way to make well-formed, durable air chambers** for soft medical devices.

<div style="max-width: 560px; margin: 1.5rem auto 0;">
  {% include figure.liquid loading="lazy" path="assets/img/projects/toe-separator-award.jpg" title="Best Design Process Award" class="img-fluid rounded z-depth-1" %}
  <div class="caption">Best Design Process Award, BIOE 498/598 Soft Robotics.</div>
</div>

## My Contributions

I was involved in every stage of the project, working closely with my two teammates:

- **Research and brainstorming:** studied post-operative toe-alignment care and existing toe separators, and helped define the problem and the functional requirements (adjustable support, comfort while walking, durability, ease of fabrication).
- **Design:** iterated on the separator geometry over three versions and designed the 3D-printed casting and sugar-insert moulds, including the choice of a Dragon Skin / Ecoflex layer stack.
- **Fabrication:** made the sugar inserts, cast and cured the silicone layers, dissolved the inserts to form the chambers, and assembled the pump, valve and inlet ports.
- **Testing and reporting:** ran the inflation and on-foot alignment tests, refined the surface finish, and co-wrote the final report and presentation.

## Next Steps

Add **pressure and force sensing** with a **microcontroller-regulated mini pump**, so the separator automatically holds the inflation level a clinician prescribes or adjusts for comfort.

## Technical Stack

- **Soft robotics:** PneuNet actuator design, multi-material silicone casting (Dragon Skin 30, Ecoflex 00-30)
- **Fabrication:** 3D-printed moulds, dissolvable sugar inserts, staged oven curing
- **Pneumatics:** Hand pump, bidirectional valve, inflation testing
