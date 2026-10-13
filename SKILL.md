---
name: data-hygiene
description: Universal discipline for any AI touching data — logs, CSV/JSON, telemetry, monitoring output, sensor or measurement pipelines. Prevents the four classic failure modes (misread field semantics, wrong denominator/units, extrapolating trends from too few points, importing thresholds across datasets) with evidence tiers, row-count conservation, and recompute recipes. Use whenever reading, transforming, aggregating, or reporting quantitative data. 数据处理通用自检：报数、算率、下趋势结论前强制过闸。
---

# Data Hygiene

## Overview

Errors in data work are rarely arithmetic. They come from **shipping a plausible narrative without returning to the primary evidence**. This skill is a gate system that applies to any dataset, any language, any AI. Four failure modes account for most real damage; each has a gate.

## Evidence Tiers (tag every number)

- **[MEASURED]** — copied from a source location (file:line, cell, log entry). Must be citable.
- **[DERIVED]** — computed from measured values. Must carry its formula.
- **[SPECULATIVE]** — extrapolated/modeled/judged. Must state uncertainty and a falsifier.

Never let DERIVED or SPECULATIVE borrow MEASURED confidence language ("the data shows…"). Say "computed as…", "extrapolated if…", "guess:".

## Gate 1 — Semantics before values (溯源)

Before interpreting any field/column/key:
- Read its **definition** (source code, schema, docs, producer). Ask explicitly: is this a fraction, a percentage, or an absolute quantity? What are its units? Accumulated or incremental?
- Sample 2–3 raw records untouched by your pipeline; confirm types and ranges match your assumption.
- Definition unavailable → any interpretation is SPECULATIVE, label it.

## Gate 2 — Denominator & conservation (分母与守恒)

- Every "per X / rate / growth / ratio": write the arithmetic once, explicitly: `Δnumerator ÷ Δwindow = value [unit]`, and verify the window is what you think (10 samples ≠ 1 sample; 10 min ≠ 1 min).
- Round-trip check: recompute one known reference value with your formula; confirm it matches before trusting the formula.
- Row-count conservation: after each transformation stage record `in → out`; any silent drop (failed join, regex miss, encoding loss, dropped NaN) must be explained, not absorbed.

## Gate 3 — Sample-size sanity (样本量)

- Trend claims ("accelerating", "plateauing", "will never reach X") require **≥4 non-overlapping intervals**; report each interval's raw endpoints, not just the fit.
- Never extrapolate an asymptote or "inevitable" verdict from ≤2 points — that is fitting noise.
- Sensitivity: if the conclusion flips when the most recent interval is excluded, say so.
- ETAs must name their assumption ("linear at 1.2/µs") and falsifier ("next interval <1.0 invalidates this").

## Gate 4 — Boundary discipline (边界)

- Thresholds, constants, criteria are **dataset/domain-specific**. Before applying a remembered or "familiar" value, confirm it in the current dataset's primary config now — not from memory or another project.
- Borrowing a criterion across domains must be flagged explicitly: "adopted from X; verify it belongs here."
- Snapshots decay: re-read current state before any verdict about "now".

## Output Discipline

Key claims as: `value [tier] (source [, formula])`.
When handing numbers to an external reviewer (human or another AI), attach a **recompute recipe** — exact files/lines, filters, arithmetic — so verification costs the reader minimum effort.

## When Corrected by Evidence

Re-derive from primary sources immediately. State which gate failed and distinguish "the data changed" from "I was wrong before". Never silently patch an old conclusion with new data.
