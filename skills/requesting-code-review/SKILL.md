---
name: requesting-code-review
description: 完成任务、实现重大功能或合并前，需要验证工作是否满足要求时使用
---

# 请求代码审阅

派发 superpowers:code-reviewer 子代理，在问题扩散前捕获它们。审阅者只获得精心构造的评估上下文，绝不获得你的会话历史。这样审阅者会聚焦工作产物，而不是你的思考过程，同时保留你自己的上下文以继续工作。

**核心原则：** 尽早审阅，经常审阅。

## 何时请求审阅

**强制：**
- 子代理驱动开发中的每个任务之后
- 完成重大功能之后
- 合并到 main 之前

**可选但有价值：**
- 卡住时（获得新视角）
- 重构前（基线检查）
- 修复复杂 bug 后

## 如何请求

**1. 获取 git SHA：**
```bash
BASE_SHA=$(git rev-parse HEAD~1)  # or origin/main
HEAD_SHA=$(git rev-parse HEAD)
```

**2. 派发 code-reviewer 子代理：**

使用 Task 工具和 superpowers:code-reviewer 类型，填写 `code-reviewer.md` 模板。

**占位符：**
- `{WHAT_WAS_IMPLEMENTED}` - 刚构建的内容
- `{PLAN_OR_REQUIREMENTS}` - 它应该做什么
- `{BASE_SHA}` - 起始提交
- `{HEAD_SHA}` - 结束提交
- `{DESCRIPTION}` - 简短摘要

**3. 处理反馈：**
- 立即修复 Critical 问题
- 继续前修复 Important 问题
- 记录 Minor 问题供之后处理
- 如果审阅者错了，用理由反驳

## 示例

```
[刚完成任务 2：添加验证函数]

你：继续前我先请求代码审阅。

BASE_SHA=$(git log --oneline | grep "Task 1" | head -1 | awk '{print $1}')
HEAD_SHA=$(git rev-parse HEAD)

[派发 superpowers:code-reviewer 子代理]
  WHAT_WAS_IMPLEMENTED: conversation index 的验证和修复函数
  PLAN_OR_REQUIREMENTS: docs/superpowers/plans/deployment-plan.md 中的任务 2
  BASE_SHA: a7981ec
  HEAD_SHA: 3df7661
  DESCRIPTION: Added verifyIndex() and repairIndex() with 4 issue types

[子代理返回]:
  Strengths: 架构清晰，测试真实
  Issues:
    Important: 缺少进度指示器
    Minor: 报告间隔使用魔法数字 (100)
  Assessment: 可以继续

你：[修复进度指示器]
[继续任务 3]
```

## 与工作流集成

**子代理驱动开发：**
- 每个任务后审阅
- 在问题叠加前捕获
- 进入下个任务前修复

**执行计划：**
- 每批后审阅（3 个任务）
- 获取反馈、应用、继续

**临时开发：**
- 合并前审阅
- 卡住时审阅

## 危险信号

**绝不要：**
- 因为“很简单”而跳过审阅
- 忽略 Critical 问题
- 带着未修复的 Important 问题继续
- 争辩有效的技术反馈

**如果审阅者错了：**
- 用技术理由反驳
- 展示证明其可工作的代码/测试
- 请求澄清

模板见：requesting-code-review/code-reviewer.md
