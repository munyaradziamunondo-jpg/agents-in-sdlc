---
title: "练习 2 - 使用 Copilot Agent Mode 添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也涉及多个文件，还需要运行测试等操作。开发者需要找到相关文件、修改代码、运行测试、调试并找出遗漏项。Copilot Agent Mode 可以在 IDE 中更自主地工作：先探索项目结构，再协调修改多个文件、运行测试并修复发现的问题。接下来使用 Agent Mode 为网站添加功能。

> [!NOTE]
> Agent Mode 和云代理虽然名称相似，但适用于不同场景。Agent Mode 在 IDE 中执行任务，可快速反馈和交互；云代理则像团队成员一样异步工作，通过问题和拉取请求与你协作。

本练习中，你将了解：

- Copilot Agent Mode 如何探索项目、找到相关文件并协调多处更改。
- 如何使用 Agent Mode 跨用户界面（UI）和数据层实现新功能。
- 合并前如何审查 Copilot 生成的更改和测试。

## 场景

游戏列表不断增加，因此需要让用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot Agent Mode 完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改前先查看网站当前功能。该网站是一个带开发者主题的桌游众筹平台，可列出游戏并显示详情。它是单个 Astro 应用，构建时将页面渲染为静态 HTML。页面通过 frontmatter 使用 Drizzle ORM 直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能由使用标准 DOM API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供 `npm run dev` 任务，用于启动单个 Astro 开发服务器。

1. 返回 Codespace。上一练习结束后应已位于 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下 URL 的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示，询问是否在浏览器中打开 `http://localhost:4321`，选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`。

> [!NOTE]
> 在 Codespace 中选择终端中的 localhost 链接会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace（现正托管 Web 服务器）的私有隧道。

### 浏览网站

网站启动后，可以浏览其功能：

- **主页**：显示桌游列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后可查看标题、说明、发行商和类别等详细信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型会因练习而异；每个步骤会在需要时说明。

网站的初始实现已可使用，但还需要新增功能。先查看待办事项。模板会自动创建问题，可请 GitHub Copilot 展示这些问题。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。Agent 可自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将它列入说明，以便尽量保持一致的体验。但 LLM 具有概率性，收到的建议可能与说明不同，这是正常现象。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问 GitHub 问题待办事项：

   ```plaintext
   请展示我的 GitHub 存储库中的待办事项，并根据对用户最有用的程度协助确定优先级。
   ```

4. 选择 **Continue** 运行列出所有问题的命令。
5. 查看生成的问题列表。

Copilot 还会根据它认为对用户最有用的程度排列事项优先级。

## 查看说明文件

开始生成代码前，先查看为 Copilot 提供上下文的说明文件。`.github/instructions/ui.instructions.md` 文件包含网站功能开发指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - Astro、Tailwind CSS 和 Drizzle 等特定文件类型说明的链接。

> [!TIP]
> 说明文件可引用其他说明文件和项目文件。路径相对于说明文件所在位置，可用于复用内容、拆分复杂说明并提供示例。

## 实现筛选功能

完成筛选至少需要对应用进行三类更新：

- 在数据层（`src/lib/`）添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

合并前还需要运行并通过测试。Copilot Agent Mode 可以执行这些任务。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图中仍选中 **Agent**。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选中模型 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据待办问题实现功能：

   ```plaintext
   请根据待办列表中相关 GitHub 问题的要求，更新网站，添加按发行商和类别筛选的功能。上一练习已创建发行商辅助函数，请在完成其余数据层、UI 和测试工作时保留并按需完善该实现。完成前确保所有测试通过。服务器已经运行，无需再次启动。
   ```

6. 观察 Copilot 探索项目并查找相关的数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试，可能需要几分钟。过程中编辑器可能会出现 **Keep** 和 **Undo**；完成后，每项更改都会提供 **Keep** 或 **Undo**，无需在处理中选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 提示用户确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 处理中可能会暂停，测试也可能暂时失败。这是正常的，Copilot 会在代码生成和测试之间迭代，直至未发现错误。

   ![Copilot Agent Mode 完整聊天会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

本练习中，你使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加了功能。你了解了：

- GitHub Copilot Agent Mode 如何跨 UI 和数据层实现新功能。
- Copilot Agent Mode 如何探索项目、查找相关文件并协调更改。
- 合并前如何审查 Agent Mode 生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习 – 实现分页

游戏列表增长后需要分页。运用本练习所学，请 Copilot 更新网站以实现分页。考虑以下事项：

- 遵循现有最佳实践，包括使用说明文件。
- 确定分页方式，例如是否允许用户选择页面大小，还是固定页面大小。
- 编写提示时提供所需指导。
- 可能需要通过追加上下文和要求更改来迭代 Copilot；这是正常的协作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能和适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
