---
name: flow-refine
description: Clean up the flow, naming, responsibilities, and duplication of recent changes or specified code while preserving existing behavior. Use when flow-refine is invoked or when a behavior-preserving refactoring is requested. Feature additions and bug fixes are handled as a separate scope.
---

# Flow Refine

First, read the [common working principles](references/common.md). The goal is to reduce the cost of understanding code and of changing it safely.

Write all user-facing output (replies, reports, documents) in the user's language unless the user or project specifies otherwise.

Invocation examples: Codex `$flow-refine`, Claude `/flow-refine src/parser.ts`. If no scope is specified, prioritize the implementation diff in the current conversation; if there is none, check and state the scope of recent changes. Do not clean up the whole project arbitrarily.

## Procedure

1. Read the existing code, its call sites, its tests, and the project's conventions. Establish inputs, outputs, exceptions, side effects, execution order, permissions, stored data, and public interfaces as the behavior-preservation baseline.
2. Find the parts that make the code hard to read. Prioritize excessive nesting, mixed responsibilities, meaningless names, and duplication that actually implements the same rule. Do not change code that is already clear just to match a style preference.
3. Start with changes whose preserved results you can explain through static behavior analysis and existing verification. If there are dynamic calls, reflection, or external contracts, do not conclude that something is unused based only on text search results.
4. Do not mix unrelated changes; clean up in small units. Run the affected tests. If existing verification is insufficient to judge public behavior, add verification proportional to the risk of the change. Do not change expected results to make a refactoring pass.
5. If a check failure was introduced by this cleanup, fix or revert only your own change in question, and preserve existing fixes. If you find a functional defect, report it separately from the cleanup and judge whether it falls within the current request.
6. Check the final diff and the necessary check, build, and execution results. Evaluate the result by readability and behavior preservation, not by line-count reduction.

## Wrap-Up

Report what was cleaned up and why, how the behavior was verified, and the remaining constraints. If there is no basis for improvement, you may finish without changes. Commit only when the user requests it.
