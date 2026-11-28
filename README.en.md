# code-companion · One-on-One Source Code Reading Tutor

[![MIT License](https://img.shields.io/badge/license-MIT-green.svg)](./LICENSE)
[![Pure Markdown](https://img.shields.io/badge/skill-pure%20Markdown-blue.svg)](#why-its-general-purpose)
[![Agent Agnostic](https://img.shields.io/badge/agent-agnostic-orange.svg)](#why-its-general-purpose)
[![Trigger: 开始学习](https://img.shields.io/badge/trigger-%E5%BC%80%E5%A7%8B%E5%AD%A6%E4%B9%A0-purple.svg)](#how-to-trigger-it)

> Language: [中文](./README.md) · [English](./README.en.md)

A general-purpose agent skill that turns "understanding a codebase" into a one-on-one tutoring experience — moving through a **living teaching map**, one card at a time, with a check at every step, advancing only after understanding is verified.

**Not tied to any agent software.** It is a set of pure Markdown instructions and calls no proprietary tools or private APIs — any agent that can load a skill / instruction directory can use it (Claude Code, Cursor, Cline, and various custom agent frameworks, among others).

The goal is not for you to recite "what this code does", but to **truly understand** it: every conclusion can be derived from foundations you have already accepted and plugged into your own mental model.

## What it solves

- You don't know where or in what order to start reading an unfamiliar codebase
- You follow call chains all the way into rendering, tokenization, truncation, post-processing — and get more lost the deeper you go
- A session ends and you forget where you stopped or what confused you
- "I finished it / I get it" doesn't actually mean mastery, yet you've already moved on

## How to trigger it

Say **"开始学习"** (the exact four Chinese characters), or say "start / continue learning this project's source code" or "read it unit by unit".

Once triggered, the tutor first determines the current state (first lesson / resume / issue a card / answer questions / advance) and reads only the files for that state — it won't dump the whole spec into context.

## How it works

At its core is a state machine revolving around a **living teaching map**, split into five rings:

| Ring | Responsibility |
|---|---|
| `engine` | How the state machine turns: entry routing, lesson building, card issuing, Q&A, persistence |
| `map` | The living teaching map: boundary probing, minimal-tree planning, evolving with the learner, progress source of truth |
| `teaching` | How to present a card: principles, steps, card spec, logistics |
| `guardrails` | Cross-cutting rules: never invent paths or line numbers, never modify source code, never narrate the methodology |
| `SKILL.md` | Thin entry point: state routing and hard red lines |

Key mechanisms:

- **Only one card at a time** — stop after presenting it, wait for the learner, never advance on your own.
- **Every card has a "stop here" boundary** — explicitly naming the downstream functions/files not to chase, preventing ever-deeper spirals.
- **One check per step, explanation before check** — diagnose and repair on the spot if it doesn't land; never build on an unverified step.
- **A single active task card** written as `当前任务卡.md` (literally "current task card") — deleting a session doesn't lose your place; it can be resent verbatim.
- **A permanent archive** at `card-archive/C<n>.md`: created on first card, knowledge fragments adjusted in place after each answer, revision versions kept after re-teaching, checked and completed when done — append-only, never cleaned, achieving "never forget".
- **Mastery is not inferred from verbal signals**: "done / OK / I get it" is not verification; record a self-test, a paraphrase, or observed evidence.
- **Capability boundary is documented and kept current**: after the first-lesson boundary probe, write `能力边界.md` (a living portrait of what the learner can currently do — each strand's floor / ceiling / misconceptions, with dates). Every answer, re-teaching, and verified advance updates it; it always reflects "what the learner can do and where they're stuck now", never a one-time snapshot.

Learning data is written to `.learning_memory/` (a hidden directory) at the project root — **source code is never modified**.

## Why it's general-purpose

- The entire content is Markdown text — no code, no build step, no platform-specific API.
- Actions like "read a file, verify a line number, write to the learning directory" all go through the agent's existing general-purpose file tools; the skill only describes rules and flow.
- State lives in ordinary files in the project directory (`.learning_memory/`), with no dependency on any platform's session storage.
- The entry file is `SKILL.md`, carrying `name` / `description` YAML frontmatter — a convention shared by most agents today.

## Installation

This skill is just a directory with no runtime dependencies. Put it wherever your agent software loads skills from.

**General approach**: copy the entire `code-companion/` directory into your agent's skills directory, restart the session (or reload skills), then say "开始学习".

Common skills directories (check your software's actual version):

| Agent | Common location |
|---|---|
| Claude Code | `~/.claude/skills/` or the project's `.claude/skills/` |
| Cline / Roo Code | the project's custom instruction directory (e.g. `.clinerules/`) |
| Other skill-capable agents | that software's skills / instructions directory |

> Different agents have different entry-point conventions: some read `SKILL.md` + frontmatter, others use a different filename or format. This skill's entry point is `SKILL.md`; if your agent uses another convention, just point it at the entry file — the five-ring content needs no changes.

Install from the repository:

```bash
git clone https://github.com/zycskylove520/code-companion.git
# then copy the whole code-companion/ directory into your agent's skills directory
```

## Directory structure

```
code-companion/
├── SKILL.md              # thin entry: state routing, hard red lines
├── CHANGELOG.md          # evolution log
├── README.md             # Chinese documentation (default)
├── README.en.md          # English documentation
├── engine/               # how the state machine turns
│   ├── INDEX.md
│   ├── session.md        # entry routing (first lesson / resume)
│   ├── card-loop.md      # main loop: issuing, verifying, advancing, finishing
│   ├── branch.md         # exception branches: Q&A, re-teaching, fatigue
│   └── persistence.md    # the single execution rule for persistence
├── map/                  # the living teaching map
│   ├── INDEX.md
│   ├── build.md          # boundary probing, minimal-tree planning, week/day units
│   ├── evolve.md         # map evolving with the learner, never forget
│   ├── progress.md       # progress source of truth template, dynamic estimation
│   └── capability.md     # 能力边界.md template, creation timing, ongoing maintenance
├── teaching/             # how to present a card
│   ├── INDEX.md
│   ├── principles.md     # teaching principles (foundations first, etc.)
│   ├── step.md           # single-step presentation, one check per step
│   ├── card-spec.md      # card spec, stop boundary
│   └── logistics.md      # logistics division
└── guardrails/           # cross-cutting rules
    ├── INDEX.md
    ├── facts.md          # facts and path rules
    └── anti-patterns.md  # self-check list
```

## License

[MIT](./LICENSE)
