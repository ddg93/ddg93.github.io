---
layout: page
title: "Experiments with particles suspended in viscous shear flows"
---

Experiments were performed with a linear shear cell setp-up. The aim was to measure the rotation of suspended particles while varying parameters like the particle shape, the particle aspect ratio and the fluid inertia.

<div style="border: 2px solid #ccc; padding: 15px; background-color: #f9f9f9; font-size: 1.2em; line-height: 1.5; border-radius: 8px;">
  Statistical models of particles suspended in fluid flows are based on accurate mathematical descriptions of how these objects rotate in simple configurations. 
  Laboratory experiments offer a unique framework to validate these mathematical models.
</div>

<figure>
  <img src="{{ site.baseurl }}/images/setup.jpg" alt="Linear shear cell setup" width="640">
  <figcaption>
    Linear shear cell setup: the tank (1) is filled with the experimental fluid, a solution of pure water, citric acid (to match the density of the suspended particles and cancel gravity effects along the z axis), and Ucon oil (a special oil miscible in pure water that allowed us to control the fluid's viscosity up to a very large value of 1 Pa s). The tank is closed with a perforated lid (2), through which two cylinders are hanging at both ends. The first cylinder is free to rotate (3), being coupled to a transmission shaft (4) through a rolling bearing. The second cylinder is fixed (5). Between them, a transparent plastic belt made of Mylar is kept under tension (6). The two cameras are also depicted: one is oriented to observe the flow-gradient (x, y) plane (7), while the other is focused on the flow-vorticity (x, z) plane (8). The operative volume where the experiments are performed is also shown in blue (9). When the free cylinder is rotated at constant velocity, a linear shear flow with constant shear rate is produced in the operative volume, where we place the particles of interest. Particle rotations are then recorded with the two cameras, synchronized by a light signal. By decreasing the experimental fluid's viscosity, we perform many experiments under increasingly inertial regimes.
  </figcaption>
</figure>

Several particle shapes were considered, including spheroids, cylinders and rings:
- Spheroids are ideal shapes typically discussed in mathematical models; 
- Cylinders include fibres and disks and represent the typical shapes of interest in industrial and environmental applications; 
- Rings are a special shape with enhanced alignment properties.
Particles were fabricated with rapid-prototyping methods such as 3D printing, CNC milling, laser cutting.

<figure>
  <img src="{{ site.baseurl }}/images/particles.jpg" alt="Particles" width="640">
  <figcaption>
    Panels a-f: side and top views of an oblate spheroids with aspect ratio 0.56 (the ratio between the particle's length and diameter);
    Panels g-i: side views of a prolate cylinder (fibre) with aspect ratio 10;
    Panels l-n: top and side views of an oblate cylinder (disk) with aspect ratio 0.1.
  </figcaption>
</figure>

A typical result for one experiment consisted of a couple of synchornized recordings imaging the selected particle from the top and side camera views. Each couple of recordings would typically display several particle rotations. Experiments were repeated several times, systematically varying parameters such as the particle shape, the particle aspect ratio, the fluid inertia.

<div style="text-align: center;">
  <p><strong>Ring with circular section and aspect ratio 0.45</strong></p>
  <div style="display: flex; justify-content: center; gap: 20px;">
    <div style="text-align: center;">
      <video width="300" autoplay loop muted>
        <source src="{{ site.baseurl }}/videos/top.mp4" type="video/mp4">
      </video>
      <p><em>Top view</em></p>
    </div>
    <div style="text-align: center;">
      <video width="300" autoplay loop muted>
        <source src="{{ site.baseurl }}/videos/side.mp4" type="video/mp4">
      </video>
      <p><em>Side view</em></p>
    </div>
  </div>
</div>

<script>
  window.addEventListener("load", () => {
    const v1 = document.getElementById("video1");
    const v2 = document.getElementById("video2");

    Promise.all([v1.play(), v2.play()]).catch(err => {
      console.log("Autoplay blocked, click to start.");
    });

    v1.addEventListener("timeupdate", () => {
      if (Math.abs(v1.currentTime - v2.currentTime) > 0.1) {
        v2.currentTime = v1.currentTime; // resync if they drift
      }
    });
  });
</script>

Experimental recordings can be found [in this repository](https://huggingface.co/datasets/ddg93/LeRing_JFM_experiments/tree/main).