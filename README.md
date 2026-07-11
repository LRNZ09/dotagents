# dotagents

My portable [`@sentry/dotagents`](https://dotagents.sentry.dev) config — the one
`agents.toml` behind my `~/.agents`. Skills are **fetched, not vendored here**:
my own live in [LRNZ09/skills](https://github.com/LRNZ09/skills), and I also pull
Sentry's official skills from [getsentry/skills](https://github.com/getsentry/skills)
and Anthropic's from [anthropics/skills](https://github.com/anthropics/skills).

## Getting started

Bootstrap the whole setup on a new machine — this repo is cloned at `~/.agents`
itself, so `agents.toml` is the live user-scope config:

```sh
git clone https://github.com/LRNZ09/dotagents.git ~/.agents
npx @sentry/dotagents --user install
```

`install` fetches every declared skill into `~/.agents/skills/`, writes the
declared MCP servers (Claude reads them from `~/.claude.json`), and wires the
symlinks — Claude discovers skills via `~/.claude/skills`; opencode reads
`~/.agents/skills/` natively.

### Adding a skill source

Sources are restricted by `[trust]`, so trust the org first, then add the repo
as a wildcard:

```sh
npx @sentry/dotagents --user trust add anthropics
npx @sentry/dotagents --user add anthropics/skills --all
```

If a skill name exists in two wildcard sources, install stops with a conflict —
add the name to one source's `exclude` list in `agents.toml` by hand, then
re-run `npx @sentry/dotagents --user install`.

Commit `agents.toml` **and** `agents.lock`: the lock is deliberately tracked
(against the dotagents default) so clones record the exact resolved commit of
every skill. `skills/` stays gitignored — managed state, refetched on install.

### Guard rails

- `[trust]` — only sources from allowed GitHub orgs can install.
- `minimum_release_age` — third-party commits must age 7 days before they can
  install; my own repo is exempt.
- Pinned `ref`s (e.g. `getsentry/dotagents` at `1.17.0`) where reproducibility
  beats freshness.

## Conventions

`~/.agents` is managed by `@sentry/dotagents` — prefer CLI edits
(`npx @sentry/dotagents --user add` / `trust add` / `remove`) over hand edits;
the exceptions are wildcard `exclude` lists and comments, which the CLI can't
express. Reconcile an existing setup with `npx @sentry/dotagents --user install`.

## What's where

- **This repo** — `agents.toml`: which sources to fetch, trust, and target;
  `agents.lock`: the exact resolved commit of every installed skill.
- **[LRNZ09/skills](https://github.com/LRNZ09/skills)** — my authored + adapted
  skills and NOTICE attribution.
