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

* [Universal Ctags] for indexing files for vim tab completion
* [Git] for version control
* [OpenSSL] for Transport Layer Security (TLS)
* [The Silver Searcher] for finding things in files
* [Tmux] for saving project state and switching between projects
* [Watchman] for watching for filesystem events
* [Zsh] as your shell
* [rlwrap] readline wrapper
* [tree]  lists contents of directory in a tree like structure
* [jq] JSON parser 
* [wget] network downloader 
* [rclone] cloud storage data synch 
* [docker] container runtime (Docker Desktop, includes CLI + GUI)

[Universal Ctags]: https://ctags.io/
[Git]: https://git-scm.com/
[OpenSSL]: https://www.openssl.org/
[RCM]: https://github.com/thoughtbot/rcm
[The Silver Searcher]: https://github.com/ggreer/the_silver_searcher
[Tmux]: http://tmux.github.io/
[Watchman]: https://facebook.github.io/watchman/
[rlwrap]: https://linux.die.net/man/1/rlwrap
[tree]: https://linux.die.net/man/1/tree
[jq]: https://stedolan.github.io/jq/
[wget]: https://www.geeksforgeeks.org/wget-command-in-linux-unix/
[rclone]: https://rclone.org
[docker]: https://www.docker.com/products/docker-desktop/

* [GitHub CLI] for interacting with the GitHub API

[GitHub CLI]: https://cli.github.com/

Image tools:

* [ImageMagick] for cropping and resizing images

Languages and editors:

* [R] - Base R installed via Homebrew
* [RStudio] - R IDE
* [Miniforge] - conda-forge-based Python/conda distribution (follows [The Carpentries' 2025 setup recommendation](https://carpentries.org/blog/2025/03/lesson-setup-instructions-task-force-recommendations/) to move off Anaconda)
* [VSCodium] - Python/shell/git editor (VS Code without Microsoft branding/telemetry), also per The Carpentries' 2025 recommendation
* [Node.js] - JavaScript runtime + npm, for working in the Astro/Jekyll/Hugo site repos across DSC (Astro/Jekyll/Hugo themselves are scaffolded per-project via `npm`/`bundler`, not installed globally)
* [Quarto] - scientific and technical publishing system used for DSC reports, slides, and stats

[R]: https://www.r-project.org/
[RStudio]: https://posit.co/products/open-source/rstudio/
[Miniforge]: https://github.com/conda-forge/miniforge
[VSCodium]: https://vscodium.com/
[Node.js]: https://nodejs.org/
[Quarto]: https://quarto.org/

It should take less than 15 minutes to install (depends on your machine).

See the [wiki](https://github.com/UCLA-DataSquad/laptop/wiki)
for more customization examples.

Contributing
------------

1. Edit the `mac` file.
2. Document in the `README.md` file.
3. Follow shell style guidelines by using [ShellCheck] and [Syntastic].

```sh
brew install shellcheck
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

