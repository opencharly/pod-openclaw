# AGENTS.md — pod-openclaw

**This repo is retired.** It owns no entities any more: its `openclaw:` candy, its
npm pin, its systemd unit reference and its owning skill moved to
`opencharly/openclaw` when the OpenClaw family was consolidated
(opencharly/opencharly#431). That repo owns the gateway layer, the gateway image
(`box/openclaw`), the disposable R10 bed (`check-openclaw-pod`) and the skills.

Do not add entities here, and do not compose this repo — it is kept only until the
operator archives it. A change that belongs to the OpenClaw family belongs in
`opencharly/openclaw` instead.

There is no build, validate or test surface left in this repo beyond the org-wide
`charly/pr-validator` check that every repo carries. The authoritative rulebook is
the umbrella `AGENTS.md` in `opencharly/opencharly`.
