---
layout: page
title: "scalable_gpu_solver"
permalink: /projects/hpc/
---

### Scalable GPU Solver for Fibre-Laden Turbulent Flows

During my PhD, I developed a Direct Numerical Simulation (DNS) solver for fibre-laden turbulent channel flows, adapting an existing pseudo-spectral code for GPU-accelerated supercomputing. While the initialization routines remained in Fortran 90, the core iterative solver was ported to CUDA C, enabling execution on distributed GPUs. 

The turbulent flow solver could be parallilised according to a classical 1D domain decomposition, with each MPI task mapped to a dedicated GPU, an inevitable choice given the heavy computational cost of the flow resolution.
Communication between GPUs was implemented using asynchronous MPI calls, and Fourier transforms were accelerated with NVIDIA’s cuFFT library. The porting process required writing and optimising hundreds of lines of CUDA code to ensure scalable high-performance execution.

<video width="640" height="360" controls>
  <source src="{{ site.baseurl }}/videos/flow_RE1200.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

Fluid-fibres coupling involved interpolating flow velocities at the fibres' centres and diffusing particle drag forces back to the flow discretisation grid. With the 1D domain decomposition, this two-way coupling was achieved by exchaning flow and particle halos via asynchronous MPI AllToAll communications, allowing particles to travel seamlessly through distributed GPU memories according to their position within the channel.

Fibres were represented using the rod-chain model, here each fibre is a chain of consecutively constrained rods. This allowed for the simulation of long and flexible fibres, but introduced major algorithmic challenges. As fibres could span to a wide section of the channel, fluid halos communications were not an option due to the huge volume of data required to be sent. Indeed, each fibre introduced a tridiagonal block-matrix system that had to be solved to keep together the consituting rod elements, sparsed across a variable number of GPU memories with an irregular communication pattern. 

To address this, I designed a novel parallelisation algorithm. A second, local static copy of each fibre was created so that all the rods of one chain resided on the same memory unit, enabling efficient constraint resolution. Fibres were load-balanced across all GPUs, and synchronisation between the travelling and static copies of the rods was performed through two on-demand MPI Send/Recv phases: one before constraint resolution to update their state, one after to broadcast the new positions.


This original scheme enabled scalable, GPU-accelerated simulations of fibre-laden turbulent flows, tested on Tier-0 supercomputers such as Marconi100, JUWELS-Booster, and Piz Daint — to our knowledge, the first approach of its kind. 