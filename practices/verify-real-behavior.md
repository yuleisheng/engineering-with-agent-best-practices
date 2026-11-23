# Verify real behavior with a small feature map

**Use when:** An agent can edit a feature but can't reliably find it, run it, or show that the user-visible behavior matches the request.

## Apply

1. Pick one important flow. Record the user's name for the feature, how to reach it, the existing run or test command, and the expected result. Draw the expectation from the request, spec, or agreed behavior, not from the implementation being checked. For a service or CLI, use the entry point and expected response, exit code, or persisted state instead of a UI route.
2. Run the changed path with disposable local or test data. For a bug fix, show the same scenario fails before the fix and passes after it when feasible. For changed behavior, include a boundary or error case that would catch a plausible wrong implementation.
3. If the acceptance check fails, triage the failure, fix the cause, and rerun the same check without weakening its expectation. Report any failure you cannot resolve.
4. Report the command or steps, assertion, and observed result. Include a screenshot for appearance or a clip for timing and interaction when either makes the result easier to review. Reuse the existing test runner or driver before building a custom one.

Copy this feature map entry for each flow you need to make runnable:

```markdown
### <User-facing feature name>
- Reach it: <UI path, CLI invocation, or API entry point>
- Run it: <existing start/test command and fixture>
- Expect: <observable result, including one relevant boundary or failure>
- Record: <command/assertion result and, if helpful, screenshot or clip>
```

## Source example

Grok Bot gives agents a reusable CLI to drive the app and access to debugging tools such as the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/). The slide lists [pstack's `/create-verification-skill`](https://github.com/cursor/plugins/blob/main/pstack/skills/create-verification-skill/SKILL.md) as a related tool for creating a repo-specific verification skill with a feature map, not as the source of Grok Bot's internal driver.

## Limits

A screenshot demonstrates one state, not persistence, accessibility, or behavior across inputs. A green check covers only its assertions. Read the code when side effects, permissions, or failure paths are not evident in the artifacts. Keep the map small and link to existing tests.

Match verification to the risk and release stage. For a production-only risk, ask the service owner to use an existing canary with representative traffic and a meaningful signal, or to review an existing post-release signal. This isn't a reason to give the agent production access.

## Sources

Lauren Tan, *All Code Is Slop (Except Mine)*: [1:50 slide](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=110s) (issues feed back into writing), [4:49 slide](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=289s) (selective code reading), [9:40–10:49](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=580s) (runtime verification), and [10:36 slide](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=636s) (pstack reference). Cindy Sridharan's testing mix appears at [12:20](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=740s), with a clearer chart at [12:29](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=749s).

The triage, repair, and rerun steps are this catalog's prescription. The [ExecCritic preprint](https://arxiv.org/html/2609.09133v1) supports using independently grounded checks held fixed during repair in its benchmark; weak checks sometimes made results worse. See the [source review](../sources/lauren-tan-all-code-is-slop.md).
