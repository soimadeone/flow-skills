# Finding and Grading Sources

## Searching

- Start from the claim, not the topic. Turn the claim into the words a source would use if it confirmed it, then into the words it would use if it disproved it.
- Go to where the fact originates: specification, statute, dataset, paper, release notes, issue tracker, financial filing. A page that summarizes others is a lead to the original, not evidence.
- Read the relevant part of the page itself before recording it. A search result snippet is not a source. If your fetch tool returns a summary of the page rather than its text, note that under the source's weak points, and fetch again with a narrower request when an exact figure or quote matters.
- When results are thin, change the vocabulary (the field's jargon, the product's old name, another language the source might be written in) rather than repeating similar queries.
- Count each search and each page read toward the depth budget in the brief. Run independent lookups at the same time when the environment allows it.
- Use whatever search, fetch, and internal document tools the environment provides. If none are available, say so and work only from sources the user supplies.

## Grading

Give each source one grade:

| Grade | Meaning | Examples |
|---|---|---|
| **P** primary | The origin of the fact | Official docs, the standard, the paper, raw benchmark data, court ruling |
| **I** independent | Informed and has nothing to gain from the answer | Peer review, third-party benchmark with published method, reputable journalism with named sources |
| **V** interested | Benefits from one answer | Vendor blog, competitor comparison, sponsored content |
| **C** community | Individual experience | Forum thread, issue comment, personal blog |

Then note anything that weakens it:

- **old**: dated before something important changed (a release, a law, a price change);
- **forecast**: describes plans or predictions, not outcomes;
- **unsourced**: makes claims with no data, method, or named origin;
- **derivative**: repeats another source already in the list (link the original instead).

Two sources count as independent only if neither relies on the other and they do not share an interest in the answer. Three vendor posts citing the same internal test are one source.

When the source you trust most disagrees with your background knowledge on a recent topic, go with the source and note it.

## Recording

Append each source to the list file as soon as you read it. Number sources with the prefix you were given (a group letter such as `A` or `B` when the work was split), or `S` if none was given. The example below uses `S`:

```markdown
## S4 · I · 2025-04
**<Title>** — <publisher or author>
<URL>
- C2: <what it says about the claim, with the exact figure or wording>
- C5: <...>
- Weak points: <old, forecast, method unclear, ...> or "none noted"
```

If a page turned out to be irrelevant or unreadable, record it in one line (`## S9 · — · unreadable: <URL>`) so it is not retried.

Never write a figure, quote, or date you did not read in the source. If something you expected to find does not exist, that absence is a finding: write it against the claim.
