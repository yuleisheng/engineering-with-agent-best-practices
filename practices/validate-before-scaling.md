# Validate one workflow before scaling it

**Use when:** You want to increase agent concurrency or trigger agent work automatically, but the current draft-and-check loop still needs frequent intervention.

## Apply

1. Pick one low-risk task class, such as adding a regression test for an already-understood behavior. Have one agent draft a change and run the existing checks while a human still controls merge.
2. On representative tasks, check that the workflow produces reviewable evidence, and deliberately try a bad candidate to confirm the checks block it. Record failures and manual interventions. Fix recurring failures in the repo or test setup before adding concurrency.
3. If results justify a wider trial, start with draft-only automation in a bounded scope. Set a task and concurrency limit, name the person who can pause it, and agree what would stop the pilot (for example, repeated flaky checks or unexpected files being edited).

## Limits

Scaling a reliable draft workflow doesn't authorize unattended merges or production changes. Keep existing approvals, particularly for access controls, migrations, releases, and other high-impact changes. Faster drafting alone is not a win if it creates more review and rework than it saves.

## Sources

Lauren Tan, *All Code Is Slop (Except Mine)*, [15:11–15:57](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=911s) (strengthen the loop before scaling) and [16:21–16:52](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=981s) (the team's release automation). The draft-only pilot is this catalog's recommendation, not the team's workflow. See the [source review](../sources/lauren-tan-all-code-is-slop.md).
