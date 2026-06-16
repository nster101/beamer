# CLAUDE.md

Guidance for AI assistants working in this repository.

## What this is

`beamer` is a LaTeX document class for producing presentations (on-screen
slides) plus support material such as handouts and speaker notes. It is a
mature, widely-used TeX package distributed via CTAN and TeX Live. There is
**no compiled application** — the "product" is a collection of TeX class
(`.cls`), package (`.sty`), and macro (`.tex`) files that users load from
their own LaTeX documents.

- Language: TeX / LaTeX2e (TeX macro programming, not a general-purpose
  language).
- Build/test system: [`l3build`](https://github.com/latex3/l3build) driven by
  `build.lua` (run with `texlua`).
- License: dual LPPL / GPL (see `LICENSE.md`).
- Current maintainers: Joseph Wright and samcarter (see `AUTHORS.md`);
  originally by Till Tantau.

## Repository layout

```
base/                 The class and all packages that ship to users
  beamer.cls          Main entry point: \documentclass{beamer}
  beamerarticle.sty   "article mode" companion (slides as flowing prose)
  beamerbase*.sty     Core engine, split by concern (one file per subsystem)
  themes/
    color/  beamercolortheme<name>.sty
    font/   beamerfonttheme<name>.sty
    inner/  beamerinnertheme<name>.sty
    outer/  beameroutertheme<name>.sty
    theme/  beamertheme<name>.sty   (full "presentation" themes)
  art/                Icon/logo sources (.tex -> .eps/.pdf)
  emulation/          Compatibility shims for other classes (prosper, seminar, ...)
  multimedia/         multimedia.sty / xmpmulti.sty (movies, multi-inclusion)
  patch/              Patches for third-party packages (e.g. paralist)
doc/                  User guide and example/demo sources
  beameruserguide.tex Master doc; \include's beamerug-*.tex chapters
  beamerug-*.tex      One chapter per topic (frames, overlays, themes, ...)
  examples/           Full worked example presentations
  solutions/          "Solution" template talks
testfiles/            l3build regression tests (.lvt source, .tlg expected log)
texmf/                Local TeX tree used during testing (e.g. lppl.tex)
build.lua             l3build configuration (the build/test entry point)
CHANGELOG.md          Keep a Changelog format; edit [Unreleased] section
.github/workflows/    CI (main.yaml = tests, deploy.yaml = release on tag)
.github/tl_packages   TeX Live packages needed to build/test
```

## Build, test, and documentation commands

All commands run through `l3build` (TeX Live ships it). They must be run from
the repository root.

```sh
l3build check            # Run the full regression test suite
l3build check <name>     # Run a single named test (e.g. beamerthemedefault)
l3build save <name>      # Regenerate the expected .tlg for a test after an
                         #   intentional, reviewed change in output
l3build doc              # Typeset the user guide + example PDFs
l3build ctan -q -H       # What CI runs: build the CTAN release bundle (also
                         #   runs checks); fails on any error
l3build clean            # Remove generated files
```

Notes:
- Tests are checked only with **pdfTeX** (`checkengines = {"pdftex"}` in
  `build.lua`). Ghostscript is installed in CI for XeTeX-related needs.
- The test suite is intentionally small: `testfiles/` contains `.lvt` inputs
  paired with `.tlg` expected-output logs. A test passes when the produced log
  matches the saved `.tlg`. After deliberately changing output, regenerate with
  `l3build save <name>` and review the `.tlg` diff carefully.
- There is no separate lint step; correctness is verified by `l3build check`.

## CI / release

- `.github/workflows/main.yaml` runs `l3build ctan -q -H --show-log-on-error`
  on every push and on PRs targeting `main`.
- `.github/workflows/deploy.yaml` runs on a pushed tag (`v*`), builds the CTAN
  zip, and creates a GitHub release. **Releases are tag-driven** — do not craft
  release artifacts by hand.
- TeX Live dependencies live in `.github/tl_packages`; add a package there if a
  new dependency is introduced.

## Versioning

Version bumping is automated by `l3build tag` via `update_tag` in `build.lua`,
which rewrites version/date strings across `base/beamer.cls`,
`base/beamerarticle.sty`, `doc/beameruserguide.tex`, and `CHANGELOG.md`.
**Do not** hand-edit the version in `\ProvidesClass`/`\ProvidesPackage` lines or
the `\beamerugversion` macro — let the tag process do it. Versions are
`major.minor` only (e.g. `v3.77`).

## Code conventions (TeX)

- **File naming is structural and significant.** Themes are discovered by name:
  `\usecolortheme{crane}` loads `beamercolorthemecrane.sty`. Keep the prefix
  pattern (`beamercolortheme`, `beamerfonttheme`, `beamerinnertheme`,
  `beameroutertheme`, `beamertheme`) and place files in the matching `themes/`
  subdirectory. When adding a theme, also register its name in the relevant list
  inside the `themes` table in `build.lua` so it is included in the doc demos.
- **Private namespace:** internal macros and registers use the `\beamer@`
  prefix (e.g. `\beamer@tempdim`). Public macros/dimensions have plain names
  (e.g. `\headdp`, `\footheight`). Follow this split — never expose a `\beamer@`
  internal as user API, and keep new internals namespaced.
- **One subsystem per `beamerbase*.sty` file** (e.g. `beamerbaseframe.sty`,
  `beamerbaseoverlay.sty`, `beamerbasecolor.sty`). Put new core code in the
  matching file rather than `beamer.cls`.
- Beamer's core abstractions are **templates**, **colors/fonts** (named, e.g.
  `\setbeamercolor`/`\setbeamerfont`), **overlays** (`\onslide`, `<...>`
  specs), and **modes** (`presentation`, `article`, `handout`, etc., via
  `beamerbasemodes.sty`). Prefer extending these mechanisms over ad-hoc code.
- Match the surrounding style of the file you edit: existing indentation,
  TeX grouping, and comment density (top-of-file copyright block, `%`-prefixed
  section banners).

## Documentation conventions

- The user guide is `doc/beameruserguide.tex`, which `\include`s one
  `beamerug-<topic>.tex` per chapter. When you add or change user-facing
  behavior, update the relevant `beamerug-*.tex` chapter.
- Demo theme galleries are generated automatically from the `themes` table in
  `build.lua` (`typeset_demo_tasks`), not maintained by hand.

## Changelog

- Add an entry to the `## [Unreleased]` section of `CHANGELOG.md` for any
  user-visible change. The file follows
  [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) with `New`,
  `Changed`, `Fixed`, `Deprecated`, `Removed` subsections.
- Reference the relevant GitHub issue/PR (e.g. `see #962`) as existing entries
  do. The release process moves `[Unreleased]` content under the new tag
  automatically.

## Workflow expectations for changes

1. Make the focused change in the correct `base/` file (and the matching
   `themes/` subdir / `beamerbase*` file).
2. Update the `doc/beamerug-*.tex` chapter if behavior is user-visible.
3. Add a `CHANGELOG.md` `[Unreleased]` entry referencing the issue/PR.
4. Run `l3build check`; if output legitimately changed, `l3build save <name>`
   and review the `.tlg` diff.
5. Keep commits focused with a concise summary line, mirroring existing history
   (e.g. `fixed shadow colour of covered blocks (fix #962)`).

## Things not to do

- Don't commit generated artifacts: `*.pdf`, `*.zip`, and `build/` are
  git-ignored (with a few checked-in exceptions under `base/art/` and `doc/`).
- Don't hand-bump versions or assemble release zips — both are automated.
- `beamer` is **not** compatible with the new LaTeX `\DocumentMetadata` /
  tagged-PDF workflow; `beamer.cls` deliberately errors in that case. Don't
  "fix" that by removing the guard — tagged output lives in the separate
  `ltx-talk` class.
