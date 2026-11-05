# PYNN

Research into deterministic neural-guided, independently verified compiler
optimization, with [MOLT](https://github.com/adpena/molt) as the intended first
integration target.

The working idea is to use small neural networks to propose or rank compiler
transformations, check equivalence independently, and feed counterexamples and
measured costs into subsequent experiments. Deterministic execution,
reproducibility, and comparisons against conventional search are core goals.

This is an initial repository scaffold. No optimizer, neural model, verifier,
benchmark harness, or MOLT adapter is implemented yet. PYNN is a standalone
research project; future integration should pin a MOLT revision and version the
IR boundary.

## Repository conventions

- Rust and Python are anticipated; commit manifests and dependency lockfiles
  when those components are introduced.
- Keep experiment definitions, seeds, toolchain versions, source revisions,
  dataset/model manifests, and checksums in version control. Record enough
  information to reproduce each accepted result.
- Put bulk datasets under `datasets/{raw,processed,cache}/`, downloaded models
  under `models/{downloads,cache}/`, and checkpoints under `checkpoints/`.
- Put generated runs in `runs/`, outputs in `outputs/`, benchmark output in
  `benchmarks/results/`, and other generated artifacts in `artifacts/generated/`.
  These paths are ignored. Keep curated reports in a separate tracked location.
- Keep small intentional fixtures and minimized counterexamples in tracked
  directories such as `tests/fixtures/` and `counterexamples/`. Model-file
  exceptions exist under `fixtures/` and `tests/fixtures/`; review their size.
- Keep credentials local. Only sanitized `.env.example` or `.env.*.example`
  templates belong in Git.
