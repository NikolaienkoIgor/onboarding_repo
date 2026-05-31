# IDE & Editors

Our primary editor is **Visual Studio Code (VS Code)**. Some backend engineers also use JetBrains IDEs (IntelliJ IDEA, PyCharm). You're free to use either, but VS Code is the standard for shared configs and recommended extensions.

---

## Visual Studio Code

### Installation

Download from [https://code.visualstudio.com](https://code.visualstudio.com) and install the stable build.

### Recommended extensions

Install these from the VS Code Extensions panel (`Ctrl+Shift+X` / `Cmd+Shift+X`):

=== "General"

    | Extension | Purpose |
    |-----------|---------|
    | **GitLens** | Enhanced Git history and blame annotations |
    | **GitHub Pull Requests** | Review PRs without leaving VS Code |
    | **EditorConfig** | Respect project `.editorconfig` settings |
    | **Prettier** | Opinionated code formatter |
    | **ESLint** | JavaScript/TypeScript linting |
    | **SonarLint** | Real-time security & code quality analysis |
    | **Spell Right** | Spell-checker for code and docs |

=== "Python"

    | Extension | Purpose |
    |-----------|---------|
    | **Python** (Microsoft) | Core Python language support |
    | **Pylance** | Fast type checking and IntelliSense |
    | **Black Formatter** | Opinionated Python formatter |
    | **Ruff** | Extremely fast Python linter |

=== "Frontend"

    | Extension | Purpose |
    |-----------|---------|
    | **ES7+ React/Redux/React-Native snippets** | Handy snippets for React |
    | **Tailwind CSS IntelliSense** | Autocomplete for Tailwind classes |
    | **Auto Rename Tag** | Rename opening/closing HTML/JSX tags together |

=== "DevOps"

    | Extension | Purpose |
    |-----------|---------|
    | **Docker** | Manage containers from VS Code |
    | **Kubernetes** | Browse and manage K8s clusters |
    | **Terraform** | Syntax highlighting & validation |
    | **YAML** (Red Hat) | YAML language support |

---

### Workspace settings

Each repository ships a `.vscode/settings.json` and `.vscode/extensions.json`. VS Code will prompt you to install the recommended extensions when you open the folder — always say **yes**.

Key settings applied company-wide:

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.tabSize": 2,
  "files.trimTrailingWhitespace": true,
  "files.insertFinalNewline": true
}
```

---

### Useful keyboard shortcuts

| Action | Windows / Linux | macOS |
|--------|----------------|-------|
| Command palette | `Ctrl+Shift+P` | `Cmd+Shift+P` |
| Quick open file | `Ctrl+P` | `Cmd+P` |
| Toggle terminal | `` Ctrl+` `` | `` Cmd+` `` |
| Find in files | `Ctrl+Shift+F` | `Cmd+Shift+F` |
| Rename symbol | `F2` | `F2` |
| Go to definition | `F12` | `F12` |
| Format document | `Shift+Alt+F` | `Shift+Option+F` |

---

## JetBrains IDEs

If you prefer JetBrains, IntelliJ IDEA Ultimate and PyCharm Professional licences are available — request one via **#it-helpdesk**.

Install the following plugins:

- **Conventional Commit** — enforces our commit message style.
- **SonarLint** — mirrors the VS Code security plugin.
- **.env files support** — syntax highlighting for `.env` files.
- **GitHub Copilot** — AI code completion (licence provided).

---

## GitHub Copilot

All engineers have access to **GitHub Copilot**. Enable it in VS Code via the GitHub Copilot extension or in your JetBrains IDE.

!!! warning "AI-generated code"
    Copilot output must be reviewed carefully before committing. You are responsible for the code you push — always understand what it does.
