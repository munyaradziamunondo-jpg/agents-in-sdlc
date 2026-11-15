---
title: "练习 2 - GitHub Copilot 云代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

许多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云代理可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿拉取请求（PR），供开发者审查。这样可以将繁琐任务交给代理，加快开发流程，让开发者专注于更重要的工作。

本课将介绍 Copilot 云代理的以下用法：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的问题。
- 将问题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务。最初承接网站开发的外包团队没有为代码编写文档。团队希望首先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能：在每款游戏的详情页显示同类别的其他游戏，方便用户继续浏览。当前只需构建用于用户验收测试的初步实现；该功能虽被阻塞，但优先级低于其他问题。

这两项任务都可能被不断延期，因此适合分配给 Copilot 云代理。代理可以在后台处理任务，让开发者继续其他工作，之后再回来审查结果。

## 了解 GitHub Copilot 云代理

[GitHub Copilot 云代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可像开发者一样在后台执行任务。可以通过多种方式交办工作，例如[将 GitHub 问题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求跟踪进展、设置环境并开始处理任务。你可以在会话进行中或完成后查看详情。解决方案准备好供审查时，Copilot 会在拉取请求中通知你。

## 明确说明范围的重要性

GitHub Copilot 并非魔法，不能替代思考。看似简单的操作也可能包含复杂环节，因此要谨慎地为 Copilot 云代理[界定任务范围][cloud-agent-best-practices]。将它视为 AI 结对编程伙伴：分阶段工作、学习、试验并不断调整。加入生成式 AI 不会改变软件开发的基本原则。

## 此存储库中的自定义说明

前面的练习介绍了自定义说明及其如何指导 Copilot。如果已完成自定义说明练习，便见过 `.github/copilot-instructions.md` 的作用；否则，现在可以快速查看。分配任务前，以只读方式检查存储库中的说明文件，之后便能辨认其影响。

在 GitHub 网站界面或已启动的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将这些说明限定于 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 问题分配给云代理时，观察其 PR 中的 TSDoc 文档注释、TypeScript 规范和文件注释头；这些都来自上述说明文件。后续练习审查 PR 时还会再次提及。

## 设置 Copilot 云代理的开发环境

代码开发通常需要特定环境和设置脚本，以确保环境正常。将任务分配给 Copilot 时也是如此，因为它会像软件工程师一样完成任务。

云代理使用 [GitHub Actions][github-actions] 执行工作。你可以通过 `.github/workflows/copilot-setup-steps.yml` 中配置的[专用设置工作流][setup-workflow]自定义环境，让代理访问所需开发工具和依赖项。实验已预先配置此工作流，以确保顺利进行并提供学习机会。它会为单个 Astro 应用准备 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你在存储库的“Actions”选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为你的步骤设置所需的最低权限。Copilot 将获得自己的操作令牌。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果设置步骤中不克隆存储库，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移本地 SQLite 数据库并填充数据，以便构建和测试使用。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几点需要注意：

- 它包含名为 `copilot-setup-steps` 的单个作业。Copilot 开始处理 PR 前，GitHub Actions 会执行此作业。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于测试工作流，而不必等待 Copilot 运行。

## 添加文档

大多数项目都存在文档过时或缺失的问题。这类技术债务常被忽视，降低工作效率，也让维护代码库或引入新成员更加困难。Copilot 擅长生成文档，因此很适合接手此类任务。它会在后台生成文档，你将在后续练习中审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新问题对话框。
4. 选择 **Blank issue** 创建空白问题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   我们组织要求在适当位置为函数和方法添加 TSDoc 文档注释。但近期更新未遵循这一标准。我们需要更新现有代码，确保在有助于阐明行为时添加文档注释。
   ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将问题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的 PR，其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办列表。

> [!NOTE]
> Copilot 的待办列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面提到的工作流）、分析项目并确定解决问题的方案。

13. 查看待办事项和即将执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理问题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何探索代码、理解当前状态、暂停思考并决定计划，以及生成代码。

此过程可能需要几分钟。Copilot 云代理的主要目标之一是异步执行任务，让你能专注于其他工作。接下来利用这一点，再为云代理分配另一项任务，同时编写应用功能代码。

## 在游戏详情页添加相关游戏部分

GitHub Copilot 云代理的一项优势是可以分派工作：你处理一组任务时，它可同时处理另一组。为设计团队添加相关游戏部分可能不需要很长时间，但也可以利用这段时间完成其他工作。现在将任务分配给云代理。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新问题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   我们希望在每款游戏的详情页显示相关游戏，帮助用户发现更多游戏。设计团队希望探索用户体验并进行验收测试。具体要求如下：

   - 在 `src/lib/` 中添加数据访问辅助函数，返回与指定游戏类别相同的其他游戏，并排除该游戏本身
   - 在游戏详情页显示使用该辅助函数的“相关游戏”部分
   - 处理游戏没有相关游戏的情况
   - 为新辅助函数创建单元测试
   - 创建 PR 前确保所有测试通过
   ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将问题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，问题中的第一条评论旁会出现 👀，表示 Copilot 已开始处理。

![Copilot 使用 eyes 表情符号表示正在处理问题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。云代理的工作方式类似软件工程师，因此无需一直监控，完成后再审查即可。现在可以转向创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云代理][copilot-agents]。可以将问题分配给云代理，让它异步处理技术债务、新功能或框架迁移等任务。

你了解了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的问题。
- 将问题分配给 Copilot。

云代理正在后台工作，现在可以着手创建和使用自定义代理。[Copilot 云代理还支持 MCP 服务器][cloud-agent-mcp]，并可使用前面模块介绍的自定义说明。

## 资源

- [关于 Copilot 云代理][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
