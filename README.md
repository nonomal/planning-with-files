# Planning with Files

> **Work like Manus** — the AI agent company Meta acquired for **$2 billion**.

## Thank You

To everyone who starred, forked, and shared this skill — thank you. This project blew up in less than 24 hours, and the support from the community has been incredible.

If this skill helps you work smarter, that's all I wanted.

---

A Claude Code plugin that transforms your workflow to use persistent markdown files for planning, progress tracking, and knowledge storage — the exact pattern that made Manus worth billions.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code Plugin](https://img.shields.io/badge/Claude%20Code-Plugin-blue)](https://code.claude.com/docs/en/plugins)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-green)](https://code.claude.com/docs/en/skills)
[![Cursor Rules](https://img.shields.io/badge/Cursor-Rules-purple)](https://docs.cursor.com/context/rules-for-ai)
[![Version](https://img.shields.io/badge/version-2.3.0-brightgreen)](https://github.com/OthmanAdi/planning-with-files/releases)

## Quick Install

```bash
/plugin marketplace add OthmanAdi/planning-with-files
/plugin install planning-with-files@planning-with-files
```

See [docs/installation.md](docs/installation.md) for all installation methods.

## Supported IDEs

| IDE | Status | Installation Guide | Format |
|-----|--------|-------------------|--------|
| Claude Code | ✅ Full Support | [Installation](docs/installation.md) | Plugin + SKILL.md |
| Cursor | ✅ Full Support | [Cursor Setup](docs/cursor.md) | Rules |
| Kilocode | ✅ Full Support | [Kilocode Setup](docs/kilocode.md) | Rules |
| OpenCode | ✅ Full Support | [OpenCode Setup](docs/opencode.md) | Personal/Project Skill |
| Codex | ✅ Full Support | [Codex Setup](docs/codex.md) | Personal Skill |

## Documentation

| Document | Description |
|----------|-------------|
| [Installation Guide](docs/installation.md) | All installation methods (plugin, manual, Cursor, Windows) |
| [Quick Start](docs/quickstart.md) | 5-step guide to using the pattern |
| [Workflow Diagram](docs/workflow.md) | Visual diagram of how files and hooks interact |
| [Git Checkpoint Workflow](docs/git-checkpoints.md) | How to use git commits as save points (Anthropic pattern) |
| [Troubleshooting](docs/troubleshooting.md) | Common issues and solutions |
| [Cursor Setup](docs/cursor.md) | Cursor IDE-specific instructions |
| [Windows Setup](docs/windows.md) | Windows-specific notes |
| [Kilo Code Support](docs/kilocode.md) | Kilo Code integration guide |
| [Codex Setup](docs/codex.md) | Codex IDE installation and usage |
| [OpenCode Setup](docs/opencode.md) | OpenCode IDE installation, oh-my-opencode config |

## Versions

| Version | Features | Install |
|---------|----------|---------|
| **v2.3.0** (current) | Codex & OpenCode IDE support | `/plugin install planning-with-files@planning-with-files` |
| **v2.2.2** | Restored skill activation language | See [releases](https://github.com/OthmanAdi/planning-with-files/releases) |
| **v2.2.1** | Session recovery after /clear, enhanced PreToolUse hook | See [releases](https://github.com/OthmanAdi/planning-with-files/releases) |
| **v2.2.0** | Kilo Code IDE support, Windows PowerShell support, OS-aware hooks | See [releases](https://github.com/OthmanAdi/planning-with-files/releases) |
| **v2.1.2** | Fix template cache issue (Issue #18) | See [releases](https://github.com/OthmanAdi/planning-with-files/releases) |
| **v2.1.0** | Claude Code v2.1 compatible, PostToolUse hook, user-invocable | See [releases](https://github.com/OthmanAdi/planning-with-files/releases) |
| **v2.0.x** | Hooks, templates, scripts | See [releases](https://github.com/OthmanAdi/planning-with-files/releases) |
| **v1.0.0** (legacy) | Core 3-file pattern | `git clone -b legacy` |

See [CHANGELOG.md](CHANGELOG.md) for details.

## Why This Skill?

On December 29, 2025, [Meta acquired Manus for $2 billion](https://techcrunch.com/2025/12/29/meta-just-bought-manus-an-ai-startup-everyone-has-been-talking-about/). In just 8 months, Manus went from launch to $100M+ revenue. Their secret? **Context engineering**.

> "Markdown is my 'working memory' on disk. Since I process information iteratively and my active context has limits, Markdown files serve as scratch pads for notes, checkpoints for progress, building blocks for final deliverables."
> — Manus AI

## The Problem

Claude Code (and most AI agents) suffer from:

- **Volatile memory** — TodoWrite tool disappears on context reset
- **Goal drift** — After 50+ tool calls, original goals get forgotten
- **Hidden errors** — Failures aren't tracked, so the same mistakes repeat
- **Context stuffing** — Everything crammed into context instead of stored

## The Solution: 3-File Pattern

For every complex task, create THREE files:

```
task_plan.md      → Track phases and progress
findings.md       → Store research and findings
progress.md       → Session log and test results
```

### The Core Principle

```
Context Window = RAM (volatile, limited)
Filesystem = Disk (persistent, unlimited)

→ Anything important gets written to disk.
```

## Usage

Once installed, Claude will automatically:

1. **Create `task_plan.md`** before starting complex tasks
2. **Re-read plan** before major decisions (via PreToolUse hook)
3. **Remind you** to update status after file writes (via PostToolUse hook)
4. **Store findings** in `findings.md` instead of stuffing context
5. **Log errors** for future reference
6. **Verify completion** before stopping (via Stop hook)

Or invoke manually with `/planning-with-files`.

See [docs/quickstart.md](docs/quickstart.md) for the full 5-step guide.

## Session Management (Anthropic Patterns)

This skill now implements key session management patterns from Anthropic's article, [Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents). This makes the workflow more robust and easier to manage across multiple sessions.

### The New Workflow

1.  **Initialize Session**: Start every session (new or existing) with the `init-session` script. It verifies your environment and summarizes the project state.

    ```bash
    # In your project directory, run:
    bash [path-to-skill]/scripts/init-session.sh
    ```

2.  **Work Through Phases**: Complete one phase from `task_plan.md` at a time.

3.  **Verify and Commit**: After each phase is complete and tested, commit your changes. This creates a **git checkpoint**.

    ```bash
    git add .
    git commit -m "feat(Phase 2): Implement user authentication"
    ```

4.  **Log Checkpoint**: Add the commit hash to your `task_plan.md`.

5.  **Context Full? Start Fresh!**: If your context window fills up, simply start a new session and run `init-session.sh` again. It will read your planning files and get you right back on track.

This workflow replaces the previous manual session recovery process.

When your context window fills up and you run `/clear`, this skill automatically recovers unsynced work from your previous session.









To use the full context window without automatic compaction:

```bash

```



## Key Rules

1. **Create Plan First** — Never start without `task_plan.md`
2. **The 2-Action Rule** — Save findings after every 2 view/browser operations
3. **Log ALL Errors** — They help avoid repetition
4. **Never Repeat Failures** — Track attempts, mutate approach

## File Structure

```
planning-with-files/
├── templates/               # Root-level templates (for CLAUDE_PLUGIN_ROOT)
├── scripts/                 # Root-level scripts (for CLAUDE_PLUGIN_ROOT)
├── docs/                    # Documentation
│   ├── installation.md
│   ├── quickstart.md
│   ├── workflow.md
│   ├── troubleshooting.md
│   ├── cursor.md
│   ├── windows.md
│   ├── kilocode.md
│   ├── codex.md
│   └── opencode.md
├── planning-with-files/     # Plugin skill folder
│   ├── SKILL.md
│   ├── templates/
│   └── scripts/
├── skills/                  # Legacy skill folder
│   └── planning-with-files/
│       ├── SKILL.md
│       ├── examples.md
│       ├── reference.md
│       ├── templates/
│       └── scripts/
│           ├── init-session.sh
│           ├── check-complete.sh
│           ├── init-session.ps1   # Windows PowerShell
│           └── check-complete.ps1 # Windows PowerShell
├── .codex/                  # Codex IDE installation guide
│   └── INSTALL.md
├── .opencode/               # OpenCode IDE installation guide
│   └── INSTALL.md
├── .claude-plugin/          # Plugin manifest
├── .cursor/                 # Cursor rules
├── .kilocode/               # Kilo Code rules
│   └── rules/
│       └── planning-with-files.md
├── CHANGELOG.md
├── LICENSE
└── README.md
```

## The Manus Principles

| Principle | Implementation |
|-----------|----------------|
| Filesystem as memory | Store in files, not context |
| Attention manipulation | Re-read plan before decisions (hooks) |
| Error persistence | Log failures in plan file |
| Goal tracking | Pass/fail status in `task_plan.md` |
| Completion verification | Stop hook checks all phases |
| **Git as Memory** | **Commit after each phase for rollback** |

## When to Use

**Use this pattern for:**
- Multi-step tasks (3+ steps)
- Research tasks
- Building/creating projects
- Tasks spanning many tool calls

**Skip for:**
- Simple questions
- Single-file edits
- Quick lookups

## Kilo Code Support

This skill also supports Kilo Code AI through the `.kilocode/rules/` directory.

The [`.kilocode/rules/planning-with-files.md`](.kilocode/rules/planning-with-files.md) file contains all the planning guidelines formatted for Kilo Code's rules system, providing the same Manus-style planning workflow for Kilo Code users.

**Windows users:** The skill now includes PowerShell scripts ([`init-session.ps1`](skills/planning-with-files/scripts/init-session.ps1) and [`check-complete.ps1`](skills/planning-with-files/scripts/check-complete.ps1)) for native Windows support.

See [docs/kilocode.md](docs/kilocode.md) for detailed Kilo Code integration guide.

## Community Forks

| Fork | Author | Features |
|------|--------|----------|
| [devis](https://github.com/st01cs/devis) | [@st01cs](https://github.com/st01cs) | Interview-first workflow, `/devis:intv` and `/devis:impl` commands, guaranteed activation |
| [multi-manus-planning](https://github.com/kmichels/multi-manus-planning) | [@kmichels](https://github.com/kmichels) | Multi-project support, SessionStart git sync |

*Built something? Open an issue to get listed!*

## Acknowledgments

- **Manus AI** — For pioneering context engineering patterns
- **Anthropic** — For Claude Code, Agent Skills, and the Plugin system
- **Lance Martin** — For the detailed Manus architecture analysis
- Based on [Context Engineering for AI Agents](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)

## Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Submit a pull request

## License

MIT License — feel free to use, modify, and distribute.

---

**Author:** [Ahmad Othman Ammar Adi](https://github.com/OthmanAdi)

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=OthmanAdi/planning-with-files&type=Date)](https://star-history.com/#OthmanAdi/planning-with-files&Date)
