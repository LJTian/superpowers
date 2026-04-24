---
name: writing-plans
description: 有规格或多步骤任务需求、且尚未开始写代码时使用
---

# 编写实现计划

## 概览

编写完整的实现计划，假设执行计划的工程师对代码库几乎没有上下文，而且品味存疑。写清他们需要知道的一切：每个任务要碰哪些文件、代码如何写、测试如何做、可能需要查看哪些文档、如何验证。把整个计划拆成小任务。DRY。YAGNI。TDD。频繁提交。

假设他们是熟练开发者，但几乎不了解我们的工具链和问题域。也假设他们不太懂好的测试设计。

**开始时声明：** “我正在使用 writing-plans 技能创建实现计划。”

**上下文：** 这应在专用 worktree 中运行（由 brainstorming 技能创建）。

**计划保存到：** `docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md`
- 用户对计划位置的偏好优先于默认位置。
- 默认使用用户当前沟通语言编写计划文档；如果用户用中文交流，计划写成中文。命令、路径、代码、标识符、API 名称和提交消息可按项目约定保留英文。

## 范围检查

如果规格覆盖多个独立子系统，它应该已在 brainstorming 阶段拆成子项目规格。如果没有，建议拆成多个计划，每个子系统一个。每个计划都应该独立产出可运行、可测试的软件。

## 文件结构

定义任务前，先列出会创建或修改哪些文件，以及每个文件负责什么。这一步会锁定拆分决策。

- 设计边界清晰、接口明确的单元。每个文件应有一个清晰职责。
- 你最擅长推理一次能放进上下文的代码；文件越聚焦，编辑越可靠。优先使用小而聚焦的文件，而不是承担太多职责的大文件。
- 会一起变化的文件应该放在一起。按职责拆分，而不是按技术层拆分。
- 在现有代码库中，遵循已有模式。如果代码库使用大文件，不要单方面重构；但如果你要修改的文件已经难以维护，把拆分写进计划是合理的。

这个结构会指导任务拆分。每个任务都应产出自洽、可独立理解的变更。

## 小粒度任务

**每一步都是一个动作（2-5 分钟）：**
- “编写失败测试” - 一步
- “运行它，确认失败” - 一步
- “实现让测试通过的最小代码” - 一步
- “运行测试，确认通过” - 一步
- “提交” - 一步

## 计划文档头部

**每个计划必须以下列头部开始：**

```markdown
# [功能名称] 实现计划

> **给代理执行者：** 必需子技能：使用 superpowers:subagent-driven-development（推荐）或 superpowers:executing-plans 逐任务实现此计划。步骤使用复选框（`- [ ]`）语法跟踪。

**目标：** [一句话描述要构建什么]

**架构：** [用 2-3 句话描述方案]

**技术栈：** [关键技术/库]

---
```

## 任务结构

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

- [ ] **Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

- [ ] **Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

- [ ] **Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

- [ ] **Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

- [ ] **Step 5: Commit**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
````

## 禁止占位符

每一步都必须包含工程师真正需要的内容。以下都是**计划失败**，绝不能写：
- “TBD”、“TODO”、“implement later”、“fill in details”
- “添加适当错误处理” / “添加验证” / “处理边界情况”
- “为上述内容编写测试”（但没有实际测试代码）
- “类似任务 N”（重复代码；工程师可能乱序阅读任务）
- 只描述做什么、不展示怎么做的步骤（涉及代码的步骤必须有代码块）
- 引用任何任务中都没有定义过的类型、函数或方法

## 记住
- 始终写精确文件路径
- 每一步都给完整代码；如果某步改代码，就展示代码
- 写精确命令和预期输出
- DRY、YAGNI、TDD、频繁提交

## 自查

写完整计划后，用全新视角查看规格，并用计划逐项对照。这是你自己执行的检查清单，不是派发子代理。

**1. 规格覆盖：** 浏览规格中的每个章节/需求。能否指出哪个任务实现了它？列出所有缺口。

**2. 占位符扫描：** 在计划中搜索危险信号，也就是上方“禁止占位符”中的任何模式。修掉它们。

**3. 类型一致性：** 后续任务中使用的类型、方法签名和属性名，是否匹配前面任务定义的内容？任务 3 叫 `clearLayers()`，任务 7 又叫 `clearFullLayers()`，这就是 bug。

如果发现问题，内联修复。不需要重新审阅，修好后继续。如果发现某个规格需求没有对应任务，就添加任务。

## 执行交接

保存计划后，提供执行选择：

**“计划已完成并保存到 `docs/superpowers/plans/<filename>.md`。有两个执行选项：**

**1. 子代理驱动（推荐）** - 我为每个任务派发一个全新子代理，在任务之间审阅，快速迭代

**2. 当前会话内执行** - 使用 executing-plans 在当前会话中执行任务，按批次执行并设置检查点

**选择哪种方式？”**

**如果选择子代理驱动：**
- **必需子技能：** 使用 superpowers:subagent-driven-development
- 每个任务一个全新子代理 + 两阶段审阅

**如果选择当前会话内执行：**
- **必需子技能：** 使用 superpowers:executing-plans
- 按批次执行，并设置审阅检查点
