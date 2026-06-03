> For AI agents (Claude Code, Copilot, Cursor): read [AGENTS.md](https://github.com/Karanjot786/TermUI/blob/main/AGENTS.md) and [packages/data/AGENTS.md](https://github.com/Karanjot786/TermUI/blob/main/packages/data/AGENTS.md) before writing code. This issue is a complete spec. Follow it exactly.

## Objective

Add a `useFileWatch` hook to `@termuijs/data` that watch a file or directory path for changes and emit an event on each change.

## Package scope

`packages/data`. All work is confined here. Import shared types from `@termuijs/core`. Follow the data AGENTS.md rules: no Bun-only types, use `node:` prefixed built-ins, cleanup in useEffect.

## Files to create or modify

```
CREATE: packages/data/src/hooks/useFileWatch.ts
CREATE: packages/data/src/hooks/useFileWatch.test.ts
MODIFY: packages/data/src/index.ts   (add the useFileWatch export)
```

## API contract

```typescript
// See the implementation notes below for the exact signature.
// Pattern: follow packages/data/src/hooks.ts (useFetch, useWebSocket).
```

## Acceptance criteria

- [ ] hook returns a typed result object with `data`, `error`, and `loading` fields
- [ ] starts fetching/polling/watching on mount
- [ ] cleans up (interval/watcher/subscription) on unmount
- [ ] re-triggers when any dependency changes
- [ ] does not use Bun-only types (`Timer`, `import from 'bun'`). Use `ReturnType<typeof setTimeout>` for timer handles
- [ ] `bun vitest run packages/data` passes
- [ ] `bun run typecheck` passes

## Test expectations

```
File: packages/data/src/hooks/useFileWatch.test.ts
Run:  bun vitest run packages/data
```

Use `vi.spyOn` or `vi.mock` for system-level calls. Never mutate shared state directly.

Cases:
- returns initial loading state
- returns data after async resolution
- cleanup runs on unmount
- error state is set when the operation fails

## Reference pattern to follow

Follow `packages/data/src/hooks.ts` (`useFetch` and `useWebSocket`) for the hook structure. Follow `packages/data/src/useFetch.test.ts` for the test layout.

## Not included (out of scope)

- No UI rendering — this hook is data-only.
- No caching (separate issue).
- No JSX intrinsic registration.
- No docs-site changes.

## Do not touch

- Any package other than `packages/data`
- `packages/core` (import shared types, do not modify them)
- `bun.lock` (no dependency is needed)
- `.github/`

## Agent prompt (copy this into your AI agent)

```
Read AGENTS.md at the repo root and packages/data/AGENTS.md.
Read the reference file packages/data/src/hooks.ts for useFetch and useWebSocket — copy their structure.
Task: implement useFileWatch exactly to the API contract in this issue.
Create packages/data/src/hooks/useFileWatch.ts and packages/data/src/hooks/useFileWatch.test.ts. Export useFileWatch from packages/data/src/index.ts.
Plan first. List the files you will change and confirm they match the issue. Touch nothing outside packages/data. Do not edit bun.lock. Do not add dependencies.
Do not use Bun-only types. Use node: prefix for built-ins. Use ReturnType<typeof setTimeout> for timer handles. Cleanup in useEffect return.
Tests use vi.spyOn or vi.mock for system calls. Never mutate shared state.
When done run: bun vitest run packages/data && bun run typecheck. Both must pass.
```

## Before you open the PR

- [ ] I run `bun run build && bun vitest run && bun run typecheck` and all pass
- [ ] My change is confined to `packages/data`
- [ ] `bun.lock` has no unrelated changes
- [ ] I starred the repo

## GSSoC 2026

This is a intermediate-level issue. Comment "I would like to work on this" to get assigned. You have 7 days to open a PR after assignment.

Read [CONTRIBUTING.md](./CONTRIBUTING.md) before starting.
