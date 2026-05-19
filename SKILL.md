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
  _template/          ← 新增公式的模板
shared/               ← 跨模型共享资源
```

## Intent → Model Routing

| 用户意图 | 对应模型 | 入口文件 |
|---------|---------|---------|
| 分析适配度 / 要不要继续 | bayesian-updating | `models/bayesian-updating/compatibility.md` |
| 评估分手风险 | bayesian-updating | `models/bayesian-updating/breakdown.md` |
| 评估挽回概率 | bayesian-updating | `models/bayesian-updating/reconciliation.md` |
| 直接算一个概率值 | bayesian-updating | `models/bayesian-updating/prompt.md` |

## 添加新公式

1. 复制 `models/_template/` → `models/<new-model-name>/`
2. 填写 `model.md`（公式定义）、`prompt.md`（计算模板）、至少一个 workflow
3. 在本文的 Intent → Model Routing 表中注册路由
4. 新模型自动可用
