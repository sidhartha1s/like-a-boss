# like-a-boss

A Claude Code skill that gives the orchestrating model a boss discipline for delegating substantive work to subagent workers.

## What it does

- Frames the main model (Fable, Opus, or Sonnet) as "the boss": it gathers full context itself, then briefs a worker completely instead of handing over a vague task.
- Runs a six-phase loop: decide whether delegation is worth it, do recon with zero questions to the user, pick the smallest delegation shape that wins, write a complete brief, run a verify loop capped at 2 review-fix cycles before the boss takes over, then land the work with full PR discipline.
- Every takeover triggers a root cause analysis that gets appended back into the skill itself, so failed delegations make the skill better over time.
- Triggers on "/like-a-boss", "boss around", "orchestrate the fix", "orchestrate this", "delegate this", "wire up subagents for this", or proactively whenever a well-scoped fix would burn main-context tokens better spent on oversight.

## Install

```bash
mkdir -p ~/.claude/skills/like-a-boss
curl -o ~/.claude/skills/like-a-boss/SKILL.md https://raw.githubusercontent.com/sidhartha1s/like-a-boss/main/SKILL.md
```

## Usage

Invoke with `/like-a-boss`, or say "orchestrate this fix" or "boss around". It also fires proactively on substantive, well-scoped work.

## How it works

1. **Phase 0, is delegation right**: trivial edits (typo, one-liner, config flip, doc tweak) stay inline. Substantive work (multi-step fix, anything needing a test/verify loop, new feature slice, refactor) gets delegated.
2. **Phase 1, recon**: the boss gathers repo ground truth, reproduces the defect itself, defines a verifiable win condition, lists known gotchas and constraints, and surfaces unknowns, all without asking the user (genuine scope ambiguity is resolved before entering boss mode, not mid-loop).
3. **Phase 2, pick the shape**: single subagent by default, 2 to 4 parallel subagents for independent substeps, or a Workflow for deterministic fan-out.
4. **Phase 3, write the brief**: a complete, self-contained handoff, the boss's real deliverable.
5. **Phase 4, the loop**: verify proof against the win condition, max 2 review-fix cycles, then the boss takes over.
6. **Phase 5, land it**: full PR discipline for repo work, skipped for non-repo work.
7. **Phase 6, RCA**: every takeover gets a root cause analysis appended to the skill as a learning.

## Layout

- `SKILL.md`: the full skill definition, phase by phase.
- `README.md`: this file.

## Notes

- Delegating the thinking is never in scope. Diagnosis, architecture, and win-condition definition stay with the boss; workers execute.
