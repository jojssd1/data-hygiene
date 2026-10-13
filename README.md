# data-hygiene

A universal [Agent Skill](https://agentskills.io) that installs a gate system against the four classic AI failure modes when working with data — logs, CSV/JSON, telemetry, monitoring output, sensor or measurement pipelines.

任何 AI 处理数据时的"四道闸"自检技能：字段语义溯源、分母/单位守恒、样本量红线、跨域边界核对，并强制区分 实测 / 推导 / 猜测 三档证据。

## Why

Most AI numeric errors are not arithmetic. They are plausible narratives shipped without verification:

| Failure mode | Example damage |
|---|---|
| Misreading field semantics (W as %, fraction vs percent) | 100× error |
| Wrong denominator / unit window (10-step interval as 1 step) | 10× error |
| Extrapolating a trend or "inevitable" verdict from ≤2 points | phantom asymptote, wrong ETA |
| Importing thresholds across projects/materials from memory | invalidating whole conclusions |

The skill turns these into four cheap pre-output gates plus an output format (`value [tier] (source)`) that makes every key number independently re-checkable at near-zero cost.

## Install

```bash
# Qoder CLI (user scope)
npx skills add jojssd1/data-hygiene -g -a qoder-cn -y

# or copy SKILL.md into your agent's skills directory:
#   ~/.qoder-cn/skills/data-hygiene/SKILL.md   (Qoder)
#   .claude/skills/data-hygiene/SKILL.md       (Claude Code)
#   or paste it into any assistant's custom instructions / project rules
```

## Use

It triggers automatically when the agent reports rates, trends, ETAs, or unit-derived numbers, or invoke explicitly:

```
/data-hygiene
```

## Contents

Single file, no scripts, no dependencies:

- `SKILL.md` — evidence tiers, four gates, output discipline, correction protocol

## License

MIT
