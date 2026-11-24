---
name: flow-plan
description: Break a confirmed requirement or spec into an implementation plan and tasks with dependency order, acceptance criteria, and verification methods. Use when flow-plan is invoked or when a request asks to decompose development work. If only a plan is requested, do not modify product code.
---

# Flow Plan

First, read the [common working principles](references/common.md). Compare the spec against the current code to build a list of tasks that can actually be executed.

Write all user-facing output (replies, reports, documents) in the user's language unless the user or project specifies otherwise.

Invocation examples: Codex `$flow-plan`, Claude `/flow-plan notification feature`. Include only the specified feature and documents in the plan's scope.

## Procedure

1. Read the existing specs, plans, tasks, and related code. If clear requirements appear in the conversation, you may use them as the basis. If core behavior is missing, ask only the necessary questions and do not invent implementation details.
2. Identify what can be reused and what must change in the current structure, and determine the dependency order. Split the work into units sized so that one user flow can be verified end to end, but place necessary foundation work first.
3. For each task, record a unique ID, its purpose, the expected change areas, dependent tasks, acceptance criteria, and the verification command or observation method. Do not present expected file paths as a confirmed change list. When there are several independent tasks, also record parallelizable groups, shared contracts that must come first, file or resource conflicts, and integration points. Do not assume independence based only on a UI or server distinction, and do not create unnecessary tasks just to enable parallelism.
4. Include in the plan the integration points that require checking several tasks together, such as integration, real-screen, or device verification. Do not insert formal approval steps between every task.
5. Prefer the plan location or tracker the project specifies. By default, use `tasks/plan.md` and `tasks/todo.md`. Writing to an external tracker follows the user's request or the project's existing permissions.
6. If an existing document has incomplete items for the same work, update it while preserving their IDs and completion status. If another work's incomplete plan exists, do not overwrite it; use a separate work folder, or ask which option to choose to resolve the conflict. State the chosen path in the result.
7. If only a plan was requested, save only the plan document and summarize it. If implementation permission was also granted, keep that permission.

## Default Record Format

`tasks/plan.md` holds the scope, the spec basis, the dependency order, the detailed content of each task, and integration verification.

`tasks/todo.md` holds a checklist with the same IDs. Each item should let the reader find the matching task in the plan. Judge completion by the evidence of implementation and required verification.

Example: `- [ ] F01 Create notification — no dependencies; acceptance criteria and verification in F01 of plan.md`

A small piece of work is adequately covered by one task. Do not add unnecessary work just to fill out the format.

## Next-Step Guidance After Completion

When the plan and tasks are written and you finish this request, summarize the result, distinguishing between completing the plan and completing the implementation of the tasks. Then hand off following "Step handoff" in the [common working principles](references/common.md). The next step is `flow-build`. Offer these choices:

- **Implement the next task**: run `flow-build` to implement and verify the next executable unfinished task, then stop. Suits reviewing tasks one at a time.
- **Implement the whole plan**: run `flow-build auto` to proceed from filling gaps in the plan for the requested scope through implementing the remaining tasks, agent-to-agent review, fixes, and integration verification. Respect dependencies, and implement independent tasks in parallel with available subagents. Reuse this invocation and the existing approval, so that a new plan within the same scope or each phase transition does not require re-approval. Still confirm approvals needed for changes outside the current request's scope, and blockers that only the user can decide (core requirements, permissions, or the external environment).
- **Ask more or revise the plan**: answer questions or revise the plan, then ask again.
- **Stop here**: end, and give the invocation for resuming later in the notation that fits the current environment (`$flow-build` or `$flow-build auto` in Codex, `/flow-build` or `/flow-build auto` in Claude), including the plan path if it is not in the default location.

Recommend one of the two implementation choices based on the plan's size and how independent its tasks are. If key decisions remain, state those first and resolve them before recommending an implementation choice. If implementation was already requested, continue according to the permission granted without asking, and do not ask for the same approval again.
