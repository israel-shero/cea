# AGENTS

This repository is a scientific computing library (CEA). Follow these rules:

- Do NOT modify `data/thermo.inp` or `data/trans.inp` unless explicitly instructed.
- Numerical correctness is paramount; preserve bitwise results and scientific behavior.
- Avoid algorithmic changes unless explicitly requested.
- Prefer clarity over cleverness or micro-optimizations.
- Keep changes small and focused; avoid drive-by formatting or whitespace churn.
- In Fortran source files, keep lines within the default free-form GNU limit (target `<= 132`
  characters) so CI builds that do not pass `-ffree-line-length-none` do not fail.
- Before finishing Fortran edits, check touched files for overlong lines with
  `rg -n '.{133,}' path/to/file.f90`.
- Maintain backward compatibility for the legacy user base.
- If numerical behavior might change, call it out and add validation or tests when possible.
- Respect layer boundaries: Fortran core in `source/`, C bindings in `source/bind/c/`,
  Python bindings in `source/bind/python/`.
- If the public-facing Python API in `source/bind/python/CEA.pyx` changes, update the
  corresponding type-hinting files in `source/bind/python/cea/` (for example `.pyi` stubs).
- Run minimal validation for touched areas:
  - Core/CMake/Fortran/C changes: run relevant `ctest` targets from the build directory.
  - Python binding changes: run `make py-rebuild` then `pytest source/bind/python/tests`.
  - Documentation-only changes: check referenced commands/paths against `README.md` and `CONTRIBUTING.md`.
- Prefer established workflows:
  - Configure/build with `cmake --preset dev` and `cmake --build build-dev` when appropriate.
  - Use focused tests first, then broader test runs when numerical behavior may be affected.

## Permanent branch policy

- `dev` is the primary development and integration branch. Start task branches from `dev` and target ordinary development pull requests to `dev`. Tested, scoped changes may be merged into `dev` without a separate owner approval for each merge, subject to existing GitHub permissions, required checks, review rules and task-claim controls. Never bypass protections or approve your own PR to satisfy required review.
- `main` is the stable milestone branch. Promote coherent, tested integration milestones from `dev` to `main` by a deliberate pull request; require explicit owner approval. Do not make routine development commits to `main`.
- Preserve both branches and their history. Do not force-push, delete either branch, or alter permissions or rulesets without explicit authorization. Existing trusted workflow dispatch refs are separate from development PR targets; do not change privileged workflow execution refs without a security review.
