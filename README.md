# Engineering with coding agents

A catalog of practices you can try in one repository without adopting a new agent framework. Each entry explains when to use it, how to check it, where it breaks down, and which source informed it.

## Start with one change

- **New repo:** Give an agent [one runnable verification path](practices/verify-real-behavior.md) and [one good example to copy](practices/make-good-patterns-discoverable.md). Put the real commands and example path in the repo's existing agent guidance.
- **Existing repo:** Pick a mistake reviewers keep correcting. If you can define pass and fail, [make it fail a check](practices/enforce-repeatable-rules.md). If the agent keeps copying the wrong example, [point it to the right one](practices/make-good-patterns-discoverable.md). Don't start with a repo-wide cleanup.

## Practices

| If you see this | Try this |
| --- | --- |
| The same review correction keeps coming back | [Enforce repeatable rules in code or CI](practices/enforce-repeatable-rules.md) |
| Agents copy outdated patterns or cross module boundaries | [Make the intended pattern easy to find and copy](practices/make-good-patterns-discoverable.md) |
| The diff looks fine, but nobody ran the affected flow | [Verify real behavior with a small feature map](practices/verify-real-behavior.md) |
| Happy-path tests miss retries, races, or invalid transitions | [Check the riskiest state transitions](practices/check-state-transitions.md) |
| Green PRs still miss standards that require judgment | [Review the diff against repo standards in a fresh context](practices/review-against-repo-standards.md) |
| A small diff might have large or irreversible effects | [Show rollback and blast radius in the PR](practices/show-merge-risk.md) |
| PR volume is up and you can't tell if users are better off | [Track outcomes alongside throughput](practices/measure-outcomes.md) |
| You want more agents before one agent can reliably verify its work | [Validate one workflow before scaling it](practices/validate-before-scaling.md) |

## Sources and contributions

- [Lauren Tan, *All Code Is Slop (Except Mine)*](sources/lauren-tan-all-code-is-slop.md): initial review, including claims kept, narrowed, rejected, and deferred.
- [Matt Pocock, *Fixing the PR Bottleneck*](sources/matt-pocock-fixing-the-pr-bottleneck.md): test quality, separate standards review, merge risk, and lessons from recurring findings.
- [Contribution requirements](REQUIREMENTS.md): how to evaluate sources and keep practices quick to apply.
