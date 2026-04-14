# SN1 Reaction Monte Carlo Simulation

A Monte Carlo simulation of t-butyl chloride ion pair separation in explicit water, written as a final project for the course *Molecular Modeling Methods* (CBE), Princeton University, October 2024.

> **Note:** This is coursework from 2024. It reflects my coding style at the time of writing — uploaded as an archive of work I'm proud of.

## What it simulates

In an SN1 reaction, a substrate spontaneously ionizes into a cation and leaving group stabilized by surrounding solvent. This simulation models the separated t-Bu⁺ / Cl⁻ ion pair in explicit water and asks: **how does the equilibrium ion-pair separation distance change with temperature?**

This is inspired by Jorgensen et al. (JACS, 1987), who studied the same system at fixed ion-pair distances to compute a free energy profile. Here, the ions are free to move, and the equilibrium separation is observed directly across 5 temperatures (280–360 K).

## Implementation

The simulation uses a **Metropolis Monte Carlo** algorithm (NVT ensemble) with:
- **Lennard-Jones + Coulombic potentials** with Lorentz-Berthelot combining rules
- **Explicit TIP4P water** (4-site model: O, H, H, and lone-pair M site)
- **Periodic boundary conditions** with minimum-image convention
- **Molecule-frame rotations** using change-of-basis matrices, so rotations respect each molecule's internal geometry regardless of its current orientation in the box
- A **smooth cutoff/truncation scheme** for O-involving interactions (following Jorgensen et al.), tapering from r_L = 7.5 Å to r_U = 8.0 Å

System: 125 molecules in a 15 Å cubic box (123 water + t-Bu⁺ + Cl⁻), density 0.0334 molecules/Å³.

## Code structure

Three OOP classes in `sn1.ipynb`:

- **`interactSite`** — an atom or interaction site with element type, relative position, and force field parameters (q, σ, ε)
- **`molecule`** — a collection of interaction sites around a center-of-mass; supports translation, rotation in the molecule's own frame via change-of-basis, and copy
- **`MC`** — the simulation engine: initializes a cubic lattice of water with the ion pair at center, runs the Metropolis walk, and writes energy/acceptance/distance trajectories to file

## Results

- Ion-pair separation distance increases with temperature across 280–360 K, consistent with entropic stabilization of the dissociated state
- Energy converges over the course of simulations at all temperatures
- Energy is not a simple function of ionic distance — solvent configuration plays a dominant role, illustrating the importance of explicit solvent in SN1 kinetics
- See `Sn1WriteUp.pdf` for full analysis and figures

## Dependencies
numpy
matplotlib
## References

Jorgensen, W. L., Buckner, J. K., Huston, S. E., & Rossky, P. J. (1987). Hydration and energetics for tert-butyl chloride ion pairs in aqueous solution. *JACS*, 109(7), 1891–1899.
