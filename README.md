# GitHub Workflow for Codex

[简体中文](README.zh-CN.md)

A user skill for local Codex sessions that use Git and GitHub CLI. Ask for a scoped commit, a push to a named branch, or a pull request from a personal fork. The skill checks where the change will go and carries out the steps that your request calls for.

## Where it fits

| Situation | What the skill does |
| --- | --- |
| You want to commit a fix in a working tree with unrelated edits | Reviews the diff and stages the files or hunks for the fix. |
| You already have a commit and want to push it | Checks the remote and destination branch, then verifies the published commit. |
| You are contributing to a repository you can read | Pushes to your verified fork and opens the PR against the upstream branch. |
| Your branch already has a PR | Looks up the matching head and base and preserves the existing PR's review state. |

The intended environment is local Codex, including Codex Desktop with a WSL shell. Install and authenticate `gh` in the environment where Codex runs. The skill also expects `git` and the repository's normal build and test tools when needed. Account permissions, network access, and repository policy still apply.

The scope is commits, pushes, and opening or updating PRs. Release automation, PR review loops, and CI repair are separate workflows.

## Install

Check that the shell used by Codex can find both tools and that `gh` is signed in:

```bash
git --version
gh --version
gh auth status --hostname github.com
```

If GitHub login is needed, run `gh auth login --hostname github.com` in that shell. Then ask Codex to install the skill:

```text
Use $skill-installer to install https://github.com/WantexZard/codex-github-workflow/tree/main/skills/github-workflow
```

For manual installation in the same location used by the built-in installer, copy [`skills/github-workflow/SKILL.md`](skills/github-workflow/SKILL.md) to `$CODEX_HOME/skills/github-workflow/SKILL.md`. The installer defaults to `~/.codex` when `CODEX_HOME` is unset. Start a new Codex session after installation. If the skill is still missing, restart Codex and verify which `CODEX_HOME` it uses.

## Example requests

```text
$github-workflow Commit the README changes from this task. Leave the other edits for later.
$github-workflow Push this branch to my fork. I will open the PR later.
$github-workflow Open a draft PR against example-org/sample-app:main from my fork.
```

Codex skill discovery, scoped staging and commit, and an explicit branch push were verified on WSL. The push verification used a temporary local Git remote. Opening a PR depends on GitHub access to the destination and any fork.

## How it compares

| Project | When to use it |
| --- | --- |
| OpenAI's [`yeet`](https://github.com/openai/skills/tree/main/skills/.curated/yeet) | A combined stage, commit, push, and PR flow. Its documented defaults stage the working tree and push to `origin`. |
| GitHub's Awesome Copilot [`git-commit`](https://github.com/github/awesome-copilot/tree/main/skills/git-commit) and [`make-repo-contribution`](https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution) | Focused guidance for commit preparation and following contribution rules. |
| [GitHub MCP Server](https://github.com/github/github-mcp-server) | Remote GitHub tools, including Issues, PRs, and repository data. |
| **This skill** | One installable rule set for separate commit, push, or PR requests, with scoped staging, explicit fork destinations, and PR reuse using the Git tools already available in a local shell. |

The workflow takes inspiration from the first two sources. The skill instructions are maintained here. See [MIT License](LICENSE).
