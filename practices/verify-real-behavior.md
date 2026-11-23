# Verify real behavior with a small feature map

**Use when:** An agent can edit a feature but can't reliably find it, run it, or show that the user-visible behavior matches the request.

## Apply

1. Pick one important flow. Record the user's name for the feature, how to reach it, the existing run or test command, and the expected result. For a service or CLI, use the entry point and expected response, exit code, or persisted state instead of a UI route.
2. Run the changed path with disposable local or test data. For a bug fix, show the same scenario fails before the fix and passes after it when feasible. For changed behavior, include a boundary or error case that would catch a plausible wrong implementation.
3. Report the command or steps, assertion, and observed result. Include a screenshot for appearance or a clip for timing and interaction when either makes the result easier to review. Reuse the existing test runner or driver before building a custom one.

Copy this feature map entry for each flow you need to make runnable:

```markdown
### <User-facing feature name>
- Reach it: <UI path, CLI invocation, or API entry point>
- Run it: <existing start/test command and fixture>
- Expect: <observable result, including one relevant boundary or failure>
- Record: <command/assertion result and, if helpful, screenshot or clip>
```

## Source example

Grok Bot gives agents a reusable CLI to drive the app and access to debugging tools such as the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/). Tan points to [pstack's `/create-verification-skill`](https://github.com/cursor/plugins/blob/main/pstack/skills/create-verification-skill/SKILL.md) for creating a repo-specific verification skill with a feature map. Neither is required if your existing runner can exercise the flow and report the result.

## Limits

A screenshot demonstrates one state, not persistence, accessibility, or behavior across inputs. A green check covers only its assertions. Keep the map small and link to existing tests so it doesn't become a second test suite to maintain. Don't give the agent production credentials just to make a flow runnable.

## Sources

Lauren Tan, *All Code Is Slop (Except Mine)*, [9:40–10:46](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=580s) (runtime artifacts, feature maps, and reusable drivers) and the [10:36 slide](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=636s) (pstack). See the [source review](../sources/lauren-tan-all-code-is-slop.md).
