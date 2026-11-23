# Enforce repeatable rules in code or CI

**Use when:** Reviewers keep catching the same mistake and can describe a case that should pass and a case that should fail.

## Apply

1. Pick one correction that came up on more than one PR. Look at the diffs and review comments; consult a relevant, redacted session excerpt if the cause is unclear. Check whether the rule has no check, the check isn't run, or it tests the wrong thing. Write down the rule and the supported alternative before choosing a tool.
2. If a small API change can remove the invalid choice, do that. Otherwise use the cheapest check that catches it: a type or lint rule for code structure, a regression test for behavior. Reuse the repo's existing tooling and run the check in CI.
3. Confirm the known bad case fails for the intended reason and the supported case passes. See if the next comparable PRs still need the correction. For a real backlog, block new violations first if the tooling supports a baseline; migrate the old examples in small batches.

If you already use ESLint, add this object to your flat config array. Adapt the UI file glob and `@app/` import paths; use the repo's existing TypeScript parser for `.ts` and `.tsx` files.

```js
{
  files: ["src/ui/**/*.{js,jsx,ts,tsx}"],
  rules: {
    "no-restricted-imports": ["error", {
      patterns: [{
        regex: "^@app/worker/internal(?:/|$)",
        message: "Use @app/worker/client; move runtime-neutral types to @app/contracts.",
      }],
    }],
  },
}
```

ESLint's [`no-restricted-imports`](https://eslint.org/docs/latest/rules/no-restricted-imports) catches matching static imports, including `import type`, and re-exports. It misses dynamic `import()` and CommonJS `require()`. Test the forms your repo uses, including relative paths or other aliases: the rule matches the written specifier, not the resolved file.

## Source examples

Tan's **Poteto pyramid** puts API design and automated checks ahead of prose for repeatable rules. **Dune**, Grok Bot's internal **Electron** client framework, restricts React's `useEffect` in product code and allows it in selected framework APIs. Separately, Tan describes lint diagnostics in Grok Bot and Dune that point agents toward the underlying fix. The Effect restriction is a local design choice, not a recommendation to ban `useEffect` in every React project.

Pocock puts deterministic checks before automated and human review, echoing Tan's preference for enforcing repeatable rules in code. He describes agent sessions or PR reviews as inputs to **Retro**. The [v1.3.0 skill](https://github.com/mattpocock/skills/blob/v1.3.0/skills/engineering/retro/SKILL.md) reads session material and suggests deterministic checks for mechanical violations and `CODING_STANDARDS.md` for judgment calls. Two diffs and their review comments are enough to start without the skill.

## Limits

A check enforces only what it measures. Don't encode a one-off preference as a lint rule or ban an API before there is a supported way to do the legitimate work. If the agent couldn't find the right pattern, [fix the pointer](make-good-patterns-discoverable.md). If the correction requires judgment, [give the reviewer the standard](review-against-repo-standards.md) instead of inventing a lint rule.

## Sources

Lauren Tan, *All Code Is Slop (Except Mine)*: [2:03–2:59](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=123s) (Poteto pyramid), [5:00](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=300s) (introducing Dune), [5:34–6:09](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=334s) (React Effect restriction), and [8:56–9:21](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=536s) (actionable lint diagnostics). See the [Tan source review](../sources/lauren-tan-all-code-is-slop.md).

Matt Pocock, *Fixing the PR Bottleneck*: [2:50–3:36](https://www.youtube.com/watch?v=LlgiOCmFG_w&t=170s) (checks before automated and human review) and [20:14–21:44](https://www.youtube.com/watch?v=LlgiOCmFG_w&t=1214s) (turn repeat findings into checks, standards, or better pointers). See the [Pocock source review](../sources/matt-pocock-fixing-the-pr-bottleneck.md). [Google's SRE Workbook](https://sre.google/workbook/postmortem-culture/) makes a similar case for concrete, verifiable prevention work after incidents; it doesn't evaluate Retro.
