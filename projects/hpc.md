---
layout: page
title: "A Scalable GPU Solver"
permalink: /projects/hpc/
---

### Scalable GPU Solver for Fibre-Laden Turbulent Flows

<div style="border: 2px solid #ccc; padding: 15px; background-color: #f9f9f9; font-size: 1.2em; line-height: 1.5; border-radius: 8px;">
  During volcanic eruptions, turbulent air is dispersed with small rocks. 
  During plankton blooms, turbulent sea water is dispersed with small phytoplankton.
  By dispersing fibres in turbulent flows, we can fabricate paper or high-performance concrete.
</div>

During my PhD, I developed a Direct Numerical Simulation (DNS) solver for fibre-laden turbulent channel flows, adapting an existing pseudo-spectral code for GPU-accelerated supercomputing. While the initialization routines remained in Fortran 90, the core iterative solver was ported to CUDA C, enabling execution on distributed GPUs. 

The turbulent flow solver could be parallilised according to a classical 1D domain decomposition, with each MPI task mapped to a dedicated GPU, an inevitable choice given the heavy computational cost of the flow resolution.
Communication between GPUs was implemented using asynchronous MPI calls, and Fourier transforms were accelerated with NVIDIA’s cuFFT library. The porting process required writing and optimising hundreds of lines of CUDA code to ensure scalable high-performance execution.

<video width="640" height="360" controls>
  <source src="{{ site.baseurl }}/videos/flow_RE1200.mp4" type="video/mp4">
  Your browser does not support the video tag.
  <figcaption>
    Turbulent channel flow at Re<sub>τ</sub> = 1200. The computational grid is made by 2048 points in the stream-wise direction, 1024 points in the span-wise direction, 1025 points in the wall-normal direction.
  </figcaption>
</video>

Fluid-fibres coupling involved interpolating flow velocities at the fibres' centres and diffusing particle drag forces back to the flow discretisation grid. With the 1D domain decomposition, this two-way coupling was achieved by exchaning flow and particle halos via asynchronous MPI AllToAll communications, allowing particles to travel seamlessly through distributed GPU memories according to their position within the channel.

Fibres were represented using the rod-chain model, here each fibre is a chain of consecutively constrained rods. This allowed for the simulation of long and flexible fibres, but introduced major algorithmic challenges. As fibres could span to a wide section of the channel, fluid halos communications were not an option due to the huge volume of data required to be sent. Indeed, each fibre introduced a tridiagonal block-matrix system that had to be solved to keep together the consituting rod elements, sparsed across a variable number of GPU memories with an irregular communication pattern. 

To address this, I designed a novel parallelisation algorithm. A second, local static copy of each fibre was created so that all the rods of one chain resided on the same memory unit, enabling efficient constraint resolution. Fibres were load-balanced across all GPUs, and synchronisation between the travelling and static copies of the rods was performed through two on-demand MPI Send/Recv phases: one before constraint resolution to update their state, one after to broadcast the new positions.

<figure>
  <img src="{{ site.baseurl }}/images/scaling.jpg" alt="Fibre-laden turbulent channel flow solver scaling" width="640">
  <figcaption>
    Scalability tests of the flow solver (blue diamonds) and the fibre-laden flow solver (pink squares) on the Tier-0 PRACE system Marconi 100. Ideal behaviours are displayed as black dashed lines. Panels: (a) strong scaling test against the number of dedicated GPUs. Data were collected for a turbulent channel flow at shear Reynolds Re<sub>τ</sub> = 600 discretized on 2048 x 1024 x 1025 grid points and laden with 20 million rods elements. (b) weak scaling test against the number of dedicated GPUs. Data were collected starting from a turbulent channel flow at shear Reynolds Re<sub>τ</sub> = 150 discretized on 512 x 256 x 257 grid points and laden with 312500 rod elements. Both Eulerian and Lagrangian computational loads were then multiplied by a factor of 8 at each increment of computational resources. Elapsed times per time-step are also reported.
  </figcaption>
</figure>

This original scheme enabled scalable, GPU-accelerated simulations of fibre-laden turbulent flows, tested on Tier-0 supercomputers such as Marconi100, JUWELS-Booster, and Piz Daint — to our knowledge, the first approach of its kind. 