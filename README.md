# 🚀 Projectile Motion Simulation with Air Resistance

A physics-based numerical simulation in Python that models and compares ideal projectile motion (no air resistance) with motion under **linear** and **quadratic** drag forces. Built using **NumPy** and **Matplotlib**, this project demonstrates how classical analytical solutions break down under realistic resistive forces, and how numerical integration can bridge that gap.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Theoretical Framework](#theoretical-framework)
- [Numerical Implementation](#numerical-implementation)
- [Repository Structure](#repository-structure)
- [System Requirements & Setup](#system-requirements--setup)
- [Key Insights Verified](#key-insights-verified)
- [Results at a Glance](#results-at-a-glance)

---

## Overview

In an ideal world, projectile motion follows clean parabolic trajectories described by closed-form equations. In practice, air resistance fundamentally alters both the shape of the trajectory and the energy profile of the projectile. This project implements three physical models side by side:

- **No drag** — ideal projectile motion solved analytically
- **Linear drag** — resistive force proportional to velocity (valid for slow-moving objects in viscous fluids)
- **Quadratic drag** — resistive force proportional to the square of velocity (valid for fast-moving objects in air)

All drag-affected cases are solved using **Euler time-stepping integration**, since the resulting differential equations have no simple closed-form solution.

---

## Theoretical Framework

### 1. No Drag (Ideal Projectile Motion)

The equations of motion under gravity alone:

$$x(t) = u \cos\theta \cdot t$$

$$y(t) = u \sin\theta \cdot t - \frac{1}{2}g t^2$$

Derived quantities:

$$T = \frac{2u\sin\theta}{g}, \quad H = \frac{u^2\sin^2\theta}{2g}, \quad R = \frac{u^2\sin 2\theta}{g}$$

where $u$ is the initial speed, $\theta$ the launch angle, $g$ the gravitational acceleration, $T$ the time of flight, $H$ the maximum height, and $R$ the horizontal range.

---

### 2. Linear Drag

The drag force is proportional to velocity: $\vec{F}_{drag} = -k\vec{v}$

This gives coupled differential equations for the accelerations:

$$a_x = -k \cdot v_x$$

$$a_y = -g - k \cdot v_y$$

where $k$ is the linear drag coefficient (units: s⁻¹). These equations do not yield a simple closed-form solution for trajectory, requiring numerical integration.

---

### 3. Quadratic Drag

The drag force is proportional to the square of the speed: $\vec{F}_{drag} = -k|\vec{v}|\vec{v}$

The accelerations become:

$$a_x = -k \cdot |\vec{v}| \cdot v_x$$

$$a_y = -g - k \cdot |\vec{v}| \cdot v_y$$

where $|\vec{v}| = \sqrt{v_x^2 + v_y^2}$ and $k$ is the quadratic drag coefficient (units: m⁻¹). This model is more realistic for objects moving through air at moderate to high speeds.

---

## Numerical Implementation

All drag cases are solved using **forward Euler integration** — a first-order numerical time-stepping method. At each time step `dt`, the current accelerations are computed from the current velocity state and used to update velocities and positions:

```python
# Euler integration loop — Linear Drag example
vx = u * np.cos(theta)   # initial horizontal velocity
vy = u * np.sin(theta)   # initial vertical velocity
x, y = 0, 0
dt = 0.01                # time step (seconds)

X, Y = [], []

while y >= 0:
    # Compute accelerations from current state
    ax = -k * vx
    ay = -g - k * vy

    # Update velocities
    vx += ax * dt
    vy += ay * dt

    # Update positions
    x += vx * dt
    y += vy * dt

    X.append(x)
    Y.append(y)
```

The loop continues until the projectile returns to ground level (`y < 0`). The same pattern applies to the quadratic drag case, with the drag terms scaled by the instantaneous speed $|\vec{v}|$.

> **Step size:** A value of `dt = 0.01 s` is used throughout, which offers a good balance between numerical accuracy and computational efficiency for the velocity and length scales involved.

---

## Repository Structure

```
📦 Projectile-Motion-Simulation
│
├── 📓 Projectile_motion_only_under_gravity.ipynb
├── 📓 Varying_Angles_for_no_drag_motion.ipynb
├── 📓 Varying_Angles_for_linear_drag_motion.ipynb
├── 📓 Varying_linear_Drag_Coefficient.ipynb
├── 📓 Comparison_of_Projectile_motion_under_no_drag__linear_drag_and_quadratic_drag.ipynb
├── 📓 KE_vs_Time_for_all_cases.ipynb
└── 📄 README.md
```

### Notebook Descriptions

**`Projectile_motion_only_under_gravity.ipynb`**
The foundation of the project. Models ideal projectile motion using closed-form analytical equations (no drag). Computes and prints key quantities — time of flight, maximum height, and range — and plots both the trajectory (x–y) and the velocity components ($v_x$, $v_y$) as functions of time. Serves as the baseline for all subsequent comparisons.

**`Varying_Angles_for_no_drag_motion.ipynb`**
Extends the ideal model by sweeping over multiple launch angles (20°, 30°, 35°, 45°, 60°, 75°) and overlaying their trajectories on a single plot. Confirms the classical result that 45° maximises range in the absence of drag, and provides intuition for how launch angle shapes the parabolic arc.

**`Varying_Angles_for_linear_drag_motion.ipynb`**
Repeats the multi-angle trajectory study under linear drag using Euler integration. Demonstrates how air resistance breaks the symmetry of the parabola, reduces range, and shifts the optimal launch angle below 45°. Each angle is simulated independently in a loop with a shared drag coefficient `k = 0.03`.

**`Varying_linear_Drag_Coefficient.ipynb`**
Fixes the launch angle at 30° and varies the quadratic drag coefficient across a range of values (`k = 0.01` to `0.05`). Isolates the effect of drag strength on trajectory, showing how increasing $k$ progressively shortens range and lowers peak height — useful for understanding the sensitivity of the model to the drag parameter.

**`Comparison_of_Projectile_motion_under_no_drag__linear_drag_and_quadratic_drag.ipynb`**
The centrepiece comparison notebook. Simulates all three physical models (no drag, linear drag, quadratic drag) for the same initial conditions (u = 40 m/s, θ = 30°) and overlays their trajectories on a single plot. Directly illustrates how each successive model departs from the ideal parabola and by how much.

**`KE_vs_Time_for_all_cases.ipynb`**
Analyses the **kinetic energy** evolution over time for all three drag cases. For the no-drag case, KE follows a smooth U-shaped curve (minimum at peak height). Under linear and quadratic drag, KE is monotonically reduced by energy dissipation. Highlights the thermodynamic consequence of resistive forces and reinforces the physical interpretation of each model.

---

## System Requirements & Setup

### Prerequisites

- Python 3.8 or higher
- Jupyter Notebook or JupyterLab

### Dependencies

| Package | Purpose |
|---|---|
| `numpy` | Array operations, vector maths, `linspace` |
| `matplotlib` | All trajectory and energy plots |

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/tianly0/Computational Simulation and Analysis of Projectile motion under Drag force.git

   ```

2. **Install dependencies**
   ```bash
   pip install numpy matplotlib jupyter
   ```

3. **Launch Jupyter**
   ```bash
   jupyter notebook
   ```

4. Open any notebook from the file browser and run all cells (`Kernel → Restart & Run All`).

> **Recommended order:** Start with `Projectile_motion_only_under_gravity.ipynb` to build intuition before moving to the drag and comparison notebooks.

---

## Key Insights Verified

**Drag reduces range and maximum height.** Both linear and quadratic drag models produce shorter, lower trajectories compared to the ideal parabola under identical initial conditions.

**The optimal launch angle shifts below 45°.** Without drag, 45° maximises range. With air resistance, the optimal angle drops (typically toward 30°–40°), depending on the drag coefficient and launch speed.

**Quadratic drag is more aggressive than linear drag at high speeds.** For fast projectiles, the $v^2$ dependence of quadratic drag dissipates energy much more rapidly, producing significantly reduced range compared to the linear model.

**Trajectory asymmetry under drag.** The descending half of the trajectory becomes steeper than the ascending half — a direct consequence of velocity-dependent drag breaking the time-reversal symmetry of the parabola.

**Kinetic energy decreases monotonically under drag.** In the no-drag case, KE reaches a minimum at peak height and recovers on descent. Under any drag force, energy is continuously removed from the system, so the projectile lands with less KE than it was launched with.

**Numerical step size matters.** A time step of `dt = 0.01 s` yields stable and visually smooth trajectories. Larger steps introduce visible integration error; smaller steps increase accuracy at the cost of runtime.

---

## Physical Parameters Used

| Parameter | Symbol | Value |
|---|---|---|
| Initial velocity | $u$ | 40 m/s |
| Gravitational acceleration | $g$ | 9.8 m/s² |
| Default launch angle | $\theta$ | 30° |
| Mass (KE calculations) | $m$ | 1 kg |
| Time step | $dt$ | 0.01 s |
| Linear drag coefficient (comparison) | $k_1$ | 0.05 s⁻¹ |
| Quadratic drag coefficient (comparison) | $k_2$ | 0.1 m⁻¹ |

