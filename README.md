# Superpowers

Superpowers 是一套面向编码代理的完整软件开发方法论。它建立在一组可组合技能和初始指令之上，确保你的代理会真正使用这些技能。

## 工作原理

从你启动编码代理的那一刻开始，它就会介入。当它发现你要构建某个东西时，*不会*直接跳进去写代码，而是先退一步，询问你真正想做什么。

当它从对话中梳理出规格后，会把规格分成足够短的小段展示给你，方便你实际阅读和消化。

在你确认设计后，代理会制定一份实现计划。这份计划清晰到让一个热情但品味糟糕、判断力不足、没有项目上下文、还讨厌测试的初级工程师也能照着执行。它强调真正的 red/green TDD、YAGNI（You Aren't Gonna Need It）和 DRY。

接下来，当你说“开始”后，它会启动 *subagent-driven-development* 流程，让代理逐项完成工程任务，检查并审阅自己的工作，然后继续推进。Claude 按你们共同制定的计划自主工作几个小时而不偏离方向，并不罕见。

系统里还有更多内容，但这就是核心机制。由于技能会自动触发，你不需要做任何特殊操作。你的编码代理会直接拥有 Superpowers。

## 赞助

如果 Superpowers 帮你做出了能产生收益的事情，并且你愿意支持，我会非常感谢你考虑[赞助我的开源工作](https://github.com/sponsors/obra)。

谢谢！

- Jesse

## 安装

**注意：** 不同平台的安装方式不同。

### Claude Code 官方 Marketplace

Superpowers 可通过 [Claude 官方插件 marketplace](https://claude.com/plugins/superpowers) 获取。

从 Anthropic 官方 marketplace 安装插件：

```bash
/plugin install superpowers@claude-plugins-official
```

### Claude Code（Superpowers Marketplace）

Superpowers marketplace 为 Claude Code 提供 Superpowers 和一些相关插件。

在 Claude Code 中，先注册 marketplace：

```bash
/plugin marketplace add obra/superpowers-marketplace
```

然后从这个 marketplace 安装插件：

```bash
/plugin install superpowers@superpowers-marketplace
```

### OpenAI Codex CLI

- 打开插件搜索界面

```bash
/plugins
```

搜索 Superpowers：

```bash
superpowers
```

选择 `Install Plugin`。

### OpenAI Codex App

- 在 Codex app 中，点击侧边栏的 Plugins。
- 你应该能在 Coding 分类中看到 `Superpowers`。
- 点击 Superpowers 旁边的 `+`，并按提示操作。

### Cursor（通过 Plugin Marketplace）

在 Cursor Agent 聊天中，从 marketplace 安装：

```text
/add-plugin superpowers
```

或者在插件 marketplace 中搜索 “superpowers”。

### OpenCode

告诉 OpenCode：

```text
Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md
```

**详细文档：** [docs/README.opencode.md](docs/README.opencode.md)

### GitHub Copilot CLI

```bash
copilot plugin marketplace add obra/superpowers-marketplace
copilot plugin install superpowers@superpowers-marketplace
```

### Gemini CLI

```bash
gemini extensions install https://github.com/obra/superpowers
```

更新：

```bash
gemini extensions update superpowers
```

## 基础工作流

1. **brainstorming** - 写代码前激活。通过提问细化粗略想法，探索替代方案，分段展示设计供确认，并保存设计文档。

2. **using-git-worktrees** - 设计批准后激活。在新分支上创建隔离工作区，运行项目设置，并验证干净的测试基线。

3. **writing-plans** - 有已批准设计时激活。把工作拆成小任务（每个 2-5 分钟）。每个任务都包含精确文件路径、完整代码和验证步骤。

4. **subagent-driven-development** 或 **executing-plans** - 有计划时激活。前者为每个任务派发全新子代理，并进行两阶段审阅（先规格符合性，再代码质量）；后者按批次执行，并设置人工检查点。

5. **test-driven-development** - 实现过程中激活。强制执行 RED-GREEN-REFACTOR：写失败测试，确认它失败，写最小代码，确认它通过，然后提交。测试前写出的代码会被删除。

6. **requesting-code-review** - 任务之间激活。对照计划审阅，并按严重程度报告问题。Critical 问题会阻塞继续推进。

7. **finishing-a-development-branch** - 任务完成时激活。验证测试，展示选项（merge/PR/keep/discard），并清理 worktree。

**代理会在任何任务前检查相关技能。** 这些是强制工作流，不是建议。

## 内含内容

### 技能库

**测试**
- **test-driven-development** - RED-GREEN-REFACTOR 循环（包含测试反模式参考）

**调试**
- **systematic-debugging** - 四阶段根因分析流程（包含 root-cause-tracing、defense-in-depth、condition-based-waiting 技术）
- **verification-before-completion** - 确保问题真的修复

**协作**
- **brainstorming** - 苏格拉底式设计细化
- **writing-plans** - 详细实现计划
- **executing-plans** - 带检查点的批量执行
- **dispatching-parallel-agents** - 并发子代理工作流
- **requesting-code-review** - 预审阅清单
- **receiving-code-review** - 响应审阅反馈
- **using-git-worktrees** - 并行开发分支
- **finishing-a-development-branch** - Merge/PR 决策工作流
- **subagent-driven-development** - 通过两阶段审阅快速迭代（先规格符合性，再代码质量）

**元技能**
- **writing-skills** - 按最佳实践创建新技能（包含测试方法论）
- **using-superpowers** - 技能系统介绍

## 理念

- **测试驱动开发** - 永远先写测试
- **系统化优先于临时处理** - 用流程取代猜测
- **降低复杂度** - 把简单性作为首要目标
- **证据优先于声明** - 声称成功前先验证

阅读[原始发布公告](https://blog.fsck.com/2025/10/09/superpowers/)。

## 贡献

下面是 Superpowers 的通用贡献流程。请记住，我们通常不接受新增技能的贡献；任何技能更新都必须能在所有受支持的编码代理上工作。

1. Fork 仓库
2. 切换到 `dev` 分支
3. 为你的工作创建分支
4. 遵循 `writing-skills` 技能来创建、测试新增或修改后的技能
5. 提交 PR，并确保完整填写 pull request 模板

完整指南见 `skills/writing-skills/SKILL.md`。

## 更新

Superpowers 的更新方式一定程度上取决于编码代理，但通常是自动的。

## 许可证

MIT License - 详情见 LICENSE 文件。

## 社区

Superpowers 由 [Jesse Vincent](https://blog.fsck.com) 和 [Prime Radiant](https://primeradiant.com) 的其他成员构建。

- **Discord**： [加入我们](https://discord.gg/35wsABTejz)，获取社区支持、提问，并分享你用 Superpowers 构建的内容
- **Issues**： https://github.com/obra/superpowers/issues
- **发布公告**： [注册](https://primeradiant.com/superpowers/) 以获取新版本通知
