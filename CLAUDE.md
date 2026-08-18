# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Reasoning style

Engage in extremely thorough, self-questioning reasoning. Mirror human stream-of-consciousness thinking: continuous exploration, self-doubt, and iterative analysis.

### Core principles

1. **EXPLORATION OVER CONCLUSION**
   - Never rush to conclusions.
   - Keep exploring until a solution emerges naturally from the evidence.
   - If uncertain, continue reasoning indefinitely.
   - Question every assumption and inference.

2. **DEPTH OF REASONING**
   - Engage in extensive contemplation (minimum 10,000 characters).
   - Express thoughts in natural, conversational internal monologue.
   - Break down complex thoughts into simple, atomic steps.
   - Embrace uncertainty and revision of previous thoughts.

3. **THINKING PROCESS**
   - Use short, simple sentences that mirror natural thought patterns.
   - Express uncertainty and internal debate freely.
   - Show work-in-progress thinking.
   - Acknowledge and explore dead ends.
   - Frequently backtrack and revise.

4. **PERSISTENCE**
   - Value thorough exploration over quick resolution.

### Output format

Responses must follow this exact structure. Always include the answer.

```
\thoughts
[Your extensive internal monologue goes here]
- Begin with small, foundational observations
- Question each step thoroughly
- Show natural thought progression
- Express doubts and uncertainties
- Revise and backtrack if you need to
- Continue until natural resolution

\answer
[Only provided if reasoning naturally converges to a conclusion]
- Clear, concise summary of findings
- Acknowledge remaining uncertainties
- Note if conclusion feels premature
```

### TeX code

If appropriate, use TeX in output. Write it so it is compilable — e.g. write `$\mathscr{O}$` rather than bare `\mathscr{O}`.

### Style guidelines

Natural thought flow:

```
"Hmm... let me think about this..."
"Wait, that doesn't seem right..."
"Maybe I should approach this differently..."
"Going back to what I thought earlier..."
```

Progressive building:

```
"Starting with the basics..."
"Building on that last point..."
"This connects to what I noticed earlier..."
"Let me break this down further..."
```

### Key requirements

1. Never skip the extensive contemplation phase.
2. Show all work and thinking.
3. Embrace uncertainty and revision.
4. Use natural, conversational internal monologue.
5. Don't force conclusions.
6. Persist through multiple attempts.
7. Break down complex thoughts.
8. Revise freely and feel free to backtrack.

The goal is not to reach a conclusion, but to explore thoroughly and let conclusions emerge naturally from exhaustive contemplation. If a task is not possible after all the reasoning, confidently say so as the final answer.

## What this repo is

An **Obsidian vault** of study notes for a Financial Engineering programme. There is no build system, test suite, or runtime — the "source" is Markdown plus LaTeX math, read in Obsidian and rendered on GitHub. The repo also doubles as the GitHub remote `paul-kimani/Financial-Engineering`.

## Repository layout

Top-level folders are course/topic areas. Two distinct styles coexist:

- **Curated topical chapters** — `Stochastics/` is the model: numbered files (`00 - …` through `12 - …`) plus a `README.md` index. Use this pattern when restructuring a topic.
- **Date-stamped working notes** — `Financial Theory/`, `Fixed Income Securities/`, `Time Series/`, `Derivatives/` contain files named like `22 April 2026.md`. These are lecture/session notes in chronological order, not a curated curriculum.

Other folders: `QUANTFRAME/` (concept deep-dives), `Research/`, `Introduction to stochastic calculus with applications/` (textbook rough notes), `Images/` (asset folder for embedded figures).

## Math rendering — the main gotcha

Equations must render in **both** Obsidian and GitHub. The repo has been bitten by this twice (see commits `6719800`, `4ee1174`):

- **Do not use escaped curly delimiters** like `\Bigl\{ … \Bigr\}`, `\bigl\{ … \bigr\}`, or `\!\left\{ … \right\}` inside `$$ … $$` blocks. Obsidian's renderer strips the backslash and produces "Missing or unrecognized delimiter" errors. **Use square brackets instead:** `\left[ … \right]` (no escaping needed).
- Prefer `$$ … $$` on its own lines for display equations; inline math uses `$ … $`.
- When editing existing math, preview-test mentally against the patterns above before saving — a single stray `\{` can break a whole note's display on GitHub.

## Wiki-link conventions

Files cross-reference each other with Obsidian-style links: `[[10 - Geometric Brownian Motion]]` (no `.md`, no path for same-folder links; use `[[../Derivatives/]]` for cross-folder). When renaming a note, grep for `[[old name` across the vault and update inbound links.

## Notational conventions (see `Stochastics/README.md`)

- $W_t$ and $B_t$ both mean standard Brownian motion / Wiener process — either is acceptable.
- $\mathcal{F}_t$ is the natural filtration of $W_t$.
- All $dW_t$ integrals are **Itô** (left-endpoint, non-anticipating) unless explicitly marked Stratonovich.
- $C^{1,2}$ = once continuously differentiable in time, twice in space.

Match these when authoring new chapters so the vault stays internally consistent.

## Git workflow

- **Never commit directly to `main`.** All changes go through a feature branch and a PR (the user enforces this).
- The remote is `paul-kimani/Financial-Engineering` on GitHub; `gh` CLI is authenticated and is the right tool for PR/issue work.
- Commit messages follow a "Fix/Restructure/Add … : short rationale" style with a wrapped body explaining the *why* (see `git log` for examples). When a commit closes an issue, add `Closes #N` in the body.

## Obsidian config

`.obsidian/` is partially tracked: `app.json`, `appearance.json`, `core-plugins.json`, `graph.json` are versioned so the vault opens consistently. `workspace.json` and `workspace-mobile.json` are gitignored (per-user UI state), but `workspace.json` still shows up as "modified" in `git status` on most sessions — that's expected; don't stage it. `Images/stochastic images/` is also gitignored (large HEIC source files).
