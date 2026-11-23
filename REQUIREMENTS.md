# Contribution requirements

This repo collects practices over time. The goal is better engineering with coding agents, not a transcript archive or a list of tools to install.

## Evaluate each source

1. Read or watch the underlying source, not just its title or summary. Record the author, title, date if known, and the original URL. Say what you reviewed and what you couldn't verify.
2. Separate **what the source says**, **what the evidence supports**, and **what this catalog recommends**. Treat a demo, anecdote, self-reported metric, or vendor claim as such. Look for a counterexample and name the assumption that would make adoption a bad idea.
3. Save a short review under `sources/`. For material claims, record a verdict (keep, keep with limits, reject, or defer), the reason, and a link to any resulting practice. Record rejected or deferred claims there, not in the catalog as recommendations. For video claims, link to the relevant timestamp in the exact recording reviewed.

## Write practices people can apply quickly

Keep one page per distinct practice under `practices/`. Revise an existing page when a new source corroborates, limits, or contradicts it; don't publish duplicate advice. Link new entries and reviews from the README.

- **Use when:** Name a symptom or a decision that tells the reader whether to apply it.
- **Apply:** Give the smallest useful first change and a check that could fail if it was done wrong. Reuse a repo's existing tools first. Include a copyable checklist, config, or `AGENTS.md` fragment when it saves time; mark what needs adapting.
- **Limits:** Say when to skip it, where it costs more than it saves, and what it doesn't prove. Distinguish a local rollout suggestion from a source claim.
- **Sources:** Cite every practice directly, even if it has a separate source review. Cite the author and title, with a deep link to the relevant timestamp (and a readable time or range) for video. Link to the analysis that explains the verdict.

Before submitting, check that the first step works for the repo type you name, the expected result is observable, and every link and video timestamp leads to the relevant material.
