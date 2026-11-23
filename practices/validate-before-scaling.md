# Validate one workflow before scaling it

**Use when:** You want to increase agent concurrency or trigger agent work automatically, but the current draft-and-check loop still needs frequent intervention.

## Apply

1. Pick one low-risk task class, such as adding a regression test for an already-understood behavior. Have one agent draft a change and run the existing checks while a human still controls merge.
2. On representative tasks, check that the workflow produces reviewable evidence. For a test-only draft, run the new test against deliberately broken behavior and known-good behavior: it should fail for the intended reason, then pass. Record failures and manual interventions. Fix recurring failures before adding concurrency.
3. Before enabling a wider draft-only pilot, protect the target branch with required human review and required checks from maintainer-controlled CI. Give the bot no bypass, no protected-branch push or merge rights even after approval and green CI, and no deploy rights. It must not be able to change the trusted checks or their results; run bot-authored code without privileged CI tokens or secrets.
4. Test those denials separately from the bad/good behavior test. In a disposable repo, have a maintainer use the actual pilot identity to confirm it cannot push or merge to the protected branch even with an approved, green PR, deploy to a test environment, change trusted checks or their results, or cause bot-authored code to run in privileged CI.
5. Before turning on the pilot, have a maintainer compare the real repo's effective branch rules, bypass list, bot access (including app and inherited permissions), and CI and deployment settings with the setup that passed. If they differ or you can't verify the boundary, leave automation off. Otherwise cap tasks and concurrency and name a human who will pause it after two flaky check runs or the first edit outside the agreed paths.

**On GitHub:** Have the bot open PRs from a fork with no upstream write access. Use `CODEOWNERS` on the base branch and require owner approval for `/.github/`, including `CODEOWNERS` itself, and any CI definitions or scripts stored elsewhere. This gates merge, not workflow execution.

Run untrusted code in isolated `pull_request` jobs with no secrets and at most a read-only `GITHUB_TOKEN`; for private forks, keep **Send write tokens** and **Send secrets** off. GitHub runs the workflow definition from the PR merge commit on `pull_request`. Same-repo PRs don't get the fork restrictions; configured secrets may be available, not automatically injected into every job. Don't run PR code in privileged jobs such as `pull_request_target`. A green job whose workflow the PR can change is not a trusted check: require a maintainer-controlled workflow or check provider whose definition and status the bot cannot change.

## Source example

Tan describes [Grok Bot connectors and routines](https://docs.x.ai/grok-bot/skills-routines-and-automations) starting **Cursor Cloud Agents** and **Cursor Projects**; a Project coordinates agents. She reports that cutting a Grok Bot release starts a Project that fuzz-tests the new build, opens fix PRs, and automerges them. The talk doesn't establish how the release trigger is wired or which merge gates apply.

Yan describes Cognition's default as “self-driving”: PRs go from bug report to merge without a human, with escalation when judgment is needed. On days with no human message, an agent keeping an Android app at parity with iOS merged “40, 50 PRs a day.” Like Tan, he doesn't say what gates a merge. His “give agents jobs, not tasks” goes further than Tan's advice to strengthen one loop first, but each of his examples depended on an environment, a metric, or a human approval built beforehand. **Devin** is Cognition's own product.

## Limits

Tan says agents can merge “most” of their own PRs; that isn't a measured merge rate or evidence that human review is dispensable. Yan's claim that most teams are “limited by the amount of ambition they have” isn't evidence either; he calls the infrastructure “the hard part.” The catalog pilot keeps merge and deployment with humans. Faster drafting alone is not a win if it creates more review and rework than it saves.

## Sources

Lauren Tan, *All Code Is Slop (Except Mine)*: [15:11–15:57](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=911s) (strengthen the loop before scaling), [1:17:31 in the livestream](https://www.youtube.com/watch?v=ZZ19zhYnDKY&t=4651s) (Grok Bot routines and Cursor tooling), [16:21–16:55](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=981s) (reported release example), and [9:31–9:39](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=571s) (broader merge claim). See the [source review](../sources/lauren-tan-all-code-is-slop.md).

Walden Yan, *40x More Merged PRs With 2.7x the Engineers*: [1:51–2:12](https://www.youtube.com/watch?v=U92J5mCDLMc&t=111s) (autonomous by default), [4:28–4:38](https://www.youtube.com/watch?v=U92J5mCDLMc&t=268s) (infrastructure as the hard part), [8:03–9:15](https://www.youtube.com/watch?v=U92J5mCDLMc&t=483s) (jobs, not tasks), [12:20–12:45](https://www.youtube.com/watch?v=U92J5mCDLMc&t=740s) (Android merges on days without a human message), and [13:34–13:45](https://www.youtube.com/watch?v=U92J5mCDLMc&t=814s) (ambition). See the [Yan source review](../sources/walden-yan-40x-more-merged-prs.md).

The pilot safeguards are catalog recommendations, not reported Grok Bot settings. See GitHub's [protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches), [code owners](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners), [workflow execution contexts](https://docs.github.com/en/actions/reference/security/securely-using-pull_request_target), [fork workflow settings](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository#enabling-workflows-for-forks-of-private-repositories), and [CI security](https://docs.github.com/en/actions/reference/security/secure-use) guidance.
