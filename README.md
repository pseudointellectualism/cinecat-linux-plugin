# Cinecat sources plugin

Compiled source-resolver plugin for the Cinecat Linux app.
No source code lives here — each release carries the built plugin, its
signature and a manifest the app reads to find it.

The app keeps the providers out of its own download so a broken source can be
fixed by publishing a plugin instead of asking everyone to install the app
again.

## What a release contains

| File | What it is |
| --- | --- |
| `cinecat-providers-<version>-x86_64.so` | the plugin |
| `cinecat-providers-<version>-x86_64.so.sig` | Ed25519 signature of that file, base64 |
| `manifest.json` | version, ABI, size, SHA-256, signature and download URL |

## Verification

Every plugin is signed with the project's Ed25519 key. The app checks the
signature against the public key built into it before installing a plugin and
again each time it loads one, and refuses anything that does not match — there
is no override in a release build. It also refuses to move to an older plugin
than the one already installed.

Public key (raw, base64):

```
5vObvMp8VpZmPHsXymatth1d500356Uotw9/MwjyP5o=
```

Check a download yourself:

```bash
sha256sum -c <<< "$(jq -r '.sha256 + "  " + .file' manifest.json)"
```

## Installing

Nothing to do by hand. The app fetches the newest plugin on first run and
checks for updates at each start; an update applies the next time it starts.
