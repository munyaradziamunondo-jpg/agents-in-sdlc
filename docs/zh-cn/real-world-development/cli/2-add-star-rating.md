---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中检查更改，并通过第一个拉取请求合并代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以开始对项目进行第一次修改了。先从小处着手：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示评分。你将请代理显示评分、审查更改，并通过自己的第一个拉取请求合并代码。

在本课中，你将：

- 在功能分支上开始一次有明确目标的 Copilot 对话。
- 请代理对项目进行小幅修改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，游戏详情页已经显示评分。但主页上的游戏卡片只显示标题、类别、发行商和说明。作为热身，你将让代理在每张卡片上显示现有评分。这是一个小而独立的修改，非常适合首次使用。

## 对话的组成

**对话**是你使用 Copilot CLI 完成任务的工作空间。与 Copilot 应用不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可供日后继续，但文件和分支仍是磁盘上的普通 Git 状态。

对话中有三类内容：你的提示和代理的回复、代理探索和编辑文件时执行的工具操作，以及可通过 `/diff` 检查的更改。

## 开始对话并提出修改要求

开始新的对话，以便着手实现功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未启动，运行以下命令：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确认已开始新会话。
5. 使用以下提示提出修改要求：

   ```plaintext
   在列表页的游戏卡片上显示每款游戏的 starRating（满分为 5 分）。如果评分为 null，则显示 "No rating yet"。保持卡片布局不变，添加测试并运行相关检查。
   ```

Copilot 会探索项目，找到用于显示游戏详情的文件，并创建所需代码。现在，你已使用 Copilot CLI 添加了一项新功能。

## 审查差异

合并前应审查所有 AI 生成的更改，即使更改很小也不例外。现在就在 Copilot CLI 中检查更改。

1. 输入 `/diff` 并检查每个已更改的文件。
2. 确认游戏卡片在有评分时显示数字评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种情况。
4. 查看 Copilot 运行的检查结果，并要求它修复任何失败项。
5. 完成审查后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> Copilot 和其他生成式 AI 工具一样具有概率性，并非每次都会产生相同结果，因此具体代码可能不同。审查实际行为，不要期待某种固定实现。

## 检查更改

不能只阅读代码就假定它能正常工作。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中查看更新后的用户界面（UI）。

1. 请 Copilot 启动应用：

   ```plaintext
   启动应用，以便我在浏览器中检查星级评分更改。告知我 URL 并让服务器保持运行。
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了满分为五分的评分。
4. 返回 Copilot，并要求它停止启动的服务器：

   ```plaintext
   停止你启动的开发服务器。
   ```

## 创建并合并第一个拉取请求

功能已创建，现在可以创建拉取请求（PR），将新代码合并到项目中。

1. 请默认代理提交更改：

   ```plaintext
   使用恰当的提交消息提交已审查的星级评分更改。
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，以打开 PR。
4. 审查更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，并使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你使用 GitHub Copilot CLI 发布了第一个更改。具体来说，你：

- 在功能分支上开始了一次有明确目标的 Copilot 对话。
- 指示代理对游戏卡片进行小幅修改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，你将[从筛选问题开始，使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
