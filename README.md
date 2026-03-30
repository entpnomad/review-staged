🌐 [English](README.md) | [Español](README.es.md) | [Français](README.fr.md) | [Italiano](README.it.md) | [Português](README.pt.md) | [Deutsch](README.de.md)

# Review Staged

A Claude Code skill that runs multi-agent parallel code review on your staged git changes.

Two independent AI reviewers examine your code simultaneously — one focused on security and bugs, the other on architecture and code quality. A code simplification pass then identifies unnecessary complexity. Findings are cross-referenced to filter false positives, then presented as a consolidated report with a PASS/FAIL/DISCUSS verdict.

## How It Works

1. **Two reviewers in parallel** — Security Engineer + Software Architect examine staged changes independently
2. **Code simplification** — Analyzes changed code for unnecessary complexity, dead code, and cleanup opportunities
3. **Cross-reference** — The orchestrator validates findings, identifies consensus issues, filters false positives
4. **Consolidated report** — Severity-ranked issues with fixes, simplification opportunities, clear verdict

## Installation

```bash
git clone https://github.com/entpnomad/review-staged.git ~/.claude/skills/review-staged
```

Or for a specific project:

```bash
git clone https://github.com/entpnomad/review-staged.git .claude/skills/review-staged
```

## Usage

Stage your changes, then:

```
/review-staged
```

Or just ask Claude to "review my staged changes."

## Customization

The skill is designed to be adapted to your project. See the Customization section in SKILL.md for how to:
- Modify reviewer roles for your tech stack
- Add project-specific blocking criteria
- Add automated tool phases

## License

MIT
