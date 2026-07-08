# Step 12 — 2D Navier-Stokes: Channel Flow with Periodic Boundary Conditions

> **Part of a self-directed learning path toward data-driven CFD research**  
> Based on [Lorena A. Barba's 12 Steps to Navier-Stokes](https://github.com/barbagroup/CFDPython)

---

## Overview

This notebook implements **Step 12** of Barba's CFDPython course: two-dimensional incompressible channel flow driven by a uniform body force, discretised with finite differences on a staggered grid.

Beyond reproducing the original tutorial, this implementation adds:

-  **Corrected periodic boundary conditions** in the pressure source term `b` — the original simplification is replaced with the proper wrapped finite-difference stencil at both `x = 0` and `x = 2`
-  **Convergence history plot** — `udiff` logged at every timestep and plotted on a log scale
-  **Analytical validation** — numerical steady-state profile compared against the exact Poiseuille solution, with L2 relative error computed
-  **Animated velocity field** — evolution from rest to steady state rendered as an inline HTML animation
-  **Full mathematical derivation** in Markdown cells — governing equations, discretisation scheme, boundary condition formulation, and expected analytical result

---

## Physical problem

We solve the **incompressible Navier-Stokes equations** on a $[0,2] \times [0,2]$ domain:

$$\frac{\partial u}{\partial t} + u\frac{\partial u}{\partial x} + v\frac{\partial u}{\partial y} = -\frac{1}{\rho}\frac{\partial p}{\partial x} + \nu\nabla^2 u + F$$

$$\frac{\partial v}{\partial t} + u\frac{\partial v}{\partial x} + v\frac{\partial v}{\partial y} = -\frac{1}{\rho}\frac{\partial p}{\partial y} + \nu\nabla^2 v$$

$$\frac{\partial u}{\partial x} + \frac{\partial v}{\partial y} = 0$$

**Boundary conditions:**
- Periodic in $x$: flow repeats in the streamwise direction
- No-slip at $y = 0$ and $y = 2$: $u = v = 0$ at both walls
- Constant body force $F$ drives the flow in the x-direction

**Analytical steady-state solution (Poiseuille flow):**

$$u_{analytical}(y) = \frac{F}{2\nu}\,y(2 - y)$$

---

## Numerical method

| Aspect | Choice |
|--------|--------|
| Time integration | Explicit first-order Euler |
| Spatial discretisation | Second-order central differences |
| Pressure solver | Iterative relaxation (Poisson equation) |
| Periodic BCs | Explicit wrapped stencil at `i=0` and `i=nx-1` |
| Convergence criterion | Relative change in $\sum u$ between timesteps |

---

## Results

| Metric | Value |
|--------|-------|
| Convergence | Reached in < 1000 timesteps |
| Poiseuille L2 error | < 1% |
| Periodic BCs | Correctly applied in `b`, `p`, `u`, and `v` updates |

The numerical solution converges cleanly to the parabolic Poiseuille profile, confirming second-order accuracy on this grid.

---

## Repository structure

```
├── Step_12_improved.ipynb   # Main notebook — run top to bottom
└── README.md                # This file
```

---

## How to run

```bash
# Install dependencies
pip install numpy matplotlib

# Launch Jupyter
jupyter notebook Step_12_improved.ipynb
```

The notebook is self-contained. All parameters are defined in a single cell near the top and clearly labelled.

---

## What comes next

This notebook is the foundation for the next stage of this learning path:

1. **POD analysis** — apply Proper Orthogonal Decomposition to the stored velocity snapshots (`history_u`) to extract the dominant flow modes and their energy content
2. **Reduced-order model (ROM)** — reconstruct the transient evolution from a truncated POD basis and compare against the full simulation
3. **Data-driven surrogate** — extend the approach to real Fluent simulation data, building a ROM that predicts flow fields at unseen parameter values without running the full solver


---

## References

- Barba, L.A. and Forsyth, G. (2018). *CFD Python: the 12 steps to Navier-Stokes*. Journal of Open Source Education. [doi:10.21105/jose.00021](https://doi.org/10.21105/jose.00021)
- Brunton, S.L., Noack, B.R., and Sagaut, P. (2020). *Machine Learning for Fluid Mechanics*. Annual Review of Fluid Mechanics. [doi:10.1146/annurev-fluid-010719-060214](https://doi.org/10.1146/annurev-fluid-010719-060214)
- Raissi, M., Perdikaris, P., and Karniadakis, G.E. (2019). *Physics-informed neural networks*. Journal of Computational Physics. [doi:10.1016/j.jcp.2018.10.045](https://doi.org/10.1016/j.jcp.2018.10.045)

---

## Author

**Markel Garrobo** — CFD Research Engineer  
[LinkedIn](https://www.linkedin.com/in/markel-garrobo/) · [GitHub](https://github.com/markelgarrobo)

*Building skills in data-driven fluid mechanics, reduced order modelling, and scientific machine learning.*

