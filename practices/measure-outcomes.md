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

## Source example

The chart shows 10,926 October PR merges, not deployments. Tan separately says about 11,000 PRs shipped to production and reports about 0.5% PR reverts, without defining the revert cohort, denominator, or follow-up window.

Tan reports faster startup, typing, and opening or switching chats since launch for Grok Bot's **Electron** client. The talk gives no benchmark numbers, measurement tool, or controlled comparison.

## Limits

PR count depends on how work is split, and revert rate depends on how the team responds to failures. PR revert rate is not [DORA change fail rate](https://dora.dev/guides/dora-metrics), the share of deployments requiring immediate intervention, such as a rollback or hotfix. A before/after comparison can inform a decision; it can't isolate the effect of agents from other changes in the same period.

## Sources

Lauren Tan, *All Code Is Slop (Except Mine)*: [13:30 chart](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=810s) (merges), [13:39–13:44](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=819s) (reported shipping), [14:25–14:29](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=865s) (reported PR reverts), and [14:38–14:56](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=878s) (reported Electron client performance). The measurement checklist is this catalog's adaptation, not the team's reported methodology. See the [source review](../sources/lauren-tan-all-code-is-slop.md).
