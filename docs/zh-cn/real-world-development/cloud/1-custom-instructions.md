---
title: "练习 1 - 自定义说明（云代理）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云代理 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应让它能够访问相关上下文。[说明文件][instruction-files]可提供此类指导，让 Copilot 明白不仅要做什么，也要如何完成。

本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定的说明文件提供给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，本练习不会比较提示执行前后的效果。Copilot 云代理会异步处理 GitHub 问题，因此难以实时并排展示差异。你将在后续练习中审查云代理创建的拉取请求，并看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 有一套开发规范，包括：

- 数据层始终需要单元测试。
- UI 应使用暗色模式并具有现代风格。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加说明文件用途的注释块。

使用说明文件可确保 Copilot 按照这些规范完成任务。

### 自定义说明

自定义说明为 Copilot 提供上下文和偏好，帮助它理解代码风格和要求。你可以指定编码约定、库和注释风格，也可以为整个存储库或特定文件类型创建说明。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。内容应是适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型创建的说明文件，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云代理在生成代码时，都会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本工作坊范围。示例项目展示了一种常见做法：

- 将 `copilot-instructions.md` 用于项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保说明清晰，并提供代码示例。

说明文件没有唯一的编写方式，AI 的使用方式也不止一种。通过实践找出适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应建立完善的说明文件集合。查看本项目的文件时，你会发现其中包括 [UI 更新说明][ui-instructions]和 [Astro 说明][astro-instructions]等。
>
> Copilot 也可以帮你生成说明文件。不同界面提供的入口各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应环境的课程会介绍相关操作。
>
> 如需模板或入门示例，可查看 [awesome-copilot][awesome-copilot]，其中包含说明文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

查看存储库自带的说明文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网站界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段指定相对于存储库根目录的 glob；例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会应用这些说明。
5. 查看此项目的单元测试编写说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。查看它链接到的其他说明文件（例如 `unit-tests.instructions.md`）和项目文件。这有助于拆分大型说明、复用内容并提供代码示例。（其中的路径相对于说明文件所在位置，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求添加代码内文档。接下来会加入 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来要编辑的内容只有一处：添加项目级规则，要求使用 TSDoc 文档注释和文件级注释头。后续本环境中分配问题时，云代理会读取该规则。

编辑前先创建分支。云代理会从问题所针对的分支读取说明，但仍应以规范方式提交这些更改：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于提供项目级指导。现在将编码标准写入其中，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。此处记录项目编码标准，但尚无代码内文档规则，这正是生成的辅助函数缺少文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可在存储库级文件 `.github/copilot-instructions.md` 中提供全局指导，也可使用 `*.instructions.md` 为特定语言、文件类型或任务提供说明。项目级文件适合放置此处添加的文档注释规则等全局标准。

## 提交、推送并合并说明更新

云代理会读取问题目标分支中的说明。下一练习中，分配给 Copilot 的问题会从 `main` 分支创建，因此必须先将说明更改合并到 `main`，云代理才能使用这些规则。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后，云代理处理下一练习中的问题时就会读取这些说明。

> [!TIP]
> 如果希望本工作坊直接在 `main` 上操作，可以跳过分支步骤并直接提交到 `main`。此处使用分支是为了模拟真实项目的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何读取项目中的说明文件，并向 `.github/copilot-instructions.md` 添加了存储库级标准。后续练习中，云代理生成的拉取请求会体现此标准。

接下来，[将第一个问题分配给 Copilot 云代理][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
