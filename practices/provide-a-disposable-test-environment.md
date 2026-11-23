# Give agents a disposable environment built from main

**Use when:** An agent can only test a change against shared staging or live third-party services, or starting the app on a fresh machine needs someone's credentials or help.

## Apply

1. On a fresh machine or container, with no personal or production credentials, clone the default branch and run the documented start command. Write down every manual step, secret, and shared service it needs. That list is the work.
2. Run the services you own, such as the database, cache, and queue, locally at the production type and version. Replace a third-party service only where an automated test can't or shouldn't call it: use the provider's test sandbox or an owned fake. Give each fake a contract test that runs the same assertions against the fake and, where permitted, the sandbox. Make the fake fail loudly on calls it doesn't support.
3. Build the environment from the default branch on each merge or on a schedule. Treat a failed build like a failing main: fix the cause, and don't skip a setup step or check to get it green. Point the agent's guidance to the same start command people use.
4. Check it: from a fresh clone with no production or third-party credentials in the environment, the start command brings the stack up and one [feature-map flow](verify-real-behavior.md) passes. Change one field in a fake's response and confirm its contract test fails.

Adapt this `AGENTS.md` fragment. Replace the command, the faked services, and the paths:

```markdown
## Run and test locally
- Start: `docker compose up --wait` from a clean checkout of `main`. No personal credentials needed.
- Faked locally: `payments` and `email`. Contract tests: `test/contract/`.
- Not covered locally: <provider behavior the fakes don't model>. Check it in <sandbox, canary, or post-release signal>.
```

With healthchecks defined, [`docker compose up --wait`](https://docs.docker.com/reference/cli/docker/compose/up/) returns once services are running or healthy, so an agent knows when it can start testing.

## Source example

Yan says Cognition stopped testing against staging, live servers, and manual click-throughs. It moved every service to **Docker Compose** and mocked every third-party service, so the whole app runs on one VM without live services. The slide shows Slack, Teams, Jira, and AWS services as dashed local stand-ins. The VMs are rebuilt from the latest code every hour, **Devin** is the first responder when a rebuild fails, and each session starts on a fresh machine. Devin's [environment docs](https://docs.devin.ai/onboard-devin/environment) describe sessions booting from a snapshot built from configuration. The same pattern works with Compose alone, a dev container, or [Codespaces prebuilds](https://docs.github.com/en/codespaces/prebuilding-your-codespaces/about-github-codespaces-prebuilds), which update on each push by default. Hourly is Cognition's cadence, not a requirement.

## Limits

A fake covers only the behavior it models. [Pocock's AudioContext stub](../sources/matt-pocock-fixing-the-pr-bottleneck.md) couldn't reproduce real failures, and Google's guidance notes that [hermeticity and fidelity often conflict](https://abseil.io/resources/swe-book/html/ch14.html). One machine won't show distributed timing, load, or a provider's real failure modes; keep a sandbox, canary, or post-release check for those. Start with the services the first flow needs, not a full local copy of a large system. Skip this if a fresh clone already starts cleanly.

Yan also claims cloud VMs are more secure than laptops. The talk gives no evidence, and a snapshot or session can carry secrets too. Keep production credentials out of the image, snapshot, and session; use test-only sandbox keys.

## Sources

Walden Yan, *40x More Merged PRs With 2.7x the Engineers*: [4:28–5:17](https://www.youtube.com/watch?v=U92J5mCDLMc&t=268s) (from staging to Docker Compose and mocks on one VM), [5:03 slide](https://www.youtube.com/watch?v=U92J5mCDLMc&t=303s) (agent-testable codebase), [5:17–5:46](https://www.youtube.com/watch?v=U92J5mCDLMc&t=317s) (hourly rebuilds), [6:10–6:37](https://www.youtube.com/watch?v=U92J5mCDLMc&t=370s) (security claim), and [6:40–7:05](https://www.youtube.com/watch?v=U92J5mCDLMc&t=400s) (multi-account Zoom test). See the [source review](../sources/walden-yan-40x-more-merged-prs.md).

Google's [larger-testing chapter](https://abseil.io/resources/swe-book/html/ch14.html) prefers hermetic, single-machine systems under test to shared staging and advises against automated tests that call real third-party APIs. Its [test-doubles chapter](https://abseil.io/resources/swe-book/html/ch13.html) says fakes need their own contract tests against the real implementation. [Twelve-Factor](https://12factor.net/dev-prod-parity) argues for the same type and version of each backing service in every environment. [OWASP](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/) advises limiting agent permissions to the minimum necessary. The contract tests and credential-free check are catalog recommendations, not Cognition's reported setup.
