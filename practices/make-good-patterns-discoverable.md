# Make the intended pattern easy to find and copy

**Use when:** An agent finds a deprecated example before the maintained one, or can't tell which side of a module boundary it's editing.

## Apply

1. Search for a representative task, such as adding a settings page. Pick the existing implementation that new work should follow. If there isn't one yet, use the next real change to make one.
2. Link to one maintained example from the guidance nearest the code being changed. Replace the stale example people are most likely to copy, or mark it as legacy and state which pattern takes precedence if migration must wait.
3. Use names that expose real ownership or runtime boundaries. If crossing one causes repeated failures, [enforce the import rule](enforce-repeatable-rules.md); a folder name isn't a check.

Adapt this `AGENTS.md` fragment to the repo's real paths and replace the example with a maintained one:

```markdown
- Model new settings screens on `src/settings/notifications/`. It shows how to load, save, and report a failed save.
- Route background calls through `src/worker/client/`. Keep modules that run only in the worker under `src/worker/internal/`.
```

Try one representative agent task. Check that the agent finds the runnable example and follows the intended pattern even when a stale example remains.

## Source example

**Dune**, Grok Bot's client framework for **Electron**, separates code into `electron-main/` and `electron-renderer/`. A dependency graph check and lint rules block cross-imports, even type imports; both directories use `shared/` for common types. The names tell agents what runs where; the checks enforce the boundary.

## Limits

Don't rename directories or rewrite working code just to make it prettier for an agent. Start where examples have actually caused rework; preserve legitimate alternate patterns and label when to use them.

## Sources

Lauren Tan, *All Code Is Slop (Except Mine)*: [4:24–4:55](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=264s) (workarounds become examples), [6:28 slide](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=388s) (Dune directories and import rules), and [15:17–15:32](https://www.youtube.com/watch?v=O_zcc6Xl12g&t=917s) (remove old patterns after preventing new ones). The [*Precedent or Intent?* preprint](https://arxiv.org/html/2610.38244v1) finds that nearby examples and explicit priority rules can influence implementation choices in the API tasks studied; it does not show a general quality gain. See the [source review](../sources/lauren-tan-all-code-is-slop.md).
