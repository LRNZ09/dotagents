# dotagents

My portable [`@sentry/dotagents`](https://dotagents.sentry.dev) config — the one
`agents.toml` behind my `~/.agents`. Skills are **fetched, not vendored here**:
my own live in [LRNZ09/skills](https://github.com/LRNZ09/skills), and I also pull
Sentry's official skills from [getsentry/skills](https://github.com/getsentry/skills).

## Use it

`~/.agents` is managed entirely by `@sentry/dotagents` — never edit it by hand.
The sources, trust, and target agents recorded in `agents.toml` were all set with
`npx @sentry/dotagents --user add` / `trust add` / `remove`; reconcile an existing
setup with `npx @sentry/dotagents --user install`.

Fetched skills land in `~/.agents/skills/` as managed state. Claude reads them via
a `~/.claude/skills` symlink; opencode reads `~/.agents/skills/` natively.

## What's where

- **This repo** — `agents.toml`: which sources to fetch, trust, and target.
- **[LRNZ09/skills](https://github.com/LRNZ09/skills)** — my authored + adapted
  skills and NOTICE attribution.
