# nuvio (mirror)

Auto-synced mirror of [`NuvioPlugin/All-in-One-Nuvio`](https://github.com/NuvioPlugin/All-in-One-Nuvio)
`providers/`.

## Why
The Tv24h apps (Android TV / Fire TV + Flutter phone) **hot-load** stream providers
from this repo at runtime (via `raw.githubusercontent.com`), the same way they load
the `hula` providers. So when upstream updates a provider, the apps pick it up within
their cache window — **no app rebuild or redistribution needed**.

This mirror sits between upstream and the apps so we control the source:
- survives upstream repo deletion / renames,
- lets us pin / roll back if upstream breaks a contract,
- adds a validation buffer.

## Sync
`.github/workflows/sync.yml` pulls `providers/` + `manifest.json` from upstream every
6 hours (and on manual `workflow_dispatch`), committing only when something changed.
`sync-info.json` records the last sync time.

## Provider contract
Each provider exposes `getStreams(tmdbId, type, season, episode)` → array of
`{ url, quality, headers | behaviorHints.proxyHeaders.request, ... }`. The apps wrap
this into their native provider format via a small adapter.

Upstream is GPLv3 / "educational purposes only" — see upstream for license terms.
