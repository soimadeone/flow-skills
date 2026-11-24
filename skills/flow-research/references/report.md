# Report Template

The reader wants the answer, how sure it is, and where to check. Write for someone who will act on it.

Write all user-facing output (replies, reports, documents) in the user's language unless the user or project specifies otherwise.

## Template

```markdown
# <Question restated as a short title>

<one line: date researched, depth, number of sources read>

## Answer
<Two to five sentences. Start with the direct answer, including the conditions under which it holds. Then the one or two facts that decide it.>

## Claims
| Claim | Verdict | Based on |
|---|---|---|
| <claim> | supported | [S2](#sources), [S5](#sources) |
| ... | ... | ... |

## Details
### <Claim or group of related claims>
<What the evidence shows, with numbers and dates. Where sources disagree, give each side and say which is stronger and why. Cite as [S3](#sources).>

### <Next group>
...

## Caveats
<Short list: evidence that is vendor-only or old, assumptions from the brief, anything that limits how far the answer travels.>

## Open checks
<What would change the answer, and how to find out: a test to run, a source that was unavailable, a question for a vendor.>

## Sources
- **S2** · I · 2025-04 · [<title> — <publisher>](<URL>)
- ...
```

## Rules

- **Only state what the claim table supports.** A verdict of `mixed` or `unknown` stays that way in the report; do not smooth it into a recommendation without saying so.
- **Keep conditions attached.** If a claim holds only above a size, in a region, or for a version, the condition appears every time the claim does.
- **Give your call.** After the evidence, say what you would choose or conclude and why, and label it as a judgment when the evidence alone does not force it.
- **List only cited sources** under Sources, with their grade and date, so the reader can weigh them at a glance.
- **Match length to the question.** A tool choice may fit on one screen. Add detail only where it changes what the reader would do.
- **Prefer tables for comparisons** across the same criteria, and plain paragraphs for explaining why.
