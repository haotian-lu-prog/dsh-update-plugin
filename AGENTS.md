# AGENTS.md — dsh-update-plugin

Guidance for coding agents (and humans) working in this repository. It covers only
project-specific facts; the shared workspace rules live in `~/dev/_shared/CONVENTIONS.md`.

One repository ships **two artifacts under one name**:

| Artifact | Source | Release channel |
| --- | --- | --- |
| CLI `dsh-update-plugin` | `dsh-update-plugin.sh`, `install.sh`, `Formula/dsh-update-plugin.rb` | tag `v*` → `.github/workflows/release.yml`; version = `UPDATER_VERSION` in `dsh-update-plugin.sh` (currently `0.2.0`) |
| DSH Web plugin (npm `dsh-update-plugin`) | `plugin/` | tag `plugin-v*` → `.github/workflows/publish-plugin.yml` (validates the tag against the package version); version = `plugin/package.json` (currently `0.3.1`) |

The two version numbers are independent — do not "fix" one to match the other.

## Commands

```bash
make help                    # list every target
make test                    # bash test suite (mocks npm/dsh/pnpm in a temp dir)
make plugin-test             # plugin unit tests: node --test plugin/test/*.test.mjs
make lint                    # bash -n + node --check + ruby -c formula + shellcheck (if installed)
make format                  # shfmt the shell scripts (if installed)
make target-version          # print the newest @deepseek-ai/dsh version
make release VERSION=x.y.z   # CLI release; requires a "## [x.y.z]" section in CHANGELOG.md
```

## Layout

- `dsh-update-plugin.sh` — the CLI (single file, bash)
- `install.sh`, `Formula/dsh-update-plugin.rb` — install paths for the CLI and the Homebrew tap
- `plugin/` — the npm-packaged DSH Web plugin: `lib/` (host + client), `test/`, `assets/`, `cordis.patch.yml`
- `tests/run-tests.sh` — mock-based CLI test suite
- `scripts/release.sh` — CLI release helper (bumps `UPDATER_VERSION`, commits, tags, pushes)
- `docs/` — announcement and awesome-dsh-plugin submission material
- `.github/workflows/` — `ci.yml` (tests on ubuntu + macos), `release.yml` (`v*`), `publish-plugin.yml` (`plugin-v*`), `upstream-check.yml` (scheduled), `conventions.yml` (workspace gate)

## Ground rules

- **Tests must never touch a real DSH installation.** Every external command
  (`npm`, `dsh`, `pnpm`) is replaced by a mock in a temp dir — keep it that way.
- Shell code must run on **bash 3.2** (macOS ships it): no `mapfile`, no
  associative arrays, no `${var,,}`.
- When a `.zh-CN.md` twin exists, update both sides in the same commit
  (`README.md`/`README.zh-CN.md`, `plugin/README.md`/`plugin/README.zh-CN.md`).
- Bump versions only through the release paths above (CLI: `make release`;
  plugin: set `plugin/package.json` + tag `plugin-v*`), each with its changelog entry.
- Never commit `node_modules/`, build output, `.env*`, or secrets.
- Self-contained by design: the CLI has no runtime dependencies, and the plugin
  ships only `lib/`, `cordis.patch.yml`, docs and assets (see `plugin/package.json` `files`).
- Read `HANDOFF.md` before starting and update it (state / next / open questions)
  before you stop.
