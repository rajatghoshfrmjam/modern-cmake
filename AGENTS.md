# AGENTS.md

This file provides guidance to agents when working with code in this repository.

## What this is

The "Modern CMake" book — a MyST Markdown site/PDF hosted at https://cliutils.gitlab.io/modern-cmake/. The canonical repo is on GitLab (https://gitlab.com/CLIUtils/modern-cmake), and CI is GitLab CI, not GitHub Actions.

## Commands

Everything runs through [pixi](https://pixi.sh), which defines two environments: `build` (mystmd, typst) and `test` (cmake, compilers, ROOT, Boost).

- `pixi run build` — build the HTML site to `_build/html` (`myst build --html`)
- `pixi run serve` — live-reload dev server (`myst start`)
- `pixi run pdf` — build the PDF via typst to `_build/pdf/modern-cmake.pdf`
- `pixi run test` — configure, build, and run ctest over all examples (`cmake -Sexamples -Bbuild && cmake --build build -v && cmake --build build -t test`)
- `prek -a --quiet` — lint (pre-commit hooks: prettier, codespell, cmake-format, etc.)

To test a single example, configure as above and use ctest's `-R` filter or build just that target, e.g. `ctest --test-dir build -R simple`.

## Structure

- `chapters/` — the book content (Markdown). The table of contents lives in `myst.yml` (`project.toc`), **not** `SUMMARY.md` (a leftover from GitBook). When adding/removing a page, update `myst.yml`.
- `examples/` — real, compiled-and-tested CMake projects, tied together by `examples/CMakeLists.txt` (each is also a standalone project). Some example READMEs are included as book pages via the toc. Code in chapters is sometimes literal-included from these examples, so changing an example can change the book.
- `myst.yml` — MyST project config: toc, PDF export, site theme.

## Conventions

- The CMake version (currently embedded in minimum-version examples like `3.15...4.1`) is managed by bump-my-version (`.bumpversion.toml`), which rewrites version strings across `README.md`, `chapters/`, and all `examples/*/CMakeLists.txt`. Don't hand-edit those version numbers inconsistently; use `bump-my-version` or update everywhere.
- CMake files are formatted by cmake-format (`.cmake-format.yaml`); Markdown/YAML by prettier.
- The book preaches CMake 3.15+ "Modern CMake" best practices — example code added to chapters should follow them (targets over directory commands, no hard-coded paths, etc.).
