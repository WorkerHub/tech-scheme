# Changelog

本文件记录《WorkerHub 通用技术方案》的内容变更，最新在上。

## 1.3.0（2026-11-22）

- 定时任务调度改版（§8、§11 与 SKILL 摘要同步）：由「waker 双模式、triggers 配置保留、删 key 自动回 Cron」改为三态物化——仓库提交的 wrangler `crons` 恒为 `[]`、不占官方槽位，项目 cron 表达式声明在仓库 Variable `CRON_SCHEDULE`（逗号分隔、可多条、UTC），部署脚本（`scripts/materialize-secrets.mjs`）在部署前按 `WAKER_SIGNING_KEY` 三态物化进 wrangler.toml：未配置 → 写入表达式走官方 Cron（默认路径，按槽位够用假设）、真值 → waker 接管、`false` → 无调度且不回落 Cron。改调度形态只改 Secret / Variable 后重新部署。

## 1.2.1（2026-11-20）

- 微修：删去 §5A.6 第 4 条的悬空引用「密钥端点与管理面端点形状一致」——管理面升级端点已于 1.2.0 撤除，该半句无所指。

## 1.2.0（2026-11-20）

- 舰队标准入册：新增 §4.4 迁移命名与账本（`schema_vN` 顺序编号、存量已应用名不改、功能名增量双条件坍缩、账本 `_<生效前缀>schema_migrations`）；§9 第 4 条改为部署后自动升级（deploy.yml 经 workers.dev 子域调 `POST /api/setup`、限流退避重试、断言全部迁移名在场、失败 Deploy 变红）；§5A.4 / §5A.6 / §8 / §11 同步撤除手动升级入口（管理面升级按钮与 `POST /api/admin/setup`、setup 页升级表单），管理面只保留数据库状态只读展示与 setup 页开关。

## 1.1.0（2026-11-19）

- 新增 §3D：Looseleaf（`kevintngo/looseleaf`）列为可选风格库——立项规格定版才启用、全项目统一不与 Nova 混搭、中文字体回退方案须在规格中先行写明；§3 的 UI 行同步加指针。默认风格仍为 shadcn/ui Nova，不变。

## 1.1.0（2026-11-26）

- §11 新项目启动清单新增「分支保护」一项：建仓后为默认分支 `main` 配置 GitHub 规则集（要求经 PR 合并、必过状态检查 `Validate`、不勾分支同步要求、Bypass 名单留空），合并前分支 CI 全绿由纪律转为平台机制；SKILL 摘要同步收录。依据 2026-11-26 模板仓 spike 实证（非绿时合并被拦、绿后正常合并），经用户拍板写入。

## 1.0.0（2026-11-15）

- 发布版：由内部提炼稿整理发布；章节编号与提炼稿一致。
