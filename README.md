# rdl-node-ambiguous

Test fixture for the `remove_duplicate_lockfiles` maintenance task.

## Scenario

Node conflict at repo root with **no** detection signals — `package.json` has no `packageManager` field and no manager-specific config file (`.yarnrc.yml`, `pnpm-workspace.yaml`, `bunfig.toml`, etc.) is present.

Lockfiles:
- `package-lock.json` (npm)
- `yarn.lock` (yarn)

## Expected MT behaviour

- **Cannot auto-detect**; issues a `select` question asking which of `npm` / `yarn` to keep.
- Since `pnpm` is the ecosystem default but is not present here, the default falls back to the first option (`npm`).
- Also issues the `update_gitignore` boolean question (default `true`).
- Plan and apply phases delete the non-chosen lockfile and append it to `.gitignore`.
