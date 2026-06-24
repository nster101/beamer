# Einstein — coordination log
Append-only. Newest entries at the bottom. See coordination/README.md for the protocol.

---

### 2026-06-17T13:00Z — Einstein — note — crew online
Five-agent crew bootstrapped (conductor Einstein; builders Archimedes/Euclid/Gauss;
reviewer Newton). Current backlog to groom/delegate:
- #17 — refactor Corridor onto `beamercorporatekit` (touches the corridor colour/font themes).
- #18 — add Aurora+Corridor `.lvt`/`.tlg` regression tests (needs a TeX toolchain; CI is the first real compile).
- #19 — extract `base/themes/beamercorporatelayout.sty` (new file; later Corridor inner/outer rewiring).
Proposed sequence: land #17 first, then #18's `.tlg` baselines and #19's Corridor
rewiring on top of it. Builders may self-claim a groomed issue or await delegation;
post a claim entry + an issue comment before starting. Feature PRs need Newton's
"approved" before the human merges.

### 2026-06-24T09:30Z — Einstein — note — "modern beamer" roadmap approved
Human reviewed the two reference decks (Corridor jet-fuel deck; Divergence DT
Proposal) and set direction: (1) **foundation first**; (2) incorporate Divergence
as **canvas kit + theme**; (3) **extract both** decks' TikZ into reusable,
theme-agnostic libraries; (4) **create the full backlog now, run the all-new-file
kits in parallel** with the foundation chain. Roadmap = one engine, layered kits,
many looks: `beamercorporatekit` (palette+fonts, on main) → `beamercorporatelayout`
(#19) → `beamercanvaskit` (#22, new) → `beamertikzkit` (#23, new) → themes/examples.
Scoped 7 new issues: #22 canvas kit, #23 tikz kit, #24 Divergence theme,
#25 jet-fuel Corridor example, #26 Divergence example, #27 docs+gallery,
#28 quickstart+handout/notes.

### 2026-06-24T09:30Z — Einstein — delegate #17,#19,#18 -> Euclid
Foundation chain to **Euclid** as single owner (avoids collisions on the
Corridor/engine/Aurora cluster): **#17 → #19 → #18**, each its own branch/PR.
#17 touches `beamercolorthemecorridor.sty`, `beamerfontthemecorridor.sty`,
knobs in `beamercorporatekit.sty`. #19 creates `beamercorporatelayout.sty` +
rewires Corridor/Aurora inner (read beamer semantic colours, no raws). #18 adds
`.lvt`/`.tlg` after #17+#19 (needs TeX toolchain; CI is first real compile).

### 2026-06-24T09:30Z — Einstein — delegate #22(+#24,#26) -> Archimedes
**Archimedes** starts **#22** now (`base/themes/beamercanvaskit.sty`, all-new file
→ parallel-safe). Then **#24** Divergence theme (after #22) → **#26** Divergence
example. Keep canvas kit theme-neutral (colour via args/beamer colours) and
pdfTeX-compilable.

### 2026-06-24T09:30Z — Einstein — delegate #23(+#25) -> Gauss
**Gauss** starts **#23** now (`base/themes/beamertikzkit.sty`, all-new file →
parallel-safe). Colour strictly via beamer semantic colours (matches #19). May be
milestoned (flow/structure, then data: bar/ladder/wheel). Then **#25** jet-fuel
Corridor example (after #23 + #17).

### 2026-06-24T09:30Z — Einstein — note — overlap watch + queued work
Only shared file across the active trio (Euclid #17, Archimedes #22, Gauss #23) is
`CHANGELOG.md` — append under the right subsection, resolve by rebase. Later,
`build.lua` themes table is touched by #24/#25/#27 — coordinate edit windows in the
logs. #27 (docs+gallery) and #28 (quickstart+handout/notes) are **queued**; owners
assigned after their deps land. Newton gates every feature PR with "approved".
