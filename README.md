Here is a clean, comprehensive, and professional GitHub README description tailored for your project. It is structured to look highly technical and visually organized for anyone visiting your repository.

---

# Projection Methods for the Kepler Problem

A comprehensive study and implementation of structure-preserving numerical integration methods applied to the classical **Kepler Problem** (modeling two-body planetary motion under an inverse-square gravitational force).

This project explores why traditional numerical solvers (like Forward/Backward Euler) suffer from long-term geometric degradation—causing orbits to unrealistically spiral inward or outward—and implements **Manifold Projection Methods** to enforce the strict preservation of physical invariants like **Total Energy** and **Angular Momentum**.

---

## Project Overview

When simulating orbital mechanics, standard numerical integrators introduce cumulative truncation errors that destroy the system's geometric structure over time. This project implements and analyzes a variety of numerical solutions, contrasting traditional methods against structure-preserving alternatives.

### Key Objectives

* **Mathematical Validation:** Proving the boundaries of existence and uniqueness of the Kepler problem via local vs. global variants of the **Picard-Lindelöf Theorem**.
* **Error Analysis:** Analyzing the local and global truncation errors ($O(h)$ vs $$O(h^4)$$) of explicit, implicit, and higher-order integrators.
* **Invariant Mapping:** Tracking the unphysical gain/loss of system invariants over extended iterations.
* **Manifold Projections:** Utilizing the **Lagrange Multipliers Method** to correct numerical drift by shifting drifted states back onto the exact Energy ($E$) and Angular Momentum ($L$) manifolds.

---

## Numerical Solvers Implemented

The repository contains implementations of several distinct ordinary differential equation (ODE) solvers to benchmark their performance:

| Solver Category | Method | Order (Global) | Structural Stability |
| --- | --- | --- | --- |
| **Standard Explicit** | Forward Euler | $O(h)$ | Poor (Spirals Outward, Gains Energy) |
| **Standard Implicit** | Backward Euler | $O(h)$ | Poor (Spirals Inward, Loses Energy) |
| **Higher-Order** | Runge-Kutta 4 (RK4) | $O(h^4)$ | High Precision (Drifts very slowly) |
| **Geometric/Symplectic** | Symplectic Euler, Störmer-Verlet, Implicit Midpoint | Variable | Excellent (Preserves phase-space volume) |
| **Structure-Enforced** | **Projected Integrators** (Energy / Angular Momentum / Both) | Dependent | Perfect Invariant Conservation |

---

## Projection Framework

To eliminate numerical drift without drastically diminishing step sizes ($h$), we solve a constrained optimization problem at each time step using Lagrange multipliers:

$$\min \left( \frac{1}{2} \|u_{n+1} - \tilde{u}_{n+1}\|^2 \right) \quad \text{subject to} \quad g(u_{n+1}) = 0$$

Where $$\tilde{u}_{n+1}$$ is the uncorrected state produced by a standard step, $$u_{n+1}$$ is the corrected target state, and $$g(u)$$ represents our physical constraints:

1. **Projection on Energy Manifold:** Enforces $$E(u_{n+1}) - E(u_n) = 0$$
2. **Projection on Angular Momentum (AM) Manifold:** Enforces $$L(u_{n+1}) - L(u_n) = 0$$
3. **Simultaneous Projection (Energy & AM):** Solves both nonlinear geometric constraints simultaneously using root-finding algorithms (e.g., `fsolve`).

---

## Key Findings & Visualizations

* **The Classical Drift:** Standard Forward Euler trajectories artificially spiral away from the central mass due to exponential energy growth, while Backward Euler collapses inward onto the singularity.
* **The Projection Remedy:** Applying state projections forces the system to preserve its underlying physics. Even when using a highly unstable baseline solver like Forward Euler with a large step size ($h = 0.005$), enforcing energy and angular momentum constraints keeps the planet locked into stable, perfectly closed elliptical orbits over long temporal durations.

---

## References & Literature

This implementation is heavily informed by core mathematical principles and reference materials in geometric integration, including:

* *Geometric Numerical Integration: Structure-Preserving Algorithms for ODEs* (Hairer, Lubich, Wanner).
* *Grundlagen der Numerischen Mathematick* (Hanke-Bourgeois).
* Classical applications of *Noether's Theorem* regarding continuous symmetries and conservation laws.
