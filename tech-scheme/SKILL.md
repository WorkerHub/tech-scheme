---
name: tech-scheme
description: WorkerHub 通用技术方案：WorkerHub 组织项目的技术栈标准、基础设施铁律（组织级共享配置、共享 D1、TABLE_PREFIX 三态）、鉴权移植、配置与密钥分层、waker 定时任务双模式、部署发版流程与 README 截图规范。新建或改造 WorkerHub 项目、写项目规格、动 CI/部署/D1/鉴权/发版时先读本 Skill；全文在 references/tech-scheme.md，按章节索引查阅。
---

# WorkerHub 通用技术方案（Skill 入口）

本 Skill 是《WorkerHub 通用技术方案》的真源入口。适用范围按属性界定：WorkerHub 组织下、共用共享 D1 的项目。对本方案的偏离须经用户拍板并登记在该项目自己的 spec 中；方案 §10 是存量例外清单，只适用于登记的项目，不要泛化。

用法：先读下面的红线摘要，细节按章节索引查 `references/tech-scheme.md` 对应章节。与单内 spec 的关系：具体工作内容以 spec 为准，但基础设施铁律（§4）与配置密钥分层（§7）不可被 spec 覆盖。

## 红线摘要（细则以全文对应章节为准）

- **仓库必须是公共仓库**（§4.0）。
- **组织级共享配置只引用、不新建**：共用组织级 secret 与 variables 清单，不建仓库级同名项、不硬编码（§4.1）。
- **共享 D1 默认只用一个库**：默认不建新库、共用单一 D1 库，迁移只动本项目表；确需例外经用户拍板并登记（§4.2）。
- **表前缀三态**：变量名只用 `TABLE_PREFIX`（未配置 = 项目默认前缀、配置 = 自定义值、显式置空 = 无前缀；CI 侧用保留字 `none` 表达置空）；生效前缀在初始化前确定、setup 须输出、最终表名不得写死（§4.3）。
- **资源 ID 走 GitHub Secrets**：`D1_DATABASE_ID`、`KV_NAMESPACE_ID`、`CLOUDFLARE_ACCOUNT_ID` 经 `secrets.` 引用，禁用 add-mask 承担 ID 掩码；`D1_DATABASE_NAME` 保持 Variable 明文（§7 红线 7）。
- **鉴权移植现行标准实现**（§5），不逐项目新发明；首次安装与数据库升级按 §5A 的 setup 契约实现。
- **定时任务接 waker 的唯一开关是 `WAKER_SIGNING_KEY` 配没配**（未配走 Cloudflare Cron Trigger、配了 waker 接管），严禁另设 `WAKER_MODE` 之类模式变量；静态周期任务只配 signing key、不用项目令牌（§8）。
- **发版与部署解耦**：手动发版走 tag.yml + release.yml（§9.1）。
- **README 截图规范**：截图存仓内 `.github/assets/imgs/`、WebP 格式、相对路径引用，不用外链图床（§12）。

## 其他定版口径

- **数据库升级随部署自动完成**：迁移命名 `schema_vN` 顺序编号（§4.4）；deploy.yml 在部署后经 workers.dev 子域调 `POST /api/setup` 自动升级、断言迁移名在场、失败 Deploy 变红（§9 第 4 条）；管理面不提供手动升级入口（§5A.4、§5A.6）。
- **邮件发送**：标准实现同时支持 SMTP 与 Resend 两种通道，部署时由用户二选一，凭据按 §7 分层存放。
- **新项目基版**：新项目从组织模板仓库 `project-template` 起步（薄骨架，基础设施条款已内建），不从空仓自搭。

## 章节索引（全文：references/tech-scheme.md）

- §1 一句话方案
- §2 总体架构
- §3 技术栈（已定标准，不再逐项目选型）
- §3A 前端设计质量标准：获奖级 + 自审循环
- §3B 主题切换：亮 / 暗 / 自动三档
- §3C 后台参考实现：shadcn-admin
- §3D 可选风格库：Looseleaf
- §4 基础设施铁律（新项目规格/派单必带，缺一条不算合格规格）
- §5 鉴权方案（移植现行标准实现，不新发明）
- §5A 首次安装、页面分区与版本信息
- §5A.6 setup 页面与数据库升级的接口契约
- §5B 登录表单与密码管理器自动填充规范
- §6 公开 / 私有隔离（共同定版的验收口径）
- §7 配置与密钥分层
- §8 业务共性模式（按需取用，不强塞）
- §9 部署与交付流程
- §9.1 手动发版：tag.yml + release.yml
- §10 例外清单（已知的、只适用于本项目，不要泛化）
- §11 新项目启动清单（写规格时逐项打勾）
- §12 README 与截图规范
