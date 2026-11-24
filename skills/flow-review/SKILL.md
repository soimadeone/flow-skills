---
name: flow-review
description: Read the specified diff, commit, or files and write an evidence-based code review covering correctness, readability, structure, security, and performance. Use when flow-review is invoked or when a code review is requested. If only a review is requested, do not modify the code.
---

# Flow Review

First, read the [common working principles](references/common.md). The review result should identify concrete problems and make their impact clear.

Write all user-facing output (replies, reports, documents) in the user's language unless the user or project specifies otherwise.

Invocation examples: Codex `$flow-review`, Claude `/flow-review`, optionally followed by a commit, file, or comparison baseline.

## Review Scope

- Prioritize the scope the user specified. If none is specified, check the current changes, including staged, unstaged, and related untracked files, and state the review scope. If there are no changes, state that the most recent commit was used as the baseline.
- Read not only the diff but also the related call sites, type and data contracts, access policies, and tests. Do not raise speculation that is not connected to actual behavior as a confirmed defect.
- Do not modify or auto-format product code, plans, or tests during the review. Fixes start only after the user chooses them at the handoff below. Perform verification only within the scope needed for the review and within current permissions. If files are needed for reproduction, use an isolated temporary space outside the repository.

## Criteria

- Correctness: requirements, boundary values, error paths, async and state transitions, regression risk
- Readability: control flow and naming that hide intent; duplication or excessive abstraction that makes changes harder
- Structure: existing conventions, module responsibilities, dependency direction, interface compatibility
- Security: concrete problems in actual input, authentication, and permission paths, and in the handling of secrets
- Performance: repeated requests, unbounded work, and memory or resource-release problems, grounded in call frequency and data scale

Do not force findings to fill every perspective. Separate matters of taste from defects, and do not expand the scope into a separate security audit or performance project.

## Output

List problems in order of importance, most important first. Each problem includes the file and line, the conditions under which it occurs, the actual impact, the evidence, and a fix direction. Distinguish what was reproduced or tested from what is inferred.

- **Critical**: serious, concrete impact such as data loss, permission bypass, or a core feature outage
- **Major**: defects that break normal behavior or a stated acceptance criterion
- **Suggestion**: optional improvements that do not block behavior but help maintainability

If there are no evidence-based problems, state that first. Then briefly note the review scope, the checks performed, and the parts you could not check. Do not conclude from the review alone that tests pass or that the change is deployable.

## Next-Step Guidance After Completion

After reporting the review, hand off following "Step handoff" in the [common working principles](references/common.md). Offer these choices:

- **Fix Critical and Major findings** (recommended): fix them now, following the implementation and verification rules of the `flow-build` skill, limited to the chosen findings. Offer this only when there are Critical or Major findings.
- **Fix all findings, including suggestions**: same as above, also covering Suggestions. Offer this only when there are Suggestions.
- **Ask more**: explain or re-check findings, then ask again.
- **Stop here**: end with the review report.

After fixing, re-review only the changed parts with this skill's criteria, report the result, and ask the same question again if findings remain. If there are no evidence-based problems, end with the report without asking. When this skill is used as the reviewer inside `flow-build auto`, do not ask; return the findings to the caller.
