# Codex GitHub 工作流

[English](README.md)

这是给本地 Codex 用的用户级技能，复用 Git 和 GitHub CLI。你可以只提交本次改动、把分支推到指定仓库，或者从个人 fork 向上游开 PR。技能先核对操作对象，再按请求做到对应一步。

## 适用场景

| 场景 | 技能怎么处理 |
| --- | --- |
| 工作区有其他改动，这次只提交一个修复 | 检查差异，只暂存本次任务相关的文件或代码片段。 |
| 本地已经提交，只需要推送 | 核对远端和目标分支，推送后确认远端提交。 |
| 给只有读取权限的项目贡献代码 | 推到已核对的个人 fork，再向上游目标分支开 PR。 |
| 当前分支已经开过 PR | 按来源仓库、分支和目标查找现有 PR，保留其审查状态。 |

适用于本地 Codex，包括在 WSL 中执行命令的 Codex Desktop。请在 Codex 实际用的环境中安装 `git`、`gh`，并完成 GitHub 登录。构建和测试沿用目标项目的工具。实际操作仍受账号权限、网络环境和仓库规则限制。

范围是提交、推送、开 PR 和更新已有 PR。自动发版、循环处理审查意见和 CI 修复属于独立工作流。

## 安装

在 Codex 会话中调用内置安装器：

```text
$skill-installer install https://github.com/WantexZard/codex-github-workflow/tree/main/skills/github-workflow
```

也可以把 [`skills/github-workflow/SKILL.md`](skills/github-workflow/SKILL.md) 放进 Codex 用户技能目录下的 `github-workflow/SKILL.md`。安装后若找不到技能，刷新或重启 Codex 后查看技能列表。

## 用法示例

```text
$github-workflow 只提交本次改动的 README，其他修改留在工作区。
$github-workflow 把当前分支推到我的 fork，PR 稍后再开。
$github-workflow 从我的 fork 向 example-org/sample-app 的 main 分支开一个草稿 PR。
```

## 与现有项目的区别

| 项目 | 适合的用法 |
| --- | --- |
| OpenAI [`yeet`](https://github.com/openai/skills/tree/main/skills/.curated/yeet) | 一次完成暂存、提交、推送和开 PR。其文档流程会全量暂存并推到 `origin`。 |
| GitHub Awesome Copilot 的 [`git-commit`](https://github.com/github/awesome-copilot/tree/main/skills/git-commit) 和 [`make-repo-contribution`](https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution) | 分别用于整理提交和遵循目标仓库的贡献规范。 |
| [GitHub MCP Server](https://github.com/github/github-mcp-server) | 提供 Issue、PR 和仓库数据等远端工具。 |
| **本技能** | 用本机已有的 Git 和 `gh`，在一份技能里支持分步执行、按任务暂存、指定 fork 和复用已有 PR。 |

本技能参考了前两项的工作流，公开版的指令在本仓库维护。使用 [MIT 许可证](LICENSE)。
