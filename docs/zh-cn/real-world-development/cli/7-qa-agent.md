---
title: "第 7 课 - 创建并使用质量保证（QA）代理"
description: "创建 QA 自定义代理，整合问题要求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并通过 Playwright MCP 在浏览器中查看筛选体验。现在将这些能力整合到具有明确质量保证（QA）流程的自定义代理中。

本课中，你将：

- 了解自定义代理如何配合说明、技能和 MCP 工具工作。
- 创建并检查可复用的质量保证配置。
- 选择 QA 代理，并根据筛选问题审查其结果。

## 场景

在创建拉取请求（PR）前，Tailspin Toys 希望统一检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义代理可协调 QA 流程并提供可复用的报告。

## 什么是自定义代理？

自定义代理是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明代理的用途、指令和可用工具。本练习中，你将在 `.github/agents/qa.agent.md` 定义 QA 角色，并在 Copilot CLI 中选择它。

不同自定义项各有职责：存储库说明描述团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置则告诉 Copilot 如何使用这些能力评估需求并报告结果。它们彼此互补，无需另开对话。

## 创建 QA 配置

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置，明确 QA 要执行的检查和必须遵守的边界。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 请默认代理创建新的自定义代理：

   ```plaintext
   在 .github/agents/qa.agent.md 中创建名为 QA 的自定义代理。它应根据问题和已商定的需求检查功能，遵循存储库说明，运行 quality-checks 技能，使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   要求它将每项需求报告为通过、失败或受阻，并提供支持证据。修改实现代码前必须征求同意，并且不得提交更改或创建拉取请求。

   现在只创建配置文件，供我审查。
   ```

## 检查配置

使用新代理前，先确认 Copilot 捕获了预期的 QA 流程和边界，避免配置不完整或范围过宽，导致只想验证功能时却修改了功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 可选，但添加后代理会有清晰的显示名称。
3. 阅读配置说明，确认 QA 会从需求出发、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会征求同意，并且不会提交更改或创建 PR。
5. 如果生成的配置缺少上述职责或边界，请先要求默认代理修改，再继续。

## 根据问题运行 QA

Copilot CLI 在对话开始时加载项目代理。创建配置后，恢复同一筛选对话并选择 QA，以便使用已有的问题和规划决策。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的代理可能暂时不会出现在可用代理列表中。使用上述命令即可启用。

2. 使用以下提示，请 QA 代理审查功能：

   ```plaintext
   根据问题和计划中的决策审查筛选功能。是否已准备好创建 PR？
   ```

3. 等待 QA 代理完成工作。
4. 阅读代理提供的报告。

## 总结和后续步骤

你为工作流添加了可复用的专门角色，并审查了其工作。本课中，你：

- 了解了自定义代理如何配合说明、技能和 MCP 工具工作。
- 创建并检查了可复用的质量保证配置。
- 选择 QA 代理，并根据筛选问题审查其结果。

现在，功能实现、技能更新、QA 配置、测试和验证报告都已准备好供审查。接下来，[创建功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
