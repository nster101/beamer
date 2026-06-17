---
description: Scaffold the current repository to use the multi-agent crew
---
Set up the **current** repository to run the multi-agent crew (Einstein,
Archimedes, Euclid, Gauss, Newton). First inspect what already exists and show me a
short plan; **merge, never clobber** existing files. Then:

1. **Coordination channel.** Create `coordination/README.md` (the protocol — copy it
   from this plugin) and the per-agent logs
   `coordination/log/{einstein,archimedes,euclid,gauss,newton}.md` (each starting
   with a `# <Name> — coordination log` header), seeding `einstein.md` and
   `archimedes.md` with a short kickoff entry.
2. **Permissions + hook.** Create or merge `.claude/settings.json` with the crew
   permissions allowlist and the `SessionStart` command hook wiring — a plugin
   **cannot** ship a permissions allowlist, so it must live in the repo. Copy
   `session-start.sh` into `.claude/hooks/` and `chmod +x` it, **or**, if you keep
   this plugin installed, rely on the plugin's own bundled hook and skip the copy.
3. **Personas (optional).** If you want plain `/einstein`-style commands instead of
   the namespaced plugin commands (`/agent-crew:einstein`), copy the five persona
   files into `.claude/commands/`.
4. **Project context.** Append a "Multi-agent collaboration" section to `CLAUDE.md`
   (create `CLAUDE.md` if missing) and add a short **PROJECT CONTEXT** block stating
   this project's direction/mandate, the files that are off-limits to edit, and the
   build/test/CI commands. These are the **only** project-specific facts the crew
   needs — every persona is otherwise project-agnostic.

Finally, print the spin-up instructions: open a session and run `/einstein`,
`/archimedes`, `/euclid`, `/gauss`, or `/newton` (namespaced as `/agent-crew:<name>`
when using the installed plugin). Do not commit or push without my approval.
