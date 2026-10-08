# AGENTS.md — pod-openclaw

Standalone candy repo for the `openclaw` candy — a headless OpenClaw AI gateway
service on port `18789`. The candy lives in `charly.yml` at the repo root plus its
npm pin and a systemd unit reference.

Canonical files:

- `charly.yml` — the `openclaw:` candy entity (description, `require`, `env`,
  `port`, `port_relay`, `volume`, `alias`, `service`, `plan`) and its `skill:`
  entity.
- `package.json` — the npm dependency pin (`openclaw@2026.9.8`).
- `openclaw.service` — a systemd unit reference for the gateway.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `CHANGELOG/` — per-CalVer history.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-openclaw:openclaw` — the owning skill: the headless gateway box, its
  port/relay architecture, and verification. Load before editing, building,
  deploying, or troubleshooting this candy.
- `/charly-automation:openclaw-deploy` — the gateway configuration, model auth,
  and browser/channel setup.
- `/charly-openclaw:openclaw-full` — the maximal variant for comparison.
- `/charly-pod:pod` — the `kind: pod` / deploy schema reference (this candy is
  composed into a box; tree-position nesting, ports, port relays).
- `/charly-check:check` — the check/R10 framework: the `check:` step verbs and
  `charly check run <bed>`.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs, service declarations).
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The live R10 witness is a composing box's `check` bed; the candy's own `check:`
  steps assert the gateway binary in the npm global bin, the exact pinned package
  version (`2026.9.8`), the in-image Node satisfying openclaw's engine range
  (`>=24.16.0 <25 || >=26.1.0` — node 22 and 25.x are excluded since 2026.9.3),
  the running gateway's `/healthz` `200`, the supervised `openclaw` service, and
  the reachable published port.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate. Its only workflow file is
  `.github/workflows/tag-on-merge.yml`.

## Modify this repo

- Edit the `openclaw:` candy entity in `charly.yml`; the `skill:` entity in the
  same file is the owning skill's source — a candy change and its skill change
  land together.
- The npm version pin lives in `package.json` and the plan's version check; keep
  the two in step.
- The gateway binds loopback and socat relays it (`port_relay: 18789`); the
  `port:` field, the `port_relay:` field, and the service exec must stay in step.
- The `data` volume at `~/.openclaw` is the persistent store; keep the service
  exec and the volume path in step.
- The `skill:` entity is the source for `/charly-openclaw:openclaw`; never edit
  the generated `SKILL.md` — regenerate it.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
