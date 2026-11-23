# Review the diff against repo standards in a fresh context

**Use when:** PRs pass CI but reviewers keep finding mistakes that require repo-specific judgment, such as duplicating a domain policy at a new call site.

## Apply

1. Pick one existing standard and a recent PR where a human caught a violation. For a new repo, write one rule with a concrete example when the first such issue comes up. Keep task requirements, safety constraints, and required checks visible to the implementer; give the reviewer the detailed rubric.
2. After the implementation checks pass, give a fresh agent session with read-only repo access the exact base and head revisions, the requirement, and the maintainer-approved standards and examples. Have it read the surrounding code and report only material findings, each with the rule, changed line, likely consequence, and smallest fix. Distinguish a rule violation from an uncertain design suggestion.
3. Try the same brief on both the pre-correction and corrected diffs, in separate sessions without the old review comments. Check that it flags the known violation in the first and doesn't repeat it in the second.
4. For a clear finding, have the reviewer propose a ready-to-apply patch, such as a suggestion block. If the tool drafts commits, confine them to a separate branch or fork where it cannot update the PR branch or merge. The author confirms and applies the fix, then reruns affected checks. Keep normal human approval on the final diff.

```text
Read-only standards pass for <base>...<head>.
Read <requirement> and <repo standard or example>.
For each material finding, cite the rule and changed line, explain the
consequence, and offer the smallest fix. Mark uncertainty. Do not edit,
approve, or merge. If nothing is supported, say so.
```

## Source example

Pocock describes a **code-review** skill that gives a subagent the diff and a standards file. He argues that giving the implementer the standards makes it perform worse, but offers no measured comparison. He prefers the reviewer to commit fixes and comment when unsure. The [published v1.3.0 skill](https://github.com/mattpocock/skills/blob/v1.3.0/skills/engineering/code-review/SKILL.md) looks for documented standards such as `CODING_STANDARDS.md` or `CONTRIBUTING.md` and reports separate standards and spec findings; it does not instruct the reviewer to commit. Pocock mentions attendees using Cursor Bugbot and CodeRabbit, not as his own review setup. [Bugbot's opt-in Autofix](https://cursor.com/docs/bugbot#autofix) can push fixes to a new branch or the PR branch. Use a separate branch only if permissions prevent the tool from touching the PR branch or merging.

## Limits

Fresh context doesn't make the reviewer right or its misses independent of the implementer's. A pass on standards isn't a pass on the spec or a substitute for human review. [Automate mechanical rules](enforce-repeatable-rules.md) instead. Skip the extra pass for trivial diffs or when checking and dismissing findings takes more time than the verified fixes save.

## Sources

Matt Pocock, *Fixing the PR Bottleneck*: [11:30–12:21](https://www.youtube.com/watch?v=LlgiOCmFG_w&t=690s) (claimed implementation penalty), [12:21–13:21](https://www.youtube.com/watch?v=LlgiOCmFG_w&t=741s) (diff, standards, and separate context), [13:51–14:22](https://www.youtube.com/watch?v=LlgiOCmFG_w&t=831s) (where to put standards), [14:22–15:21](https://www.youtube.com/watch?v=LlgiOCmFG_w&t=862s) (third-party tools), and [15:22–16:12](https://www.youtube.com/watch?v=LlgiOCmFG_w&t=922s) (preference for commits). See the [source review](../sources/matt-pocock-fixing-the-pr-bottleneck.md). [Google's review guide](https://google.github.io/eng-practices/review/reviewer/looking-for.html) backs reviewing design, tests, and surrounding code against explicit standards; it doesn't test agent context splitting.
