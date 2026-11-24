---
name: flow-research
description: Settle a question with evidence from several sources. Lists the claims that would answer it, collects and grades sources for and against each claim, and writes a report where every verdict links to its sources. Use when flow-research is invoked or when a question needs more than one or two sources to answer with confidence, such as choosing between tools, checking whether something is true, or finding out where a field stands today. A quick lookup does not need it.
---

# Flow Research

Answer a question by testing concrete claims against sources, not by collecting everything that mentions the topic. The work leaves three files behind (a brief, a source list, a claim table) and one report built from them, so anyone can check how each conclusion was reached.

Write all user-facing output (replies, reports, documents) in the user's language unless the user or project specifies otherwise.

## Invocation

- Codex: `$flow-research should we run pgvector or a dedicated vector database`
- Claude: `/flow-research should we run pgvector or a dedicated vector database`
- Natural language: `flow-research is it true that WebAssembly is faster than JavaScript for image processing`

Treat everything after the invocation, plus the conversation so far, as the question. Inside a project, read its `AGENTS.md` and applicable rules first; they may name trusted sources or a place for documents.

## Files

All files go in `research/<slug>/` under the current working directory, unless the project or user names another place. The slug is short English kebab-case (`pgvector-vs-dedicated`). If that folder already exists for a different question, choose a more specific slug. If it holds earlier work on the same question, keep its files and extend them instead of starting over.

| File | Holds |
|---|---|
| `brief.md` | The question, what it is for, limits, depth, and the claims to settle |
| `sources.md` | Every source consulted, numbered `S1`, `S2`, …, with grade and what it says |
| `claims.md` | One row per claim: verdict and the source numbers behind it |
| `report.md` | The answer for the reader |

## Steps

### 1. Write the brief

Put these in `brief.md`:

- **Question**, in one sentence, and **purpose**: the decision or understanding it supports.
- **Limits**: time range, region, budget, platform, sources to prefer or avoid. Copy what the user said; add nothing they did not imply.
- **Depth**: `quick` (a handful of claims, roughly 10 lookups in total), `standard` (about 30), or `deep` (60 or more, only when the user asks for thoroughness or the stakes clearly call for it). Default to `standard`.
- **Claims to settle**: three to eight specific, checkable statements whose truth would answer the question. Write each so it can turn out false. "Qdrant needs less memory than pgvector for 10M 768-dimension vectors" works; "Qdrant's memory use" does not. Include the claim you expect to be wrong, not only the ones you expect to hold.

If a limit is genuinely unclear and would change which claims matter (which country's law, which product of that name), ask the user one question with short options before going further. Use the environment's structured question tool if it has one; otherwise ask in plain text with numbered choices and put nothing after the question. Otherwise state the assumption in the brief and continue.

### 2. Gather sources

Read [references/sources.md](references/sources.md) for how to search, grade, and record sources. For each claim, look for evidence on **both** sides: search for the claim and for its opposite. Record every source you actually read in `sources.md`, including ones that turned out useless, so the same page is not read twice.

Stop gathering when every claim has either two independent sources that agree, one primary source that settles it, or a recorded dead end; or when the depth budget from the brief is spent. New claims that come up along the way can be added to the brief if they matter to the question.

**Splitting the work.** When the brief has several claims that need separate searches and the environment can hand tasks to sub-agents, give each sub-agent a group of related claims, the brief's path, the path of `references/sources.md` in this skill's folder, its own file `sources-<group>.md`, its share of the depth budget, and its group letter for numbering sources (`A1`, `A2`, …). Ask it to return a short list of which claims it covered and which way the evidence points. Check the numbering before merging and fix any collisions. Merge the group files into `sources.md` afterwards, keeping the numbers. Without sub-agents, work through the claims yourself in order of importance.

### 3. Judge each claim

Fill `claims.md` with one row per claim:

```markdown
| # | Claim | Verdict | Sources | Why |
|---|---|---|---|---|
| C1 | <claim text> | supported | S2, S5 | Two independent benchmarks agree; S5 is the maintainer's own docs |
```

Verdicts:

- **supported**: independent sources agree, or one primary source settles it, and nothing credible contradicts it.
- **refuted**: credible evidence shows it is false.
- **mixed**: credible sources disagree, or it holds only under conditions. Name the conditions.
- **vendor-only**: the only evidence comes from a party that benefits from the answer.
- **unknown**: not enough evidence either way.

Do not upgrade a verdict because the claim sounds plausible. A table with honest `unknown` rows is more useful than a confident one that is wrong.

### 4. Write the report

Read [references/report.md](references/report.md) and write `report.md` from the brief, the sources, and the claim table. The report may only state what the claim table supports. If writing it reveals a gap that changes the answer and budget remains, go back to step 2 for that claim; otherwise list it as an open check.

### 5. Hand it over

Reply with the answer in a few sentences, the claims that decided it, the most important caveat, and the absolute path of `report.md`. If the environment has a tool that sends files to the user, send the report with it too.

Do not continue into specs, plans, implementation, or knowledge-base entries unless the user asks.
