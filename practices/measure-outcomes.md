# Track outcomes alongside throughput

**Use when:** You're trying to decide whether an agent workflow helped, and the easiest numbers to collect are PR count and revert rate.

## Apply

1. Before the pilot, choose one user outcome that matters (for example, time from app launch to interactive on a fixed hardware profile) and one way to detect harm (for example, a report of user-visible regressions, including those fixed forward).
2. Write down the baseline, measurement method, and follow-up window. Compare similar cohorts and workloads. If reporting reverts, count reverted PRs and the merged PRs in the same cohort, and give each PR the same follow-up time.
3. Read throughput next to those outcomes. If throughput rises while the chosen user outcome or regression signal worsens, investigate before expanding the workflow.

```markdown
- Scope and baseline window:
- User outcome and how we measure it:
- Regression signal (including fixes that don't revert) and follow-up window:
- Throughput in the same scope:
- Result, caveats, and decision:
```

## Limits

PR count depends on how work is split, and revert rate depends on how the team responds to failures. A before/after comparison can inform a decision; it can't isolate the effect of agents from other changes in the same period.

## Sources

Lauren Tan, *All Code Is Slop (Except Mine)*, [13:32–14:51](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=812s) (team-reported throughput, reverts, and performance). The measurement checklist is this catalog's adaptation, not the team's reported methodology. See the [source review](../sources/lauren-tan-all-code-is-slop.md).
