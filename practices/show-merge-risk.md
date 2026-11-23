# Show rollback and blast radius in the PR

**Use when:** The diff alone doesn't tell a reviewer what merging, deploying, or enabling the change could affect, or whether those effects can be undone.

## Apply

1. On one meaningful PR, name the trigger (merge, rollout, or later activation). Write what recovery actually requires: disabling a flag, restoring a previous build, repairing data, or accepting a side effect that can't be undone. Separately name who or what could be affected and how far a failure could spread.
2. Ask the owner to check both claims against the code and release path. For example, if the new CLI writes a new file format, write a disposable file with the new version and try opening it with the old one. A clean Git revert doesn't prove rollback works on data already changed.
3. Use lighter **attention**, not weaker gates, for changes with verified easy recovery and limited impact. For wide, irreversible, or uncertain effects, ask the relevant human owner to review the risky path and its rollout or recovery plan before it takes effect. Don't change merge or deployment permissions to try this.

Add this to the PR body (one line per field is enough to start):

```markdown
- Trigger and rollback: <when the effect starts; how to stop or undo it; what remains>
- Blast radius: <who or what is affected; scope and worst plausible effect>
- Review focus and evidence: <risky assumption, owner, and linked check or open question>
```

## Source examples

Pocock presented his then-in-progress **PR skill** as a way to put a “merge danger” summary in the PR: one-way or two-way door and the blast radius. The [released template](https://github.com/mattpocock/skills/blob/v1.3.0/skills/engineering/pr/SKILL.md) also asks for before/after evidence. He credits Dex Horthy's HumanLayer [**Show Me** skill](https://github.com/humanlayer/skills/blob/4d69d20f6e9b57b4fcf0d501ccfb1be961f0f329/plugins/show-me/skills/show-me/SKILL.md) for compact visuals and shows a CLI command sketch. A sketch helps if it makes a boundary clearer; a short text note is enough otherwise.

## Limits

Reversible is not the same as low impact, and “localized” isn't evidence unless the boundary is enforced. Don't trust the author agent's label without checking the actual execution path. A summary doesn't replace diff review, tests, approval, or a rollback drill when the stakes warrant one.

## Sources

Matt Pocock, *Fixing the PR Bottleneck*: [16:23–16:58](https://www.youtube.com/watch?v=LlgiOCmFG_w&t=983s) (PR skill in progress), [17:44–18:43](https://www.youtube.com/watch?v=LlgiOCmFG_w&t=1064s) (irreversible email and blast radius), and [18:44–19:43](https://www.youtube.com/watch?v=LlgiOCmFG_w&t=1124s) (Show Me and the CLI sketch). See the [source review](../sources/matt-pocock-fixing-the-pr-bottleneck.md). Amazon's [rollback guidance](https://aws.amazon.com/builders-library/ensuring-rollback-safety-during-deployments/) describes verifying that the old version still works after the new one runs; its [deployment guidance](https://aws.amazon.com/builders-library/automating-safe-hands-off-deployments/) keeps team review before merge even with automated rollout. The template and unchanged gates above are catalog recommendations.
