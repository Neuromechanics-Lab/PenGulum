# PenGulum

MATLAB tools for simulating and analyzing a single‑link inverted pendulum (“pen*g*ulum”) with human‑balance style controllers. Includes example data and several state‑derivative models (e.g., velocity/force feedback variants).

---

## Features

* Clean MATLAB implementation of a planar pendulum model
* Multiple dynamics functions (state derivatives) for alternative control/feedback assumptions
* Reproducible script to run simulations end‑to‑end
* Example inputs and data files to get started quickly

## Repository structure

```
PenGulum/
├─ PenGulum.m                       # Main script / entry point
├─ pendulumStateDerivative.m        # Base dynamics (no explicit feedback)
├─ pendulumStateDerivative_SRS.m    # Dynamics variant (e.g., sway‑referenced/feedback)
├─ pendulumStateDerivative_SRS_Ffb.m# Dynamics with force‑feedback
├─ pendulumStateDerivative_SRS_vfb.m# Dynamics with velocity‑feedback
├─ inputdata.mat                    # Example parameter/input struct(s)
├─ Data Examples/                   # Optional example outputs/plots
└─ LICENSE                          # GPL‑3.0
```

> **Note:** File names are taken from the repo; if a description is off, please update it here.

## Requirements

* MATLAB (R2020b or newer recommended)
* No toolboxes are strictly required for ODE solving (uses built‑in `ode45/ode15s`‑style solvers). If a specific solver or toolbox is required in your environment, add it here.

## Quick start

1. **Clone the repo**

   ```bash
   git clone https://github.com/Neuromechanics-Lab/PenGulum.git
   cd PenGulum
   ```
2. **Open MATLAB** and add the folder to your path:

   ```matlab
   addpath(genpath(pwd))
   ```
3. **Load example inputs** (optional):

   ```matlab
   S = load('inputdata.mat');
   params = S.params;     % or adjust to the variable name actually saved
   u      = S.inputs;     % e.g., perturbations / reference signals
   ```
4. **Run the main script**:

   ```matlab
   PenGulum   % or run section‑by‑section inside the script
   ```
5. **Switch model variants** by editing the function handle used inside `PenGulum.m`, e.g.:

   ```matlab
   f = @pendulumStateDerivative_SRS_vfb;  % or _SRS_Ffb, or base variant
   ```

## Typical workflow

* Define model parameters (mass, COM, length, gravity, damping, controller gains) in a struct, or edit defaults in the script.
* Pick a state‑derivative function reflecting the control assumption you want (base / SRS / velocity‑feedback / force‑feedback).
* Provide an input/perturbation (e.g., platform rotation/translation or torque), or use the included example input(s).
* Integrate with `ode45` (or your preferred solver) and plot state trajectories and outputs.

## Key I/O conventions (proposed)

Adjust this section to match the code:

* **State** $x = [\theta; \dot{\theta}]$ in radians and rad/s.
* **Parameters (struct)**: `params.g`, `params.m`, `params.l`, `params.b`, controller gains like `params.kp`, `params.kd`, etc.
* **Input** `u(t)`: external torque/acceleration or platform motion; verify units in your chosen derivative function.
* **Output**: time vector `t`, state `x(t)`, and optional derived measures (ankle torque, COP proxy, etc.).

## Reproducing the included examples

* Load `inputdata.mat` and run `PenGulum.m`.
* The script should generate figures similar to those in **Data Examples/** (if present). If figure names/paths differ, please update this section.

## Troubleshooting

* **States blow up / NaNs:** Try a stiff solver (`ode15s`), reduce step size, or check units/gains.
* **Wrong direction or sign:** Confirm the sign conventions in your selected state‑derivative function.
* **Input not applied:** Ensure your `u(t)` is passed through to the derivative via a closure or nested function, or is accessible as a shared variable.
* **MATLAB path issues:** Use `restoredefaultpath; rehash toolboxcache; addpath(genpath(pwd));`.

## Extending the model

* Add a new derivative function, e.g., `pendulumStateDerivative_myVariant.m` that matches the signature of the existing ones.
* Expose any new parameters in the `params` struct and document defaults at the top of your file.
* Add a small test case and (ideally) a saved plot in **Data Examples/**.


* Neuromechanics Lab

---

**Changelog**

* 2025‑08‑12: Initial README drafted.


