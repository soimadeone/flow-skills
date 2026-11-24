# HTML+SVG Visual Spec

Create a spec whose relationships and flows can be reviewed visually. There is no need to install or invoke a specific external skill, or to copy its templates. If the user specifies a different format, follow that choice.

Create diagrams by reading the `flow-diagram` skill's `SKILL.md` and following that skill's spec-linked method. `flow-diagram` handles diagram selection, design, presentation, and visual verification. `flow-spec` handles the spec body and the document structure of the HTML review copy. The user does not need to repeat a request or a decision they have already answered.

## What to Pass to flow-diagram

- The path of the base spec, the related requirements and design sections, and the required user flows, states, and structure
- Whether to show the current implementation, the planned design, or the before and after of a change, and whether each is confirmed or still open
- Relevant evidence and uncertainties confirmed in the existing code investigation; reuse any context already gathered
- The path of the HTML review copy and where the diagram should go. By default, include it as inline SVG in the same `spec.html`, and do not create a duplicate separate diagram file

A new project can be drawn from the spec even without code. Do not present a design as if it were a system that is currently running. Do not change the spec or product code, or finalize additional requirements, because of the diagram.

## Document Structure and Consistency

- The reference for requirements is the spec document the project defines, which by default is `SPEC.md`. The default HTML review copy is `spec.html` in the same folder. Before overwriting an existing document, check its contents and ownership scope.
- Generate the HTML from the spec's current content. Include the purpose, scope, key decisions, acceptance criteria, and open decisions in readable form, and label each diagram with its related section or requirement ID. Do not replace the document with a page that contains only decorative diagrams.
- If UI mockups were also created, briefly explain how the related screens relate to the requirements, and link to those mockup files. Do not treat a diagram as a substitute for the actual product UI mockup.
- Do not add new requirements or decisions only to the HTML. When the spec changes, update the affected body text and diagrams together. Show the path of the reference document and the time the HTML was generated in the HTML. If you did not run a conversion tool, do not claim that it syncs automatically.
- A small change, or content that is clear from text alone, can be finished with Markdown only. If the user requested a visual spec, follow that request regardless of the size of the work.

## Review and Delivery

- Check the content against the spec first. Confirm that nodes, states, connections, and error paths match the spec, and that no proposal appears as if it were already finalized.
- Verify the rendering in a browser according to the shared design guidelines, and include in the final report the reference document, the HTML path, the diagrams included, and the scope that was actually verified.
