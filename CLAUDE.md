# CLAUDE.md

ArchipelaWoW Repack: `.github/workflows/repack.yml` builds AzerothCore with the ArchipelaWoW modules
and bundles it with MySQL into the server archive the ArchipelaWoW Launcher installs; `README.md`
covers the archive and the releases.

## Related repositories

- `archipelawow-launcher` — installs the archives of the latest release, relying on their names
  (`ArchipelaWoW-Repack-<build>-<version>.zip`), their layout (`server/`, `mysql/`, see its `AppPaths`)
  and `server/release.json`; change them together. It writes the server configuration
  (`config-defaults.json`).
- `mod-i-found-your-sword` — the AzerothCore module. It finds its SQL by its directory name, so the
  workflow clones modules into `modules/<repository name>`; renaming either needs a matching change on
  the other side.

## Code

- Write straightforward code. Skip minor edge cases; point out notable ones and let the user decide
  whether they are worth handling.
- Comment only when the code isn't obvious or there is an implication a future maintainer could
  easily miss. Keep comments brief and don't restate the code.
- No machine-specific paths or credentials in committed files.

## The server

- The configs go in `server/configs`: on Windows the core reads `configs/` from its working directory,
  not next to the executables, and the launcher runs the servers from `server`.
- The database updater opens SQL files as `server\source\...` and isn't long-path aware: past
  259 characters they fail to open. Keep SQL file and module names short.
- Ship `data/sql/archive`: the base schemas list its updates as applied, and missing files are reported
  on every start.

## Verifying changes

- `scripts/package.ps1` has to stay runnable under Windows PowerShell 5.1 for local runs; against a
  Debug core build, its dependency check fails on the debug C runtime, as expected.
- The workflow only runs on GitHub: at least parse its `run:` blocks with PowerShell.

## Commits

- Conventional Commits with concise messages; one logical change per commit.
