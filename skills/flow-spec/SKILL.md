---
name: flow-spec
description: Turn the requirements, scope, and done criteria of a new feature or significant change into a spec, review needed, missed, and unexpected features with the user, and create a review mockup if the feature needs UI. Use when flow-spec is invoked or when the user explicitly asks for a spec or requirements document. Do not force the spec step for simple fixes or implementation requests alone.
---

# Flow Spec

First, read the [common working principles](references/common.md). The output of this step is a spec that can be reviewed before implementation.

Write all user-facing output (replies, reports, documents) in the user's language unless the user or project specifies otherwise.

Examples: Codex `$flow-spec login feature`, Claude `/flow-spec login feature`. Treat the natural language after the invocation and the conversation context as the requirements.

## Process

1. From the user's brief description, identify the purpose and the core user flow. For an existing project, check the related code and docs. For a new project, do not assume the user has already prepared detailed requirements. Accept any technology the user specified as the default, and keep any policies already decided and any conditions that cannot change.
2. Run the feature review below, and present its results together with the open decisions needed to make the spec concrete, starting with questions that would change the outcome, such as target users, core behavior, and data retention. Group them so they are easy to answer in one go. Apply the platform and structure proposals below only when there is a specific trigger. Do not ask again about anything already reviewed or chosen unless there is new evidence. Keep investigating the parts you can check independently, and for minor choices, record the assumption and the reason and proceed.
3. If there are several features that can be released and verified separately, briefly propose the scope of each, the dependency direction, and the implementation order. If the request already breaks the work down clearly, use that breakdown as is. Do not create unnecessary module layers for a single feature.
4. If an existing spec location and format exist, update them there. If not, use `SPEC.md` at the project root. Do not replace documents from other work, and clearly separate existing content from the scope of your changes. Keep items awaiting a choice in a separate proposal section, and do not mix them into confirmed requirements.
5. Decide whether visualizing user flows, state transitions, or component relationships would make the spec easier to understand. If it would help, or if the user requested it, read the [visual spec guidelines](references/visual-spec.md) and the `flow-diagram` skill's `SKILL.md` (installed alongside this skill), create the diagram using `flow-diagram`'s spec-linked method, and include it in the HTML review copy. Pass along the spec path, design scope, open decisions, and output location. The user does not need to invoke `flow-diagram` separately. Do not add Mermaid or an external diagram skill as a default dependency.
6. For features that need a user-facing screen, read the [UI mockup guidelines](references/ui-mockups.md) and create an HTML mockup where the main flows and states can be reviewed. Apply this even if the user did not separately ask for a mockup, but follow any explicitly stated scope, such as skipping the mockup or writing documents only. Do not add UI to work that has no screens.
7. If the request covers only spec writing, report the saved documents, mockups, and remaining questions, then finish. Creating the mockup is part of the spec review and does not mean that product implementation starts automatically. If follow-up planning or implementation was also explicitly requested, proceed within that scope and under the existing approval, and do not ask for the same approval again. Do not proceed with implementation that depends on an unselected alternative until that choice is made.

## Feature Review

Always run this before writing the spec, so the user does not have to ask for it. Base it on the stated purpose, the core user flow, and, for an existing project, the related code. Sort what you find into three groups:

- **Needed features:** what the purpose cannot be achieved without, including parts the user implied but did not state. Note which of these the request already covers.
- **Missed features:** what is easy to leave out but breaks or weakens the core flow when missing, such as error recovery, empty and loading states, permissions, input validation, undo, data retention and deletion, and feedback or notifications. Include only the ones that apply to this feature.
- **Unexpected features:** ideas the user likely has not considered that serve the same purpose better, such as fewer steps, automation, or reusing data the project already has.

For each item, give one line on why it matters for this feature and a recommended choice. Skip a group that has nothing worth raising, and do not pad the list with generic checklists or features unrelated to the purpose. This review does not include researching or comparing existing products or services. Ask the user to sort the items into "include in this scope," "later candidate," or "exclude," following the choice-question rules below, and reflect the answers in the spec. For a small change, keep the review short.

## Improvement Proposals and Choices

While making the spec concrete, propose changes when you find important behavior that is missing, conflicts between requirements, constraints on the current choices, or clear improvement opportunities that directly serve the purpose. First understand the requirements, and base the proposal on evidence found in that process. Do not research replacement technologies based on a technology name alone, and do not run technology comparisons or upgrade questions for every spec. An appropriate existing choice can be kept as is.

- **Features and UX:** Handle missing behavior, usability improvements, and ways to reach the same goal with fewer steps and less cost through the feature review above. Do not attach features unrelated to the purpose or vague future extensions.
- **Platform and structure:** Do not treat replacing a technology the user specified as an improvement goal in itself. Consider alternatives only when you can specifically explain a conflict with the requirements or an important constraint, and the change is possible. Compare the points relevant to the decision, such as existing code and library dependencies, team experience, required features, supported OSes, deployment and updates, and switching cost. If the user explicitly asked for the comparison itself, research it within that scope.
- **Basis for proposals:** Do not pad the list with candidates. If there are useful platform or structure proposals, present usually one to three, starting with the highest impact. The feature review is not bound by this count, but keep it to items worth a decision. For each candidate, briefly note its relation to the current request, the problem or opportunity found, the expected benefit, the drawbacks and added cost, and the conditions under which it is recommended. Also present the existing choice it would be compared against.
- **Fact checking:** Check only the facts needed to judge the current requirements or the actual choice. Do not start with unrelated technology research to justify a proposed alternative. When you use facts that can change, such as versions, supported scope, or library compatibility, verify them against the latest official sources. Do not make up performance or memory figures without actual measurements. Mark anything you could not verify as an assumption or as something to verify, and do not use it as the basis for a definitive recommendation.
- **Choice questions:** When there is a substantive improvement candidate, ask so the user can choose between "keep the existing plan" and "adopt the proposal." Additional features can be sorted into "include in this scope," "later candidate," or "exclude." Group independent choices so they can be answered in one go, and do not list every combination. State the recommendation and its reason clearly, but keep the original request as a valid option.
- **Applying choices:** Do not replace the platform or expand the feature scope just because you proposed it. Reflect the user's choice in the spec. If the user has explicitly delegated the decision, decide according to the stated conditions and record the reasoning. If there is no answer, leave the proposal undecided and write the parts that can be finalized based on the original request.

## What the Spec Should Include

Organize the following information to match the project's size. A small change needs only a short document.

- Purpose, users, and the problem to solve
- Scope included, and scope explicitly excluded this time
- Representative user flows and the expected behavior for normal, failure, and edge cases
- Acceptance criteria that can be judged by observing the results, and how to verify them
- Current structure, and the changes needed to related files, modules, interfaces, data, and access policies
- Development and check commands you confirmed, and existing code conventions; mark commands you did not run as unverified
- Constraints, assumptions, open decisions, and the decisions needed for the next step
- Feature review items and the choice for each (included, later candidate, or excluded)
- Proposed improvements that were adopted, deferred, or dropped, with the reason for each choice; the sources used for comparison and any unverified assumptions
- If there is a UI: the main screens, states, and user flows, the mockup path, related acceptance criteria, and confirmed or open design choices

If you could not confirm a technology choice or a command, do not write it as established fact. Do not leave success criteria vague, such as "works well."

## Next-Step Guidance After Completion

When the spec is finished and you complete this request, report the paths and verification scope of the generated documents and mockups. Then hand off following "Step handoff" in the [common working principles](references/common.md). The next step is `flow-plan`. Offer these choices:

- **Create the plan** (recommended): run `flow-plan` now with this spec's path.
- **Ask more or revise the spec**: answer questions or revise the spec, then ask again.
- **Stop here**: end, and give the invocation for resuming later (`$flow-plan` in Codex, `/flow-plan` in Claude), including the spec path if it is not in the default location.

If key requirement decisions are still open, present those decisions first, and do not describe the whole spec as finalized. Recommend creating the plan only after they are resolved. If follow-up work has already been requested, continue within the permission you were given without asking, and do not tell the user to invoke a step that has already been completed.
