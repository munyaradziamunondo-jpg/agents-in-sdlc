---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、分享会话和可选委派到云代理的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可访问高级功能、后台信息和其他配置选项。你已使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令，接下来了解一些其他实用命令。

本课中，你将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或分享会话。
- 探索并行工作、工作树以及委派给云代理的可选命令。

## 场景

核心 CLI 工作流已完成。现在来了解其他功能：管理上下文、切换模型、分享会话，以及选择是否将工作委派给 [Copilot 云代理][about-cloud-agent]。

## 查看 Copilot CLI 上下文

处理大型或复杂任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话，你也可以使用斜杠命令检查或手动压缩。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及上下文在系统说明、工具、消息和可用空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话内容较少，结果可能不会有明显变化。

> [!NOTE]
> 上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。使用 `/compact` 可自行选择压缩时机。切换到无关任务并需要全新对话时，使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发者也有不同偏好。Copilot CLI 可以列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 分享会话

团队合作并分享经验有助于共同改进 AI 工具的使用方式。`/share` 命令可将会话导出为 Markdown 或 HTML 文件、创建可分享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中 `/share` 的选项。
2. 如需分享会话，请选择适合的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 在发送给他人或发布之前，审查导出内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算公开的存储库或对话内容。

## 可选：扩展规模或委派任务

核心工作坊已完成。Copilot CLI 还为较大型任务提供以下命令：

- `/fleet` 可将彼此独立的子任务分配给多个子代理并行执行。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 Copilot 云代理，代理会异步工作并可能创建拉取请求。

这些命令可能创建额外工作树或触发远程工作，因此属于可选内容。尝试前，请开始一个范围明确的新任务，并按常规流程审查结果。若要深入了解异步代理工作，请继续学习[云代理工作坊][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以配置工具、分享会话并查看后台状态。本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或分享会话。
- 探索了用于并行工作、工作树和委派给云代理的可选命令。

Copilot CLI 还有更多命令和功能可供探索！最后[回顾所学内容][next-lesson]，并了解后续学习方式。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云代理][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
