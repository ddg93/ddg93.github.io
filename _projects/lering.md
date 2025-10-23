---
layout: page
title: "A Deep Learning approach to measure particles suspended in fluid flows"
---
<div style="border: 2px solid #ccc; padding: 15px; background-color: #f9f9f9; font-size: 1.2em; line-height: 1.5; border-radius: 8px;">  
  Measuring the orientation of axi-symmetric particles, like cylinders or spheroids, can be quite challenging, but geometrical relations simplify the problem.
  When considering more complex shapes, like axi-symmetric and asymmetric rings, we can train Convolutional Neural Networks on synthetic data to perform this task. 
</div>

During my post-Doc at the Aix-Marseille Université, I designed a pipeline to measure particle orientations in a linear shear cell with two perpendicular cameras. This approach is inspired by the [LeNet-5](https://en.wikipedia.org/wiki/LeNet), introducing two heads that process both **top** and **side** frames at the same times.


<figure>
  <img src="{{ site.baseurl }}/images/pipeline.jpg" alt="Two-headed LeNet pipeline" width="640">
  <figcaption>
    The pipeline is as follows: a given particle geometry, represented by a '.stl' file (a), is used as the basis for the generation of a synthetic data set in Blender (b). This data set is then used to train a Deep Learning model, with the objective of estimating the particle orientation given two perpendicular projections (c). A physical particle corresponding to the '.stl' file is also created through rapid prototyping (d) and employed in the experiments (e). Subsequently, the Watershed method is applied to the recorded data from the experiments prior to the Deep Learning model inference operation, which estimates the time-evolution of the three-dimensional particle orientation vector $\mathbf{n}$ in the given experiment.
  </figcaption>
</figure>


## Pipeline Overview  

### 1. Input  
The pipeline starts with the creating of a particle **STL file** with given charateristics, like particle shape and particle aspect ratio.

### 2. Two separated preparation branches
Starting from a particle STL file, two parallel branches are initiated:  
- **Synthetic dataset generation**: Using Blender, we render paired frames from two perpendicular camera views, similar to [the experimental setup of our experiments](/projects/experiments/). Each couple of images is annotated with the particle’s imposed orientation (the ground truth label). This kind of synthetic data is made available in [this repository](https://huggingface.co/datasets/ddg93/LeRing_JFM_experiments/tree/main) for selected particle shapes. 
- **Experimental preparation**: The same STL file is **3D printed** to fabricate real particles for laboratory experiments. 

<div style="text-align: center;">
  <p><strong>Samples of coupled side-top frames for a ring with circular section and aspect ratio 0.45</strong></p>
  <div style="display: flex; justify-content: center; gap: 20px;">
    <div style="text-align: center;">
      <img src="{{ site.baseurl }}/images/training_dataset.jpg" width="300" alt="Top view">
      <p><em>Synthetic training dataset. Particle orientations are known.</em></p>
    </div>
    <div style="text-align: center;">
      <img src="{{ site.baseurl }}/images/time_evolution.jpg" width="300" alt="Side view">
      <p><em>Binarized recordings of one experiment.</em></p>
    </div>
  </div>
</div>

### 3. Experiments  
Particles are recorded simultaneously from **two orthogonal views** (top and side) while rotating in the viscous shear flow. These videos provide the raw experimental dataset, partially hosted in [this repository](https://huggingface.co/datasets/ddg93/LeRing_JFM_experiments/tree/main). Minimal **CV preprocessing** is applied for particle tracking.

### 4. Training & Inference  
- A **two-headed CNN**, adapted from the LeNet-5 architecture, is trained on the synthetic dataset, learning to regress the particle’s 3D orientation from the two-view image pairs.  
- The trained model is then applied to the experimental recordings to infer the 3D particle orientation for each frame.

### 5. Output  
The pipeline returns one **time series of 3D orientations** for each experiment, reconstrcuting the **rotational dynamics** of particles suspended in viscous shear flows.
We can visualize a typical result in the following animation, where the top and side experimental recordings are displayed on the left together with the reconstructed particle rotational dynamics for a ring with triangular section. The corresponding training data and experimental recordings are freely available in [this repository](https://huggingface.co/datasets/ddg93/LeRing_JFM_experiments/tree/main/TR_r008).

## LeRing: a Two-headed LeNet-5 implementation  
You can find the Python implementation of our two-headed CNNs in [this GitHub repository](https://github.com/ddg93/LeRing_JFM).

## Data
I am uploading experimental recordings as well as synthetic datasets for multiple particle geometries in [this Hugging Face repository](https://huggingface.co/datasets/ddg93/LeRing_JFM_experiments/tree/main). Researchers interested in this problem and inspired by our approach are invited to play with our data and contribute.
