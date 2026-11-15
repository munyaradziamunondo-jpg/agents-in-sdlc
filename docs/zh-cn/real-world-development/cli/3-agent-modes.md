---
title: "第 3 课 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确认方案，使用 Autopilot 根据问题构建筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先前为项目添加了一个小功能，但较大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的开发流程，帮助确保功能正确、实现方式恰当。在本课以及后续几课中，你将遵循典型的代理辅助开发流程：从问题出发创建功能，确保代码有效、行为符合预期，最终成功合并到项目。

> [!NOTE]
> 继续实现此功能时，请使用同一对话和分支。通常会针对不同文件类型使用不同分支或拉取请求（PR）；本练习为聚焦核心概念而简化了流程。

本课中，你将：

- 根据 GitHub 问题开始新的 Copilot 对话。
- 在 Plan 模式下明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

后续实现过程中，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加模型上下文协议（MCP）验证、创建质量保证（QA）代理并打开功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访问者需要按类别和发行商筛选游戏。待办问题描述了该功能，但组合类别等细节需要在编写代码前达成共识。你将使用 Plan 模式确定这些细节，然后授权 Autopilot 在明确范围内实现功能。

## 背景

引入 AI 编码代理不会改变软件开发的基本原则；这些原则反而更加重要。大多数开发者的流程大致如下：

1. 从描述待办事项的问题开始。
2. 制定实现方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码审查完成且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多与上述流程类似。

遵循标准流程，可以确保 AI 生成的代码满足需求，并像手写代码一样经过审查。

## 对话模式

**对话模式**决定代理拥有多少自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与代理协作；代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主编写代码、运行测试并反复迭代，直至任务完成。

先从 Plan 模式开始并审查计划，然后使用 Autopilot 实现计划。

## 从问题开始

开始筛选功能前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有问题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的问题。
9. 按 <kbd>C</kbd> 将问题添加到提示中，并返回 **Session** 选项卡。

现在提示开头会显示 `#7`（或类似编号）。`#` 可将 GitHub 上的问题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划让你在交由 Copilot 实施前，先确定功能或任务的实现方式。对于复杂任务，花些时间规划通常很有帮助。切换到 Plan 模式并要求 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的问题引用后输入以下提示：

   ```plaintext
   为实现此功能制定计划。
   ```

Copilot 会先探索项目，再确定合适的实现方案。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看计划。
6. 如果计划与已确定的决策不符，请要求 Copilot 修改。

## 批准 Autopilot

计划制定并审查后，就可以开始实现了。Autopilot 会持续处理问题，直至认为任务完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或已安装版本中名称相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

生成的代码与其他代码一样，必须在合并前审查。审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 将结果与问题及规划阶段确定的决策进行比较。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 修复任何失败项。

## 体验新功能

代码看起来没问题，但实际运行情况如何？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   启动应用，以便我在浏览器中试用筛选功能。告知我 URL 并让服务器保持运行。
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及按计划约定的组合方式筛选。
4. 确认重置和无结果时的行为符合问题要求和已作出的决定。
5. 返回 Codespace，并请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。本课中，你：

- 根据 GitHub 问题开始新的 Copilot 对话。
- 在 Plan 模式下明确需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证功能。

接下来，通过[使用自定义说明][next-lesson]深入了解如何生成符合文档规范的代码。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
