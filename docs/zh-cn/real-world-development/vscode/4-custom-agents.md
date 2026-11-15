---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发流程中特定任务或领域定制的 AI 助手。通过在存储库 `.github/agents` 文件夹中的 Markdown 文件定义代理，可以提供专门的说明、最佳实践、编码模式和领域知识，指导 Copilot 更有效地完成特定工作。团队可以将专业知识编写成可复用代理，例如确保符合《Web 内容无障碍指南》（WCAG）的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可全局定义在 `~/.copilot/agents` 中。每个文件都包含至少具有 `name` 和 `description` 的 YAML frontmatter，后接定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与代理技能

自定义代理与[代理技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并指导 AI 执行任务。最简单的区分方式是：**自定义代理是执行者，技能是工具**。

自定义代理拥有独立的上下文窗口，并负责协调技能（甚至其他代理）完成工作。本实验中，无障碍自定义代理会根据无障碍指南审查和更新网站；它可调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义代理没有唯一的正确写法。与其他 AI 用法一样，需要测试和迭代，找到适合环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，无论用户的视觉能力或偏好如何。近期反馈指出，一些用户觉得当前暗色主题难以阅读，因为文本与背景颜色对比度不足。设计团队因此要求实现可由用户开启或关闭的高对比度模式。无障碍至关重要，你将使用自定义代理尽快生成此功能。

本练习中，你将：

- 查看现有的无障碍自定义代理。
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 查看无障碍自定义代理

已为无障碍任务创建自定义代理。返回 Codespace，然后为无障碍工作从 `main` 新建分支，避免与练习 3 中的筛选 PR 混在一起：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` frontmatter 是自定义代理的必需项。

3. 查看后续部分，了解其中的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 中定义的所有自定义代理。选择自定义代理可让聊天会话使用该代理的说明和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型会因练习而异；每个步骤会在需要时说明。

1. 如果代理下拉菜单中尚未选中 **Agent**，请选择它。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

2. 选择聊天视图底部的代理下拉菜单（默认活动代理为 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

   ```plaintext
   为网站添加高对比度模式。应提供用户可设置的高对比度切换开关，并使用浏览器本地存储在页面重新加载后保留设置。
   ```

5. Copilot Chat 会探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中显示文件路径和差异供你审查。
6. 随着编辑发生，**Files changed** 指示器会更新，显示代理修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 正在实际修改存储库，包括 Astro 组件、CSS 和相关测试。

下一练习会审查并引导此项进行中的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即针对特定任务和领域定制的 AI 助手。自定义代理可以将团队专业知识和标准编写成可复用配置，指导 Copilot 更有效地完成任务。

你了解了以下内容：

- 自定义代理的定义方式。
- 如何在 Copilot Chat 代理模式中使用自定义代理。

接下来[监控并引导代理工作][next-lesson]，审查实时更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 中的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
