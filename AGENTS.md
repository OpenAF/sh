# Repository Guidelines

## Project Structure & Module Organization

This repository builds self-extracting OpenAF executables for Linux, Alpine Linux, and macOS. Build definitions are oJob YAML files:

- `buildStable.yaml`, `buildNightly.yaml`, and `buildT8.yaml` select distribution, Java version, target OS/architecture, and output name.
- `common.yaml` contains the shared build pipeline: download a target JRE, install OpenAF, repack its shared archive, and create the single-file executable.
- `genSelfExtract.yaml` implements the self-extracting tarball wrapper.
- `ops.yaml` provides S3 upload/download operations used by CI.
- `.github/workflows/` runs stable and daily/T8 builds and publishes artifacts.

## Build, Test, and Development Commands

Use an installed OpenAF runtime to run oJob files:

```sh
ojob buildStable.yaml
ojob buildNightly.yaml
ojob buildT8.yaml
```

These create `oaf-*`, `nightly/oaf-*`, or `t8/oaf-*` artifacts respectively. A full build downloads JREs and may require Docker/emulation for non-native targets; prefer GitHub Actions for the complete matrix.

Validate manifest syntax before committing:

```sh
ruby -e 'require "yaml"; ARGV.each { |f| YAML.load_file(f) }' *.yaml .github/workflows/*.yaml
git diff --check
```

Smoke-test a native generated artifact by creating its symlinks and running JavaScript:

```sh
chmod u+x oaf-linux-x86_64
./oaf-linux-x86_64 --install
./oaf -c 'print("ok")'
```

## Coding Style & Naming Conventions

Use two-space YAML indentation and retain the existing aligned-key style where editing nearby entries. Keep job names descriptive and title-cased (for example, `Build step 3`); use lowercase kebab-free filenames such as `buildNightly.yaml`. Put shared packaging behavior in `common.yaml`, not copied into each distribution manifest.

## Testing Guidelines

There is no standalone test framework. Treat YAML parsing, `git diff --check`, and an executable smoke test on a native target as the minimum validation. Changes to `common.yaml` must be checked against stable, nightly, and T8 consumers; ensure `./oaf --repack` remains before `genSelfExtract` packages `src`.

## Commit, Pull Request, and Security Guidelines

Use short, imperative commit subjects, commonly `fix: ...`, `Update ...`, or `Refactor ...`. Keep each change focused. PRs should state affected distributions, validation performed, and any untested target architectures. Never commit S3 credentials or `OS_OAFSH`; CI obtains publishing credentials from GitHub Secrets.
