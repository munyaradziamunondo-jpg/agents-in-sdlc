---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的连续开发流程中使用了 GitHub Copilot CLI。你：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选问题。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并第一个拉取请求（PR）。
- 从筛选问题开始，在 Plan 模式下确定方案，在 Autopilot 模式下实现，并在 Interactive 模式下审查。
- 使用自定义说明指导代理，自定义现有 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议（MCP）服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证（QA）自定义代理，以评估需求、覆盖率、技能结果和浏览器证据。
- 审查完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR。
- 探索用于管理上下文、模型、分享和可选云端委派的斜杠命令。

## 已交付的内容

本工作坊包含两个 PR 里程碑，各自基于更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选、更新说明并将其应用到功能、自定义 `quality-checks` 报告、创建 QA 配置并加入相关测试。

从规划筛选功能到创建 PR，你始终使用同一对话和分支。为简化工作坊，这些更改合并在一个 PR 中。

## 不同类型的验证

你通过自动化测试、浏览器手动检查和 Copilot 使用 MCP 浏览器体验等方式检查了代码。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 将这些结果与需求和测试覆盖率审查结合起来，再供 PR 使用。

新增测试应解决实际缺口；如果 QA 检查后认为无需新增测试，也可能是合理结果。授权合并前，请审查代码和证据；更改后应更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作。在本工作坊中，你更新说明、自定义技能、创建 QA 配置并配置 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。说明设定标准，技能描述可重复执行的任务，自定义代理定义专门角色，MCP 服务器连接外部工具。请审查实际更改和工具结果，而不只是代理总结。

根据任务选择**模式和模型**。使用 **Plan** 先考虑实现方案，使用 **Interactive** 参与范围明确的更改，使用 **Autopilot** 处理范围清晰的任务。日常修改可选择速度较快的模型，复杂任务则可选择能力更强的模型。

上下文仍然和基础设施同样重要。清晰说明要构建什么、为什么构建以及如何实现，会显著影响输出。

## 更多可探索内容

你已学习核心工作流，还可以了解其他 CLI 功能：

- `/review`：请代码审查代理分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调独立子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot 云代理。

## 后续步骤

持续使用工具是提升技能的好方法！可以将它用于生产代码、个人项目，或一直想做却尚未动手的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想从终端扩展 Tailspin Toys，可继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较不同环境，可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot 应用工作坊][app]或 [Copilot 云代理工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
