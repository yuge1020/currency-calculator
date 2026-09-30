# 汇率计算器 V1.0 AI 开发工作流（Development Workflow）

| 项 | 内容 |
|---|---|
| 文档版本 | V1.0 |
| 状态 | 已确认 |
| 入库日期 | 2026-09-30 |

---

## 1. 角色分工

| 角色 | 承担者 | 职责 |
|---|---|---|
| Product Owner（产品负责人） | **用户** | 需求决策、产品验收、与 ChatGPT 确认需求与规范 |
| Product Technical Lead / Architect / Reviewer | **ChatGPT** | 需求转化、技术设计、Phase 指令下发、代码与技术审查 |
| Implementation Engineer（实现工程师） | **Trey**（本仓库 AI 编程工程师） | 按指令实现代码与文档、自检、提交交付报告 |
| Repository（唯一真实版本源） | **GitHub（yuge1020/currency-calculator）** | 代码、文档、变更历史的唯一权威存放地 |

## 2. 开发流程（每个功能 / Phase 循环执行）

```
需求
 ↓
技术设计
 ↓
Implementation（实现）
 ↓
自动化测试
 ↓
Build
 ↓
自检
 ↓
开发报告
 ↓
ChatGPT 技术审查
 ↓
修复
 ↓
再次验证
 ↓
产品验收（Product Owner）
```

## 3. 各环节要求

| 环节 | 要求 |
|---|---|
| 需求 | 以 `docs/requirements/` 入库文档为唯一依据；不允许依赖聊天记录作为唯一需求来源 |
| 技术设计 | 遵循 ARCHITECTURE-V1.0.md；新决策先更新文档再写代码 |
| Implementation | 遵循 DEVELOPMENT-SPECIFICATION-V1.0.md；只做当前 Phase 范围 |
| 自动化测试 | CalculatorEngine 等核心逻辑必须有单元测试；测试通过才算实现完成 |
| Build | 构建必须成功；不提交构建产物 |
| 自检 | 实现工程师对照 VALIDATION-STANDARD-V1.0.md 逐项自检 |
| 开发报告 | 每个 Phase 结束输出交付报告（做了什么 / 验证结果 / 遗留问题） |
| 技术审查 | ChatGPT 审查代码与报告；审查意见修复后**再次验证**，直到通过 |
| 产品验收 | Product Owner 按 VALIDATION-STANDARD-V1.0.md Level 4 验收 |

## 4. 异常处理规则（红线）

1. **规范冲突**：不要自行选择、不要自行修改——记录冲突点并**暂停相关任务**，上报 Product Owner / ChatGPT 裁决
2. **技术要求无法实现**：不要自行删除需求、不要自行降低标准——报告原因与**替代方案**，等待决策
3. **需求歧义**：一律记录到文档 Open Questions 清单，不得猜测实现

## 5. 阶段推进规则

- 本文档即阶段推进的唯一流程依据；Phase 划分与指令由 ChatGPT 下发
- 每个 Phase 完成后**暂停**，经审查与确认后才进入下一 Phase（例如：Phase 1A 完成后不得自行进入 Phase 1B）
