# GitHub Profile Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the near-empty GitHub profile README with a polished hybrid-style profile featuring a typing-animation header, categorized tech stack badges, an auto-updating activity graph, and an auto-updating recent activity feed.

**Architecture:** Three files change — `README.md` is rewritten entirely, the existing devcard workflow is deleted, and a new `update_activity.yml` workflow is added. The activity graph is a static embed URL (no workflow needed). The Recent Activity section is auto-updated by the new workflow using `jamesgeorge007/github-activity-readme`.

**Tech Stack:** GitHub-flavored Markdown, shields.io, readme-typing-svg, github-readme-activity-graph, GitHub Actions

---

## File Map

| Action | File | Purpose |
|--------|------|---------|
| Rewrite | `README.md` | New profile content |
| Delete | `.github/workflows/cron_update_daily_dev.yml` | Remove devcard automation |
| Create | `.github/workflows/update_activity.yml` | Auto-update Recent Activity section |

---

### Task 1: Rewrite README.md

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Replace README.md with the new content**

Write the following to `README.md` (replacing all existing content):

```markdown
<div align="center">

### Hi, I'm Craig Gazimbi 👋

[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&weight=500&size=20&duration=3000&pause=1000&color=FABD2F&center=true&vCenter=true&width=500&lines=Full+Stack+Developer+%7C+TypeScript+%C2%B7+Go+%C2%B7+Flutter)](https://git.io/typing-svg)

[![LinkedIn](https://img.shields.io/badge/LinkedIn-craiggazimbi-0a66c2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/craiggazimbi/)

</div>

---

### 🛠 Tech Stack

**Frontend**

![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=flat-square&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=61DAFB)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white)

**Backend**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=flat-square&logo=django&logoColor=white)
![Go](https://img.shields.io/badge/Go-00ADD8?style=flat-square&logo=go&logoColor=white)
![Gin](https://img.shields.io/badge/Gin-008ECF?style=flat-square&logo=go&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)

**Mobile**

![Flutter](https://img.shields.io/badge/Flutter-02569B?style=flat-square&logo=flutter&logoColor=white)
![Dart](https://img.shields.io/badge/Dart-0175C2?style=flat-square&logo=dart&logoColor=white)

---

### 📈 GitHub Activity

[![Activity Graph](https://github-readme-activity-graph.vercel.app/graph?username=craiggazimbi&theme=gruvbox&hide_border=true&area=true)](https://github.com/craiggazimbi)

---

### ⚡ Recent Activity

<!--START_SECTION:activity-->
<!--END_SECTION:activity-->
```

- [ ] **Step 2: Verify the file looks correct locally**

```bash
cat README.md
```

Expected: full content with all 4 sections, no truncation.

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "feat: rewrite profile README with typed header, badges, and activity sections"
```

---

### Task 2: Remove the devcard workflow

**Files:**
- Delete: `.github/workflows/cron_update_daily_dev.yml`

- [ ] **Step 1: Delete the workflow file**

```bash
git rm .github/workflows/cron_update_daily_dev.yml
```

- [ ] **Step 2: Commit**

```bash
git commit -m "chore: remove devcard workflow"
```

---

### Task 3: Add the Recent Activity workflow

**Files:**
- Create: `.github/workflows/update_activity.yml`

- [ ] **Step 1: Create the workflow file**

Write the following to `.github/workflows/update_activity.yml`:

```yaml
name: update-activity

on:
  schedule:
    - cron: "0 0 * * *"
  workflow_dispatch:

jobs:
  update-readme:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: jamesgeorge007/github-activity-readme@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          COMMIT_MSG: "chore: update recent activity"
          MAX_LINES: 5
```

- [ ] **Step 2: Verify the file**

```bash
cat .github/workflows/update_activity.yml
```

Expected: valid YAML with `schedule`, `workflow_dispatch`, and the `jamesgeorge007/github-activity-readme` step.

- [ ] **Step 3: Commit**

```bash
git add .github/workflows/update_activity.yml
git commit -m "feat: add GitHub activity auto-update workflow"
```

---

### Task 4: Push and verify on GitHub

- [ ] **Step 1: Push to main**

```bash
git push origin main
```

- [ ] **Step 2: Trigger the activity workflow manually**

```bash
gh workflow run update_activity.yml
```

Wait ~30 seconds, then check the run completed:

```bash
gh run list --workflow=update_activity.yml --limit=1
```

Expected: `completed  success`

- [ ] **Step 3: Check profile renders correctly**

Open `https://github.com/craiggazimbi` in a browser and verify:
- Typing animation appears in the header
- LinkedIn badge is clickable
- All tech stack badges render with correct logos and colors
- Activity graph loads (may take a few seconds on first visit)
- Recent Activity section shows 5 recent events populated by the workflow

- [ ] **Step 4: Verify dependabot.yaml still present**

```bash
cat .github/dependabot.yaml
```

Expected: file exists and is unchanged (keeps actions up to date automatically).
