# Auto Collaboration and Parallel Implementation Standard

This applies to `flow-build auto` and `all`. Follow the scope, permissions, and sequential-execution preference the current user has set, along with any delegation restrictions in the execution environment. Do not arbitrarily expand the scope of the default single-task mode.

## Selecting Tasks to Run

1. Compare the spec, the plan, and the actual code and working tree to distinguish completed, in-progress, executable, and waiting-on-prerequisite tasks. Even if the existing plan has no parallel markers, you can judge by checking dependencies and file scope.
2. Treat a task as executable only when the required results of its prerequisite tasks are ready. Even in different folders, if there are unconfirmed assumptions about the same type, API, DB schema, or auth policy, the tasks are not yet independent.
3. If a shared contract is the bottleneck, first finalize and implement it within the spec's scope. Fix in documents or code only what is needed among the API, request/response, errors, types, and schema, and assign an owner. Do not arbitrarily finalize a contract that requires a user decision.
4. Then distribute, at the same time, the tasks that can be implemented and verified independently. The UI may be developed against mocks of an agreed contract, but do not consider integration with the real server complete based only on mock verification.
5. Do not split small tasks to match the number of parallel workers. Distribute only work whose benefit outweighs the coordination and conflict cost. Do not re-plan the whole project by UI layer and server layer; keep the existing feature-unit tasks and integration criteria.

## Assigning Workers

- Use the subagent tool of the current environment. Do not assume a tool that is not provided, and do not install or run a replacement agent CLI. Models, permissions, and concurrency limits follow the current environment and any explicit user settings.
- Consider the available slots, including the main agent, as well as memory and build cost. Start with about two independent tasks and adjust to spare capacity and the actual workload. When slots are full, integrate or reuse existing workers' results.
- Give each worker the task ID, goal, acceptance criteria, related spec and source, results of completed prerequisites, shared contracts, the files or directories it may modify, check commands, and the reporting format. State that it is not working alone and must not revert other workers' changes.
- Only one owner modifies a given file at a time. Shared types, package manifests, lockfiles, shared styles, route registration, migration order, and similar items are managed by one owner or by the main agent. If a change outside your assigned scope is needed, notify the main agent first.
- Only the main agent updates the full checklist and plan state. Workers report their own code, check results, and remaining integration conditions. Additional delegation, commit, push, or deployment is not considered permitted merely because work was assigned.

## Shared Environment and Integration

- In a shared workspace, respect the boundaries of your assignment and do not overwrite files other workers are editing. Do not use arbitrary branch switching, `reset`, or `clean`. If file scopes cannot be separated, run the task sequentially.
- If a separate worktree is needed, preserve existing changes and use only an isolated space you created. Do not mark a task complete before its result is integrated into the actual target working tree.
- Assign owners or isolate shared resources such as build artifacts, dev servers on the same port, and test databases or fixtures. Do not run package installs and checks that change shared state in the same directory at the same time.
- While waiting, the main agent proceeds with other independent work, result review, and integration preparation instead of duplicating implementation. When a task finishes, check its changes and check evidence, then distribute any newly executable tasks.
- If a contract or an owned file needs to change, coordinate with the affected workers and stop concurrent edits to that part. Changes that affect a completed integration require re-running the necessary verification.
- If a worker is blocked or fails, recover the cause and the incomplete state, then reassign the work or resolve it directly. Do not run follow-up tasks while ignoring a shared prerequisite. Unrelated independent work may continue.
- At the end, confirm the plan's integration criteria, such as the actual UI and server connection, contracts and error paths, and required tests and builds. Distinguish and report each worker's partial checks from the main agent's integration checks. If required verification remains, distinguish "implementation complete" from "verification pending."

## Inter-Agent Consultation and Independent Review

- Implementers pass to reviewers the changed files, the acceptance criteria met, the commands run and their results, and any remaining questions. Reviewers do not accept conclusions as given; they directly check the diff, the relevant call paths, and the check evidence. Re-run only the necessary part of already passing checks when there are new changes or doubtful evidence.
- If a direct messaging tool is available, implementers and reviewers exchange reproduction conditions, contracts, findings, and fix results, and share conclusions with the main agent. Without direct communication, the main agent relays. Do not assume agent-to-agent conversation that the tool does not support.
- A reviewer reports a problem with its location, the failure condition, its impact on the acceptance criteria, and a way to verify it. The implementer answers with the fix and verification results or counter-evidence, and the reviewer confirms whether the finding was resolved. Keep the editing owner so that the reviewer does not edit the same file at the same time.
- When opinions differ, narrow the disagreement with an executable reproduction or spec evidence, and the main agent makes the decision within scope. Core policy or permission questions that only the user can decide are gathered by the main agent and asked once, not repeated per worker.
- If slots are insufficient, assign a reviewer after the implementation worker finishes, or reuse a worker from another task for cross-review. The implementer of a change must not be its only reviewer. When delegation is not possible, switch to the sequential self-review defined in this document and state the limitation.
- When something fails, narrow the cause, fix it, and re-run the affected checks. If the same failure or opinion repeats without new evidence, change approach or have the main agent diagnose directly. If external conditions prevent further progress, leave the evidence and the incomplete items; do not lower the criteria or complete by agreement alone.
- Reviews under `auto/all` are limited to the requested change and the required integration scope. Unrelated cleanup of existing code or additional features are left as follow-up candidates. The final report summarizes the key findings resolved and the actual verification results, rather than listing each worker's conversation.

## Reporting Results

Summarize the tasks run in parallel and their assigned scope, the dependent tasks run sequentially, the integrated changes, the actual verification, and the remaining constraints. Do not claim a speed-up, or present time savings that were not measured, merely because parallelization was possible.
