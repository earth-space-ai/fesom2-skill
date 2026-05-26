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

## Acknowledgments

**Gold-standard references for FESOM2** (use these to cross-check anything in this skill):
- FESOM/fesom2 repository: https://github.com/FESOM/fesom2
- FESOM2 wiki and docs: https://github.com/FESOM/fesom2/wiki
- pyfesom2 analysis toolkit: https://github.com/FESOM/pyfesom2
- tripyview analysis toolkit: https://github.com/FESOM/tripyview

This scaffold exists only because of the work of other people, and any value
it has is borrowed from theirs.

- The **Alfred Wegener Institute (AWI)** and the FESOM2 developer community
  for building and maintaining
  [FESOM/fesom2](https://github.com/FESOM/fesom2), the unstructured-mesh
  dynamical core, the mesh partitioning toolchain, and the analysis
  ecosystem (`pyfesom2`, `tripyview`) this skill points users toward.
- **Zesen Huang** for [laps-skill](https://github.com/huangzesen/laps-skill),
  the progressive-disclosure layout this repo borrows.

Any errors, oversimplifications, or out-of-date claims in this skill are the
skill author's responsibility, not the upstream community's. This is a
scaffold; operational depth is being filled in iteratively.

## License

MIT (skill content). FESOM2 is GPL/LGPL; see `LICENSE.txt` and `COPYING.LESSER.txt` upstream.
