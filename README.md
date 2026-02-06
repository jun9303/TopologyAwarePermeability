# UnitPorousOpenFOAM

This repository contains an **OpenFOAM v11** configuration package for simulating flow through a unit porous lattice cell. The simulation uses a tri-periodic boundary condition setup to calculate the permeability of spatially periodic porous structures.

The case assumes a "Design-by-Morphing" (DbM) or similar lattice unit cell workflow, where the geometry is provided as an STL file.

## Features
* **Solver**: `foamRun` configured for `incompressibleFluid` (Laminar, Newtonian).
* **Mesh**: Automated generation using `blockMesh` (background hex mesh) and `snappyHexMesh` (conformal lattice meshing).
* **Boundary Conditions**: Fully cyclic (tri-periodic) in x, y, and z directions.
* **Forcing**: Automatic calculation of a volumetric body force to normalize the driving gradient based on the fluid volume fraction.
* **Parallelization**: Pre-configured for parallel execution (default: 20 processors).

## Prerequisites
* **OpenFOAM v11** (Foundation release, openfoam.org).
* MPI (for parallel execution).

## Directory Structure
* `Allrun` / `Allclean`: Main execution and cleanup scripts.
* `0/`: Initial conditions for Velocity (`U`) and Pressure (`p`), set to cyclic.
* `constant/`:
    * `triSurface/`: Directory for input geometry.
    * `g`: Used to apply the driving body force.
    * `fvModels`: Configures the `buoyancyForce` to drive the flow.
* `system/`: Solver and mesh settings (`snappyHexMeshDict`, `controlDict`, etc.).

## Quick Start

### 1. Prepare Geometry
You must provide your porous unit cell geometry as an STL file.
* **File Name**: `porousUnitLattice.stl`
* **Location**: `constant/triSurface/`
* **Dimensions**: The domain is configured as a 1x1x1 cube (vertices from -0.5 to 0.5). Ensure your STL fits within this unit box.

### 2. Run Simulation
Execute the `Allrun` script to automate mesh generation, setup, and solution:
```bash
./Allrun