# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Removed

- `VERSION` file; the README version badge now reads the version from PyPI ([#301](https://github.com/danieldeer/seriousdb/pull/301)).
- Nix development environment (`flake.nix`, `flake.lock`) and related documentation; `uv` is the only supported setup now.

## [0.2.2] - 2026-09-25

> [!NOTE]
> 0.2.0 and 0.2.1 were published to PyPI on the same day and superseded by
> 0.2.2 within minutes. They are not listed separately; this entry covers all
> changes since 0.1.0.

### Added

- Python module-level API: `seriousdb.get`, `set`, `delete`, `exists`, `get_all`, `get_bulk` and `count` let other Python projects use SeriousDB as an embedded storage layer ([#227](https://github.com/danieldeer/seriousdb/pull/227), [#287](https://github.com/danieldeer/seriousdb/pull/287)).
- Performance benchmarks for the storage engine, including multi-process workloads ([#215](https://github.com/danieldeer/seriousdb/pull/215), [#263](https://github.com/danieldeer/seriousdb/pull/263)).
- API reference generated with Sphinx under `docs/reference/`, checked for staleness in CI ([#283](https://github.com/danieldeer/seriousdb/pull/283)).
- Conventional Commits enforced by a pre-commit hook and a pull request title check in CI ([#238](https://github.com/danieldeer/seriousdb/pull/238)).
- Project files: `CHANGELOG.md`, `CODE_OF_CONDUCT.md`, `CONTRIBUTORS`, `SECURITY.md`, `TODO.md` and `VERSION` ([#33](https://github.com/danieldeer/seriousdb/pull/33)), an AI usage policy in `AGENTS.md` ([#289](https://github.com/danieldeer/seriousdb/pull/289)), `.editorconfig` ([#245](https://github.com/danieldeer/seriousdb/pull/245)) and README badges ([#265](https://github.com/danieldeer/seriousdb/pull/265)).

### Changed

- Persistence uses a write-ahead log instead of rewriting the whole database file after every change. Each write is durably appended to `<file>.wal` before it returns, and the log is periodically compacted into the JSON snapshot ([#258](https://github.com/danieldeer/seriousdb/pull/258)).
- Documentation updated to describe the Python API ([#240](https://github.com/danieldeer/seriousdb/pull/240), [#262](https://github.com/danieldeer/seriousdb/pull/262), [#267](https://github.com/danieldeer/seriousdb/pull/267), [#274](https://github.com/danieldeer/seriousdb/pull/274)).

### Fixed

- The database file is written atomically, so a crash while saving or while creating the default database no longer leaves a corrupt file ([3208b11](https://github.com/danieldeer/seriousdb/commit/3208b11), [#279](https://github.com/danieldeer/seriousdb/pull/279)).
- Backups of corrupt database files are no longer overwritten when two are created within the same second ([#260](https://github.com/danieldeer/seriousdb/pull/260)).

### Removed

- The HTTP server, the FastAPI dependency, the Docker image and the `SERIOUSDB_HOST` and `SERIOUSDB_PORT` settings. SeriousDB is now used as a Python library ([#246](https://github.com/danieldeer/seriousdb/pull/246), [#282](https://github.com/danieldeer/seriousdb/pull/282)).

## [0.1.0] - 2026-09-18

First release. SeriousDB 0.1.0 is a FastAPI service that keeps a key-value
store in memory and persists it to a JSON file.

### Added

- HTTP API on `/db`:
  - `PUT /db` stores a value, returning `201` for a new key and `200` when overwriting ([#162](https://github.com/danieldeer/seriousdb/pull/162)).
  - `GET /db` returns a value; `HEAD /db` checks whether a key exists ([#37](https://github.com/danieldeer/seriousdb/issues/37)).
  - `DELETE /db` removes a key and returns its previous value ([#121](https://github.com/danieldeer/seriousdb/pull/121), [#150](https://github.com/danieldeer/seriousdb/pull/150)).
  - `GET /db/all`, `GET /db/bulk` ([#203](https://github.com/danieldeer/seriousdb/pull/203)) and `GET /db/count` ([#180](https://github.com/danieldeer/seriousdb/pull/180)).
- `GET /health` readiness endpoint ([#107](https://github.com/danieldeer/seriousdb/pull/107)).
- JSON persistence to a `.sdb` file, written in the background after each change ([#84](https://github.com/danieldeer/seriousdb/pull/84)).
- Recovery from corrupt database files: an unreadable or non-object file is moved to `<file>.corrupt-<timestamp>` and a fresh database is created ([#72](https://github.com/danieldeer/seriousdb/pull/72), [#85](https://github.com/danieldeer/seriousdb/pull/85)).
- Consistent error responses: `404` for missing keys, `503` when the database is unavailable ([#44](https://github.com/danieldeer/seriousdb/pull/44), [#116](https://github.com/danieldeer/seriousdb/pull/116), [#119](https://github.com/danieldeer/seriousdb/pull/119)).
- Configuration through environment variables or a `.env` file: `SERIOUSDB_DB_FILE`, `SERIOUSDB_LOG_LEVEL`, `SERIOUSDB_HOST` and `SERIOUSDB_PORT` ([#151](https://github.com/danieldeer/seriousdb/pull/151)). The server binds to `127.0.0.1` by default ([#129](https://github.com/danieldeer/seriousdb/pull/129)).
- Central logging configuration ([#200](https://github.com/danieldeer/seriousdb/pull/200)).
- Docker image with a multi-stage build ([#49](https://github.com/danieldeer/seriousdb/pull/49), [#86](https://github.com/danieldeer/seriousdb/pull/86)).
- Nix development environment ([#67](https://github.com/danieldeer/seriousdb/pull/67), [#118](https://github.com/danieldeer/seriousdb/pull/118)).
- Documentation for the API, architecture, configuration, persistence, testing and contributing, plus OpenAPI metadata for all endpoints ([#28](https://github.com/danieldeer/seriousdb/pull/28), [#198](https://github.com/danieldeer/seriousdb/pull/198)).
- Unit, API and end-to-end tests, with CI for tests, Ruff linting and `ty` type checking ([#27](https://github.com/danieldeer/seriousdb/pull/27), [#29](https://github.com/danieldeer/seriousdb/pull/29), [#105](https://github.com/danieldeer/seriousdb/pull/105), [#159](https://github.com/danieldeer/seriousdb/pull/159), [#195](https://github.com/danieldeer/seriousdb/pull/195)).

[Unreleased]: https://github.com/danieldeer/seriousdb/compare/0.2.2...HEAD
[0.2.2]: https://github.com/danieldeer/seriousdb/releases/tag/0.2.2
[0.1.0]: https://pypi.org/project/seriousdb/0.1.0/
