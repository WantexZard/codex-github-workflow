---
name: github-workflow
description: Handle requests to make a Git commit, push a branch to GitHub, or open or update a pull request. Use local Git and GitHub CLI for personal repositories, shared working trees, and contributions from forks.
---

# GitHub workflow

Use `git` in the current execution environment for worktrees, commits, and pushes. Use `gh` for GitHub repository and PR operations. A commit-only request ends locally. A push-only request ends after remote verification. For a requested PR, prepare and publish the branch as needed. Read the relevant `AGENTS.md` and `CONTRIBUTING.md`.

## Confirm the working copy

- Check the repository root, current branch, worktree status, and relevant history. Prefer a separate branch or worktree when starting from the default branch or when another task has unrelated changes.
- Identify the intended remote and branch from the request and repository configuration. Use `gh api user --jq .login` if you need to verify which account will act. Diagnose connectivity and authentication separately. Avoid printing credentials embedded in remote URLs.

## Commit

- Inspect the diff and stage the files or hunks that belong to this task. Check `git diff --cached` before committing. Preserve unrelated staged changes and isolate the intended work where practical.
- Run relevant checks and follow the repository's hooks and commit conventions. Report the commit SHA and any checks that failed or remain unrun.

## Push and open a PR

- Verify the base repository, base branch, and your permission when the destination is unclear. For contributions to a third-party repository with read access, push to a verified personal fork and open the PR against its upstream. Reuse a fork when it belongs to the correct upstream. Inspect remotes before adding or renaming one.
- Review the commits being published. Push the task branch to the verified remote and name the destination explicitly, for example `git push --set-upstream "$push_remote" "HEAD:refs/heads/$branch"`. Inspect rejected pushes and remote history before retrying.
- Look up open PRs by base repository and branch, and head owner and branch. Reuse an exact match. A push to that head branch updates the existing PR. Preserve its review state unless the user requests a change.
- Follow the repository's PR template when present. Describe the problem, change, and observed verification. Use repository-relative paths and sanitized examples. Link issues with closing keywords only when the changes resolve them.
- Create a PR with `gh pr create --repo "$base_repo" --base "$base_branch" --head "$head" --title "$title" --body-file "$body_file"`. Use `branch` for the same repository or `fork-owner:branch` for a fork. Add `--draft` when requested. Supplying `--head` uses the branch you already pushed.
- Verify the published branch and PR. Report the commit SHA, destination, PR URL, and review state as applicable. After an ambiguous create response, look up the same head and base before retrying.
