# 汇率计算器 V1.0 工程结构蓝图（Project Structure）

| 项 | 内容 |
|---|---|
| 文档版本 | V1.0 |
| 状态 | **蓝图**——本阶段不创建任何代码目录，此文件仅记录未来 Android 工程的目标结构 |
| 入库日期 | 2026-09-30 |
| 约束 | SDK 已定稿（minSdk 26 / compileSdk 36 / targetSdk 36，2026-09-30，见 ARCHITECTURE-V1.0.md）；applicationId / 最终 package 名称与更细目录由技术架构设计决定，此处以 `<package>` 占位 |

---

## 1. 未来工程目录蓝图

```
currency-calculator/
├── app/
│   ├── build.gradle.kts
│   ├── proguard-rules.pro
│   └── src/
│       ├── main/
│       │   ├── java/<package>/
│       │   │   ├── ui/                # UI 层（Compose）
│       │   │   ├── viewmodel/         # ViewModel 层
│       │   │   ├── domain/            # Domain 层（CalculatorEngine 等）
│       │   │   ├── data/              # Data 层（Repository 实现、本地/远程数据源）
│       │   │   ├── model/             # 数据模型
│       │   │   └── util/              # 工具类
│       │   ├── res/
│       │   │   ├── values/            # 默认语言字符串、颜色、主题
│       │   │   ├── values-<lang>/     # 各语言字符串（i18n）
│       │   │   ├── drawable/          # 矢量图标（含国旗图标）
│       │   │   └── mipmap/            # 应用图标
│       │   └── AndroidManifest.xml
│       ├── test/                      # JVM 单元测试（CalculatorEngine 等）
│       └── androidTest/               # 设备/模拟器测试（Compose UI 测试等）
├── docs/                              # 本项目全部正式文档（已建立）
│   ├── requirements/
│   ├── architecture/
│   ├── development/
│   ├── validation/
│   └── release/
├── README.md
└── .gitignore                         # 随工程初始化一并建立
```

## 2. 层级职责划分

| 归属 | 位置 | 允许内容 | 禁止内容 |
|---|---|---|---|
| **UI** | `ui/` | Composable、主题、图标渲染、用户事件上报 | 业务逻辑、直接访问数据源、金额计算 |
| **ViewModel** | `viewmodel/` | State（StateFlow）、事件处理、协调 Domain/Data | Android UI 依赖、Compose 依赖 |
| **Domain** | `domain/` | CalculatorEngine、纯业务规则 | 依赖 Android Framework、依赖网络/存储实现 |
| **Data** | `data/` | Repository 实现、本地缓存、网络客户端 | 出现在 UI 层可访问的路径 |
| **Repository** | `data/`（接口可在 `domain/` 定义） | 数据访问唯一收口、缓存回落策略 | 绕过接口直连数据源 |
| **Model** | `model/` | Currency / ExchangeRate 等实体 | 混入展示逻辑（国旗映射独立配置） |
| **Utility** | `util/` | 通用工具（时间格式化等） | 业务规则（业务规则一律进 Domain） |
| **Test** | `test/`、`androidTest/` | 单元测试、UI 测试 | — |

## 3. 建立时机

- 本蓝图对应的所有代码目录与文件，在**工程初始化阶段（后续 Phase）**一次性建立
- 届时必须同步建立 `.gitignore`（排除 `build/`、`local.properties`、IDE 文件等）
- 实际结构与本蓝图发生偏差时，须先更新本蓝图并记录变更原因，再动代码
