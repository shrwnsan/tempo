# tempo

**Safety-proportional process for agentic SDLC.** Trivial changes skip ceremony;
risky changes earn it.

This is the namespace and landing repo for the `tempo` Claude Code plugin. The
plugin is developed and released through the
[VibeKit plugin marketplace](https://github.com/shrwnsan/vibekit-claude-plugins)
— canonical source:
[`plugins/tempo`](https://github.com/shrwnsan/vibekit-claude-plugins/tree/main/plugins/tempo).

## Results at a glance

On the fixed benchmark harness (medians of 3 runs per task, every task passed
hidden verification):

- **Risky changes**: 34% fewer tokens than bare Claude Code, with the plan
  file and review gate included
- **Trivial changes**: fewer turns and faster wall-clock than bare
- **Standard changes**: test-first plus full verification costs about 1.9×
  bare, roughly 38% cheaper than the process-heavy reference plugin
- 75 of 75 tasks passed hidden verification across every benchmark run

## Install

```
claude plugin install https://github.com/shrwnsan/vibekit-claude-plugins
```

## What it does

- A ~10-line routing table injected once per session — not a philosophy lecture
- Every task tiers **0 / 1 / 2** by risk; tier 0 usually loads nothing at all
- Tier 1: one-line plan note, test-first, single agent
- Tier 2: short plan file → build → review gate → verify gate
- **Fail-twice auto-promotion**: the same fix failing twice moves the task up a
  tier automatically
- **Verify before done at every tier**: run the checks, quote the evidence

## Why

On a fixed golden-task harness, the reference process-heavy plugin
(superpowers) cost 1.2–2.7× tokens and 1.45–1.8× turns more than bare Claude
Code at identical correctness, on every tier (33/33 hidden tests in both
conditions). tempo keeps the safety disciplines where they pay and drops the
flat tax everywhere else.

Latest benchmark, same harness and backend, medians of 3 runs per task (bare
re-based on the same CLI; superpowers is the October baseline):

| Tier | bare | superpowers | tempo |
|---|---|---|---|
| **0** trivial | 16.5 turns · 16.2k tok · 37s | 24 turns · 26.4k tok · 69s | **16 turns · 24.8k tok · 30s** |
| **1** standard | 17 turns · 16.6k tok · 81s | 29 turns · 51k tok · 199s | **25.5 turns · 31.6k tok · 106s** |
| **2** risky | 24.5 turns · 44.1k tok · 196s | 40.5 turns · 60.9k tok · 308s | **25 turns · 29.1k tok · 189s** |

Hidden tests passed in every run recorded (75/75 across baselines, smoke, and
reruns). Tier 2 beat bare outright: 0.66× tokens at parity turns and
wall-clock. Tier 0 held the bare floor on turns and wall-clock. Tier 1 sits
between bare and superpowers: about 62% of superpowers' tokens with the
safety net included, and the session forensics show that cost is the
disciplines themselves (test-first, plan note, full verify), not skill
content.

- Spec: [prd-010 — Lean SDLC plugin](https://github.com/shrwnsan/vibekit-claude-plugins/blob/main/docs/plans/prd-010-lean-sdlc-plugin.md)
- Measurement: [ceremony-cost eval harness](https://github.com/shrwnsan/vibekit-claude-plugins/tree/main/evals/ceremony)
  and [tasks-005 baselines](https://github.com/shrwnsan/vibekit-claude-plugins/blob/main/docs/plans/tasks-005-tempo-ceremony-eval-harness.md)

## Status

v0.1.0 MVP: triage router, tier flows, session hook. Ships dual manifests —
Claude Code plugin and [Agent Plugins](https://agent-plugins.org/) v1.0.0
package; skills conform to the [Agent Skills specification](https://agentskills.io/).
Decision-model triage (calibrated providers, Jev-compatible) is planned but
not required.

License: Apache-2.0.
