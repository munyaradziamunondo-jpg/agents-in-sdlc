---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应提供相关上下文。[说明文件][instruction-files]可以指导 Copilot 如何生成代码。

本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定的说明文件提供给 Copilot。
- 使用当前说明生成筛选功能的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选功能切片提交到 `main`，供下一练习中的 Copilot 使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合部分标准。Copilot 的结果具有非确定性。本练习旨在观察更新说明后行为的整体变化，而不是要求输出逐字一致。

## 说明文件

### 场景

Tailspin Toys 有一套开发规范，包括：

- 数据层始终需要单元测试。
- 用户界面（UI）应使用暗色模式并具有现代风格。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加说明文件用途的注释块。

使用说明文件可确保 Copilot 按照这些规范完成任务。

### 自定义说明

自定义说明为 Copilot 提供上下文和偏好，帮助它理解代码风格和要求。你可以指定编码约定、库和注释风格，也可以为整个存储库或特定文件类型创建说明。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。内容应是适用于大多数请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型创建的说明文件，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在集成开发环境（IDE）中，说明文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云代理在生成代码时，都会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

- 将 `copilot-instructions.md` 用于项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用清晰的自然语言，并提供代码示例。

说明文件没有唯一的编写方式，AI 的使用方式也不止一种。通过实践找出适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应建立完善的说明文件集合。项目中包含 [UI 更新说明][ui-instructions]和 [Astro 说明][astro-instructions]等。
>
> Copilot 也可以帮你生成说明文件。不同界面提供的入口各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应环境的课程会介绍相关操作。
>
> 如需模板或入门示例，可查看 [awesome-copilot][awesome-copilot]，其中包含说明文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

查看存储库自带的说明文件：核心 `copilot-instructions.md` 文件和针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网站界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指导。
3. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段指定相对于存储库根目录的 glob；例如匹配 `**/*.test.ts` 的 TypeScript 测试文件会应用这些说明。
5. 查看此项目的单元测试编写说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。查看它链接到的其他说明文件（如 `unit-tests.instructions.md`）和项目文件。路径相对于说明文件所在位置，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求添加代码内文档。接下来会加入 TSDoc 文档注释和文件注释头。

## 创建更改分支

先为更改创建分支。

1. 返回上一练习使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型会因练习而异；每个步骤会在需要时说明。

先在现有说明下发送提示，观察自定义说明的影响。稍后更新说明后，再次发送相同提示并比较结果。

1. 关闭之前练习留下的编辑器选项卡，避免 Copilot 获取不需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，其中包含一个数据访问辅助函数，返回所有发行商的列表，并为每个发行商提供名称和 ID。应用文件更改。
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件及其测试。
6. 注意生成的辅助函数是类型化函数，以 `db` 客户端为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使未明确要求，也可能添加文档注释。如果发生这种情况也没关系，本练习的重点仍是说明更新后行为的一致性改进。

## 添加新的存储库标准

`.github/copilot-instructions.md` 用于提供项目级指导。现在将编码标准写入其中，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。它记录了项目标准，但尚无代码内文档规则，这正是生成的辅助函数缺少文档注释的原因。
3. 在现有标准下方添加以下内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可在存储库级文件 `.github/copilot-instructions.md` 中提供全局指导，也可使用 `*.instructions.md` 为特定语言、文件类型或任务提供说明。项目级文件适合放置此处添加的文档注释规则等全局标准。

## 再次运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   创建或更新 src/lib/publishers.ts，其中包含一个数据访问辅助函数，返回所有发行商的列表，并为每个发行商提供名称和 ID。应用文件更改。
   ```

4. 注意生成的文件现在以类似以下注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 注意生成的函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行商的 ID 和名称列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行商对象数组的 Promise。
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用于真实代码，后续练习会继续使用这些代码。

## 提交说明并推送分支

说明文件与存储库中的其他资源一样，由源代码管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 运行以下命令，确认说明更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他辅助性更改（例如测试），也请添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并关联待办列表中的文档标准问题。Copilot 可以使用已连接到项目的 GitHub 工具查找问题、创建并合并 PR。

> [!NOTE]
> 这是 Copilot 首次代表你操作 GitHub。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按照提示授权即可。后续练习会介绍这种连接所用的模型上下文协议（MCP）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在代理选择器中选中 **Agent**。
3. 请 Copilot 查找与更新说明文件相关的问题，并从当前分支创建 PR，说明说明更新和新的发行商辅助函数基础：

   ```plaintext
   查找与更新说明文件相关的问题。从当前分支创建新的 PR，说明此 PR 会关闭该问题，并指出我们还为后续筛选工作添加了发行商辅助函数基础。
   ```

4. Copilot 会查找问题并创建 PR。
5. 当 Copilot 提示允许其代表你执行 GitHub 操作时，检查操作并酌情选择 **Allow**。
6. 创建 PR 后，使用以下提示要求 Copilot 合并 PR 并将分支切回 main：

   ```plaintext
   将 PR 合并到 main。然后在本地切回 main，并拉取最新代码以确保代码已更新。
   ```

7. 当 Copilot 提示允许其代表你执行 GitHub 操作和 shell 命令时，检查操作并酌情选择 **Allow**。

你已在 GitHub Copilot 的帮助下创建并合并拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何读取项目中的说明文件，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察 Copilot 在**现有**说明下生成的内容。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和辅助函数基础提交到 `main`，供下一练习继续使用。

接下来，在[代理模式][next-lesson]中运用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
