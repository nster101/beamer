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
