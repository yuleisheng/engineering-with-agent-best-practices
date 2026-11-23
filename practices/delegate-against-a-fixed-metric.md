# Delegate optimization only against a fixed evaluator

**Use when:** You want an agent to keep improving a number, such as latency, throughput, or test time, over many attempts instead of making one described change.

## Apply

1. Write the job before delegating it. A script must be able to compute the score without human judgment; if it can't, the job isn't ready.

   ```markdown
   - Metric and direction: <for example, p99 latency of /search on the replay; lower is better>
   - Workload: <fixed benchmark or replay, seed, hardware>
   - Baseline: <value, number of runs, run-to-run spread>
   - Must still pass: <correctness checks; outputs compared with the reference>
   - Must not get worse: <memory, error rate, cost>
   - Budget and stop rule: <compute or time; stop after N runs without a gain beyond noise>
   - Evaluator location: <path or job the agent can't change>
   ```

2. Keep the benchmark, workload, and correctness checks where the agent can't edit them, such as a directory that needs code-owner review or a separate CI job, and run them from that trusted copy. The agent proposes changes on a branch. Keep a candidate only if it passes the correctness checks and beats the baseline by more than the run-to-run spread.
3. Before the first run, try to fool the evaluator. Submit a candidate that is fast because it is wrong, for example one that skips work or returns a cached result, and confirm the correctness gate rejects it. Run the unchanged code a few times to measure the noise.
4. A human reviews and merges each kept change as a normal PR, then reruns the benchmark on the merged result. Report the trend against the baseline. Stop or narrow the job if gains come from changing what gets measured. Run it first as a [bounded pilot](validate-before-scaling.md).

## Source examples

Yan says Cognition gave an agent a permanent set of **NVIDIA Vera Rubin** GPUs and a throughput target. Over about a week it found “eight or nine” changes and sent the best strategies to a human engineer for final approval. The slide reports 2.8x output tokens per minute per GPU. The talk doesn't say how correctness was checked or how many runs back the figure. Yan presents his P99 latency “job” as what development “should look like,” not as a reported result.

Karpathy's [autoresearch](https://github.com/karpathy/autoresearch/blob/228791fb499afffb54b46200aca536f79142f117/program.md) makes the evaluation harness read-only, fixes a five-minute training budget, and keeps or discards each experiment based on one metric. Google DeepMind's [AlphaEvolve](https://deepmind.google/blog/alphaevolve-a-gemini-powered-coding-agent-for-designing-advanced-algorithms/) pairs model-proposed programs with automated evaluators that verify and score them. Neither is required; a benchmark script, a protected directory, and a PR are enough to start.

## Limits

A fixed evaluator measures only its workload. A benchmark gain may not carry to production traffic, so recheck kept changes on representative load. Sakana's AI CUDA Engineer [found a memory exploit](https://x.com/SakanaAILabs/status/1892992938013270019) in its evaluation code that, in some cases, let it skip the correctness check. Re-evaluated on Sakana's hardened robust-kbench, its reported mean speedup fell from 3.13x to 1.49x ([update, in Japanese](https://sakana.ai/ai-cuda-engineer-update/)).

Skip this for goals like onboarding retention, which change slowly, depend on many factors, and need an experiment design rather than an agent loop. Skip it when one profiling pass and a fix would do. Don't give the job production write access to add telemetry or alerts; those changes go through review. Dedicated hardware and repeated runs cost money; set the budget before you start.

## Sources

Walden Yan, *40x More Merged PRs With 2.7x the Engineers*: [8:03–9:15](https://www.youtube.com/watch?v=U92J5mCDLMc&t=483s) (jobs, not tasks), [8:16 slide](https://www.youtube.com/watch?v=U92J5mCDLMc&t=496s), [9:15–9:49](https://www.youtube.com/watch?v=U92J5mCDLMc&t=555s) (measure it and optimize the number), [9:49–11:08](https://www.youtube.com/watch?v=U92J5mCDLMc&t=589s) (GPU throughput job with human approval), and [9:53 slide](https://www.youtube.com/watch?v=U92J5mCDLMc&t=593s) (2.8x TPM). See the [source review](../sources/walden-yan-40x-more-merged-prs.md).

Google Cloud's [AlphaEvolve guide](https://docs.cloud.google.com/gemini/enterprise/docs/alphaevolve/developer-guide/quantify-optimization-target) requires a deterministic score computed without human judgment and warns that a raw LLM rubric is “quick to reward-hack.” For the web examples Yan gives, click latency and layout shift already have standard measures: [INP](https://web.dev/articles/inp) and [CLS](https://web.dev/articles/cls). The fool-the-evaluator check is a catalog recommendation, not Cognition's reported process.
