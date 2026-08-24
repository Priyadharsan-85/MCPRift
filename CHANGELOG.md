# Changelog

All notable changes to MCPRift are documented in this file.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and the project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.5.0] - 2026-08-24

### Added

- Precise authorization outcomes for authentication denial, authorization
  denial, tool errors, protocol errors, transport errors, and unavailable
  targets.
- HTTP-boundary authentication in the disposable lab so only explicit `401`
  or `403` responses satisfy a declared denial.
- Repository-relative contract source locations in SARIF results.
- A checked-in 22-case lab contract for reproducible CI runs.
- A Trusted Publishing workflow for tagged PyPI releases.

### Changed

- Evidence records now use schema version 3 and preserve the precise observed
  outcome.
- The authorization workflow now checks formatting, uploads a seeded regression
  to GitHub code scanning, and stores both clean and failing evidence.
- Package metadata, license declaration, project links, and source-distribution
  contents are prepared for public installation.
- The package and both disposable labs now use one shared version value.

### Fixed

- A tool or protocol failure can no longer make `expected: denied` pass.
- The README no longer claims that the existing Apache-2.0 license is missing.

## [0.4.0] - 2026-08-22

### Added

- Contract-driven access, visibility, session, and protocol checks.
- Sanitized terminal, JSON, and SARIF evidence.
- A deterministic disposable MCP lab and a seeded CI regression.
- OAuth and PKCE boundary checks for the separate local OAuth lab.

