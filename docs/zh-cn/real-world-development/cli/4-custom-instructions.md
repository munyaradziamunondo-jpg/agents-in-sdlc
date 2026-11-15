---
title: "第 4 课 - 使用自定义说明指导 Copilot"
description: "了解存储库说明，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应让 Copilot 能够访问相关指导。[说明文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。现在你已构建筛选功能，接下来将了解 Copilot 使用的说明、添加文档标准并将其应用于代码。

本课中，你将：

- 了解存储库说明和按路径限定的说明文件如何提供给代理。
- 更新说明文件，确保遵循编码标准。
- 了解说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发规范，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 契约；相关代码更改时应同步更新注释。
- 应保留现有格式和 lint 指南。

通过说明文件，可以确保 Copilot 按照这些规范完成任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解代码风格和要求。你可以指定编码约定、库以及注释风格，也可以为整个存储库或特定文件类型创建说明。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。内容应是适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的说明文件。

> [!NOTE]
> 不同环境对其他说明格式的支持情况各异。使用特定格式前，请查看[自定义说明支持参考][custom-instructions-support]。

## 查看项目中的自定义说明文件

入门项目已包含一组说明文件。先了解现有内容，再进行修改，以便观察效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。此文件适用于该存储库中与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹，查看其中的文件，包括 Astro、Drizzle 数据层和测试说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它指定这些说明适用的文件 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它链接到的其他说明文件和项目文件。这样可以将较大的说明集拆分成可复用的小文件，并提供可参考的示例。

## 更新说明文件以符合团队规范

现有文件是不错的起点，但仍缺少一项指导。修改核心 `copilot-instructions.md`，要求新生成的 TypeScript 包含文档注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常位于第 35 行附近。
2. 在最后一条项目符号后添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 应用更新后的指导

Copilot CLI 在对话开始时加载存储库说明。保存更新后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   我们刚刚更新了说明和代码指导。请更新你生成的代码，使其符合这些指导。
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。检查新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何读取说明文件中的上下文，并为功能添加了新标准。具体来说，你：

- 了解了存储库说明和按路径限定的说明文件如何提供给代理。
- 更新了说明文件，确保遵循编码标准。
- 观察了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持情况][custom-instructions-support]
- [创建有效自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot — 说明文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
