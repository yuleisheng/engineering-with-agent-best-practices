# Enforce repeatable rules in code or CI

**Use when:** Reviewers keep catching the same mistake and can describe a case that should pass and a case that should fail.

## Apply

1. Pick one correction that came up on more than one PR. Look at the diffs and review comments; consult a relevant, redacted session excerpt if the cause is unclear. Check whether the rule has no check, the check isn't run, or it tests the wrong thing. Write down the rule and the supported alternative before choosing a tool.
2. If a small API change can remove the invalid choice, do that. Otherwise use the cheapest check that catches it: a type or lint rule for code structure, a regression test for behavior. Reuse the repo's existing tooling and run the check in CI.
3. Confirm the known bad case fails for the intended reason and the supported case passes. See if the next comparable PRs still need the correction. For a real backlog, block new violations first if the tooling supports a baseline; migrate the old examples in small batches.

Example diagnostic: `ui/ cannot import worker/internal/. Use the public client in worker/client/; move runtime-neutral types to contracts/.` Use the actual paths and check the import forms the repo uses before enabling the rule. For example, ESLint's [`no-restricted-imports`](https://eslint.org/docs/latest/rules/no-restricted-imports) covers static imports, not dynamic ones.

## Source examples

Tan's **Poteto pyramid** puts API design and automated checks ahead of prose for repeatable rules. **Dune**, Grok Bot's internal **Electron** client framework, restricts React's `useEffect` in product code and allows it in selected framework APIs. Separately, Tan describes lint diagnostics in Grok Bot and Dune that point agents toward the underlying fix. The Effect restriction is a local design choice, not a recommendation to ban `useEffect` in every React project.

Pocock describes [**Retro**](https://github.com/mattpocock/skills/blob/v1.3.0/skills/engineering/retro/SKILL.md), a skill that uses agent sessions or PR reviews to suggest checks, standards, or navigation improvements for future work. It suggests changes; it doesn't establish that they prevent recurrence. Two diffs and their review comments are enough to start without the skill.

## Limits

A check enforces only what it measures. Don't encode a one-off preference as a lint rule or ban an API before there is a supported way to do the legitimate work. If the agent couldn't find the right pattern, [fix the pointer](make-good-patterns-discoverable.md). If the correction requires judgment, [give the reviewer the standard](review-against-repo-standards.md) instead of inventing a lint rule.

## Sources

Lauren Tan, *All Code Is Slop (Except Mine)*: [2:03–2:59](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=123s) (Poteto pyramid), [5:00](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=300s) (introducing Dune), [5:34–6:09](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=334s) (React Effect restriction), and [8:56–9:21](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=536s) (actionable lint diagnostics). See the [Tan source review](../sources/lauren-tan-all-code-is-slop.md).

Matt Pocock, *Fixing the PR Bottleneck*: [20:14–21:44](https://www.youtube.com/watch?v=LlgiOCmFG_w&t=1214s) (turn repeat findings into checks, standards, or better pointers). See the [Pocock source review](../sources/matt-pocock-fixing-the-pr-bottleneck.md). [Google's SRE Workbook](https://sre.google/workbook/postmortem-culture/) makes a similar case for concrete, verifiable prevention work after incidents; it doesn't evaluate Retro.
