# 汇率计算器 V1.0 Google Play 发布要求（Release Requirements）

| 项 | 内容 |
|---|---|
| 文档版本 | V1.0 |
| 状态 | 基线 + 动态核对机制（见 §1 政策时效性规则） |
| 入库日期 | 2026-09-30 |
| 政策核查记录 | 见 §13 |

---

## 1. 政策时效性规则（红线）

Google Play 政策与目标 API 要求**随时间变化**。本文件不写死未经验证的旧信息：

1. 涉及版本号 / 政策数值的内容，必须**以 Google 官方文档为准**，并注明来源与核查日期
2. 每次提交 Play 前（TC-028），必须**按当日官方文档重新核对** §13 清单
3. 发现官方要求变化时：先更新本文档并记录，再执行发布

## 2. SDK 版本要求

| 项 | 要求 |
|---|---|
| Target SDK | 不得低于 Google Play 当期要求。**2026-09-30 核查**：自 2026-08-31 起，新应用与应用更新必须 target Android 16（API level 36）或更高（手机/平板）方可提交 Google Play |
| Compile SDK | 建议与当期最新稳定 SDK 对齐（随 OQ-13 由技术审查定稿） |
| 来源 | Google 官方：[Meet Google Play's target API level requirements](https://developer.android.com/google/play/requirements/target-sdk) |

## 3. 构建与签名

| 项 | 要求 |
|---|---|
| 交付格式 | Android App Bundle（**AAB**），Play 不接受新应用 APK 直传 |
| Release signing | 使用 Play App Signing；上传密钥与签名密钥分离；**密钥与 keystore 严禁入库** |
| R8 / resource shrinking | Release 构建开启代码混淆与资源压缩；混淆后必须通过 TC-026 全功能回归 |
| Versioning | 语义化版本 + 递增 versionCode；版本命名与 tag 规则随发布阶段定稿 |

## 4. 合规与政策材料

| 项 | 要求 |
|---|---|
| Privacy Policy | 必须提供可公开访问的隐私政策 URL（内容与托管方式待 OQ-07）；Play 商店与 App 内（更多信息页）都要可达 |
| Data Safety | 在 Play Console 如实填写数据安全表单（收集哪些数据、是否共享）；必须与实际 SDK 行为一致（Analytics / Push 供应商定稿后更新） |
| Content Rating | 完成 IARC 内容分级问卷 |
| Target Audience | 如实申报目标年龄段；涉及受众选择与政策联动，以 Play Console 当期表单为准 |
| App access | 如需凭据访问应用功能必须提供演示凭据；本应用无账号体系，应申报「全部功能无需凭据」 |
| Permissions | 最小权限（V1 预期仅 `INTERNET`）；申报用途与实际一致，禁止提权后改变用途 |
| Third-party SDK compliance | 第三方 SDK（网络 / Push / Analytics）须核对各自的数据收集与合规要求，并同步到 Data Safety 声明 |

## 5. 测试与发布轨道

| 轨道 | 用途 |
|---|---|
| Internal testing | 快速冒烟（TC-026 / TC-027） |
| Closed testing | 外部小范围验收；新个人开发者账号需满足 Play 当期的封闭测试要求（人数与时长**以官方文档当日核对为准**）后才能申请 Production |
| Production | 正式发布；发布前执行 TC-028 全清单核对 |

## 6. 商店素材（Store Listing）

| 项 | 要求 |
|---|---|
| App icon | 512×512 PNG；与 App 内图标一致 |
| Screenshots | 至少手机截图；展示计算器主界面（真实 UI，与已确认 UI 规范一致）；规格以 Play Console 当期要求为准 |
| Feature graphic | 1024×500 横幅 |
| 文案 | 应用名称 / 简介与已确认的产品名称、六种首发语言对齐，不得出现未实现的功能宣传 |

## 7. 质量与稳定性

| 项 | 要求 |
|---|---|
| Crash / ANR | 关注 Play Console Android Vitals；发布基线：无 P0 崩溃，ANR 率满足 Play 当期政策阈值（以官方文档当日核对为准） |
| Play policy compliance | 遵守 [Play Policy Center](https://play.google.com/about/play-policies/) 全部适用政策 |

## 8. 发布前检查清单（对应 TC-028）

- [ ] Target SDK 满足当日官方要求（§2）
- [ ] AAB 构建成功并通过内部 / 封闭轨道验证
- [ ] R8 后全功能回归通过（TC-026）
- [ ] 隐私政策 URL 可访问且内容与实际一致
- [ ] Data Safety 表单与实际数据行为一致
- [ ] 内容分级、目标受众、App access 申报完成
- [ ] 权限申报与 Manifest 一致
- [ ] 第三方 SDK 合规核对完成
- [ ] 商店素材齐全且与真实 UI 一致
- [ ] Crash / ANR 基线达标

## 9. 官方文档索引（每次发布前逐项核对）

| 主题 | 官方来源 |
|---|---|
| Target API 要求 | https://developer.android.com/google/play/requirements/target-sdk |
| Play 政策中心 | https://play.google.com/about/play-policies/ |
| Play Console 帮助（Data Safety / 分级 / 轨道等） | https://support.google.com/googleplay/android-developer |
| 应用质量 / Vitals | https://developer.android.com/topic/quality |

## 10. 政策核查记录

| 日期 | 核查项 | 结论 | 来源 |
|---|---|---|---|
| 2026-09-30 | Target API Level | 新应用/更新须 target API 36+（Android 16），自 2026-08-31 生效 | developer.android.com/google/play/requirements/target-sdk |
