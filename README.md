# pod-openclaw

The `openclaw` candy of the OpenCharly candy library, as a standalone repo
(kind-prefixed naming). It ships a headless OpenClaw AI gateway service on port
`18789` — no desktop, no browser, just the gateway.

## What it provides

Installs the `openclaw` npm package globally (`package.json` → `npm install -g`)
so the gateway binary lands at `~/.npm-global/bin/openclaw`, then runs it as the
supervised `openclaw` service (`openclaw gateway --port 18789
--allow-unconfigured --bind loopback`). The loopback bind needs no auth and
matches the socat relay; `--allow-unconfigured` lets the headless gateway start
before any operator setup. socat relays the loopback-bound gateway onto the
container interface, and a `data` volume persists `~/.openclaw`.

| Property | Value |
|---|---|
| Service | `openclaw` (`%(ENV_HOME)s/.npm-global/bin/openclaw gateway --port 18789 --allow-unconfigured --bind loopback`, `restart: always`) |
| Port | `18789` (`port_relay: 18789` via socat) |
| Requires | `layer-nodejs`, `layer-socat`, `layer-supervisord` |
| Volume | `data` at `~/.openclaw` |
| Env | `NODE_ENV=production` |
| Alias | `openclaw` |
| Package | `openclaw@2026.9.8` |

## How to use it

```bash
charly box build openclaw
charly config openclaw
charly start openclaw
# gateway at http://localhost:18789
```

The candy's own `check:` steps assert the gateway binary in the npm global bin,
the exact pinned package version (`2026.9.8`), the in-image Node satisfying
openclaw's engine range (`>=24.16.0 <25 || >=26.1.0` — node 22 and 25.x are
excluded since 2026.9.3), the running gateway's `/healthz` liveness endpoint
(`200`), the supervised `openclaw` service, and the reachable published port.

## Layout

- `charly.yml` — the `openclaw:` candy entity (description, `require`, `env`,
  `port`, `port_relay`, `volume`, `alias`, `service`, `plan`) plus its `skill:`
  entity.
- `package.json` — the npm dependency pin (`openclaw@2026.9.8`).
- `openclaw.service` — a systemd unit reference for the gateway.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `CHANGELOG/` — per-CalVer history.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-openclaw:openclaw` — the headless gateway box, its
  ports, and verification.
- `/charly-openclaw:openclaw-full` — the maximal variant (gateway + browser +
  all tools).
- `/charly-automation:openclaw-deploy` — the gateway configuration, model auth,
  and browser/channel setup.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
