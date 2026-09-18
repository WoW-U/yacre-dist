# YACRE

Yet Another Combat Rotation Engine — a combat-rotation and automation engine for World of Warcraft
retail, running inside the [NilName](https://docs.nilname.com) unlocker.

This repository is the **distribution channel**: released builds and the manifest clients poll. The
engine sources live elsewhere.

## Install

1. Download **`_yacre.nilname.lua`** from the [latest release](../../releases/latest).
2. Put it in `{NilNameDir}\scripts` — the folder holding your NilName install's `scripts` directory.
3. Log in.

The engine downloads itself in the background and starts on its own, in that same session. No
`/reload`, no unpacking, nothing else to fetch.

**Do not rename the file.** NilName runs a script only when its name starts with `_` and ends with
`.lua`. Renamed, it does nothing and reports nothing.

The other asset on the release, `yacre-engine-<version>.nilname.lua`, is what the updater fetches
for you — you never download it yourself.

## Updates

Every launch, the installed build starts first and a check against [`manifest.json`](manifest.json)
runs in the background. A newer build is downloaded, verified against its SHA-256, and comes up at
your next login — `/reload` if you want it sooner. The updater replaces itself the same way. You are
told in chat whenever something arrived.

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

- **Nothing happens at all** — check the file really is at `{NilNameDir}\scripts\_yacre.nilname.lua`,
  spelled exactly like that, and that NilName itself is loading. Messages are prefixed `YACRE:`.
- **Stuck on an old build** — check `autoUpdate` is not `false` and `channel` is not `"local"`.
- **Anything unexplained** — delete `{NilNameDir}\scripts\yacre` and relaunch. Everything in it is
  downloaded again.

The build you are on is shown in the main window's title bar. Quote it in bug reports.
