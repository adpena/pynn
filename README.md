# PYNN

Experimental research into agent-designed neural networks for compiler
optimization, with [MOLT](https://github.com/adpena/molt) as the intended first
integration target.

The vision is a Python-to-optimized-IR pipeline in which small networks guide
search or propose equivalent compiler transformations. An agent could design the
network and directly author its weights, use conventional training, or combine
both approaches. Independent verification and measured results would determine
which candidates are accepted.

**Status:** initial research scaffold. No compiler pipeline, neural model,
verifier, proofs, search harness, benchmarks, or MOLT adapter is implemented yet.

## Proposed research loop

1. Define a bounded Python/typed-IR fragment and its exact semantics, including
   relevant arithmetic, exceptions, and effects.
2. Use a deterministic network to rank transformations or propose rewritten IR.
3. Check equivalence independently. Lean 4 and solver-based approaches such as
   SMT are under evaluation; inconclusive candidates would be rejected or fall
   back to the baseline.
4. Fuzz and differential-test the surrounding pipeline, minimize failures, and
   benchmark accepted candidates against MOLT and equal-budget conventional
   search. Measure compile-time cost as well as output quality.
5. Feed counterexamples and measurements back to the agent for recursive
   refinement, keeping specifications, verification criteria, and held-out
   evaluations fixed for each experiment.

## Research principles

- **Determinism and correctness are separate.** Fixed inputs, weights, numeric
  semantics, inference, and tie-breaking can make a network deterministic.
  Equivalence checks must separately establish whether a proposed rewrite
  preserves the specified behavior.
- **Reproducibility is part of the design.** Pin toolchains, target features,
  dependencies, seeds, search budgets, and ordering; record experiment inputs
  and compare repeated runs.
- **Optimality needs a defined scope.** A future result might establish that no
  cheaper candidate exists within a bounded search space under an explicit cost
  model. That would not establish global optimality for arbitrary Python
  programs. Speed, size, and instruction count are distinct objectives.
- **Integration follows evidence.** Keep research standalone, then test a narrow
  MOLT adapter against a pinned revision and versioned IR boundary. Merge useful
  components only after correctness and performance are demonstrated.

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
