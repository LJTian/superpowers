---
name: finishing-a-development-branch
description: 实现已完成、所有测试通过，并需要决定如何集成工作时使用；通过结构化选项引导合并、PR 或清理
---

# 完成开发分支

## 概览

通过展示清晰选项并处理所选工作流，完成开发工作。

**核心原则：** 验证测试 → 展示选项 → 执行选择 → 清理。

**开始时声明：** “我正在使用 finishing-a-development-branch 技能完成这项工作。”

## 流程

### 步骤 1：验证测试

**展示选项前，先验证测试通过：**

```bash
# Run project's test suite
npm test / cargo test / pytest / go test ./...
```

**如果测试失败：**
```
测试失败（<N> 个失败）。完成前必须修复：

[展示失败]

测试通过前不能继续合并/创建 PR。
```

停下。不要进入步骤 2。

**如果测试通过：** 继续步骤 2。

### 步骤 2：确定基准分支

```bash
# 尝试常见基准分支
git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null
```

或者询问：“这个分支是从 main 分出来的，对吗？”

### 步骤 3：展示选项

精确展示以下 4 个选项：

```
实现已完成。你想怎么处理？

1. 本地合并回 <base-branch>
2. 推送并创建 Pull Request
3. 保留当前分支不动（我之后处理）
4. 丢弃这项工作

选择哪个？
```

**不要添加解释**，保持选项简洁。

### 步骤 4：执行选择

#### 选项 1：本地合并

```bash
# 切换到基准分支
git checkout <base-branch>

# 拉取最新内容
git pull

# 合并功能分支
git merge <feature-branch>

# 在合并结果上验证测试
<test command>

# 如果测试通过
git branch -d <feature-branch>
```

然后：清理 worktree（步骤 5）

#### 选项 2：推送并创建 PR

```bash
# 推送分支
git push -u origin <feature-branch>

# 创建 PR
gh pr create --title "<title>" --body "$(cat <<'EOF'
## Summary
<2-3 bullets of what changed>

## Test Plan
- [ ] <verification steps>
EOF
)"
```

然后：清理 worktree（步骤 5）

#### 选项 3：保持原样

报告：“保留分支 <name>。Worktree 保留在 <path>。”

**不要清理 worktree。**

#### 选项 4：丢弃

**先确认：**
```
这会永久删除：
- 分支 <name>
- 所有提交：<commit-list>
- 位于 <path> 的 worktree

输入 'discard' 确认。
```

等待精确确认。

如果已确认：
```bash
git checkout <base-branch>
git branch -D <feature-branch>
```

然后：清理 worktree（步骤 5）

### 步骤 5：清理 Worktree

**对于选项 1、2、4：**

检查是否位于 worktree 中：
```bash
git worktree list | grep $(git branch --show-current)
```

如果是：
```bash
git worktree remove <worktree-path>
```

**对于选项 3：** 保留 worktree。

## 快速参考

| 选项 | 合并 | 推送 | 保留 Worktree | 清理分支 |
|--------|-------|------|---------------|----------------|
| 1. 本地合并 | ✓ | - | - | ✓ |
| 2. 创建 PR | - | ✓ | ✓ | - |
| 3. 保持原样 | - | - | ✓ | - |
| 4. 丢弃 | - | - | - | ✓（强制） |

## 常见错误

**跳过测试验证**
- **问题：** 合并损坏代码，创建失败 PR
- **修复：** 展示选项前始终验证测试

**开放式问题**
- **问题：** “接下来怎么办？”→ 含糊
- **修复：** 精确展示 4 个结构化选项

**自动清理 worktree**
- **问题：** 在可能仍需要时删除 worktree（选项 2、3）
- **修复：** 只为选项 1 和 4 清理

**丢弃前不确认**
- **问题：** 意外删除工作
- **修复：** 要求输入 “discard” 确认

## 危险信号

**绝不要：**
- 在测试失败时继续
- 不在合并结果上验证测试就合并
- 未确认就删除工作
- 未明确请求就 force-push

**始终：**
- 展示选项前验证测试
- 精确展示 4 个选项
- 对选项 4 获取输入确认
- 只为选项 1 和 4 清理 worktree

## 集成

**被以下技能调用：**
- **subagent-driven-development**（步骤 7）- 所有任务完成后
- **executing-plans**（步骤 5）- 所有批次完成后

**配合使用：**
- **using-git-worktrees** - 清理由该技能创建的 worktree
