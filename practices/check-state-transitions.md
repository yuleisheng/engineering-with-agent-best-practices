# Check the riskiest state transitions

**Use when:** A flow behaves differently after retries, cancellation, or reordered events, and a happy-path test won't distinguish the right behavior from a plausible bug.

## Apply

1. Write the few relevant states and transitions in the user's terms. State an invariant: a property that must hold across the transitions. Ask the feature owner to resolve ambiguous outcomes before writing assertions.
2. Exercise the most revealing sequence against the running code. Example: cancel an export, then deliver its late completion; the export must stay canceled and no download should appear. Verify a normal completion still produces the download.
3. If there are too many sequences to choose by hand, generate sequences that check the invariant and retain any failing seed or minimized trace. Consider model checking for consequential event interleavings or a proof for a critical algorithm only when you can state what it must prove and how it corresponds to the implementation.

## Limits

A transition sketch helps people agree on intent; it isn't a proof. A proof or model-check result covers the property and assumptions encoded in the model, not necessarily the deployed behavior. Keep a real-system check; skip the heavier tool when a few deliberate tests cover the risk.

## Sources

Lauren Tan, *All Code Is Slop (Except Mine)*, [10:50–11:48](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=650s) (TLA+, Lean, and property-based UI testing) and [12:54–13:16](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=774s) (statecharts for reviewing intent alongside runtime evidence). See the [source review](../sources/lauren-tan-all-code-is-slop.md).
