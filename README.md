# Engineering with coding agents

A catalog of practices you can try in one repository without adopting a new agent framework. Each entry explains when to use it, how to check it, where it breaks down, and which source informed it.

## Start with one change

- **New repo:** Give an agent [one runnable verification path](practices/verify-real-behavior.md) and [one good example to copy](practices/make-good-patterns-discoverable.md). Put the real commands and example path in the repo's existing agent guidance.
- **Existing repo:** Pick a mistake reviewers keep correcting. [Make that mistake fail a check](practices/enforce-repeatable-rules.md), then [remove a misleading example](practices/make-good-patterns-discoverable.md) that keeps getting copied. Don't start with a repo-wide cleanup.

## Practices

| If you see this | Try this |
| --- | --- |
| The same review correction keeps coming back | [Enforce repeatable rules in code or CI](practices/enforce-repeatable-rules.md) |
| Agents copy outdated patterns or cross module boundaries | [Make the intended pattern easy to find and copy](practices/make-good-patterns-discoverable.md) |
| The diff looks fine, but nobody ran the affected flow | [Verify real behavior with a small feature map](practices/verify-real-behavior.md) |
| Happy-path tests miss retries, races, or invalid transitions | [Check the riskiest state transitions](practices/check-state-transitions.md) |
| PR volume is up and you can't tell if users are better off | [Track outcomes alongside throughput](practices/measure-outcomes.md) |
| You want more agents before one agent can reliably verify its work | [Validate one workflow before scaling it](practices/validate-before-scaling.md) |

## Sources and contributions

- [Lauren Tan, *All Code Is Slop (Except Mine)*](sources/lauren-tan-all-code-is-slop.md): initial review, including claims kept, narrowed, rejected, and deferred.
- [Contribution requirements](REQUIREMENTS.md): how to evaluate sources and keep practices quick to apply.
