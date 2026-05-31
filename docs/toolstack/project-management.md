# Project Management

We use **Jira** for task and sprint tracking and **Confluence** for documentation and knowledge management. Both tools are part of the Atlassian suite and are deeply integrated with our GitHub and Slack workflows.

---

## Jira

Jira is where all engineering work is tracked. Every piece of work — feature, bug, chore, or spike — lives as a Jira ticket.

### Access

Log in at `https://fintom8.atlassian.net` using your company Google account.

### Ticket lifecycle

```
Backlog → To Do → In Progress → In Review → Done
```

| Status | Meaning |
|--------|---------|
| **Backlog** | Not yet scheduled for a sprint |
| **To Do** | In the current sprint, not started |
| **In Progress** | Actively being worked on |
| **In Review** | PR open, awaiting review/merge |
| **Done** | Merged and deployed |

### Ticket naming

Tickets follow the format `FIN-<number>`, e.g. `FIN-420`.

Always reference the ticket number in:

- Your **branch name**: `feature/FIN-420-add-payment-retry`
- Your **PR title**: `feat(payments): add retry logic [FIN-420]`
- Your **commit messages** (optional but encouraged)

### Sprint ceremonies

| Ceremony | When | Duration |
|----------|------|----------|
| Sprint Planning | Every other Monday | 1 hour |
| Daily Standup | Daily (async in Slack `#standup`) | 15 min |
| Sprint Review | Every other Friday | 30 min |
| Retrospective | Every other Friday | 45 min |
| Backlog Refinement | Mid-sprint | 30 min |

!!! info "Async standup"
    Post your daily update in **#standup** before 10:00 CET using the format:
    
    - **Yesterday:** what you worked on
    - **Today:** what you're working on
    - **Blockers:** anything stopping you

---

## Confluence

Confluence is our internal wiki — the single source of truth for decisions, architecture docs, runbooks, and team knowledge.

### Where to find things

| Space | Contents |
|-------|---------|
| **Engineering** | Architecture decisions, RFCs, runbooks |
| **Product** | Feature specs, roadmap, user research |
| **HR & Culture** | People policies, benefits, values |
| **Onboarding** | This guide lives here too! |

### Writing good documentation

- Write in plain language — assume the reader is a smart person new to the topic.
- Use headings, bullet points, and tables to aid scanning.
- Link to related Jira tickets and GitHub PRs.
- Keep docs up to date — stale docs are worse than no docs.

!!! warning "Don't hoard knowledge"
    If you figure out how something works, write it down in Confluence. The next person to join will thank you.

---

## Figma

Design mockups and prototypes live in **Figma** at `https://figma.com`. Engineers access Figma in view/comment mode. If you need edit access, request it via **#it-helpdesk**.
