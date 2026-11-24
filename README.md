English · [한국어](README.ko.md)

# flow-skills

Agent skills for taking a feature from idea to reviewed code: write the spec, plan the work, build it, clean it up, and review it. Two standalone skills draw diagrams and run multi-source research.

They are plain `SKILL.md` folders, so the same skills work in Claude Code, Codex, and other agents that support the Agent Skills format.

## Install

```bash
# all skills
npx skills add soimadeone/flow-skills

# only some of them
npx skills add soimadeone/flow-skills --skill flow-spec flow-plan flow-build

# install for your user instead of the current project
npx skills add soimadeone/flow-skills -g
```

Installing all of them is recommended: each step hands off to the next skill, and `flow-spec` uses `flow-diagram` for diagrams in the spec.

See what the repository contains without installing:

```bash
npx skills add soimadeone/flow-skills --list
```

Update later with `npx skills update`.

## The flow

```
flow-spec  →  flow-plan  →  flow-build  →  flow-review
                                 ↑               │
                                 └── fixes ──────┘
```

At the end of each step the agent asks whether to continue, revise, or stop, and moves to the next skill in the same session when you say so.

| Skill | What it does |
|---|---|
| `flow-spec` | Turns a feature request into a spec with scope and done criteria, checks for missed or unexpected features with you, and makes a review mockup when the feature has UI. |
| `flow-plan` | Breaks a spec into ordered tasks with acceptance criteria and how each will be verified. Does not touch product code. |
| `flow-build` | Implements the next task and runs the required checks. `flow-build auto` finishes the whole plan without stopping for check-ins. |
| `flow-review` | Reviews a diff, commit, or files for correctness, readability, structure, security, and performance, with evidence. Does not change code. |
| `flow-refine` | Cleans up naming, flow, responsibilities, and duplication while keeping behavior the same. |
| `flow-diagram` | Draws architecture, data flow, sequence, or state diagrams as a single HTML+SVG file from the actual code and config. |
| `flow-research` | Turns a question into checkable claims, grades sources for and against each, and writes a report where every verdict links to its sources. |

## Usage

Call a skill by name and describe what you want:

```text
# Claude Code
/flow-spec password reset by email
/flow-build auto

# Codex
$flow-plan
$flow-review HEAD~3..HEAD
```

Plain language works too: `flow-diagram draw the data flow from login to session storage`.

Replies and documents follow the language you write in.

## What gets written where

| Skill | Default output |
|---|---|
| `flow-spec` | `SPEC.md` at the project root, or the project's existing spec location |
| `flow-plan` | `tasks/plan.md` and `tasks/todo.md` |
| `flow-diagram` | `docs/diagrams/<topic>.html` |
| `flow-research` | `research/<topic>/report.md`, with `brief.md`, `sources.md`, and `claims.md` beside it |

Each skill reads your repository's `AGENTS.md` (and `CLAUDE.md` if present) first and follows the project's own conventions over its defaults.
