# Validate one workflow before scaling it

**Use when:** You want to increase agent concurrency or trigger agent work automatically, but the current draft-and-check loop still needs frequent intervention.

## Apply

1. Pick one low-risk task class, such as adding a regression test for an already-understood behavior. Have one agent draft a change and run the existing checks while a human still controls merge.
2. On representative tasks, check that the workflow produces reviewable evidence. For a test-only draft, run the new test against deliberately broken behavior and known-good behavior: it should fail for the intended reason, then pass. Record failures and manual interventions. Fix recurring failures before adding concurrency.
3. Before enabling a wider draft-only pilot, protect the target branch with required human review and required checks from maintainer-controlled CI. Give the bot no bypass, no protected-branch push or merge rights even after approval and green CI, and no deploy rights. It must not be able to change the trusted checks or their results; run bot-authored code without privileged CI tokens or secrets.
4. Test those denials separately from the bad/good behavior test. In a disposable repo with equivalent controls, have a maintainer use the actual pilot identity to confirm it cannot push or merge to the protected branch even with an approved, green PR, deploy to a test environment, change trusted checks, or cause bot-authored code to run in privileged CI. If the platform cannot enforce the boundary, leave automation off. If it can, cap tasks and concurrency and name who can pause the pilot and when.

## Source example

Tan describes [Grok Bot connectors and routines](https://docs.x.ai/grok-bot/skills-routines-and-automations) starting **Cursor Cloud Agents** and **Cursor Projects**; a Project coordinates agents. She reports that cutting a Grok Bot release starts a Project that fuzz-tests the new build, opens fix PRs, and automerges them. The talk doesn't establish how the release trigger is wired or which merge gates apply.

## Limits

Tan says agents can merge “most” of their own PRs; that isn't a measured merge rate or evidence that human review is dispensable. The catalog pilot keeps merge and deployment with humans. Faster drafting alone is not a win if it creates more review and rework than it saves.

## Sources

Lauren Tan, *All Code Is Slop (Except Mine)*: [15:11–15:57](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=911s) (strengthen the loop before scaling), [1:17:31 in the livestream](https://www.youtube.com/watch?v=ZZ19zhYnDKY&t=4651s) (Grok Bot routines and Cursor tooling), [16:21–16:55](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=981s) (reported release example), and [9:31–9:39](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=571s) (broader merge claim). See the [source review](../sources/lauren-tan-all-code-is-slop.md).

The pilot safeguards are catalog recommendations, not reported Grok Bot settings. See GitHub's [protected branch](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches) and [CI security](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions) guidance for implementation details.
