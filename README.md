# Unified Convergence Theory for Non-Stationary RL — paper repository

*A four-strand composition for cumulative dynamic regret in non-stationary reinforcement learning.* Single-author theory paper. Two-Gap diagnostic separating goal-feasibility from policy-quality; point-mass reverse-KL/TV identity strictly improving Pinsker and Bretagnolle–Huber at the deterministic-π\* corner; strategic tempo with forgetting prerequisite as structural survival inequality; loop-as-causal-engine for learnable bounds. Cumulative dynamic regret rate $O(\sqrt{B_T \cdot T})$.

Repository follows the segmented-paper workflow: paper segments live in `src/re/`, assembled by the `OUT.unified-rl-neurips-2026.md` manifest into the 9-pp-main-text submission form (31pp with appendices). Submitted to NeurIPS 2026 (submission 33915) from tag `submitted/neurips-2026`. The `OUT.full-paper.md` / `OUT.neurips-2026-paper.md` manifests an earlier version of this README advertised were migration-era and are archived in `_archive/`; there is one live manifest.

**As submitted.** Reviews returned 2026-07-23. `submitted-neurips-2026.pdf` is a frozen blind build of the `submitted/neurips-2026` state — no manifest owns that stem, so `bin/build` cannot overwrite it, unlike `unified-rl-neurips-2026.pdf` which is regenerated on every run. Cite page numbers from the frozen copy during author response.

Consumed as a submodule by an umbrella workspace.
