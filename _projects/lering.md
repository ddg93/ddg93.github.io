---
layout: page
title: "A Deep Learning approach to measure particles suspended in fluid flows"
---

<div style="border: 2px solid #ccc; padding: 15px; background-color: #f9f9f9; font-size: 1.2em; line-height: 1.5; border-radius: 8px;">
  Statistical models of particles suspended in fluid flows are based on accurate mathematical descriptions of how these objects rotate in simple configurations. 
  Laboratory experiments offer a unique framework to validate these mathematical models.
</div>

### Experiments with particles suspended in viscous shear flows

During my PhD, I performed experiments to measure the rotations of particles suspended in a simple flow configuration (a viscous shear flow). Many particle shapes were considered, and the influence of parameters like the aspect ratio and the inertia was systematically explored. 

Experiments were imaged with a dual-camera setup, where one **top** and one **side** cameras captured recordings from two perpendicular points of view.

Typical results consisted of a couple of synchronized videos of a given particle performing several rotations.

<div style="display: flex; justify-content: center; gap: 20px;">
  <video width="300" autoplay loop muted>
    <source src="{{ site.baseurl }}/videos/top.mp4" type="video/mp4">
  </video>
  <video width="300" autoplay loop muted>
    <source src="{{ site.baseurl }}/videos/side.mp4" type="video/mp4">
  </video>
</div>



<div style="border: 2px solid #ccc; padding: 15px; background-color: #f9f9f9; font-size: 1.2em; line-height: 1.5; border-radius: 8px;">
  
  Measuring the orientation of axi-symmetric particles, like cylinders or spheroids, can be quite challenging, but geometrical relations can be exploited.
  When considering more complex shapes, like axi-symmetric and asymmetric rings, we can train Convolutional Neural Networks on synthetic data to perform this task. 
</div>



https://en.wikipedia.org/wiki/LeNet

### LeRing: two-headed CNNs for particle orientation regression