# 汇率计算器（Currency Calculator）

一款 Android 汇率换算计算器 App：一页式设计，打开即进入计算器，核心换算完全离线可用，网络仅用于更新汇率。

## 核心功能

- 一页式计算器：首屏直接进入计算器，输入即时换算
- 多币种转换：15 种首发货币（ISO 4217），支持 ⇅ 交换与货币选择
- 金额计算：5×4 键盘，四则运算、百分比、C / 删除 / 长按删除
- 汇率数据：离线计算 + 本地汇率缓存 + 在线更新（底部 ↻ 刷新、显示更新时间与当前汇率）
- 更多信息：••• 入口（关于、隐私政策、用户协议、用户反馈、Google Play 官方评价）
- 首次启动引导、六种首发语言（简体中文、English、日本語、Deutsch、Français、Español）、跟随系统浅色 / 深色模式、Push、Analytics
- 无账号体系

## 技术栈

- Android · Kotlin · Jetpack Compose
- 架构：单向数据流 + Repository 模式 + Offline First（详见 [ARCHITECTURE-V1.0.md](docs/architecture/ARCHITECTURE-V1.0.md)）

## 当前开发阶段

> **Phase 1A — Documentation & Architecture Blueprint**
>
> 当前仅完成项目规范、需求文档与工程蓝图。**尚未开始 Android 功能开发**（不存在 app/ 与 Gradle 工程）。

## 文档入口

| 类别 | 文档 |
|---|---|
| 产品需求 | [PRD-V1.0.md](docs/requirements/PRD-V1.0.md) |
| UI 规范 | [UI-SPECIFICATION-V1.0.md](docs/requirements/UI-SPECIFICATION-V1.0.md) |
| 货币规范 | [CURRENCY-SPECIFICATION-V1.0.md](docs/requirements/CURRENCY-SPECIFICATION-V1.0.md) |
| 技术架构 | [ARCHITECTURE-V1.0.md](docs/architecture/ARCHITECTURE-V1.0.md) |
| 工程结构蓝图 | [PROJECT-STRUCTURE-V1.0.md](docs/architecture/PROJECT-STRUCTURE-V1.0.md) |
| 开发规范 | [DEVELOPMENT-SPECIFICATION-V1.0.md](docs/development/DEVELOPMENT-SPECIFICATION-V1.0.md) |
| 开发工作流 | [DEVELOPMENT-WORKFLOW-V1.0.md](docs/development/DEVELOPMENT-WORKFLOW-V1.0.md) |
| 验证标准 | [VALIDATION-STANDARD-V1.0.md](docs/validation/VALIDATION-STANDARD-V1.0.md) |
| 测试用例矩阵 | [TEST-CASE-MATRIX-V1.0.md](docs/validation/TEST-CASE-MATRIX-V1.0.md) |
| Google Play 发布要求 | [GOOGLE-PLAY-RELEASE-REQUIREMENTS-V1.0.md](docs/release/GOOGLE-PLAY-RELEASE-REQUIREMENTS-V1.0.md) |

## GitHub 项目说明

- 本仓库是本项目的**唯一真实版本源（Single Source of Truth）**：代码、规范文档与变更历史均以此为准
- 协作模式：Product Owner（用户）确认需求 → ChatGPT 担任技术负责人 / 审查 → Trey（AI 实现工程师）编码与文档同步 → GitHub 存档
- 所有开发以 `docs/` 中的正式规范为依据，工作流见 [DEVELOPMENT-WORKFLOW-V1.0.md](docs/development/DEVELOPMENT-WORKFLOW-V1.0.md)
- 规范文档变更必须留痕：修改需说明变更原因，作为产品升级 / 改动 / 变更的追溯记录
