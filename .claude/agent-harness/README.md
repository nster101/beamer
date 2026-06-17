# agent-crew — a reusable 5-agent Claude Code harness

A portable crew of five Claude Code agents that collaborate on a repository through
git, with a skeptical review gate before anything merges:

- **Einstein** — conductor: scopes future work, delegates & sequences, keeps
  everyone in check (process + scope). Writes no code; never merges.
- **Archimedes / Euclid / Gauss** — peer builders: plan → approval → issue → branch
  → draft PR.
- **Newton** — independent skeptic reviewer/verifier: re-verifies every PR himself
  and is the merge gate — the human merges only after Newton comments **"approved."**

Each agent is a slash command, so you (re)load a persona in any fresh window by
typing its command — the identity lives in the repo, not in chat memory.

## What's in this plugin

```
.claude-plugin/plugin.json        # plugin manifest (name: agent-crew)
.claude-plugin/marketplace.json   # single-plugin marketplace catalog (name: nster-crew)
commands/                         # /einstein /archimedes /euclid /gauss /newton + /agents-init
hooks/hooks.json                  # wires the non-blocking SessionStart reminder
hooks/session-start.sh            # the reminder script (reminds; performs no writes)
```

## Install it in another project

This directory is both a plugin and a single-plugin marketplace, so add it from a
GitHub repo, a git URL, or a local path:

```
/plugin marketplace add <owner/repo | git-url | ./path-to-this-dir>
/plugin install agent-crew@nster-crew
```

Installed commands are namespaced (e.g. `/agent-crew:einstein`); the SessionStart
hook ships with the plugin and applies once enabled (run `/reload-plugins` if it
isn't picked up immediately).

## Scaffold the new repository

A plugin **cannot** ship a permissions allowlist or per-repo state (the
`coordination/` logs), so run the bundled scaffolder once per repo:

```
/agent-crew:agents-init
```

It creates `coordination/` (README + per-agent logs), writes a
`.claude/settings.json` permissions allowlist + the hook wiring, optionally copies
plain `/einstein`-style commands into `.claude/commands/`, and appends a
"Multi-agent collaboration" section plus a short **PROJECT CONTEXT** block to
`CLAUDE.md`.

## Customising per project

The personas are deliberately **project-agnostic**: they defer every project
specific (direction, off-limits files, build/test commands, conventions) to the
target repo's `CLAUDE.md`. To adapt the crew to a new project, edit that repo's
`CLAUDE.md` — not the personas.

## How coordination works

- Agents have **no shared memory**; they coordinate only through git.
- Each agent appends only to its own `coordination/log/<agent>.md` (one writer per
  file → no merge conflicts). Notes are posted as **small log-only PRs** the human
  merges; feature PRs never touch `coordination/`.
- Only the **human** merges, and only after Newton's "approved" review (coordination
  / doc-only PRs are exempt).

## Notes / limitations

- Plugin `settings.json` currently supports only a narrow set of keys, so the
  permissions allowlist is intentionally written into each repo by `/agents-init`
  rather than shipped here.
- To share the crew across a team, copy this directory out to its own git repo and
  add that repo as a marketplace.
