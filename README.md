# pod-openclaw — RETIRED

This repo is retired: the OpenClaw family was consolidated into
[`opencharly/openclaw`](https://github.com/opencharly/openclaw)
(opencharly/opencharly#431), which now owns the gateway layer, the gateway image,
the disposable R10 bed and both skills.

What moved in this cutover, and where it lives now:

| was here | is now |
|---|---|
| the `openclaw:` candy (`charly.yml`) | `candy/openclaw/charly.yml` |
| the npm pin (`package.json`) | `candy/openclaw/package.json` |
| the systemd unit reference (`openclaw.service`) | `candy/openclaw/openclaw.service` |
| the owning skill (`openclaw-skill:`) | `box/openclaw/charly.yml`, projected as `/charly-openclaw:openclaw` |

The gateway image is `box/openclaw` there, its layer skill is
`/charly-openclaw:openclaw-layer`, and the disposable R10 witness is
`check-openclaw-pod`. This repo's released tags keep the old content exactly as it
was released; nothing should compose this repo. It is kept only until the operator
archives it.
