[![Smoke Test CI](https://github.com/UCLA-DataSquad/laptop/actions/workflows/smoke.yml/badge.svg)](https://github.com/UCLA-DataSquad/laptop/actions/workflows/smoke.yml)

DSC - DataSquad Laptop Setup (for MacBook)
======

**Laptop** is a script to set up a macOS laptop for data science tools for DSC staff & Data Squad.

* It can be run multiple times on the same machine safely.
* It installs, upgrades, or skips packages
based on what is already installed on the machine.


Requirements
------------

This script supports:

* macOS Big Sur (11.2) - Apple Silicon 

To make backward compatable we need to add some logic to test for architecture. 

NOTE: Homebrew installs to `/opt/homebrew` on Apple Silicon rather than `/usr/local` so `HOMEBREW_PREFIX` probably needs to be dependant on the architecture. see: <https://github.com/thoughtbot/laptop/issues/589>

* macOS Mavericks (10.9)
* macOS Yosemite (10.10)
* macOS El Capitan (10.11)
* macOS Sierra (10.12)
* macOS High Sierra (10.13)
* macOS Mojave (10.14)
* macOS Catalina (10.15)

Older versions may work but aren't tested.
Bug reports for older versions are welcome.

To Install
-------

Download the script:

```sh
curl --remote-name https://raw.githubusercontent.com/UCLA-DataSquad/laptop/master/mac
```

Review the script (avoid running scripts you haven't read!):

```sh
less mac
```

Execute the downloaded script:

```sh
sh mac 2>&1 | tee ~/laptop.log
```

You'll need to use your laptop password. If it's the first time running this script, it will trigger an installation of Xcode and you will need to hit return. 

Optionally, review the log:

```sh
less ~/laptop.log
```

Optionally, [install thoughtbot/dotfiles][dotfiles].

[dotfiles]: https://github.com/thoughtbot/dotfiles#install

Debugging
---------

Your last Laptop run will be saved to `~/laptop.log`.
Read through it to see if you can debug the issue yourself.
If not, copy the lines where the script failed into a
[new GitHub Issue](https://github.com/UCLA-DataSquad/laptop/issues/new) for us.
Or, attach the whole log file as an attachment.

What it sets up
---------------

macOS tools:

* [Homebrew] for managing operating system libraries.

[Homebrew]: http://brew.sh/

Unix tools:

* [Universal Ctags] for indexing files for vim tab completion (built from HEAD; under review — see Known Issues)
* [Git] for version control
* [OpenSSL] for Transport Layer Security (TLS)
* [RCM] dotfiles management (thoughtbot template default; under review — see Known Issues)
* [reattach-to-user-namespace] tmux/pasteboard integration shim (thoughtbot template default; under review — see Known Issues)
* [ripgrep] fast recursive code/text search (replaces the older `the_silver_searcher`)
* [Tmux] for saving project state and switching between projects
* [Watchman] for watching for filesystem events (under review — see Known Issues)
* [Zsh] as your shell
* [coreutils] GNU command-line utilities for BSD/macOS parity (under review — see Known Issues)
* [rlwrap] readline wrapper
* [pandoc] markup converter (also used under the hood by Quarto)
* [tree]  lists contents of directory in a tree like structure
* [tealdeer] fast `tldr` — short, example-driven command help for the CLI tools on this list
* [jq] JSON parser 
* [yq] YAML/XML processor, same idea as jq -- used on `project-registry.yaml`, Carpentries lesson `config.yaml`, and GitHub workflow files
* [wget] network downloader 
* [rclone] cloud storage data synch 
* [git-lfs] Git support for large data files
* [xan] fast CSV toolkit for data work
* [duckdb] local SQL engine for querying/joining CSV, JSON, and Parquet files directly from the CLI, no server required
* [docker] container runtime (Docker Desktop, includes CLI + GUI)

[Universal Ctags]: https://ctags.io/
[Git]: https://git-scm.com/
[OpenSSL]: https://www.openssl.org/
[RCM]: https://github.com/thoughtbot/rcm
[reattach-to-user-namespace]: https://github.com/ChrisJohnsen/tmux-MacOSX-pasteboard
[ripgrep]: https://github.com/BurntSushi/ripgrep
[Tmux]: http://tmux.github.io/
[Watchman]: https://facebook.github.io/watchman/
[coreutils]: https://www.gnu.org/software/coreutils/
[rlwrap]: https://linux.die.net/man/1/rlwrap
[pandoc]: https://pandoc.org
[tree]: https://linux.die.net/man/1/tree
[tealdeer]: https://github.com/tealdeer-rs/tealdeer
[jq]: https://stedolan.github.io/jq/
[yq]: https://github.com/mikefarah/yq
[wget]: https://www.geeksforgeeks.org/wget-command-in-linux-unix/
[rclone]: https://rclone.org
[git-lfs]: https://git-lfs.com/
[xan]: https://github.com/medialab/xan
[duckdb]: https://duckdb.org
[docker]: https://www.docker.com/products/docker-desktop/

* [GitHub CLI] for interacting with the GitHub API

[GitHub CLI]: https://cli.github.com/

Image tools:

* [ImageMagick] for cropping and resizing images

Geospatial:

* [QGIS] desktop GIS application — added because GIS is DSC's single largest
  consultation topic and QGIS is explicitly named in patron requests
* [GDAL] (`ogr2ogr`, `gdalinfo`) — CLI companion to QGIS for scripted format
  conversion and reprojection

[QGIS]: https://qgis.org/
[GDAL]: https://gdal.org/

Apps:

* [OpenRefine] for cleaning and transforming messy data
* [iTerm2] terminal emulator
* [Positron] Posit's data science IDE (R/Python/Jupyter/Quarto in one editor);
  installed alongside RStudio and VSCodium, not a replacement for either

[OpenRefine]: https://openrefine.org/
[iTerm2]: https://iterm2.com/
[Positron]: https://positron.posit.co/

Languages and editors:

* [R] - Base R installed via Homebrew
* [RStudio] - R IDE
* [Miniforge] - conda-forge-based Python/conda distribution (follows [The Carpentries' 2025 setup recommendation](https://carpentries.org/blog/2025/03/lesson-setup-instructions-task-force-recommendations/) to move off Anaconda)
* [pixi] - fast, project-based Python/conda package manager, taught in DataSquad workshops
* [uv] - fast Python package/project manager (pip/venv/poetry replacement); installed so students and staff can drop into whatever setup a researcher's own project already expects (`pyproject.toml`, `uv.lock`, `requirements.txt`)
* [VSCodium] - Python/shell/git editor (VS Code without Microsoft branding/telemetry), also per The Carpentries' 2025 recommendation
* [Node.js] - pinned to the Active LTS line (`node@24`), for working in the Astro/Jekyll/Hugo site repos across DSC (Astro/Jekyll/Hugo themselves are scaffolded per-project via `npm`/`bundler`, not installed globally). Unversioned `node` tracks Homebrew's Current release, which changes out from under you and isn't recommended for production.
* [Quarto] - scientific and technical publishing system used for DSC reports, slides, and stats

[R]: https://www.r-project.org/
[RStudio]: https://posit.co/products/open-source/rstudio/
[Miniforge]: https://github.com/conda-forge/miniforge
[pixi]: https://pixi.sh
[uv]: https://docs.astral.sh/uv/
[VSCodium]: https://vscodium.com/
[Node.js]: https://nodejs.org/
[Quarto]: https://quarto.org/

Linting / repo maintenance:

* [ShellCheck] for shell script linting (used on `mac` itself, see Contributing below)
* [yamllint] for YAML linting

[yamllint]: https://yamllint.readthedocs.io/

It should take less than 15 minutes to install (depends on your machine).

See the [wiki](https://github.com/UCLA-DataSquad/laptop/wiki)
for more customization examples.

Known Issues
------------

This script is a fork of [thoughtbot/laptop](https://github.com/thoughtbot/laptop)
(a general web-dev setup script), adapted over time for data science
consulting work.

A July 2026 evidence-based review (`validation-prompt-tooling-2026-07-26.md`,
cross-checked against DSC's own consultation-request data and verified against
primary sources — Homebrew, Node.js, Astro, and Positron docs — before
adopting) confirmed and removed several genuinely redundant/unused packages
(`gcc`, `xz`, `cairo`, `libxt`, `libyaml` — either transitive dependencies of
`r` already, or depended on by nothing installed here) and added tools backed
by real demand (`qgis`, `gdal`, `tableau`-adjacent GIS/viz gap; `duckdb`,
`ripgrep`, `tealdeer`). One caveat from that review: an external
keyword-frequency claim that R now outranks Python in consultation requests
could not be reproduced from the underlying data (the merged consultation
dataset blends real patron intake with Trello project-card text duplicated
once per comment, which distorts any raw frequency count) — treat any future
frequency claim from this dataset with that in mind.

Still marked "under review," pending a decision (not yet confirmed either
way): `rcm`, `reattach-to-user-namespace`, `universal-ctags` HEAD build,
`vim`, `watchman`, `coreutils`. Also open: whether to add `tableau-public`
(real demand, but Tableau Public auto-publishes saved workbooks to a public
gallery by default — needs a documented opt-in rather than a silent install
given patron-data handling) and whether to pilot `jamovi` as a gentler
stats-GUI on-ramp for students.

Contributing
------------

1. Edit the `mac` file.
2. Document in the `README.md` file.
3. Follow shell style guidelines by using [ShellCheck] and [Syntastic]. `shellcheck` is installed by `mac` itself (see above), so if you've already run the script once you have it:

```sh
shellcheck -s sh mac
```

[ShellCheck]: http://www.shellcheck.net/about.html
[Syntastic]: https://github.com/scrooloose/syntastic

4. Check out the GitHub Actions tab to make sure the build runs without error.

Thank you, [contributors]!

[contributors]: https://github.com/thoughtbot/laptop/graphs/contributors

By participating in this project,
you agree to abide by the thoughtbot [code of conduct].

[code of conduct]: https://thoughtbot.com/open-source-code-of-conduct

License
-------

Laptop is © 2011-2020 thoughtbot, inc.
It is free software,
and may be redistributed under the terms specified in the [LICENSE] file.

[LICENSE]: LICENSE

