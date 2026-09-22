# HANDOFF

> 三家接力（DSH / Codex / Claude Code）用这个文件：**开工先读，收工更新并提交。**
> 上游工作区约定见 `~/dev/_shared/CONVENTIONS.md`。本文件用中文（内部接力）；
> `AGENTS.md` 用英文，与这个仓库的公开文档保持一致。

## 当前写者

- 工具：（空 —— 2026-09-23 02:10 DSH 会话收工）
- 分支：main
- 开始时间：—

> 一个仓库同一时刻只允许一个写者；下一位开工时把上一行改成自己。

## 当前状态（2026-09-23 核对）

- 两个产物各自发版，互不影响：
  - **CLI**：`UPDATER_VERSION=0.2.0`（`dsh-update-plugin.sh`），标签 `v*` 走 `.github/workflows/release.yml`
  - **npm 插件**：`plugin/package.json` = `0.3.1`，标签 `plugin-v*` 走 `.github/workflows/publish-plugin.yml`
- 远端 CI：`Upstream check` 计划任务在跑且成功；`CI`（ubuntu + macos）配置在案
- 本地可复现的验证：`make lint`、`make test`（mocks，不碰真实 DSH）、`make plugin-test`（`node --test`）
- 工作区干净，`main` 与 `origin/main` 一致
- 2026-09-23：补 `AGENTS.md`（英文项目指令）、本文件（中文接力）、`.github/workflows/conventions.yml`（工作区公约的远端闸门：本地钩子能被 `--no-verify` 绕过，CI 不能）

## 下一步

- [ ] awesome-dsh-plugin 提交：`docs/awesome-submit.md` 里流程与材料都已备好（仓库年龄门槛早已满足），**仓库里看不到「已提交」的痕迹**，需要人确认后推进（`docs/awesome-pr-body.md`、`docs/awesome-dsh-plugin-entry.yml` 可直接用）
- [ ] 若继续发 CLI 版本：先在 `CHANGELOG.md` 写 `## [x.y.z]` 小节，再 `make release VERSION=x.y.z`（脚本会自己跑测试）

## 未决问题

- CLI 版本（0.2.0）与 npm 插件版本（0.3.1）要不要对齐一次，还是保持各自独立发版？（当前两条 tag 通道独立，倾向保持独立）
- `docs/announcement.md` 的公告是否还要发、发到哪里，仓库里没有记录
