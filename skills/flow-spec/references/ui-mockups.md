# UI Mockups for Spec Review

For features that need a UI, create a mockup alongside the spec so the user can review the main screens and behavior and adjust the spec. Do not make the user request the mockup separately. Read this document only when the feature has a UI.

## Scope

- Applies to scopes that need real user-facing screens, such as web, mobile, or desktop apps. If the change affects only part of an existing app, focus on the relevant screens and related states. If only a server, batch job, or library was requested, do not add new admin screens just for the mockup.
- First understand the spec's purpose, core flows, permissions, and acceptance criteria. Ask only about open decisions that would change the outcome. Once the features and information structure are set, do not stop work over minor color or spacing choices.
- Give priority to the existing UI and design system, and to any reference screens the user provided. For a new product, propose one consistent direction that fits its purpose, users, and platform. Do not force the document style of diagrams onto real app screens. Unless multiple alternatives were explicitly requested, do not generate the same screen in several designs.
- Do not try to implement the whole app. Build only to the level where the core user flows can be reviewed. A small feature can be covered by one screen. If a feature only makes sense when moving between several screens, link those screens along the path.

## Outputs and Linking to the Spec

- If an existing mockup location and format exist, reuse them. If not, default to `mockups/<feature-name>/index.html`, using a short English kebab-case folder name. Multiple screens can be built as screen transitions within one HTML file, and can be split when needed for size or review convenience. Do not overwrite mockups from other work.
- In the spec, record each screen's ID, its purpose, entry path and main actions, related acceptance criteria, and the mockup path. If an HTML review copy exists, make it possible to navigate to the mockup from there as well. Make clear what each one is for reviewing: the spec's structure diagrams, or the actual product screen mockups.
- The mockup expresses the spec's requirements. Do not quietly add features or policies to fill out a screen. Record any feature or policy choices you discover while creating the mockup back in the spec's proposals or open decisions.
- When the spec or a design decision changes, update the affected mockups as well. Distinguish confirmed content from proposed status so that follow-up planning can implement it. Do not assume the user has approved the design just because a mockup was created or merely viewed.

## Screens and States

- Use realistic UI copy and sample data of the kind the actual product would use, written in the product's language (the user's language by default). Make any sample personal information fictional, and do not pull in real user records or secrets.
- Build the normal state and flow for the core tasks. Include whichever of these states are needed to review the feature's acceptance criteria: empty list, loading, failure, validation error, permission restriction, and success feedback. Do not mechanically fill every screen with the same list of states.
- Make important interactions such as list-to-detail, search and filter, form validation, and modal or tab switching demonstrable with local example state. Do not make something look functional when pressing a button does nothing. Disable features outside the scope, or explain the demo's scope.
- Write the screens in HTML/CSS with meaningful information hierarchy, primary actions, labels, spacing, and contrast. Do not replace the entire UI with a single image or SVG. Give forms labels, and give buttons keyboard access and a visible focus indicator.
- Use screen sizes that fit the target platform and requirements. For a responsive web app, design for narrow screens too. For a desktop-only app, handle window resizing and content overflow without adding an unnecessary mobile version.

## Execution Boundaries and Review

- By default, use a standalone HTML file that opens without a build step, including its CSS and the minimum necessary JavaScript. Do not include external frameworks, CDNs, or network requests as default dependencies. If the existing mockup environment is a better fit, follow its conventions.
- Simulate APIs, authentication, uploads, saving, and similar operations with example data, and make clear that it is a mockup. Do not claim that anything was actually saved or that server processing succeeded. Do not call production APIs, or implement or deploy product code, to build the mockup.
- Use whatever browser tool is available in the environment to verify actual rendering and the core interactions. If the user or project specifies a browser tool, use that one. Check the target screen size and any needed smaller sizes together in one pass, fix any clipping, overlap, poor readability, or non-working key actions, and then verify again. Do not keep fine-tuning endlessly without changing content.
- If no browser is available, run file and reference checks, and state that screen and interaction verification was not performed. Distinguish static file checks from actual browser verification.
- In the final result, briefly note the mockup path, the screens and states included, the scope of the local simulation, what was verified, and the remaining decisions. Open a separate window or panel only when the user asks for it, and in the way they asked.
