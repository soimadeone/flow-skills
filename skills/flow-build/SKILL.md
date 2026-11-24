---
name: flow-build
description: Implement tasks from a spec or plan and verify them with the required checks. A plain flow-build call completes the next single task. auto or all completes the whole requested scope without pausing for progress check-ins, covering gap-filling in the plan, agent consultation, implementation, review, fixes, and integration verification. Do not shrink a general implementation request down to a single task.
---

# Flow Build

First, read the [common working principles](references/common.md). Implement in verifiable units, checking for differences between the spec or plan and the actual code.

Write all user-facing output (replies, reports, documents) in the user's language unless the user or project specifies otherwise.

## Modes and Input

- Codex `$flow-build`, Claude `/flow-build`: Complete the next executable, unfinished task in the selected plan, then stop.
- `$flow-build F03` or `/flow-build F03`: Check the specified task and its required prerequisite state, then carry out that task.
- `$flow-build auto` / `/flow-build auto`, or the mode argument `all`: An instruction to proceed until the requested full scope is complete. As one flow, fill any needed gaps in the spec or plan, implement the remaining tasks, consult between agents and run independent reviews, fix and re-verify the findings, and run final integration verification. Do not ask whether to continue at each task or phase transition, and do not hand follow-up skill invocations back to the user.
- Interpret `auto` or `all` when it appears in the mode position of the arguments. Do not mistake the same word inside a feature description for a mode.
- When this skill is selected for a natural-language implementation request, follow the completion scope the user requested. Do not impose the one-task limit of an explicit default invocation on a user's full implementation request.

## Preparation

1. Prefer the spec or plan path the user specified. If none, check the project's existing conventions, the root `SPEC.md`, `docs/SPEC.md`, `spec/`, and `tasks/`. If there are several candidates, select the one that matches the current request and state why.
2. Check the working tree and the completion state. Even when a task is marked complete, verify that its code and verification evidence are still valid for the current work. Do not skip a blocked prerequisite task and mark it complete.
3. For `auto`, compare the existing spec or plan against the current request. If there are no documents but the purpose, scope, and acceptance criteria in the conversation are sufficient, write the necessary documents based on them and continue. To complete the spec, read the `flow-spec` skill's `SKILL.md`; to write the plan, read the `flow-plan` skill's `SKILL.md` (both are installed alongside this skill). When invoking those skills, also pass along the full execution scope of `auto` and the permissions already granted. Do not stop just because document writing is finished, and do not ask again for approval of the same scope. If the core requirements themselves are missing, do not invent them; follow the question criteria below.
4. An `auto` or `all` invocation counts as approval to write or complete the plan, execute it, and review and fix within the current request's scope. Do not require re-approval just because a new plan was created. Existing approvals and stated restrictions remain in effect. Optional improvements that would substantially widen the scope become follow-up candidates, and the current work continues. Do not extend approval to external changes that need separate permission.
5. For a default single-task invocation with no plan, when the request is clear and small, treat that request as one task and briefly state its acceptance criteria. If several features are mixed together or the core requirements are empty, first organize the plan and requirements.

## Collaboration and Completion Loop in Auto

For `auto` and `all`, read and apply the [parallel implementation standard](references/parallel-implementation.md). If the user requested sequential execution, or a higher-level instruction restricts delegation, that condition takes precedence.

- The main agent checks each task's dependencies, shared interfaces, the files it touches, and its verification resources. When two or more meaningful tasks are unblocked and do not conflict, automatically create or reuse subagents to distribute them. Do not repeatedly ask for separate approval just for parallel distribution.
- Do not treat tasks as independent just because they are labeled UI and server. They can be implemented separately only once the shared contracts, such as APIs, types, and error responses, are settled. Finish the required contract work first, then proceed in parallel.
- The main agent handles splitting, integration, and task state. Implementers and reviewers pass evidence, reproductions, and fix proposals to each other, and the main agent resolves disagreements based on code, spec, and execution results. Do not pass implementation choices or review disagreements straight to the user as questions.
- Even when implementation cannot be parallelized, if subagents are available, assign a separate reviewer after implementation. The reviewer independently checks the relevant diff, the acceptance criteria, the failure and boundary paths, and the execution evidence needed. For review criteria, read and apply the `flow-review` skill's `SKILL.md`. The review itself is read-only; send fixes back to the owner who holds implementation permission under `auto`.
- Repeat `implement → independent review and required verification → fix findings → re-verify the affected scope`. Resolve defects that are within scope and backed by evidence, then move on to the next executable task. If only matters of taste or out-of-scope improvements remain, record the rationale and proceed. Do not repeat the same discussion without new evidence; narrow it through reproduction, experiments, or the main agent's judgment.
- Judge full completion only after checking, in the final integrated state, how the acceptance criteria, required checks, and review findings were handled. Do not substitute agreement in conversation or a subagent's completion report for verification. Items with no actual execution evidence stay marked as unverified.
- If subagents are unavailable or delegation is prohibited, the main agent performs implementation and review sequentially, distinguishing the two, and reports the limitation. Do not stop work that can be executed just because tools are restricted, and do not claim a mutual review that was not actually performed.

## When Auto Needs to Ask the User

- First check the existing conversation, documents, code, and check results, and have the agents review alternatives among themselves. Decide reversible implementation details, task order, test selection, and clear defect fixes yourself, and record the rationale. Do not block completion of the current scope with optional improvements or preference questions.
- Limit questions to cases where user input is actually required: conflicts or omissions in core requirements that cannot be decided from evidence, required credentials or access rights that only the user holds, and destructive or external changes not covered by an existing approval. Agreement among agents does not replace user approval or a tool's approval process.
- Before asking, proceed with the investigation, implementation, and verification that can be done independently. Bundle only the blocked scope, the evidence confirmed, and the decisions needed, and ask the minimum. If an asynchronous question tool is available, continue independent work while waiting. Do not proceed arbitrarily with, or mark complete, any work that needs an answer.
- Keep reporting progress, but do not insert confirmations such as "Shall I move to the next step?", "Should I review?", or "Should I fix this?" Unless the user asks you to stop or there is an actual blocker, continue to full completion.

## Task Execution

1. Check the acceptance criteria, call paths, existing patterns, and check commands.
2. For bug fixes, secure a failing reproduction first where possible. For new behavior, write a meaningful test first or use the existing verification path. Distinguish whether a failure is an environment problem or the absence of the expected behavior. Do not force formal tests on low-risk static changes.
3. Implement the smallest complete change that satisfies the acceptance criteria. Run the related tests and resolve any failures.
4. Run the task's required static checks, builds, and execution verification. Run full regression checks in line with the project's required standards and the scope of the change. Do not repeat the same full check when nothing has changed and nothing has failed.
5. Review the diff, and record on the task the acceptance criteria actually met and the verification evidence. Each time a task's implementation and required verification are finished (for `auto/all`, including independent review and handling of findings; if delegation is not possible, replace these with sequential self-review and a recorded limitation), immediately update that task's ID entry in the checklist the plan specifies (default `tasks/todo.md`) from `- [ ]` to `- [x]` and save. Do not defer checking until the whole plan is finished, and preserve the state of other tasks. If required acceptance criteria or verification remain, do not check the item; distinguish "implementation complete, verification pending." In parallel execution, the main agent updates the checklist after reviewing the results. If a defect in a completed task is found during integration, reopen that item and fix and re-verify it.
6. In default mode, report the result of that task and finish. In `auto`, re-determine the next executable task, distribute and proceed, and finally run the necessary integration verification.

## Blockers and Wrap-Up

- For reproducible code or check problems, narrow down the cause and keep resolving. When a requirement decision, an access permission, or an unavailable external environment is the blocker, record the specific point and the input needed. Unrelated independent work may proceed.
- Confirm the scope before executing external changes or irreversible work that the existing approval does not cover. Do not repeatedly ask for the same approval for work that has already been authorized.
- Report the completed tasks, changed files, check results, and remaining tasks and constraints. Do not commit automatically. If a commit was explicitly requested, include only the relevant files, and do not extend to push or deployment without separate permission.

## Review Guidance After Completion

`auto/all` includes review and fixes within the completion loop above. After completion, report the reviews and verification performed and the remaining constraints, then finish. Do not ask again whether to review, and do not suggest that completion requires separately invoking `flow-review`.

The handoff below applies only to the default single-task and specified-task modes. Once implementation and required verification for this invocation are done, summarize the result. Then hand off following "Step handoff" in the [common working principles](references/common.md). Offer these choices:

- **Review this change** (recommended): run `flow-review` now on the task, files, or commit range implemented this time, so the review does not expand to unrelated existing changes.
- **Continue with the next task**: run `flow-build` again for the next executable unfinished task. Offer this only when unfinished tasks remain.
- **Ask more or fix something**: answer questions or adjust the implementation, then ask again.
- **Stop here**: end, and give the invocation for resuming later (`$flow-review` or `$flow-build` in Codex, `/flow-review` or `/flow-build` in Claude).

Review is an optional follow-up that examines correctness, readability, structure, security, and performance. It does not replace required testing, build, or execution verification. If verification is pending or blocked, report that status first. If the user has already explicitly requested review after implementation, follow that permission and do not ask the same choice again.
