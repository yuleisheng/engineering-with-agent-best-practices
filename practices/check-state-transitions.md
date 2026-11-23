# Check the riskiest state transitions

**Use when:** A flow behaves differently after retries, cancellation, or reordered events, and a happy-path test won't distinguish the right behavior from a plausible bug.

## Apply

1. Write the few relevant states and transitions in the user's terms. State an invariant: a property that must hold across the transitions. Ask the feature owner to resolve ambiguous outcomes before writing assertions.
2. Exercise the most revealing sequence against the running code. Example: cancel an export, deliver its late completion, and wait until the handler has processed the event. Then assert that the export is still canceled and no download appeared. Verify a normal completion still produces the download.
3. If there are too many sequences to choose by hand, generate sequences that check the invariant and retain any failing seed or minimized trace. Consider model checking for consequential event interleavings or a proof for a critical algorithm only when you can state what it must prove and how it corresponds to the implementation.

## Source examples

- **TLA+** for modeling and checking cross-device notification flows; **Lean** for proving stated properties of the team's UI virtualization algorithm, not every runtime flicker path.
- **[Bombadil](https://github.com/antithesishq/bombadil)** for checking supplied properties as it explores the running UI, not for exhaustive proof.
- **pstack**: [`/statechart`](https://github.com/cursor/plugins/blob/main/pstack/skills/statechart/SKILL.md) checks a configured TLA+ model with TLC and shows modeled states, transitions, and counterexamples in user terms. It does not read the running app or establish which checker Grok Bot used for notifications. The formal-verification slide lists [`/create-formal-verification-skill`](https://github.com/cursor/plugins/blob/main/pstack/skills/create-formal-verification-skill/SKILL.md) as a related tool, not as the source of the team's models.

## Limits

A transition sketch helps people agree on intent; it isn't a proof. A proof or model-check result covers the property and assumptions encoded in the model, not necessarily the deployed behavior. Keep a real-system check; skip the heavier tool when a few deliberate tests cover the risk.

## Sources

Lauren Tan, *All Code Is Slop (Except Mine)*: [10:50–11:50](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=650s) (TLA+, Lean, and Bombadil), [10:56 slide](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=656s) (pstack formal-verification reference), and [12:54–13:19](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=774s) (pstack statecharts). See the [source review](../sources/lauren-tan-all-code-is-slop.md).
