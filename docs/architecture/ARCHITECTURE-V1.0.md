# 汇率计算器 V1.0 技术架构（Architecture）

| 项 | 内容 |
|---|---|
| 文档版本 | V1.0 |
| 状态 | 已确认基线（具体技术选型标注处须经 ChatGPT 技术审查后定稿） |
| 入库日期 | 2026-09-30 |
| 上游依据 | PRD-V1.0.md |

---

## 1. 技术栈

| 层面 | 选型 | 说明 |
|---|---|---|
| 平台 | Android | — |
| 语言 | Kotlin | 唯一开发语言 |
| UI | Jetpack Compose | 声明式 UI，不使用 XML 布局开发新界面 |
| API 策略 | 官方 Android / Jetpack API 优先 | 引入任何第三方库必须给出理由并通过技术审查 |
| min / target SDK | **待确认**（OQ-13） | target SDK 不得低于 Google Play 当前要求（见 GOOGLE-PLAY-RELEASE-REQUIREMENTS-V1.0.md，2026-09-30 核查：新应用/更新须 target API 36+） |

## 2. 核心原则

### 2.1 Offline First（最高优先级）

1. **启动时绝不能等待网络**：App 冷启动直接进入计算器界面，网络状态不阻塞首屏
2. **网络只负责更新汇率**：联网是「锦上添花」，不是功能前提
3. **计算器核心功能必须可以完全离线工作**：输入、运算、⇅ 交换、即时换算（基于缓存汇率）在飞行模式下全部可用
4. 汇率缓存持久化在本地；无网络时展示缓存汇率与对应更新时间

### 2.2 单向数据流（UDF）

```
UI (Compose) ──事件──▶ ViewModel ──调用──▶ Domain / Repository
     ▲                                          │
     └──────────────State (StateFlow)───────────┘
```

- UI 只渲染 State、只上报事件；不在 UI 层写业务逻辑
- State 用 StateFlow 暴露；单一数据源（Single Source of Truth）

### 2.3 UI 与数据层分离 + Repository 模式

- UI 层不直接访问数据源（网络 / 缓存）
- 所有数据访问经 Repository 收口

## 3. 架构分层与核心组件

| 组件 | 层 | 职责 |
|---|---|---|
| CalculatorScreen / Keyboard 等 Composable | UI | 渲染 5×4 计算器、货币行、底部信息栏 |
| CalculatorViewModel | ViewModel | 持有计算器 State（StateFlow），接收按键 / 交换 / 选择货币事件 |
| **CalculatorEngine** | Domain | 纯 Kotlin 计算引擎：四则、%、C/⌫/长按删除、精度与小数位规则；**不依赖 Android**，可独立单测 |
| **ExchangeRateRepository** | Data | 汇率的获取 / 缓存 / 更新时间维护；网络失败时回落缓存 |
| **CurrencyRepository** | Data | 15 种首发货币清单、小数位、货币扩展配置 |
| **Preferences / Settings Repository** | Data | 用户偏好（如已选货币对等）；无账号体系 |
| 本地缓存 | Data | 汇率持久化（具体存储方案：DataStore / Room 等**待技术审查确认**，OQ-14） |
| 网络层 | Data | 汇率 API 客户端（**汇率数据源选型待确认**，OQ-15） |

> 汇率 API 未确认前，网络层只允许出现接口定义与可替换实现，不得把具体供应商写死在业务代码中。

## 4. 关键数据流

### 4.1 即时换算
```
按键事件 → ViewModel → CalculatorEngine 计算金额
        → ViewModel 读取 ExchangeRateRepository 缓存汇率
        → 合成 UI State（输入货币金额 + 目标货币换算结果）→ UI 渲染
```

### 4.2 汇率刷新
```
↻ 点击 / App 进前台触发（触发时机待确认 OQ-16）
→ ExchangeRateRepository 请求网络
→ 成功：写缓存 + 更新「汇率更新时间」；失败：保留缓存 + 展示失败提示（文案走字符串资源）
```

## 5. 可扩展性原则

- 架构支持未来增加货币、增加页面，但 **V1 不过度工程化**：
  - 不引入多模块拆分（单模块 `app` 起步，包结构分层）
  - 不引入 V1 用不到的抽象层
  - 依赖注入框架等基建选型**待技术审查确认**（OQ-17）

## 6. 禁止事项

- ❌ 启动路径上出现任何同步网络调用
- ❌ 金额计算使用 Float / Double（必须使用 Decimal 语义的实现，详见 CURRENCY-SPECIFICATION-V1.0.md）
- ❌ 货币清单 / 汇率写死在 Composable 中
- ❌ 未经审查引入第三方 SDK（网络 / Push / Analytics 的选型必须走审查流程）

## 7. 待确认事项

| 编号 | 事项 | 状态 |
|---|---|---|
| OQ-13 | minSdk / targetSdk / compileSdk 定稿 | 待确认 |
| OQ-14 | 本地缓存方案（DataStore / Room 等） | 待确认 |
| OQ-15 | 汇率数据源（API）选型 | 待确认 |
| OQ-16 | 汇率自动更新的触发时机（仅手动 / 进前台 / 定时） | 待确认 |
| OQ-17 | 依赖注入方案 | 待确认 |
| OQ-18 | 首次启动且无缓存汇率时的兜底策略 | 待确认（关联 PRD OQ-11） |
