# Verify real behavior with a small feature map

**Use when:** An agent can edit a feature but can't reliably find, run, or verify it. Also use this when CI is green but the tests would pass a wrong implementation.

## Apply

1. Pick one important flow. Record the user's name for the feature, how to reach it, the existing run or test command, and the expected result. Draw the expectation from the request, spec, or agreed behavior, not from the implementation being checked. For a port or rewrite, the existing implementation can be the expectation; list the differences you intend. For a service or CLI, use the entry point and expected response, exit code, or persisted state instead of a UI route.
2. Run the changed path with disposable local or test data through an interface real callers use. For a bug fix, show the same scenario fails before the fix and passes after it when feasible. For changed behavior, include a boundary or error case that would catch a plausible wrong implementation. If the risk is in a dependency, check it with the real dependency in a safe test setup; an always-successful stub can't tell you how it fails. If the flow only runs against shared staging or live services, [give agents a disposable environment](provide-a-disposable-test-environment.md) first.
3. If the acceptance check fails, triage the failure, fix the cause, and rerun the same check without weakening its expectation. Report any failure you cannot resolve.
4. Report the command or steps, assertion, and observed result. Include a screenshot for appearance or a clip for timing and interaction when either makes the result easier to review. Reuse the existing test runner or driver before building a custom one. When a run shows the feature map or test instructions are stale, update them in the same PR, and review a changed expectation like a changed assertion.

Example: if a CLI's spec allows up to 280 ASCII characters, verify that it accepts 280 and rejects 281 without saving. A test that only asserts the limit constant is 280 still passes if the CLI rejects exactly 280. Try that off-by-one change locally and check that the behavior test fails for the right reason.

Copy this feature map entry for each flow you need to make runnable:

```markdown
### <User-facing feature name>
- Reach it: <UI path, CLI invocation, or API entry point>
- Run it: <existing start/test command and fixture>
- Expect: <observable result, including one relevant boundary or failure>
- Record: <command/assertion result and, if helpful, screenshot or clip>
```

## Source examples

Grok Bot gives agents a reusable CLI to drive the app and access to debugging tools such as the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/). The slide lists [pstack's `/create-verification-skill`](https://github.com/cursor/plugins/blob/main/pstack/skills/create-verification-skill/SKILL.md) as a related tool for creating a repo-specific verification skill with a feature map, not as the source of Grok Bot's internal driver.

Pocock shows tests that assert a constant, inspect TypeScript source text instead of a rendered page, and stub the browser's **AudioContext** in `useAudioBoost`, missing real failure modes. He uses Ousterhout's **deep modules** to illustrate testing substantial behavior through a small interface. His [codebase-design](https://github.com/mattpocock/skills/blob/v1.3.0/skills/engineering/codebase-design/SKILL.md) skill supplies the vocabulary and [improve-codebase-architecture](https://github.com/mattpocock/skills/blob/v1.3.0/skills/engineering/improve-codebase-architecture/SKILL.md) proposes refactors in a before/after HTML report. Start at an existing entry point; only reshape one interface if reaching the behavior is repeatedly hard.

Yan describes a customer's bug video forwarded to **Devin** in Slack; Devin opens a PR and replies with a video of the fix. Devin also writes and updates its own **skills** for testing the codebase as the code changes. For an Android port, the team's iOS app is the reference, and Devin QA-tests against it “pixel for pixel.” That is [differential testing](https://www.cs.tufts.edu/comp/150FP/archive/bill-mckeeman/DifferentailTesting.pdf); McKeeman notes that two correct programs can still differ. Yan's goal of working without opening an IDE goes further than Tan's selective code reading. This page keeps code review for what the recording doesn't show.

## Limits

A screenshot demonstrates one state, not persistence, accessibility, or behavior across inputs. A green check covers only its assertions. Read the code when side effects, permissions, or failure paths are not evident in the artifacts. Keep the map small and link to existing tests.

Match verification to the risk and release stage. For a production-only risk, ask the service owner to use an existing canary with representative traffic and a meaningful signal, or to review an existing post-release signal. This isn't a reason to give the agent production access.

## Sources

Lauren Tan, *All Code Is Slop (Except Mine)*: [1:50 slide](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=110s) (issues feed back into writing), [4:49 slide](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=289s) (selective code reading), [9:40–10:49](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=580s) (runtime verification), and [10:36 slide](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=636s) (pstack reference). Cindy Sridharan's testing mix appears at [12:20](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=740s), with a clearer chart at [12:29](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=749s).

The triage, repair, and rerun steps are this catalog's prescription. The [ExecCritic preprint](https://arxiv.org/html/2609.09133v1) supports using independently grounded checks held fixed during repair in its benchmark; weak checks sometimes made results worse. See the [source review](../sources/lauren-tan-all-code-is-slop.md).

Matt Pocock, *Fixing the PR Bottleneck*: [5:55](https://www.youtube.com/watch?v=LlgiOCmFG_w&t=355s) (constant assertion), [6:34](https://www.youtube.com/watch?v=LlgiOCmFG_w&t=394s) (source-order test), [7:22–7:52](https://www.youtube.com/watch?v=LlgiOCmFG_w&t=442s) (AudioContext stub), [8:41–9:35](https://www.youtube.com/watch?v=LlgiOCmFG_w&t=521s) (deep interfaces for tests), and [9:52–10:52](https://www.youtube.com/watch?v=LlgiOCmFG_w&t=592s) (refactor report and design vocabulary). See the [Pocock source review](../sources/matt-pocock-fixing-the-pr-bottleneck.md). Google's [unit testing](https://abseil.io/resources/swe-book/html/ch12.html) and [test doubles](https://abseil.io/resources/swe-book/html/ch13.html) guidance corroborate testing through public interfaces and using real dependencies where practical; doubles are still useful for safe, deliberate failure injection.

Walden Yan, *40x More Merged PRs With 2.7x the Engineers*: [3:31–4:16](https://www.youtube.com/watch?v=U92J5mCDLMc&t=211s) (bug video in, fix video out), [4:16–4:28](https://www.youtube.com/watch?v=U92J5mCDLMc&t=256s) (working without an IDE), [5:46–6:10](https://www.youtube.com/watch?v=U92J5mCDLMc&t=346s) (agent-maintained testing skills), and [11:08–12:20](https://www.youtube.com/watch?v=U92J5mCDLMc&t=668s) and [12:45](https://www.youtube.com/watch?v=U92J5mCDLMc&t=765s) (Android parity against iOS). See the [Yan source review](../sources/walden-yan-40x-more-merged-prs.md).
