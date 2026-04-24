# 代码质量审阅者提示模板

派发代码质量审阅子代理时使用此模板。

**目的：** 验证实现质量良好（清晰、经过测试、可维护）。

**只有规格符合性审阅通过后才派发。**

```
Task tool (superpowers:code-reviewer):
  使用 requesting-code-review/code-reviewer.md 中的模板

  WHAT_WAS_IMPLEMENTED: [来自实现者报告]
  PLAN_OR_REQUIREMENTS: [plan-file] 中的任务 N
  BASE_SHA: [任务前的提交]
  HEAD_SHA: [当前提交]
  DESCRIPTION: [任务摘要]
```

**除标准代码质量问题外，审阅者还应检查：**
- 每个文件是否只有一个清晰职责，并提供明确接口？
- 单元是否已拆分到能独立理解和测试？
- 实现是否遵循计划中的文件结构？
- 本次实现是否创建了已经很大的新文件，或显著增大了现有文件？（不要标记原本就存在的文件大小问题，聚焦本次变更带来的影响。）

**代码审阅者返回：** 优点、问题（Critical/Important/Minor）、评估
