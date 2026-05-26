# FESOM2 Skill

Progressive-disclosure skill for [FESOM2](https://github.com/FESOM/fesom2), AWI's multi-resolution unstructured-mesh ocean and sea-ice general-circulation model.

> **Skill author:** Koutian Wu (ktwu01@gmail.com)
> **Skill version:** 0.1.0-scaffold

> ⚠️ **Disclaimer — please read before using this skill.**
> This skill is **not a gold-standard reference**. It is a helper that lowers
> the barrier for new users to **get their hands dirty** with the model. AI
> agents (and the humans drafting this material) make mistakes; commands, file
> paths, namelist options, and physics explanations here can be wrong,
> incomplete, or out of date. **Always cross-check with the official model
> documentation, the source code, and a human expert before trusting any
> output for research, publication, or operational use.**

## What This Is

A guide to building, meshing, configuring, running, and post-processing FESOM2. Covers the CMake/bundle build, the four namelists (`namelist.config`, `namelist.oce`, `namelist.ice`, `namelist.forcing`), mesh partitioning, and the `pyfesom2` / `tripyview` analysis ecosystem.

## Status

Scaffold. Layout verified against the cloned `FESOM/fesom2` tree. Operational depth being filled in.

## License

MIT (skill content). FESOM2 is GPL/LGPL; see `LICENSE.txt` and `COPYING.LESSER.txt` upstream.
