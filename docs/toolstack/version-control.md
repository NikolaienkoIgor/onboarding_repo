# Version Control

We use **Git** for all source code, and **GitHub** as our hosting platform. Every project lives inside the `fintom8` GitHub organisation.

---

## Git basics

If you're new to Git, the core workflow you'll use every day is:

```bash
# 1. Clone a repository
git clone https://github.com/fintom8/<repo-name>.git
cd <repo-name>

# 2. Create a feature branch
git checkout -b feature/your-feature-name

# 3. Stage and commit your changes
git add .
git commit -m "feat: short description of what you did"

# 4. Push and open a Pull Request
git push origin feature/your-feature-name
```

!!! tip "Commit message convention"
    We follow [Conventional Commits](https://www.conventionalcommits.org/). Use prefixes like `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`.

---

## Branching strategy

We use a **trunk-based development** approach:

| Branch | Purpose |
|--------|---------|
| `main` | Production-ready code. Protected – no direct pushes. |
| `develop` | Integration branch for ongoing work. |
| `feature/<name>` | Short-lived branches for individual features or fixes. |
| `hotfix/<name>` | Emergency fixes that go directly to `main`. |

---

## Pull Requests

All code changes must go through a Pull Request (PR):

1. Open a PR from your feature branch into `develop` (or `main` for hotfixes).
2. Fill in the PR template — describe *what* changed and *why*.
3. At least **one approval** is required before merging.
4. All CI checks must pass.
5. Merge using **Squash and merge** to keep history clean.

!!! info "Linked tickets"
    Always reference the Jira ticket in your PR title or description, e.g. `FIN-123`.

---

## GitHub at Fintom8

- Organisation: `https://github.com/fintom8`
- All repos have branch protection rules on `main`.
- We use GitHub Issues for small tasks and GitHub Projects for sprint boards.
- Secrets and credentials are never committed — use GitHub Secrets or 1Password.

---

## Useful Git commands

```bash
# See what changed
git status
git diff

# Undo last commit (keep changes staged)
git reset --soft HEAD~1

# Pull latest changes from remote
git pull --rebase origin main

# View commit log
git log --oneline --graph --decorate

# Stash work in progress
git stash
git stash pop
```

---

## Resources

- [Pro Git Book (free)](https://git-scm.com/book/en/v2)
- [GitHub Docs](https://docs.github.com)
- [Conventional Commits spec](https://www.conventionalcommits.org/)
