# Bubble Breaker Fact Check Skill

A Codex skill for reviewing Bubble Breaker knowledge cards before they are published.

## What it does

This skill helps fact-check Bubble Breaker card ideas and keeps the copy fun without overclaiming.

It can:

- split a card into atomic fact claims,
- judge source quality,
- assign a `confidence_level`,
- flag cards that need human review,
- rewrite risky or exaggerated claims into more honest Chinese card copy.

## Repo structure

```text
.agents/skills/bubble-breaker-fact-check/
├── SKILL.md
├── references/
│   └── confidence_labels.md
└── assets/
    └── output_schema.json
examples/
└── roman-urine-laundry-tax.md
```

## Examples

The `examples/` folder stores completed fact-check cases so they can be reused as review standards and regression examples.

- [Roman Urine Laundry Tax](examples/roman-urine-laundry-tax.md)

## Install locally

Copy the skill folder into your user-level Codex skills folder:

```bash
mkdir -p ~/.agents/skills
cp -R .agents/skills/bubble-breaker-fact-check ~/.agents/skills/
```

## Use in Codex

Ask Codex to use the skill explicitly:

```text
Use the bubble-breaker-fact-check skill to review this Bubble Breaker card:

「1807年，拿破仑办过一场兔子狩猎庆祝活动，结果兔子没有四散逃跑，反而冲向人群。」

Return the result in the required skill output format.
```
