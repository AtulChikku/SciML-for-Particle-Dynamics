# 🧲 Universal Differential Equations for Charged Particle Tracking (HEP)

> Physics-anchored, differentiable modeling of charged particle dynamics in high-energy physics detectors using Julia's SciML ecosystem.

---

## 📌 Project Overview

This project develops a **Universal Differential Equation (UDE)** framework to model charged particle trajectories inside a solenoidal magnetic field, inspired by tracking environments in high-energy physics (HEP) experiments.

Instead of using purely numerical solvers or black-box neural networks, this work:

- Anchors dynamics to the **relativistic Lorentz force**
- Introduces a **neural correction term** to learn unmodeled momentum distortions
- Trains using trajectory-matching loss on high-fidelity simulated detector data
- Produces a **fully differentiable particle tracking surrogate**

The primary dataset used is the TrackML particle tracking simulation dataset.

---

## 🎯 Motivation

Particle tracking in modern collider experiments is traditionally performed using numerical propagation in magnetic fields combined with Kalman filtering and detailed transport simulations.

However:

- Classical simulators are not differentiable
- Unknown medium effects are manually approximated
- Large-scale simulations are computationally expensive

This project explores:

> Can we learn missing corrections to charged particle motion using a physics-informed neural differential equation while preserving known physical laws?

---

## ⚙️ Mathematical Formulation

The baseline equation of motion is the Lorentz force:

**dp/dt = q (𝒗 × B)**

where:
- q = particle charge
- B = solenoidal magnetic field (assumed along z-direction)
- 𝒗 = p / |p|  (ultra-relativistic approximation)

The full state vector is:

**u = [x, y, z, px, py, pz]**

The corresponding ODE system is:

**dx/dt = v**  
**dp/dt = q (v × B)**

---

### Universal Differential Equation (UDE)

We augment the classical Lorentz dynamics with a neural correction term:

**dp/dt = q (v × B) + fθ(p, x, s)**

where:

- fθ = neural network correction term  
- s = arc-length proxy for time  
- The neural network learns residual momentum distortions

Training is performed via **trajectory matching**, minimizing:

**|| x_pred(s) − x_true(s) ||**

rather than directly supervising dp/dt.

## 📂 Dataset

The project uses the **TrackML dataset**:

- Simulated charged particle tracks in a solenoidal magnetic field
- Provides:
  - Detector hit positions
  - Ground-truth momenta at each hit
  - Particle charge information

Preprocessing steps include:

- Filtering charged particles
- Grouping hits by `particle_id`
- Constructing arc-length ordering
- Removing looping or low-momentum tracks
- Saving reduced `.jld2` datasets for training

---

## 🛠️ Tech Stack

- Julia
- DifferentialEquations.jl
- DiffEqFlux.jl
- Flux.jl
- DataFrames.jl
- StaticArrays.jl
- JLD2.jl

---

## 🚧 Current Status (Ongoing)

### ✔ Completed
- TrackML preprocessing pipeline
- Arc-length trajectory construction
- Looper filtering
- Lorentz-force anchor ODE implementation
- Reduced dataset generation

### 🔄 In Progress
- Magnetic field magnitude estimation from curvature
- Neural correction term integration
- UDE training loop implementation
- Trajectory-matching loss definition
- Stability analysis and regularization

### ⏳ Planned
- Generalization tests on unseen tracks
- Comparison to pure Lorentz propagation
- Visualization of learned correction term
- Performance benchmarking

---

## 🧠 Research Goals

- Build a differentiable surrogate for charged particle transport
- Learn physically meaningful momentum correction terms
- Explore SciML applications in detector simulation acceleration
- Evaluate robustness under realistic tracking distortions

---

## 🔬 Notes

- Magnetic field is modeled as a uniform solenoidal field (validated from track curvature).
- Electric field is assumed negligible within the tracking volume.
- Momentum derivatives are not directly supervised; trajectory consistency is used instead.
- Project is currently under active development.

---

## 📬 Author

Physics undergraduate exploring Scientific Machine Learning applications in High-Energy Physics.
