# tempo

**Safety-proportional process for agentic SDLC.** Trivial changes skip ceremony;
risky changes earn it.

This is the namespace and landing repo for the `tempo` Claude Code plugin. The
plugin is developed and released through the
[VibeKit plugin marketplace](https://github.com/shrwnsan/vibekit-claude-plugins)
— canonical source:
[`plugins/tempo`](https://github.com/shrwnsan/vibekit-claude-plugins/tree/main/plugins/tempo).

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

Measured on a fixed golden-task harness, the reference process-heavy plugin
(superpowers) costs **1.2–2.7× tokens and 1.45–1.8× turns** versus bare Claude
Code at identical correctness (33/33 hidden tests, all tiers). tempo keeps the
safety disciplines where they pay and drops the flat tax everywhere else.

First MVP smoke (same harness, same backend, n=1 per task — directional;
baselines are medians of n=4–7):

| Tier | bare | superpowers | tempo (smoke) |
|---|---|---|---|
| **0** trivial | 16 turns · 21.3k tok · 46s | 24 turns · 26.4k tok · 69s | **9–13 turns · 14.6–23.8k tok · 12–16s** |
| **1** standard | 16.5 turns · 19.1k tok · 82s | 29 turns · 51k tok · 199s | **18–22 turns · 21–23.8k tok · 42–44s** |
| **2** risky | 28 turns · 51.4k tok · 397s | 40.5 turns · 60.9k tok · 308s | **24–49 turns · 33.1–35.4k tok · 67–223s** |

Hidden tests: 6/6 tempo, 33/33 baseline runs. Tier 0 at/below the bare floor;
tier 1 at bare cost with the safety net (44% of superpowers' tokens); tier 2
at 0.67× bare tokens with the plan file and review gate included.

- Spec: [prd-010 — Lean SDLC plugin](https://github.com/shrwnsan/vibekit-claude-plugins/blob/main/docs/plans/prd-010-lean-sdlc-plugin.md)
- Measurement: [ceremony-cost eval harness](https://github.com/shrwnsan/vibekit-claude-plugins/tree/main/evals/ceremony)
  and [tasks-005 baselines](https://github.com/shrwnsan/vibekit-claude-plugins/blob/main/docs/plans/tasks-005-tempo-ceremony-eval-harness.md)

## Status

v0.1.0 MVP — triage router, tier flows, session hook. Decision-model triage
(calibrated providers, Jev-compatible) is planned; not required.

License: Apache-2.0.
