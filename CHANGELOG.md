# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.3] - 2026-05-23

### Changed

- **Darwin releases are now Developer ID signed and Apple-notarized.**
  `csv-to-json-v1.0.3-darwin-{amd64,arm64}.zip` carry full Apple
  Developer ID Application signatures and notarization tickets from
  Apple. End users on macOS no longer need to bypass Gatekeeper
  with right-click → Open or `xattr -d com.apple.quarantine` on
  first launch; local users who place `csv-to-json` under
  Dropbox-synced (or any other FileProvider-managed) paths are no
  longer killed by macOS's ad-hoc + provenance distrust policy.
  Pipeline: `scripts/codesign-darwin.sh` +
  `scripts/notarize-darwin.sh`, driven by `make package`. Adopts
  the org-wide convention in `nlink-jp/.github` CONVENTIONS.md
  §Code Signing.

No behaviour change to the binary itself — feature-wise this is
identical to v1.0.2.

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
