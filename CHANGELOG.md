# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.2] - 2026-03-28

### Changed
- Unified Makefile: replaced macOS universal binary with separate `darwin/amd64` and `darwin/arm64` targets; standardized targets (`build`, `build-all`, `test`, `lint`, `check`, `package`, `clean`, `help`) and output layout (`dist/` flat directory, `.zip` archives).

## [1.0.1] - 2026-03-28

### Internal

- Updated Go module path to `github.com/nlink-jp/csv-to-json` following repository transfer to nlink-jp organization.

## [1.0.0] - 2025-09-10

### Added

- Initial release of `csv-to-json`.
- Converts CSV data to a JSON array.
- Reads from a file path argument or from `stdin`.
- Cross-platform build support for macOS, Windows, and Linux.
- macOS Universal Binary support.
- Version information available via `-v` or `--version` flags.
