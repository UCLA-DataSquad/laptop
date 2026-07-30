# Validation request: DataSquad laptop-setup tool list

## Context

UCLA Library's Data Science Center runs "DataSquad," a program that trains
undergrad/grad students to do data science consulting work for campus
researchers (cleaning data, stats, viz, some light web/GIS work). New
students and DSC staff get a MacBook (Apple Silicon only — no Intel support
needed) and run a one-shot onboarding script (`mac`, forked from the
thoughtbot/laptop project) to install their whole dev environment via
Homebrew.

The script was last touched in July 2026 (three merged PRs) to fix dead
Homebrew taps and a CI bug that had silently let the smoke test pass for
years. That pass also cross-checked the maintainer's own `brew leaves` and
added missing tools, but did **not** critically re-examine the older
thoughtbot-inherited packages — several of those may be leftover cruft from
the original template rather than things DataSquad actually uses.

## Evidence: what DSC consultations actually ask for

Rather than guess, we pulled real consultation-request text from DSC's own
stats-reporting data (`dsc_consult_merged.rds`, 1,525 logged requests with a
free-text reason, spanning 2021-01 through 2024-06 — this is DSC/DataSquad
direct-consultation intake, not a survey; **coverage caveats**: keyword
matching on patron-written text, so it undercounts tools patrons don't name
by brand, and it's silent on internal consultant/DataSquad-member tooling
since patrons describe their research problem, not the consultant's
toolchain choice). Frequency = % of the 1,525 requests whose text matched
each keyword group (a request can match more than one):

| Topic | % of requests | Matches |
|---|---|---|
| GIS / ArcGIS / QGIS / mapping / LARIAC | 11.5% | 175 |
| Data cleaning/wrangling | 7.5% | 114 |
| Regression/statistics | 5.0% | 76 |
| Tableau | 4.7% | 72 |
| Dataverse | 3.8% | 58 |
| Visualization | 3.1% | 47 |
| Python/pandas/Jupyter | 3.0% | 45 |
| Git/GitHub | 2.4% | 37 |
| CSV | 1.3% | 20 |
| Survey design | 1.2% | 18 |
| Excel | 1.1% | 17 |
| SPSS | 0.4% | 6 |
| R/RStudio | 0.3% | 4 |
| Qualtrics / Machine learning | 0.3% each | 4 each |
| SQL / Google Sheets / Text analysis-NLP | 0.2% each | 3 each |
| OpenRefine | 0.1% | 2 |
| MATLAB / LaTeX / Quarto-RMarkdown / Zotero / Web scraping | 0.1% each | 1 each |
| Stata, SAS, Power BI, NVivo, Docker, JMP, Access, API, Drone, 3D print/scan | 0.0% | 0 |

Two findings that directly bear on the tool list:

1. **Neither Tableau (4.7% of requests) nor QGIS (part of the 11.5% GIS
   bucket, with 3 of the sampled GIS quotes explicitly naming QGIS) is
   installed by `mac` today.** Both are real, repeated, named patron
   requests, and both are Homebrew-cask-installable (`tableau` /
   `tableau-public`, `qgis`).
2. **R is named far less often than Python** in what patrons ask for (4 vs.
   45 mentions) even though R/RStudio is the project's most heavily
   documented and championed language. This doesn't mean drop R — DSC staff
   use R heavily for internal reporting/Quarto work regardless of patron
   volume — but it's worth knowing patron-facing demand skews Python.
3. Zero hits for Docker, Stata, SAS, Power BI, NVivo despite Docker Desktop
   being installed by the script — this is at least consistent with "nobody
   asked for it in consultations," though Docker could still be justified by
   internal infra work (Dataverse, etc.) rather than patron consulting.

This doesn't settle the "under review" thoughtbot-leftover packages below
(`rcm`, `reattach-to-user-namespace`, ctags, `vim`, `watchman`, `the_silver_searcher`,
`gcc`/`xz`/`libxt`/`cairo`/`libyaml`/`coreutils`) — none of those would ever
show up in patron-facing consultation text since they're consultant-side dev
tooling, not something a patron would name. Judge those on the criteria
already listed below instead.

## What's actually installed (current `mac` script, full list)

Taps:
- `universal-ctags/universal-ctags`

Formulae:
```
universal-ctags (HEAD build), git, openssl, rcm, reattach-to-user-namespace,
the_silver_searcher, tmux, vim, watchman, zsh, gcc, xz, libxt, cairo,
rlwrap, pandoc, tree, jq, yq, wget, rclone, git-lfs, xan, gh, imagemagick,
libyaml, coreutils, shellcheck, yamllint, r, node
```

Casks:
```
miniforge, openrefine, iterm2, docker-desktop, rstudio, vscodium, quarto
```

## What we already know / have decided (don't relitigate)

- **Apple Silicon only.** No need to flag Intel-Mac compatibility gaps.
- **Miniforge over Anaconda** and **VSCodium over VS Code** were deliberate
  2025-2026 changes, made to follow The Carpentries' official 2025 setup-tooling
  recommendation. Keep both.
- **R + RStudio, Quarto, Node** are confirmed DSC needs (stats reporting,
  Quarto reports/slides, Astro/Jekyll/Hugo site tooling across DSC repos).
  Don't question whether DSC needs R or Quarto — that's settled.
- **`gh` (GitHub CLI)** is confirmed needed.
- **`git-lfs`, `xan`, `yq`, `yamllint`, `shellcheck`, `pandoc`** were added in
  the July 2026 pass specifically because they matched the maintainer's real
  `brew leaves` on his own working machine — treat these as confirmed current
  needs, not candidates for removal.

## What we want challenged

This script is a direct fork of thoughtbot/laptop (a general Rails/web-dev
setup script from ~2011-2020), adapted over time for a *data science
consulting* audience. Several packages look like they may never have been
re-evaluated for that audience:

- `rcm` — thoughtbot's own dotfiles-management tool. Is there any evidence
  DataSquad students use a shared dotfiles repo? If not, this is almost
  certainly dead weight from the original template.
- `reattach-to-user-namespace` — a tmux/pasteboard-integration shim that was
  only necessary on old macOS + tmux versions before clipboard integration
  was fixed upstream. Recent tmux (and recent macOS) may not need this at all.
- `universal-ctags` built from HEAD (not the stable bottle) — building from
  source on every fresh install is slow and fragile compared to installing
  the stable formula. Is ctags even used by anyone here (vim tag-jumping),
  or is this vestigial from a vim-centric workflow the group has moved away
  from (note: VSCodium is now the primary recommended editor)?
- `gcc`, `xz`, `libxt`, `cairo`, `libyaml`, `coreutils` — these look like
  build-dependency leftovers (e.g., cairo/libxt are typically pulled in as
  R-package system deps, libyaml for Ruby's psych, coreutils for GNU-vs-BSD
  utility parity). Are they still required as direct dependencies of
  something in the current toolchain, or would Homebrew pull them in
  automatically as transitive deps of `r`/other formulae if actually needed
  (making the explicit `brew` lines redundant)?
- `vim` — installed alongside VSCodium and RStudio. Given VSCodium is now the
  documented primary editor, is `vim` still doing anything beyond being a
  thoughtbot-template holdover, or is it a deliberate terminal-editor fallback
  worth keeping?
- `watchman` — a Facebook/Meta filesystem-watching daemon, historically used
  for JS toolchains (Jest, React Native). Given Node is only installed here
  for Astro/Jekyll/Hugo static-site builds (not JS test suites), is watchman
  actually exercised by anything in that workflow?
- `the_silver_searcher` (`ag`) — we're leaning toward replacing this outright
  with `ripgrep` (`rg`): actively maintained, faster, and there's no
  functionality `ag` offers that `rg` lacks. Push back if that's wrong.

## Six specific tool proposals (evaluate each, don't brainstorm alternatives)

Based on the consultation-frequency evidence above plus known gaps in the
current CLI toolkit, here are six concrete candidates. For each: **do you
agree, and why/why not** — not "here are some other tools to consider."

1. **`qgis` (cask)** — GIS is the single largest consultation topic (11.5% of
   requests) and QGIS is named explicitly multiple times in the raw request
   text, yet nothing GIS-capable is installed today.
2. **`gdal` (formula)** — the CLI engine behind most geospatial tooling
   (`ogr2ogr`, `gdalinfo`). Pairs with QGIS: lets students script format
   conversions/reprojections instead of doing everything by hand in a GUI.
   Given GIS is the top topic, this may be the single highest-leverage
   addition on this whole list.
3. **`tableau-public` (cask, not paid Desktop)** — Tableau is the 4th-most
   frequently named tool in consultations (4.7%). Public avoids the
   licensing-cost question a paid Desktop cask would raise.
4. **`duckdb` (formula)** — local SQL engine that queries CSV/Parquet directly
   from the CLI, no server. Fills the "quick tabular data profiling" gap;
   complements `xan`/`jq`/`yq` (those transform/filter; duckdb lets you
   actually join/query across files).
5. **`visidata` (formula)** — terminal-based interactive CSV/Excel/JSON
   explorer (arrow-key sort/filter/pivot). Useful for the walk-in
   DataSquad-consultant workflow: eyeball a patron's messy file fast without
   opening RStudio/Jupyter first.
6. **`ripgrep` (formula)** — see `the_silver_searcher` above; proposing this
   as a straight replacement, not an addition.

## What we're also asking

1. For each package under "what we want challenged": keep, drop, or replace
   — and with what, if a replacement?
2. For each of the six proposals above: adopt, reject, or adopt-with-changes
   — and why?
3. Is `imagemagick` still the right choice for image manipulation in 2026, or
   has something better emerged for this use case (crop/resize for reports)?
4. `openrefine` (cask) — still the standard recommendation for messy-data
   cleanup, or has tooling in this space moved on?
5. Beyond the six proposals above, is there anything else obviously missing
   for *data science consulting work specifically* — but only flag it if
   you're confident it fills a real gap these six don't already cover.

## What we need back

For each of the ~8 flagged existing packages: a clear **keep / drop /
replace** verdict with one-line reasoning. For each of the six proposals: a
clear **adopt / reject** verdict with one-line reasoning — not a generic
"here are popular data science tools" survey. Also give an overall
confidence score (1-5) on how much this list needs to change versus how much
is genuinely fine as-is.
