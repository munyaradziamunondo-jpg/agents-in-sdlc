---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 协作完成了多项改善用户体验和添加功能的任务：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式等无障碍功能。现在检查代码更改，必要时向 Copilot 提供反馈。

### 场景

生成式 AI 不会改变软件设计和 DevOps 的基本原则。始终要审查生成的代码，并遵循常规 DevOps 流程。接下来审查 GitHub Copilot 提出的文档、相关游戏功能和无障碍功能更改，再决定是否允许团队其他成员审查。

## 安全和 GitHub Copilot 云代理

由于 Copilot 云代理会异步且无人监督地执行任务，因此设置了一些安全限制：

- Copilot 只能读取存储库，并且只能写入它用于修改代码的分支。
- 云代理在 GitHub Actions 中运行，并在独立的临时环境中工作。
- GitHub Actions 工作流必须经人工批准后才能运行。
- 默认情况下，[对外部资源（包括 MCP 服务器）的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 审查生成的文档

先通过 GitHub.com 上的标准拉取请求（PR）界面，查看 GitHub Copilot 云代理创建的第一个 PR，即为代码添加文档的 PR。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如有需要，可以[自定义或禁用 Copilot 云代理防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 查看 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 尚未完成任务，PR 标题会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作，可能需要几分钟。

4. PR 准备好后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 文档注释及其他文档。具体更改会有所不同。检查 TSDoc 文档注释、TypeScript 规范和文件注释头；这些要求来自练习 2 开始时查看的自定义说明文件。
6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 检查区域排队。顺利的话，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

你可以在 PR 评论或代码行内评论中像提及团队成员一样提及 Copilot。Copilot 会读取评论，并启动新会话处理修改。由于结果具有非确定性，无法规定必须使用的具体文字。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论并提及 **@copilot**，请求修改生成的文档。可以采用上述建议，也可以提出其他文档改进要求。
2. 选择 **View Session** 观察 Copilot 工作。注意它会启动新会话进行更新。
3. 选择 **Back to pull request** 返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成修改后，PR 中会出现新提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。之后可将 PR 从草稿转换为准备审查状态，并合并到 main 分支。

![将 PR 转为准备审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

返回 Copilot 为“在游戏详情页显示相关游戏”问题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流会在 PR 检查区域排队。顺利的话，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能！你可以继续迭代并要求云代理修改，例如调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时尚未更新自定义说明）。与之前一样，可以在 **Conversation** 选项卡添加评论，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后审查由无障碍自定义代理实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制中心追加请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 尚未完成任务，PR 标题会带有 **[WIP]** 标记。请等待它完成，可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查实现，重点检查：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 检查区域排队。顺利的话，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可在 Codespace 中切换到此分支手动测试无障碍功能。打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认页面重新加载后设置仍然有效。

无障碍自定义代理帮助 Copilot 遵循无障碍最佳实践实现功能。如有任何疑虑或改进建议，可像之前一样在评论中提及 **@copilot**。

## 可选练习 — 继续委派任务

云代理适合处理积压工作，让你可以转而处理其他事务。你可以为存储库创建更多问题并分配给 Copilot，练习这一工作方式。例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云代理环境中的练习。通过这些课程，你：

- **检查了存储库自带的自定义说明文件**，以便之后观察其对云代理输出的影响。
- **将问题分配给 Copilot 云代理**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用无障碍自定义代理**，实现高对比度和浅色模式切换。
- **使用 Copilot 代理页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代云代理的拉取请求**，通过提及 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

你已完成云代理环境的练习。若想继续探索，其他环境可补充本次实践：

- 🖥️ **[VS Code 环境](../../vscode/)** — 在 IDE 中体验 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)** — 从终端交付两个经过审查的更改，使用 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云代理桥接。

在自己的存储库中，还可以尝试：

- 将重构或测试覆盖率问题分配给云代理。
- 为其他领域创建自定义代理。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云代理防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
