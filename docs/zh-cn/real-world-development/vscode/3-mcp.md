---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚使用代理模式构建了筛选功能。创建拉取请求前，应在浏览器中确认功能确实正常。本练习将连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

本练习中，你将：

- 了解模型上下文协议（MCP）及 MCP 服务器如何扩展 Copilot 工具。
- 将 Playwright MCP 服务器添加到工作区。
- 请 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

## 什么是模型上下文协议 (MCP)？

当代理模式能够访问编辑器之外的资源时，功能会更强大。Copilot 使用模型上下文协议（MCP）与外部工具和服务通信。

![代理模式内部工作原理示意图，显示其与上下文、大型语言模型（LLM）和工具的交互，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 代理提供了与外部工具和服务通信的方式。代理可使用资源获取最新信息，或使用工具代表你执行操作。

MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁，负责管理代理与外部工具（例如现有 API 或 npm 包等本地工具）的通信。每个服务器提供一组代理可用的工具和资源。

常见服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API，可创建存储库、更新内容、管理问题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，可浏览网页、填写表单和选择按钮。

还有许多提供不同工具和资源的 MCP 服务器。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，帮助发现并参与生态系统。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样谨慎对待 MCP 服务器。使用前请仔细审查源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 查看已配置的 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

项目模板预先配置了 `github` 条目，因此上一练习中 Copilot 能够读取待办问题。该条目使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，Copilot 首次使用工具时需要使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，可用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，使文件内容如下：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会运行 `npx @playwright/mcp@latest` 启动它。`--headless` 标志使 Playwright 在无可见窗口的情况下运行浏览器；Codespace 没有桌面环境，因此需要此选项。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，通常已安装 Playwright 所需的浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

## 启动并信任服务器

VS Code 按需启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选的；若跳过，VS Code 会在下一步首次需要服务器工具时启动它并询问是否信任。

## 测试筛选功能

Playwright MCP 服务器为 Copilot 提供真实浏览器。代理可以打开页面、导航、应用筛选并读取结果，然后向你总结观察结果，无需离开对话。

Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]而非屏幕截图。代理通过结构化且有标签的元素（按钮、链接和列表项）进行判断，与辅助技术的工作方式类似，因此功能检查也能简单验证无障碍情况。

连接服务器并启动应用后，请 Copilot 测试刚构建的筛选功能：

```text
使用 Playwright MCP 服务器，在 http://localhost:4321 打开正在运行的应用，并验证新的游戏筛选功能：

1. 前往游戏页面并记录列出的游戏数量。
2. 应用类别筛选，并确认列表只显示该类别的游戏。
3. 清除筛选，再应用发行商筛选，并确认列表只显示该发行商的游戏。
4. 同时应用类别和发行商筛选，并确认结果同时符合两项条件。

报告每个步骤中的观察结果，并指出任何行为异常之处。
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步测试并报告结果。根据问题中的验收标准审查报告；如发现问题，可提出后续问题，或要求修复代码后再创建 PR。

> [!NOTE]
> 测试时应用必须在 `http://localhost:4321` 运行。如果已停止开发服务器，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；若报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，可以创建拉取请求（PR）供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板并审查 Copilot 所做更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 所做更改的 Source Control 面板](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到存储库。

## 创建拉取请求

可通过 github.com 或 GitHub 命令行界面（CLI）等方式创建 PR。也可以让 Copilot 查找相关问题并创建关联 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat** 开始新会话。
2. 确认代理下拉菜单中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   查找存储库中与按类别和发行商筛选相关的问题。为当前分支创建新的拉取请求，并将其关联到正确的问题。
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，可能会提示使用 GitHub 身份验证；按照提示授权。
5. 观察 Copilot 搜索问题、找到正确的问题并创建 PR。
6. 选择 Copilot 生成的链接审查 PR，但**暂时不要合并**。

## 总结和后续步骤

本练习中，你：

- 了解了模型上下文协议（MCP）及 MCP 服务器如何扩展 Copilot 工具。
- 将 Playwright MCP 服务器添加到工作区。
- 使用它在发布前通过浏览器手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来[创建自定义代理][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
