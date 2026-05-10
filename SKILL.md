---
name: fesom2
description: >
  Progressive-disclosure skill for FESOM2 (Finite-volumE Sea ice-Ocean
  Model), the AWI multi-resolution ocean general circulation model on
  unstructured triangular meshes. Covers the CMake/bundle build, mesh
  partitioning, namelists (namelist.config, namelist.oce, namelist.ice,
  namelist.forcing), the configure_*.sh helpers, the docs site, and
  the pyfesom2 / tripyview analysis ecosystem.
version: 0.1.0-scaffold
tags:
  - earth-science
  - ocean-model
  - sea-ice
  - unstructured-mesh
  - finite-volume
  - finite-element
  - fesom
  - awi
  - fortran
---

# FESOM2 (Finite-volumE Sea ice-Ocean Model) Guide

> **FESOM2** = AWI Finite-volumE Sea ice-Ocean Model, multi-resolution unstructured-mesh ocean GCM.
> Maintainer: Alfred Wegener Institute (AWI)
> Source: https://github.com/FESOM/fesom2
> Docs: https://fesom2.readthedocs.io
> Website: https://fesom.de
> Analysis: https://github.com/FESOM/pyfesom2 , https://github.com/FESOM/tripyview
> Skill author: Koutian Wu (ktwu01@gmail.com)
> Skill version: 0.1.0-scaffold

**What FESOM2 does:** Solves the ocean and sea-ice equations on an **unstructured triangular mesh** using **finite-volume** methods. (FESOM v1.4 used finite-element; the FE → FV transition was the primary motivation for v2 and gained ~3-5x performance.) The mesh is triangular horizontally and prismatic/layered vertically, with a **cell-vertex (A-grid)** discretization, velocities and tracers co-located. The unstructured mesh allows native multi-resolution: dense triangulation in a region of interest (Arctic, Gulf Stream, eddy-rich coast) coexisting with coarse mesh elsewhere. Used in AWI-CM (atmosphere–ocean coupled with OpenIFS or ECHAM) and standalone.

**Reference:** Danilov, S., Sidorenko, D., Wang, Q., and Jung, T. (2017), *The Finite-volumE Sea ice–Ocean Model (FESOM2)*, Geosci. Model Dev., 10, 765–789, https://doi.org/10.5194/gmd-10-765-2017.

**Who this skill is for:** Researchers running FESOM2 standalone, building a new mesh, post-processing FESOM2 output, or coupling it to an atmosphere.

---

## Quick Decision Tree

```
"What do I need?"
│
├─ 🆕 What is FESOM2 and why unstructured meshes?
│  └─ Read: reference/overview.md
│
├─ 🛠️ Build FESOM2 (configure.sh / configure_any.sh / CMake)
│  └─ Read: reference/build.md
│
├─ 🔺 Meshes: where to get one, how to partition
│  └─ Read: reference/meshes.md
│
├─ 📝 Configure a run (namelist.config, namelist.oce, namelist.ice, namelist.forcing)
│  └─ Read: reference/namelists.md
│
├─ 📊 Output and post-processing (pyfesom2, tripyview)
│  └─ Read: reference/output-and-analysis.md
│
├─ 🔗 Coupling (OASIS3-MCT, OpenIFS via AWI-CM)
│  └─ Read: reference/coupling.md
│
└─ 🐛 Build or run failures
   └─ Read: reference/debugging.md
```

---

## Repo Layout (verified from clone)

```
fesom2/
├── bin/                    # Helper scripts
├── bundle.yml              # Bundle config (FESOM2 + dependencies)
├── cmake/                  # CMake support
├── CMakeLists.txt          # Top-level CMake build
├── CMakePresets.json       # CMake presets for common machines
├── config/                 # Default namelists for distributed setups
├── configure_any.sh        # Generic configure helper
├── configure.sh            # Configure helper for known machines
├── docs/                   # Sphinx documentation source
├── env/                    # Per-machine environment (modules)
├── env.sh                  # Source-this env helper
├── fpost2/                 # Fortran post-processing utilities
├── lib/                    # External libraries vendored
├── mesh_part/              # Mesh partitioning tool (pre-run)
├── setups/                 # Example experiment setups
└── src/                    # FESOM2 Fortran source
```

---

## Critical Rules

1. **Mesh partitioning is a pre-run step.** Run `mesh_part` once per (mesh, MPI ranks) combination to generate the partition files; only then can the model run.
2. **CMake is the modern build path.** `configure.sh` / `configure_any.sh` wrap CMake for convenience. Presets in `CMakePresets.json`.
3. **Four core namelists, plus more if specific schemes are enabled.** `namelist.config` (run control), `namelist.oce` (ocean physics), `namelist.ice` (sea ice), `namelist.forcing` (atmospheric forcing fields). If you enable CVMix vertical mixing (the standard AWI configuration), you also need `namelist.cvmix`. All required namelists must be in the run directory.
4. **Output is on the unstructured mesh.** You cannot open it in standard tools without mesh metadata; use `pyfesom2` / `tripyview` or interpolate to a regular grid first.
5. **Mesh choice dominates everything.** Resolution, walltime, memory, and physics behavior are all controlled by the mesh. Don't tune namelists before checking mesh quality.

---

## Reference Index

| File | Topic |
|---|---|
| reference/overview.md | FESOM2 design, multi-resolution rationale |
| reference/build.md | configure.sh, CMake, presets |
| reference/meshes.md | Mesh formats, partitioning, where to get meshes |
| reference/namelists.md | The four namelists |
| reference/output-and-analysis.md | pyfesom2, tripyview, regridding |
| reference/coupling.md | OASIS3-MCT, AWI-CM |
| reference/debugging.md | Common errors |

## Critical agent gotchas (Gemini-reviewed)

- **Restart and time tracking via `clock.txt`.** Job-chain management requires reading and updating this file; the model state is not self-describing through restart files alone.
- **`mesh_part` produces `dist_info`** (a directory or file set with PE-decomposition info). Verify `dist_info` exists for your target rank count before launching, not just the partition output.
- **METIS or SCOTCH required for large meshes.** The partitioner needs one of these graph-partitioning libraries linked in to handle high-resolution meshes.
- **Vertical coordinate options matter:** $z^*$ (ALE-style), $z$-level with partial cells, sigma. Selected in `namelist.config`; affects stability and conservation properties.
- **Required input files beyond mesh:** `bathymetry.nc` and initial conditions (commonly `ts.ocean.nc` or similar). A bare mesh is not a runnable configuration.

## Status

Scaffold (v0.1.0-scaffold). Source-grounded layout verified, with Gemini critique pass on 2026-05-09 to clarify FV (not FE), grid type (cell-vertex/A-grid), and required additional namelists/files. Operational depth being filled in.
