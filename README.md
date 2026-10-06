# 📦 ArchipelaWoW Repack

The server the [ArchipelaWoW Launcher](https://github.com/r-o-b-o-t-o/archipelawow-launcher) installs:
[AzerothCore](https://www.azerothcore.org/) built with the
[ArchipelaWoW](https://github.com/r-o-b-o-t-o/archipelawow) modules, and MySQL, ready to run on Windows.
Install it from the launcher's setup, which also updates it.

> [!WARNING]
> Repacks like this one aren't supported by AzerothCore, so don't ask the AzerothCore community for
> help with it. Join the [Unofficial Archipelago Discord server](https://discord.gg/Nu4X9gmGDR) instead,
> and head to the
> [ArchipelaWoW thread](https://discord.com/channels/1345801058609270794/1436315171969568859) in the
> `future-game-design` forum.

## 📁 The archive

Each release has an archive per build, `ArchipelaWoW-Repack-<build>-<version>.zip`, which the launcher
extracts into its folder:

| Path | Contents |
| --- | --- |
| `server\bin` | authserver, worldserver, dbimport, the client data extractors, their DLLs, and `configs\` with the `.conf.dist` files |
| `server\source` | The SQL files of the core and its modules, read by the database updater |
| `server\licenses` | The licenses of the bundled software, besides MySQL's, which are in `mysql` |
| `server\release.json` | The version and the build, and what they were built from: versions and commits |
| `mysql` | MySQL Community Server, trimmed down to what running it takes |

Every DLL the binaries need is next to them, the Visual C++ runtime included, so nothing has to be
installed.

The launcher relies on this layout, on the archive names, and on the `version` and `build` of
`release.json`: change them along with it.

## ⚙️ Releases

[`repack.yml`](.github/workflows/repack.yml) builds a release every Saturday at 06:45 (Paris time).
It builds the latest AzerothCore with the modules listed at the top of the workflow, bundles MySQL,
publishes the archive, and deletes all but the latest three releases it made.

To add a module, add its repository to `MODULES`, optionally followed by a branch or tag.

`BUILD` names the archive's build, `standard` for now. The launcher offers each archive of the latest
release as a build, and updates the installed server to the same build.

To be told on Discord when a release fails, add a `DISCORD_WEBHOOK_URL` repository secret holding the
URL of a channel's webhook.

## 🛠️ Packaging locally

[`scripts/package.ps1`](scripts/package.ps1) assembles `server\` and `mysql\` from a built core, as the
workflow does. It runs on Windows PowerShell 5.1 as well as PowerShell 7, and takes Visual Studio with
the C++ tools, for the Visual C++ runtime and the check that no DLL is missing:

```powershell
./scripts/package.ps1 -CoreInstallDir <cmake install prefix> -CoreSourceDir <azerothcore-wotlk checkout> -MySqlDir <extracted mysql-8.4.x-winx64> -OpenSslDir <OpenSSL> -OutputDir package
```

To try the result, copy `server` and `mysql` into a launcher's folder.
