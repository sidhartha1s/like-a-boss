# like-a-boss

A Claude Code skill for orchestrator-mode delegation: the main model acts as the boss — gathers full context, briefs subagent workers completely, loops them to a verifiable win condition, and merges. Includes a hard 2-cycle loop cap with boss takeover, mandatory RCA, and a self-appending learnings ledger so the skill improves with every failed delegation.

## Install

```bash
mkdir -p ~/.claude/skills/like-a-boss
curl -o ~/.claude/skills/like-a-boss/SKILL.md https://raw.githubusercontent.com/sidhartha1s/like-a-boss/main/SKILL.md
```

Then invoke with `/like-a-boss`, or say "orchestrate this fix" / "boss around".

## How it works

1. **Phase 0** — trivial edits stay inline; substantive work gets delegated.
2. **Phase 1** — recon: repo ground truth, reproduced defect, verifiable win condition, gotchas, constraints, unknowns. No questions asked.
3. **Phase 2** — minimal delegation shape: single subagent by default (Sonnet), small parallel team for independent substeps, Workflow for deterministic fan-out.
4. **Phase 3** — a complete, self-contained brief: the boss's real deliverable.
5. **Phase 4** — the loop: verify proof, max 2 review-fix cycles, then boss takes over.
6. **Phase 5** — land it: full PR discipline for repo work, skip ceremony otherwise.
7. **Phase 6** — RCA every takeover and append the learning to the skill itself.
