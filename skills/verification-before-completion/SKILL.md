---
name: verification-before-completion
description: 准备声称工作已完成、已修复或已通过时使用，尤其是在提交或创建 PR 前；要求先运行验证命令并确认输出，先有证据再下结论
---

# 完成前验证

## 概览

未经验证就声称工作完成，是不诚实，不是高效。

**核心原则：** 永远先有证据，再下结论。

**违反这条规则的字面要求，就是违反其精神。**

## 铁律

```
NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE
```

如果你没有在当前消息中运行验证命令，就不能声称它通过。

## 门禁函数

```
在声称任何状态或表达满意之前：

1. 识别：什么命令能证明这个说法？
2. 运行：执行完整命令（新鲜、完整）
3. 阅读：完整输出，检查退出码，统计失败数
4. 验证：输出是否确认这个说法？
   - 如果否：带证据说明实际状态
   - 如果是：带证据给出结论
5. 只有此时：才可以下结论

跳过任一步 = 撒谎，不是验证
```

## 常见失败

| 声称 | 需要 | 不足以证明 |
|-------|----------|----------------|
| 测试通过 | 测试命令输出：0 failures | 之前的运行、“应该通过” |
| Linter 干净 | Linter 输出：0 errors | 部分检查、推断 |
| 构建成功 | 构建命令：exit 0 | Linter 通过、日志看起来不错 |
| Bug 已修复 | 原始症状测试通过 | 代码已改、假设已修复 |
| 回归测试有效 | 已验证 red-green 循环 | 测试通过一次 |
| 代理已完成 | VCS diff 显示变更 | 代理报告“success” |
| 需求已满足 | 逐行清单检查 | 测试通过 |

## 危险信号 - 停下

- 使用“应该”、“可能”、“看起来”
- 验证前表达满意（“Great!”、“Perfect!”、“Done!” 等）
- 未验证就准备 commit/push/PR
- 相信代理成功报告
- 依赖部分验证
- 想着“就这一次”
- 疲惫并想尽快结束
- **任何在未运行验证时暗示成功的措辞**

## 防止合理化

| 借口 | 现实 |
|--------|---------|
| “现在应该能工作” | 运行验证 |
| “我有信心” | 信心 ≠ 证据 |
| “就这一次” | 没有例外 |
| “Linter 通过了” | Linter ≠ 编译器 |
| “代理说成功了” | 独立验证 |
| “我累了” | 疲惫不是借口 |
| “部分检查够了” | 部分验证什么都证明不了 |
| “我换个说法就不适用” | 精神高于字面 |

## 关键模式

**测试：**
```
✅ [Run test command] [See: 34/34 pass] "All tests pass"
❌ "Should pass now" / "Looks correct"
```

**回归测试（TDD Red-Green）：**
```
✅ Write → Run (pass) → Revert fix → Run (MUST FAIL) → Restore → Run (pass)
❌ "I've written a regression test" (without red-green verification)
```

**构建：**
```
✅ [Run build] [See: exit 0] "Build passes"
❌ "Linter passed" (linter doesn't check compilation)
```

**需求：**
```
✅ Re-read plan → Create checklist → Verify each → Report gaps or completion
❌ "Tests pass, phase complete"
```

**代理委派：**
```
✅ Agent reports success → Check VCS diff → Verify changes → Report actual state
❌ Trust agent report
```

## 为什么重要

来自 24 条失败记忆：
- 用户说过“我不相信你” - 信任破裂
- 未定义函数被交付 - 会崩溃
- 缺失需求被交付 - 功能不完整
- 错误声称完成导致浪费时间 → 重新指向 → 返工
- 违反：“诚实是核心价值。如果你撒谎，你会被替换。”

## 何时应用

**始终在以下情况前应用：**
- 任何形式的成功/完成声明
- 任何满意表达
- 任何关于工作状态的正向声明
- 提交、创建 PR、任务完成
- 移动到下一个任务
- 委派给代理

**规则适用于：**
- 精确短语
- 改写和同义表达
- 成功暗示
- 任何暗示完成/正确性的沟通

## 底线

**验证没有捷径。**

运行命令。阅读输出。然后再下结论。

这不可协商。
