---
name: flow-diagram
description: Create system architecture, data flow, sequence, state transition, and other diagrams as HTML+SVG from a project's code, configuration, and documents. Use when flow-diagram is invoked or when a request asks to visualize a project's structure or behavior. Works standalone even when there is no spec document.
---

# Flow Diagram

First, read the [common working principles](references/common.md) and the [diagram design guidelines](references/diagram-style.md). Express the project's structure and behavior as HTML+SVG grounded in actual evidence. Do not start writing specs or plans, or implementing the product.

Write all user-facing output (replies, reports, documents) in the user's language unless the user or project specifies otherwise.

## Invocation

- Codex: `$flow-diagram draw the system architecture diagram of this project`
- Claude: `/flow-diagram draw the system architecture diagram of this project`
- Natural language: `flow-diagram draw the data flow from login to session storage`

From the description after the invocation, identify the target, scope, and purpose. The user does not need to know which diagram type or file location to use. If the current folder is a project, target that project; if a path or feature is specified, give it priority. Ask a question only when there is no target, or when you cannot tell which of several projects is meant.

## Spec Integration

`flow-spec` also uses this skill in its diagram step. Apply the same diagram selection, design, presentation, and verification procedure as a standalone call, but give priority to the input and output scope below.

- Draw based on the spec, related sections, design decisions, and open decisions that were passed along. A design proposal can be written even when there is no implementation. Reuse the code investigation and user decisions already gathered. Do not substitute a design proposal for the default of standalone calls, which draw the current implementation.
- Read additional code only where it is needed to confirm differences between the current implementation and the design. Show the relevant evidence and uncertainties in the diagram description, and do not present a design proposal as if it were running or deployed.
- Write HTML+SVG into the diagram area of the designated review copy, or provide fragments to insert. The default target is `spec.html`. The spec body, requirements, and UI mockups are outside this skill's scope, and do not add other diagram files by default.
- Perform the final rendering check after the diagram is integrated into the review copy, and do not duplicate the review done by `flow-spec`. Pass the results, verification scope, and remaining uncertainties back to the calling work. Do not stop the overall spec writing by giving a separate completion reply or moving on to the next step.

This is a division of authoring roles. It does not require spawning subagents, and the current agent can apply the instructions of both skills in sequence.

## Understanding the Project

1. Check the applicable project instructions and any existing diagrams or documents. Read the relevant entry points, package and build configuration, routes and call sites, data models and storage paths, and deployment configuration, as much as needed. Do not conclude that a real connection exists based only on file names or installed dependencies.
2. Unless otherwise specified, show the current implementation. If there is only a spec and no implementation, label it as a design proposal. If the current code, deployment configuration, and documents differ, distinguish the current structure from the plan, and briefly record any known differences. If a proposed structure or a before-and-after comparison was requested, draw within that scope.
3. For each node and connection, organize the evidence you confirmed, such as source paths, symbols, and configuration. Show connection direction, transmitted content and protocols, and process or host boundaries only to the level you have confirmed. Do not read unrelated source or entire sets of user data.
4. Distinguish the structure confirmed in code from the status of running services. Do not start or deploy services, or explore production environments, in order to build the diagram. Check actual running status only when requested, and then follow that request's scope and permissions. Do not include secret values read from configuration in the diagram or its description.

## Authoring

1. Choose the diagram type and level of detail that best explain the user's question. For the whole system, start with the main components and key connections; for a specific feature, start with the actual execution path. Do not generate every diagram type at once.
2. Split a complex system into an overview and the detailed diagrams it needs. When adding a node, confirm which relationship it is meant to explain. Do not invent features or technical alternatives that are not actually used just to fill out the diagram.
3. Give priority to the specified output location and insertion format, and follow the project's existing diagram style. For standalone calls, the default output is `docs/diagrams/<topic>.html`, with a short English kebab-case file name. Include the CSS and inline SVG within a single HTML file. Do not add external libraries or Mermaid as default dependencies.
4. If a file already exists, check whether the request is to update the same topic, and modify only that scope. Preserve material that serves a different purpose, and use a distinct file name for it. Do not change product code or the spec on your own initiative.
5. In the HTML, include a title, scope, a short explanation, and a legend, along with the source evidence supporting the key components and connections and any unverified items. State whether the basis is the current code or a design document, the time it was checked, and the version or commit where needed. Present source paths concisely, and do not create an unnecessary separate report.
6. Add standalone SVG or image output only when requested or when the delivery format requires it. In no case should this be interpreted as a request for a product UI mockup.

## Verification and Wrap-Up

- Check the diagram's nodes, boundaries, connection directions, states, and error paths against the code or the specified design basis. Mark inferred or unverified relationships with the legend and annotations.
- Verify the actual rendering in a browser according to the shared design guidelines, and fix any overlap, clipping, or incorrect connections you find. If visual verification is not possible, state that limitation.
- In the final answer, briefly include a link to the result file, the scope shown, the scope of the evidence check and browser verification, and the remaining uncertainties. Do not describe completing the diagram as verification of service behavior or operational status.
- Do not continue into specs, plans, implementation, knowledge-base entries, or opening separate screens unless the user requested them.
