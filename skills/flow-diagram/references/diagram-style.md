# HTML+SVG Diagram Design Guidelines

These are the guidelines shared by the visual specs of `flow-diagram` and `flow-spec`. Keep the presentation approach of existing visual specs. There is no need to install or invoke a specific external diagram skill, or to copy its templates. If the user specifies a different format, follow that choice.

## What to Draw

First decide the question you want to answer, and select only the diagrams that are needed.

| Review question | Representation |
|---|---|
| In what order does the user reach the goal? | User flow and key failure and recovery branches |
| Which components exchange what? | System architecture diagram with labeled connections |
| What is the order of calls and responses? | Sequence diagram and swimlanes |
| Which events change the state? | States and transition conditions |
| How is the data connected? | Required entities, relationships, and cardinality |
| Which processes are placed where? | Runtime and deployment boundaries and components |
| What changes if the proposal is adopted? | Comparison of the current plan and the proposal, aligned to the same criteria |

Do not fill in every type. Split complex content into an overview and detail diagrams, and distinguish undecided alternatives from confirmed structure. Decide what lines, arrows, and containment relationships mean, and make sure they match the specified code or design basis.

## Presentation and Implementation

- Put CSS and inline SVG within a single HTML file. The default output must not require a framework, build step, CDN, external fonts, or network requests. Use system fonts, and do not include unnecessary JavaScript for small diagrams.
- Use semantic HTML for the document body and for diagram titles and descriptions, and draw related shapes and connecting lines in SVG. Give each SVG a `viewBox` and an accessible title and description, and make sure IDs do not collide across multiple figures.
- If the project has existing diagrams or a visual language, use it. If not, keep it calm: a neutral paper-like background, crisp body text, thin lines, and a limited accent color. Do not include strong shadows, gradients, meaningless icons, or decorative animation by default.
- Keep sizes, margins, alignment, and line weights consistent. Use the accent color for key paths or important decisions, and do not distinguish states by color alone. Make sure connecting lines do not cross text, and arrows do not land on the wrong node.
- Fit SVG text to the node size and break lines explicitly. Do not shrink the whole figure so much that the text becomes hard to read. Split large diagrams, or provide scrolling for their area.
- Give each figure a short caption that explains what it shows. Make sure the important structure and flow can also be understood from the body text.

## Content and Visual Verification

- Check the content against the evidence first. Confirm that nodes, states, connections, and error paths match the actual evidence, and that no design proposal or inference appears as if it were the current implementation. Do not confuse whether something is implemented with whether it has actually been run or deployed.
- Verify actual rendering with whatever browser tool is available in the environment. If the user or project specifies a browser tool, use it. Check at a typical desktop width and at a narrow screen width for clipped text, overlapping nodes, arrow paths, readability, and document navigation. Do not replace visual verification with DOM or coordinate checks alone.
- Fix the problems found in the visual review together, then verify again. Do not repeatedly capture the same result, or keep fine-tuning when no new defects are found.
- If no browser is available, perform only file and reference checks, and state that visual verification was not performed. If a specified browser tool exists, do not switch to a different browser for verification.
- Do not open a separate window or panel just by generating the HTML. If the user asks to see it in a specific way, follow that method.
