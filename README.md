# YACRE

Yet Another Combat Rotation Engine — a combat-rotation and automation engine for World of Warcraft
retail, running inside the [NilName](https://docs.nilname.com) unlocker.

This repository is the **distribution channel**: released builds and the manifest clients poll. The
engine sources live elsewhere.

## Install

1. Download **`_yacre.lua`** from the [latest release](../../releases/latest).
2. Put it in `{NilNameDir}\scripts`.
3. Launch WoW. When the chat says the engine has been downloaded, `/reload` once.

Do not rename the file: NilName runs a script only if its name starts with `_` and ends with
`.lua`. Renamed, it sits there doing nothing, without an error.

That is the whole install, and the last time you download anything by hand.

## Updates

On every launch that file starts the engine build it already has on disk, then checks
[`manifest.json`](manifest.json) in the background. A newer build is downloaded, verified against
its SHA-256 and applied on your next `/reload` — you are told in chat when one is waiting. It
updates *itself* the same way.

Two builds are kept side by side under `{NilNameDir}\scripts\yacre\slot_a` and `slot_b`. A download
always fills the slot that is **not** running, so an interrupted download cannot damage the build
you are using, and if a new build fails to start, the next launch falls back to its predecessor on
its own.

No GitHub account and no token are needed: this repository is public.

## Settings

Optional. `{NilNameDir}\scripts\yacre\config.json` is not created for you and is not required:

```json
{
  "channel": "stable",
  "autoUpdate": true
}
```

| Key | Default | Meaning |
|---|---|---|
| `channel` | `"stable"` | `"stable"`, `"beta"`, or `"local"` to run a build of your own and never touch the network |
| `autoUpdate` | `true` | `false` keeps the installed build and stops checking |
| `localEntrypoint` | `"/scripts/_yacre_dev_entrypoint.lua"` | what `"local"` runs |
| `manifestUrl` | the channel's URL | override, for testing an unpublished manifest |

## Troubleshooting

- **Nothing happens on login** — confirm the file is at `{NilNameDir}\scripts\_entrypoint.lua` and
  that NilName itself is loading. Errors are printed in chat, prefixed `YACRE:`.
- **Stuck on an old build** — check `autoUpdate` is not `false` and that `channel` is not `"local"`.
- **Anything unexplained** — delete `{NilNameDir}\scripts\yacre` and relaunch. Everything in it is
  re-downloaded.

The build you are on is shown in the main window's title bar. Quote it in bug reports.
