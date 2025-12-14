# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is an article reading management system that uses GitHub as the infrastructure. It manages articles from quick capture (as GitHub Issues) to structured reflection (as Markdown files in the repository). The system emphasizes **automation with explicit control** - automated discovery and reminders, but manual curation.

## Architecture Overview

### Two-Entity System

1. **Issues = Inbox/Task System**
   - Lightweight capture of articles to read
   - Required fields: Title, URL
   - Optional: Category labels (`topic:pl/compilers`, `topic:math/category-theory`, etc.)
   - Scheduling via `due: YYYY-MM-DD` in issue body
   - Issue template: `.github/ISSUE_TEMPLATE/interesting-article.yml`

2. **Markdown Files = Curated Knowledge**
   - Created only via manual GitHub Actions dispatch (not automated)
   - Requires explicit promotion from an issue number
   - Must include: Summary, Interesting points, Unfamiliar points

### Automation Design

The system follows a "low-friction capture, high-quality reflection" philosophy:

- **Periodic Action (daily)**: Scans issues with `due: YYYY-MM-DD`, creates a single "Today's reading (YYYY-MM-DD)" issue if none exists
- **Manual Action**: Promotes a specific issue to a curated Markdown file (by issue number)

### Key Principles

- **Explicit promotion**: No accidental knowledge inflation - markdown files only created when manually triggered
- **Labels = taxonomy**: Use GitHub labels for categorization (format: `topic:category/subcategory`)
- **Actions = behavior**: GitHub Actions handle automation
- **Issues = state**: Issues track reading status and scheduling

## Planned Workflows (Not Yet Implemented)

Based on NOTE.md, the following GitHub Actions workflows are planned:

1. **Daily Reading Reminder** (`.github/workflows/daily-reading.yml`)
   - Scans issues for `due: YYYY-MM-DD`
   - Creates "Today's reading (YYYY-MM-DD)" issue with links to due articles
   - Idempotent: won't create duplicate daily issues

2. **Manual Article Promotion** (`.github/workflows/promote-article.yml`)
   - Input: issue number
   - Creates markdown file from issue content
   - Should include summary, interesting points, unfamiliar points

## File Organization

- `/` - Root contains NOTE.md (design doc) and README.md
- `/.github/ISSUE_TEMPLATE/` - Issue templates for article capture
- `/.github/workflows/` - GitHub Actions (to be implemented)
- Markdown files for curated articles will be added to the repository root or a dedicated directory (structure TBD)
