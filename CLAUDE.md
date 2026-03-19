# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Reusable composite GitHub Actions for building and deploying Sphinx documentation sites. Used by other JETHOME-IOT repositories as shared CI/CD components.

## Architecture

Three composite actions, each in its own directory:

- **prepare-env/** — Sets up Python with pip caching via `actions/setup-python`, installs dependencies from `requirements.txt` (of the calling repo)
- **ci/** — Runs `make <target>` to build Sphinx docs, captures warnings to `warning.txt`, optionally fails on warnings, optionally uploads `_build/html` as artifact
- **deploy/** — Deploys built HTML via rsync over SSH (using `shimataro/ssh-key-action` for key setup)

Typical workflow in a consuming repo: `prepare-env` -> `ci` -> `deploy`.

## Key Constraints

- All CI jobs in consuming repos use `runs-on: self-hosted` — never GitHub-hosted runners
- Actions require self-hosted runner >= 2.327.1 (Node.js 24 runtime)
- Dependabot monitors all three action directories for GitHub Actions updates (`.github/dependabot.yml`)

## Action Versions (as of 2026-03)

| Action | Version | Verify |
|--------|---------|--------|
| `actions/upload-artifact` | @v7 | `gh api repos/actions/upload-artifact/releases/latest --jq .tag_name` |
| `actions/setup-python` | @v6 | `gh api repos/actions/setup-python/releases/latest --jq .tag_name` |
| `shimataro/ssh-key-action` | @v2 | `gh api repos/shimataro/ssh-key-action/releases/latest --jq .tag_name` |
