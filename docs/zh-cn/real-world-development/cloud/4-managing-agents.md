---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几个练习中，你让 Copilot 云代理处理了三个改善用户体验和添加功能的任务。云代理虽然以异步、自主方式运行，但监控任务仍然很重要。

你可以使用多种工具管理分配给云代理的任务，包括 GitHub.com 上的[代理页面][agents-page]。在此任务控制中心，可以查看所有具有未关闭拉取请求（PR）的代理任务、检查已执行的操作，并引导正在进行的会话。

本课中，你将：

- 通过代理页面监控云代理任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

分配高对比度模式任务后，团队意识到也可以添加浅色模式。既然正在更新网站样式并添加切换功能，就适合一并实现。你将引导代理同时添加浅色模式和高对比度模式。

## 查看 Copilot 云代理任务

查看当前分配给 Copilot 云代理的任务状态。

1. 打开 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 当前的工作状态和执行方式而变化。

4. 同时查看右侧显示的 PR 窗格，以监控 PR 和更改的文件。

## 引导云代理

了解当前活动任务后，请求 Copilot 在实现高对比度模式的同时添加浅色模式。

1. 选择与添加高对比度模式相关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。此时应能看到类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   在实现高对比度模式的同时，也添加浅色模式。还应提供相应的切换开关，让用户选择所需的显示模式。
   ```

   ![代理页面中的云代理任务，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

Copilot 会继续处理更新后的任务！收到消息时，正在执行的具体步骤完成后，它会将新请求加入流程。

这同样需要几分钟。等待时可以回顾迄今为止学到和体验的内容。

## 总结和后续步骤

本课介绍了 Copilot 代理页面，它是监控和引导 GitHub Copilot 云代理任务的中心位置。你可以在此跟踪活动和已完成的任务、审查工作，也可以重新引导进行中的任务以调整范围或补充指导。

你了解了以下内容：

- 使用任务控制中心和代理页面监控云代理任务。
- 重新引导正在进行的会话，请求添加功能。

Copilot 正在完成无障碍功能。接下来[迭代审查 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
