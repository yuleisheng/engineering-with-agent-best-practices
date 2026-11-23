# Enforce repeatable rules in code or CI

**Use when:** Reviewers keep catching the same mistake and can describe a case that should pass and a case that should fail.

## Apply

1. Pick one recent violation. Write down the rule and the supported alternative before choosing a tool.
2. If a small API change can remove the invalid choice, do that. Otherwise use the cheapest check that catches it: a type or lint rule for code structure, a regression test for behavior. Reuse the repo's existing tooling and run the check in CI.
3. Confirm the known bad case fails for the intended reason and the supported case passes. For a real backlog, block new violations first if the tooling supports a baseline; migrate the old examples in small batches.

Example diagnostic: `ui/ cannot import worker/internal/. Use the public client in worker/client/; move runtime-neutral types to contracts/.` Use the actual paths and check the import forms the repo uses before enabling the rule. For example, ESLint's [`no-restricted-imports`](https://eslint.org/docs/latest/rules/no-restricted-imports) covers static imports, not dynamic ones.

## Source example

Tan's **Poteto pyramid** puts API design and automated checks ahead of prose for repeatable rules. **Dune**, Grok Bot's internal **Electron** client framework, restricts React's `useEffect` in product code and allows it in selected framework APIs. Separately, Tan describes lint diagnostics in Grok Bot and Dune that point agents toward the underlying fix. The Effect restriction is a local design choice, not a recommendation to ban `useEffect` in every React project.

## Limits

A check enforces only what it measures. Don't encode a one-off preference as a lint rule or ban an API before there is a supported way to do the legitimate work. Keep prose for rationale and judgment, and human review for tradeoffs the check can't decide.

## Sources

Lauren Tan, *All Code Is Slop (Except Mine)*: [2:03–2:59](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=123s) (Poteto pyramid), [5:00](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=300s) (introducing Dune), [5:34–6:09](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=334s) (React Effect restriction), and [8:56–9:21](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=536s) (actionable lint diagnostics). See the [source review](../sources/lauren-tan-all-code-is-slop.md).
