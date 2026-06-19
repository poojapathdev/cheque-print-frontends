# Contributing Guidelines

## Table of Contents

- [Branch Protection Rules](#branch-protection-rules)
- [Development Workflow](#development-workflow)
- [Commit Messages](#commit-messages)
- [Pull Requests](#pull-requests)
- [Code Structure](#code-structure)

---

## Branch Protection Rules

The `main` branch is protected with the following enforced rules:

| Rule | Detail |
|---|---|
| **No direct pushes** | All changes must go through a pull request |
| **No force pushes** | Rewriting history on `main` is blocked |
| **No deletion** | The `main` branch cannot be deleted |
| **Linear history required** | Merge commits are not allowed — only squash merges |
| **1 approving review required** | At least one team member must approve before merge |
| **Stale reviews dismissed** | Approval is invalidated on every new push to the PR |
| **Only squash merge allowed** | All PRs are squashed into a single commit on `main` |
| **Copilot code review** | Automatic Copilot review runs on every push to an open PR |

> Repository admins may bypass these rules in exceptional circumstances.

---

## Development Workflow

### 1. Pick up a JIRA ticket

All work must be tied to a JIRA ticket (e.g. `COND-6075`). Do not begin development without one.

### 2. Create a branch from `main`

Branch names follow this pattern:

```
<type>/COND-XXXX-short-description
```

| Type | When to use |
|---|---|
| `feature/` | New functionality |
| `fix/` | Bug fixes |
| `chore/` | Tooling, config, deps, refactors with no behaviour change |

**Examples:**

```bash
git checkout main
git pull
git checkout -b feature/COND-6075-pdf-generation-for-cheques
```

### 3. Make your changes

Follow the [Code Structure](#code-structure) conventions. Keep commits focused — one logical change per commit.

### 4. Push and open a PR

```bash
git push -u origin feature/COND-6075-pdf-generation-for-cheques
```

Open a pull request against `main`. Use the [PR template](#pull-request-template) when filling in the description.

### 5. Request a review

Once the PR is open and all checklist items are ticked:

- Assign at least one reviewer from the team.
- Set the label to **`Waiting for Review`**.
- Link the JIRA ticket in the PR description.

### 6. Address feedback

- Push fixes as new commits on the same branch — do not amend or force-push.
- Stale approvals are automatically dismissed on every new push, so the reviewer will be prompted to re-approve.
- Resolve all Copilot and reviewer comments before re-requesting review.

### 7. Merge

Once approved, the PR is **squash-merged** into `main`. The squash commit message is taken from the PR title, so make sure it follows the [PR title format](#pr-title).

---

## Commit Messages

Use the [Conventional Commits](https://www.conventionalcommits.org/) format:

```
<type>(<scope>): <short description>
```

**Types:** `feat` · `fix` · `refactor` · `chore` · `docs` · `test` · `style`

**Examples:**

```
feat(cheque-portal): add PDF preview before printing
fix(mgmt-portal): resolve blank screen on session timeout
chore: upgrade vite to v8
```

---

## Pull Requests

### PR Title

Format: **`TICKET: Brief description in past tense, sentence case`**

```
COND-6075: Implemented PDF generation for cheque printing
COND-5821: Fixed blank screen on session timeout
COND-6100: Added role-based access control to mgmt portal
```

Rules:
- Start with the JIRA ticket key followed by a colon and a space.
- Use **past tense** and **sentence case** (capitalise only the first word and proper nouns).
- Keep it under 72 characters.
- No trailing period.

### PR Template

The PR description template is at [`.github/pull_request_template.md`](.github/pull_request_template.md). Fill it in completely — PRs with incomplete descriptions will not be reviewed.

---

## Code Structure

This project enforces a strict modular structure for all React code. The full rules are defined in [`.claude/commands/structure.md`](.claude/commands/structure.md). Key points:

- **File names:** kebab-case everywhere (`package-card.tsx`, `use-package-filters.ts`).
- **React exports:** PascalCase inside files (`export const PackageCard = ...`).
- **No barrel exports:** always import directly from the source file.
- **Absolute imports:** always use `@/` — never relative paths (`../../../`).
- **Screen files:** orchestrate only — no inline logic, no API calls, no inline styles.
- **Business logic:** lives in `hooks/`.
- **No `any`:** use `unknown` + type narrowing instead.
- **No magic values:** extract to `constants/`.

Refer to the architecture checklist at the bottom of `structure.md` before marking any feature complete.
