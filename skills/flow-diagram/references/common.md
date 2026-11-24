# Flow Common Working Principles

This file is the shared baseline for `flow-spec`, `flow-plan`, `flow-build`, `flow-review`, `flow-refine`, and `flow-diagram`. The current user request and higher-level instructions from the runtime environment take precedence.

## Scope and context

- Read the current repository's `AGENTS.md` and any applicable project rules. If `CLAUDE.md` holds project conventions, consult it as well, but do not assume another agent's commands or tools exist in the current environment.
- Reuse decisions already confirmed in existing specs, plans, and conversation. Ask only about open questions that would change the outcome or scope, and do not ask again for approval already given. Proceed with reversible implementation details and state the reasoning.
- If the user or project points to a knowledge base for project context, follow its rules when reading it. Invoking this skill is not itself a request to write to that knowledge base or to run automation.
- Before editing, check the working tree and related diff to tell existing changes apart. Do not overwrite or bulk-revert changes made by the user or by other work. If a conflict arises, adjust only that part, and ask only about overlaps whose intent is unclear.
- Stay within the requested scope. Do not mix in unrelated cleanup, dependency updates, or large formatting changes. Commit only when the user asks, including only the relevant files and the change log in its existing format. Push, merge, tag, and deploy each follow the scope of the user's request.

## Tools and verification

- Prefer `rg` for search. For builds and checks, look up and use the repository's scripts, wrappers, and CI commands. Do not presume a language or framework.
- When a browser is needed, use the browser tool available in the environment, or the one the user or project specifies. If none is available, state the limitation.
- Choose tests to match the risk and behavior of the change. Use meaningful regression tests or reproduction checks for behavior changes and bug fixes. Do not write tests that merely restate the implementation for copy, static style, or simple config changes.
- Do not change requirements or expected values to hide a failing check or make it pass. Distinguish pre-existing failures from new ones, and keep going when a reasonable fix can resolve them. Leave parts that need external conditions or a user decision documented with evidence.
- File and static checks, automated tests and builds, a real browser or device, install and relaunch, and production deploy or public access are different kinds of evidence. Claim only what you actually verified. For real UI issues, confirm the rendered screen and the relevant behavior where possible.
- Run verification that affects real services or user data only when you have the permissions the task requires. Do not conclude that a recovery or deploy succeeded just because a process ran or a build passed.

## Reporting

- Explain the result first, in the user's language. Add the reason for the change, key files, verification performed, and remaining limitations as needed.
- Separate what was planned, what was implemented, and what was actually verified. If a required acceptance criterion was not verified, do not mark the whole task complete.

## Step handoff

`flow-spec`, `flow-plan`, `flow-build`, and `flow-review` form a sequence: spec → plan → build → review, and a review can lead back to fixes. At the end of each step, do not leave the user to type the next prompt. Ask what to do next:

- Use the environment's structured question tool if it has one (for example, `AskUserQuestion` in Claude Code). Otherwise ask in plain text with numbered choices as the last part of the reply, and put nothing after it.
- Offer the step-specific choices from the skill. Always include proceeding to the next step, asking more or revising the current step, and stopping. Put the recommended choice first.
- When the user chooses to proceed, run the next skill right away in the same session: load it with the skill tool, or read its `SKILL.md` if no skill tool is available. Pass along the paths of the documents just produced and the scope already confirmed, and do not ask again for decisions already made.
- When the user asks more or wants changes, handle that, then ask the same handoff question again.
- Do not proceed without the user's choice. Skip the question when the current invocation already authorizes continuing, such as `flow-build auto` or an earlier explicit request for the following steps.
- If key decisions are still open, ask about them before the handoff question.
