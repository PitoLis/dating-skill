---
name: bayesian-dating
description: >
  Lip贝叶斯Dating模型 — 用概率论量化恋爱关系走向。基于贝叶斯更新与逻辑回归，
  通过观察到的互动事件动态更新后验概率。支持模型扩展，可新增其他公式模块。
  Triggers: "dating", "situationship", "感情分析", "恋爱概率", "适配度",
  "要不要继续", "该不该分手", "能挽回吗", "贝叶斯dating", "帮我分析一下".
---

# Lip贝叶斯Dating模型

## 模型架构

```
SKILL.md              ← 入口：意图识别 → 公式选择
models/               ← 公式模块（可扩展）
  bayesian-updating/  ← [当前] 贝叶斯概率更新模型
    compatibility.md          ← 简易模式：大/中/小
    compatibility-advanced.md ← 高级模式：27 信号 + EV
    breakdown.md              ← 破裂风险评估
    reconciliation.md         ← 挽回概率评估
  _template/          ← 新增公式的模板
shared/               ← 跨模型共享资源
  ethical-boundaries.md ← 伦理边界（所有模型引用）
  signal-catalog.md     ← 27 项默认信号参数表
  weight-reference.md   ← 通用权重参考
```

## Intent → Model Routing

| 用户意图 | 对应模型 | 入口文件 |
|---------|---------|---------|
| 分析适配度 / 要不要继续 | bayesian-updating | `models/bayesian-updating/compatibility.md` |
| 精细化适配分析（含 EV、自定义参数） | bayesian-updating | `models/bayesian-updating/compatibility-advanced.md` |
| 评估分手风险 | bayesian-updating | `models/bayesian-updating/breakdown.md` |
| 评估挽回概率 | bayesian-updating | `models/bayesian-updating/reconciliation.md` |
| 直接算一个概率值 | bayesian-updating | `models/bayesian-updating/prompt.md` |

### 路由判断逻辑

1. 用户提到"默认参数""27 个信号""EV""期望价值""自定义 BF"等关键词 → `compatibility-advanced.md`
2. 用户用大/中/小描述或只说"帮我分析适配度" → `compatibility.md`
3. 用户不确定时，先走简易模式，过程中根据用户反馈再决定是否切换

## 添加新公式

1. 复制 `models/_template/` → `models/<new-model-name>/`
2. 填写 `model.md`（公式定义）、`prompt.md`（计算模板）、至少一个 workflow
3. 在本文的 Intent → Model Routing 表中注册路由
4. 新模型自动可用
