# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Changed

- `make vuln` — and so `make package` — scans every platform in `PLATFORMS`,
  with the environment `build-all` builds each with. It scanned the host's
  build alone, so an advisory in a Linux- or Windows-only package could not
  stop a release.

## [1.1.1] - 2026-11-14

### Security

- **Rebuilt with Go 1.27.4, which `go.mod` now requires** (it was `go 1.25.1`).
  Nothing the tool does changed. v1.1.0 was built with Go 1.26.4, and 24
  standard-library advisories have been fixed since. `govulncheck -mode binary`
  finds none of the affected functions in the published binary — the tool links
  neither `net/http` nor `crypto/tls` — so this brings the build onto a current
  toolchain rather than closing a known exposure.

### Changed

- `make package` runs `govulncheck ./...` first (`make vuln`) and stops on a
  finding, so a release cannot be built past one.
- `make check` passes again. Lint findings stopped it before the tests ran;
  they are cleared by making ignored errors explicit, with no change in what
  the tool does.

### Fixed

- **`make verify-release` now fails closed.** Its last block chained unzip, the
  packaged binary's `--version` and `spctl` with `&&` and ended the whole chain
  in `|| true`, so a zip that did not unpack or a binary that did not run exited
  0 and the upload proceeded. Each step is now judged on its own, the packaged
  binary's `--version` must contain the tag being released, and only the
  informational `spctl` line may be ignored. Matches the org template
  (CONVENTIONS.md §Code Signing → Verifying a release).
- **The Linux archives no longer carry macOS file metadata.** macOS `tar` wrote
  each bundled file's extended attributes (`com.apple.provenance`, and a Dropbox
  attribute where the tree is synced) into the `.tar.gz` twice: as AppleDouble
  `._` members, which GNU tar extracts as stray `._<name>` files beside the real
  ones, and as `LIBARCHIVE.xattr.*` / `SCHILY.xattr.*` pax headers, which it
  reports as unknown keywords. `make package` now archives with
  `COPYFILE_DISABLE=1 tar --no-xattrs`; each setting stops one of the two.
  Archives already published still carry them; the files themselves are
  unaffected.

### Internal

- `make verify-release` also judges each Linux archive: no AppleDouble or other
  macOS metadata members — listed with `--options 'tar:!mac-ext'`, because a
  plain macOS listing folds `._` members away — no extended attributes as pax
  headers, and exactly the canonical binary, `README.md` and `LICENSE`, compared
  in the C locale.
- The Linux-archive check in `make verify-release` reads each archive's pax
  headers with Python's `tarfile` instead of grepping the decompressed stream,
  which also matched file text that names the keywords (a bundled CHANGELOG,
  for one).

## [1.1.0] - 2026-07-12

### Removed

- **darwin/amd64 (Intel) pre-built binary.** macOS releases now ship
  **arm64 only**, per the org-wide policy (darwin is Apple-Silicon only; no
  universal binaries). Intel Mac users can build from source.

### Changed

- **Linux release archives are now `.tar.gz`** (darwin/windows remain `.zip`),
  per `nlink-jp/.github` CONVENTIONS.md §Release Archive Standard.
- **darwin code-signature identifier** is now the canonical `csv-to-json`.

No change to the binary's behaviour — a packaging / build-config release.

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
