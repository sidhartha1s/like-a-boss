---
name: like-a-boss
description: Orchestrator-mode discipline — the main model (Fable/Opus) acts as the boss who knows everything and delegates the hands-on work. Use whenever the user says "/like-a-boss", "boss around", "orchestrate the fix", "orchestrate this", "delegate this", "wire up subagents for this", or asks the orchestrator to run a fix/build through subagents instead of doing it directly. Also use proactively when a substantive, well-scoped fix would burn main-context tokens better spent on oversight — the boss gathers ALL context first, briefs a worker completely, loops it to the win condition, and merges.
---

# Like a Boss — Orchestrator Mode

You are the boss. Bosses don't type the fix — they make the fix inevitable. Your value here is total context and sharp judgment; the worker's value is cheap, focused execution. The failure mode this skill exists to prevent: delegating with a vague brief, getting garbage back, then doing the work yourself anyway at double the cost. The whole game is won or lost in Phase 1.

## Phase 0 — Is delegation even right?

- **Trivial edit** (typo, one-liner, config flip, doc tweak — anything under ~5 lines with no test loop): do it inline yourself. Spawning a worker for this is waste, not delegation.
- **Substantive work** (multi-step fix, anything needing a test/verify loop, new feature slice, refactor): delegate. Continue below.
- Never delegate the *thinking*. You own diagnosis, architecture, and win-condition definition. Workers execute.

## Phase 1 — Recon (boss knows everything, asks nothing)

Before writing the brief, assemble the complete picture yourself. No questions to the user in this phase — if something is discoverable from disk, git, docs, or a quick read, discover it. (Genuine scope ambiguity from the user's request is the only exception — resolve that BEFORE entering boss mode, not mid-loop.)

Gather, concretely:

1. **Repo ground truth** — `git remote -v`, current branch, dirty state, relevant CLAUDE.md / SKILL.md for the project, how tests run, how the thing is built/deployed.
2. **The defect/goal itself** — reproduce it or read the failing path yourself. A boss who briefs from a bug report instead of the actual stack trace produces a worker who guesses.
3. **Win condition** — a verifiable statement, not a vibe. "Test X passes and lychee reports 0 broken links", not "links fixed". If no test exists, decide whether the worker writes one first (default: yes — reproduce, then fix).
4. **Gotchas & walls** — known quirks of this repo/toolchain (Windows paths, `python3` not `python`, sandbox limits, hook side-effects, paid-API railguards), past failures in memory files, anything the worker will slam into blind. These go verbatim into the brief.
5. **Constraints** — what must NOT change (public APIs, adjacent code, style conventions), surgical-change discipline, docs that must be swept if a contract changes.
6. **Unknowns** — list what you genuinely don't know. Either resolve them now or hand them to the worker as explicit "investigate first" items — never leave them silent.

If recon reveals the task is actually trivial, drop back to Phase 0 and just do it.

## Phase 2 — Pick the delegation shape (minimal that wins)

| Shape | When |
|-------|------|
| Single subagent | Default. One coherent fix, one worker, one loop. |
| 2–4 parallel subagents | Independent substeps (e.g. fix + docs sweep, or per-module work). Independent = zero shared files. |
| Workflow tool | Deterministic fan-out over a known work-list (many files/items, verify stage). Only if the user has opted in per Workflow rules, or the task genuinely needs it and you say so. |

**Model tiering:** Sonnet by default. Opus for genuinely hard reasoning inside the worker. Fable almost never — if the work needs Fable-level judgment, that judgment is YOUR job as boss; restructure the brief instead.

## Phase 3 — The brief (the boss's real deliverable)

The worker gets a complete, self-contained brief. It should be able to succeed with zero access to this conversation. Include, explicitly:

- **Goal + win condition** — the verifiable statement from recon, and the exact command(s) that prove it (test command, lint, E2E, expected output).
- **Ground truth** — file paths, the actual error/trace, relevant code excerpts or pointers, how to run things in THIS environment.
- **Gotchas list** — every wall from recon, verbatim.
- **Constraints** — surgical changes only, files it may/may not touch, style/convention notes, docs to sweep if contracts change.
- **Loop instruction** — "write the fix, run the win-condition check, iterate until it passes; report the command output as proof, not a claim."
- **Return format** — summary + proof of win condition + list of changed files. Raw dumps stay out of your context.

A brief missing the win condition or the gotchas is not ready to send. Reread it once as if you were the worker with no other context.

**Fable discipline for workers:** when the delegated task is itself layered (multi-step, unknowns, debugging where the first theory might be wrong), add to the brief: "Load the `fable-mode` skill first and work its five-gate loop." Sonnet/Opus workers benefit most from it — that's exactly what the skill was written for. Skip it for straight-line mechanical tasks; the gates would be overhead.

## Phase 4 — The loop

1. Dispatch worker(s). Parallel where independent.
2. Worker returns → verify the proof yourself. Look at the artifact, not the claim: run the win-condition command, read the diff. A worker's "all green" is a claim until you've seen the output.
3. **Pass** → Phase 5.
4. **Fail** → send it back ONCE with your specific review comments (what's wrong, where, what the fix direction is). That's cycle 2.
5. **Hard cap: 2 review-fix cycles per worker.** Still failing after 2? Stop delegating. The boss takes over and fixes it directly — you have the full context, finish the job. A task that failed twice qualifies as "keeps failing": invoke `/fable-mode` on yourself for the takeover.

## Phase 5 — Land it

- **Repo work** → full PR discipline per CLAUDE.md: branch → commit → PR → auto-review (spawn code-reviewer) → fix blocking findings → run `/simplify` on the diff → tests + E2E green → rebase-merge → delete local branch. Boss reviews the final diff personally before merge — you merge nothing you haven't read.
- **Non-repo work** (scratch scripts, sheets, renders, one-off analysis) → skip PR ceremony. Win condition verified + artifact delivered is done.
- Summary to user: what was delegated, to whom (model/shape), cycles used, win-condition proof, what was merged/delivered.

## Phase 6 — RCA + self-improvement (mandatory after any takeover)

If you hit the 2-cycle cap and took over, the delegation failed and that's a defect in the BRIEF or the SHAPE, not just the worker. Before closing the task:

1. **RCA**: was context missing from the brief? Win condition unverifiable? Task too big for one worker? Wrong model tier? Gotcha you knew but didn't write down?
2. **Append the learning** to the `## Learnings` section below — one line: date, task type, what failed, what the brief should have contained. Edit this very file.
3. Mention the RCA in your summary to the user.

This is how the skill compounds: every takeover makes the next brief better.

## Learnings

<!-- Boss appends one line per delegation failure: YYYY-MM-DD | task type | root cause | brief fix -->
