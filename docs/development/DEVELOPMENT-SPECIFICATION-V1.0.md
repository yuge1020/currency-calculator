# 汇率计算器 V1.0 开发规范（Development Specification）

| 项 | 内容 |
|---|---|
| 文档版本 | V1.0 |
| 状态 | 已确认基线 |
| 入库日期 | 2026-09-30 |
| 适用范围 | 本仓库全部代码与文档工作 |

---

## 1. Kotlin 编码规范

- 遵循 [Kotlin 官方编码规范](https://kotlinlang.org/docs/coding-conventions.html)
- 静态检查工具（ktlint / detekt 等）随工程初始化引入并纳入检查流程（工具选型随 OQ-17 一并确认）
- 禁止未使用的导入、变量与死代码；提交前必须通过编译与既有测试

## 2. Android 项目规范

- 单模块 `app` 起步，包结构遵循 PROJECT-STRUCTURE-V1.0.md
- 资源命名：`snake_case`（如 `ic_refresh`、`label_calculator`）
- 版本目录（libs.versions.toml）管理依赖版本，禁止散落的魔法版本号
- min / target / compile SDK 以 OQ-13 定稿为准，定稿后不得随意变更

## 3. Jetpack Compose 规范

- Composable 命名：名词 PascalCase（`CalculatorKeyboard`），非 Composable 函数 camelCase
- **无状态优先**：尽量设计 Stateless Composable，状态由上层 hoisting 提供
- 预览（@Preview）覆盖关键状态（浅色 / 深色、多语言）
- UI 层禁止：业务逻辑、金额计算、直接访问 Repository

## 4. 状态管理

- 单向数据流：事件向上、State 向下（见 ARCHITECTURE-V1.0.md §2.2）
- 对外暴露 `StateFlow`，禁止对外暴露 MutableStateFlow
- State 是唯一事实来源；派生状态在 ViewModel 计算，不在 UI 重复计算

## 5. 命名规范

| 对象 | 规则 | 示例 |
|---|---|---|
| 类 / 接口 | UpperCamelCase | `ExchangeRateRepository` |
| 函数 / 变量 | lowerCamelCase | `convertAmount()` |
| 常量 | SCREAMING_SNAKE_CASE | `DEFAULT_DECIMAL_DIGITS` |
| 包名 | 全小写、单词直连 | `com.<domain>.calculator.domain` |
| 字符串资源 | `<类别>_<页面/作用>_<含义>` | `label_bottom_rate_line` |
| Git 分支 | `<type>/<简述>` | `feat/instant-conversion` |

## 6. 包结构规范

按 PROJECT-STRUCTURE-V1.0.md §1/§2 执行；层级职责（UI / ViewModel / Domain / Data / Repository / Model / Utility / Test）不得混用。

## 7. 字符串资源与国际化规范（红线）

- **不允许硬编码任何产品文案**：所有用户可见文字（含按钮、提示、无障碍描述）必须来自 `strings.xml`
- 六种首发语言（清单待确认 OQ-01）各建 `values-<lang>/strings.xml`
- 默认 `values/` 使用基准语言；翻译以 Product Owner 提供的内容为准，不得机翻自造
- 金额、时间格式化使用系统本地化能力（`NumberFormat` / `DateTimeFormatter` + ISO 4217 小数位）

## 8. 权限规范（最小权限原则）

- 仅申请功能确需的权限；V1 预期只有 `INTERNET`
- 任何新增权限必须在 PRD 记录用途并通过技术审查
- 运行时权限必须优雅处理拒绝场景（对应 TC-025）

## 9. 依赖规范（最少第三方依赖）

- 官方 Android / Jetpack API 优先
- 每个第三方依赖必须：有明确不可替代的理由、经 ChatGPT 技术审查、记录在架构文档
- 禁止引入「可能用得上」的依赖

## 10. 错误处理

- 业务错误用类型化结果（sealed Result）显式传递，禁止吞异常
- 用户可见的错误信息必须走字符串资源（如「汇率更新失败，当前使用缓存汇率」类文案以 PO 确认稿为准）
- 网络异常（超时、无网、服务端错误）分类处理，任何失败**不得阻塞计算器使用**

## 11. 日志

- 仅使用统一日志工具；Release 构建关闭 verbose/info 输出
- **禁止**在日志输出用户输入金额以外的敏感信息；禁止输出完整汇率响应体等大文本

## 12. 网络异常与数据缓存

- 所有网络请求设置超时；失败按指数退避重试（次数上限随实现确认）
- 汇率缓存持久化，附带更新时间；界面永远可展示「缓存汇率 + 更新时间」
- 网络层只服务汇率更新（Offline First 红线）

## 13. Git 规范

### 13.1 Commit message

格式：`<type>(<scope>): <subject>`

| type | 用途 |
|---|---|
| feat | 新功能 |
| fix | 缺陷修复 |
| docs | 文档 |
| refactor | 重构（不改行为） |
| test | 测试 |
| chore | 构建 / 工具 / 杂项 |

- subject 用英文小写祈使句，不加句号；正文（可选）说明动机
- 示例：`feat(calculator): implement instant currency conversion`

### 13.2 分支与提交纪律

- 按阶段 / 功能切分支，功能完成后合并回 `main`
- 不允许提交：`local.properties`、`build/` 等构建产物、IDE 个人配置、任何 API Key / Secret
- 历史文档一经确认，修改必须留痕（变更记录），不允许无说明覆盖

## 14. 分阶段开发纪律（红线）

1. 严格按照 ChatGPT 下发的 Phase 指令执行，**一次只做当前 Phase**
2. **不允许 AI 自行扩大需求**：发现「顺手可做」的改进，只能记录到待确认清单，不得实现
3. 不允许自行修改 PRD / UI 已确认内容
4. 每个 Phase 结束输出交付报告，经审查通过后才进入下一阶段
