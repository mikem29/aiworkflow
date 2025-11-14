# AI-Powered GitHub Workflows

Automated issue fixing and pull request review using AI coding assistants (Claude Code, OpenAI Codex, and Cursor AI).

## Overview

This repository contains GitHub Actions workflows that enable you to trigger AI coding assistants directly from GitHub issues and pull requests. Simply mention an AI agent in a comment, and it will automatically analyze the code, implement fixes, or provide comprehensive code reviews.

## Features

- **Automated Issue Fixing**: Trigger AI agents to analyze and fix bugs automatically
- **Intelligent Code Review**: Get detailed PR reviews from multiple AI perspectives
- **Multiple AI Agents**: Choose from Claude Code, OpenAI Codex, or Cursor AI
- **Security First**: Authorization checks and permission controls
- **Production Ready**: Battle-tested workflows with proper error handling

## Quick Start

### Triggering a Fix

Comment on any issue with:
- `@claude-fix` - Use Claude Code to fix the issue
- `@codex-fix` - Use OpenAI Codex to fix the issue
- `@cursor-fix` - Use Cursor AI to fix the issue

The AI agent will analyze the issue, implement a fix, and create a pull request automatically.

### Requesting a Review

Comment on any pull request with:
- `@claude-review` - Get a code review from Claude
- `@codex-review` - Get a code review from Codex
- `@cursor-review` - Get a code review from Cursor

The AI agent will perform a comprehensive code review and post detailed feedback.

## Setup

1. **Copy Workflow Files**: Copy the `.github/` directory to your repository
2. **Configure Secrets**: Add API keys as repository secrets:
   - `CLAUDE_CODE_OAUTH_TOKEN`
   - `OPENAI_API_KEY`
   - `CURSOR_API_KEY`
3. **Authorize Users**: Edit workflow files to add authorized GitHub usernames
4. **Customize Prompts**: Modify `issue_fix_prompt.md` and `pr_review_prompt.md` for your project

## Documentation

Open `index.html` in a browser to view the complete interactive documentation, including:
- Detailed workflow explanations
- Setup instructions
- Usage examples
- Best practices
- Troubleshooting guide
- Architecture deep dive

## Available Workflows

- `claude-fix.yml` - Claude Code issue fixing (write access)
- `claude-review.yml` - Claude Code PR review (read-only)
- `codex-fix.yml` - OpenAI Codex issue fixing (write access)
- `codex-review.yml` - OpenAI Codex PR review (read-only)
- `cursor-fix.yml` - Cursor AI issue fixing (write access)
- `cursor-review.yml` - Cursor AI PR review (read-only)

## How It Works

1. User comments on an issue/PR with a trigger phrase (e.g., `@claude-fix`)
2. Workflow checks authorization and loads context
3. AI agent analyzes the issue/PR and relevant code
4. For fixes: Creates a branch, implements solution, opens PR
5. For reviews: Posts comprehensive feedback as a comment
6. Workflow updates the original issue/PR with results

## Security

- Only authorized users can trigger workflows
- Separate permissions for fix (write) and review (read-only) workflows
- API keys stored as encrypted GitHub Secrets
- All workflow runs logged in Actions tab
- AI-created PRs require human review before merging

## Requirements

- GitHub repository with Actions enabled
- API keys for desired AI services (Claude, OpenAI, Cursor)
- GitHub Secrets configured
- Authorized user list maintained in workflow files

## License

Open source - feel free to use and customize for your projects.

## Credits

Based on workflows from the [PydanticAI Research Agent](https://github.com/coleam00/PydanticAI-Research-Agent) project.
