# GitHub Profile Redesign — Design Spec

**Date:** 2026-04-04
**Status:** Approved

## Overview

Redesign the GitHub profile README from a near-empty placeholder (only a Daily.dev devcard) into a polished, hybrid-style profile that communicates Craig Gazimbi's identity as a Full Stack Developer without over-engineering the page.

## Style Direction

**Hybrid** — clean centered header with typing animation, categorized tech stack badges, auto-updating GitHub activity graph, and auto-updating recent activity feed. No featured projects section (most work is in private org repos).

Reference inspirations:
- [UranusLin](https://github.com/UranusLin) — detailed, structured layout
- [VdustR](https://github.com/VdustR) — clean, animated, centered

## Sections

### 1. Header

- Greeting: `Hi, I'm Craig Gazimbi 👋`
- Typing animation (readme-typing-svg): `Full Stack Developer | TypeScript · Go · Flutter`
- LinkedIn badge: `https://www.linkedin.com/in/craiggazimbi/`

### 2. Tech Stack

Displayed as shields.io badges organized in three labeled rows:

| Category | Technologies |
|----------|-------------|
| Frontend | TypeScript, React, Vite |
| Backend  | Python, Django, Go, Gin, Docker |
| Mobile   | Flutter, Dart |

### 3. GitHub Activity Graph

- Service: `github-readme-activity-graph.vercel.app`
- Theme: `gruvbox`
- No workflow needed — the graph is an embedded image URL, always up-to-date

### 4. Recent Activity

- Service: `github-readme-activity` GitHub Action (`jamesgeorge007/github-activity-readme`)
- Shows latest 5 GitHub events (PRs merged, pushes, releases, etc.)
- Auto-updated on a daily cron schedule via GitHub Actions
- Wrapped in `<!--START_SECTION:activity-->` / `<!--END_SECTION:activity-->` markers

## GitHub Actions

One new workflow required:

| Workflow | Trigger | Purpose |
|----------|---------|---------|
| `update_activity.yml` | daily cron (`0 0 * * *`) + workflow_dispatch | Updates Recent Activity section |

## Existing Workflow to Remove

`cron_update_daily_dev.yml` — **delete this file**. The Daily.dev devcard is removed from the profile and this workflow is no longer needed.

## Non-Goals

- No featured projects section
- No GitHub streak stats
- No top languages widget
- No Buy Me A Coffee or other monetization links
- No Twitter/X or other social links beyond LinkedIn

## Implementation Notes

- All badges use `shields.io` flat-square style for consistency
- Activity graph uses `hide_border=true` to match minimal aesthetic
- README is center-aligned for the header, left-aligned for body sections
- `.superpowers/` should be added to `.gitignore`
