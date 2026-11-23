# Make the intended pattern easy to find and copy

**Use when:** A newcomer would find more examples of a deprecated approach than of its replacement, or can’t tell which side of a module boundary they’re editing.

## Apply

1. Search for a representative task, such as adding a settings page. Pick the existing implementation that new work should follow. If there isn't one yet, use the next real change to make one.
2. Link to that example from the nearest agent guidance. Replace the stale example people are most likely to copy, or mark it as legacy and point to the replacement if migration must wait.
3. Use names that expose real ownership or runtime boundaries. If crossing one causes repeated failures, [enforce the import rule](enforce-repeatable-rules.md); a folder name isn't a check.

Adapt this `AGENTS.md` fragment to the repo's real paths and replace the example with a maintained one:

```markdown
- Model new settings screens on `src/settings/notifications/`. It shows how to load, save, and report a failed save.
- Route background calls through `src/worker/client/`. Keep modules that run only in the worker under `src/worker/internal/`.
```

Repeat the original search. The recommended example should be findable and runnable, and any remaining legacy example should point users away from copying it.

## Source example

**Dune**, Grok Bot's client framework for **Electron**, separates code into `electron-main/` and `electron-renderer/`. A dependency graph check and lint rules block imports between them; both use `shared/` for common types. The directory names tell agents what runs where, and the checks enforce that boundary.

## Limits

Don't rename directories or rewrite working code just to make it prettier for an agent. Start where examples have actually caused rework; preserve legitimate alternate patterns and label when to use them.

## Sources

Lauren Tan, *All Code Is Slop (Except Mine)*, [4:24–4:55](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=264s) (workarounds become examples), [6:10–6:55](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=370s) (directory structure as context), and [15:17–15:29](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=917s) (remove old patterns after preventing new ones). See the [source review](../sources/lauren-tan-all-code-is-slop.md).
