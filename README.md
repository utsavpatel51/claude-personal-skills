# Claude Personal Skills

A collection of reusable [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skills that can be installed into any project. Each skill is self-configuring, on first run it asks for any machine-specific paths and saves them for future use.

## Skills

| Skill              | Description                                                                                | Example Usage                                      |
| ------------------ | ------------------------------------------------------------------------------------------ | -------------------------------------------------- |
| **save-session**   | Save your Claude conversation as an Obsidian-compatible markdown note                      | `/save-session api-refactor setup-auth-middleware` |
| **stock-analysis** | Comprehensive Indian stock (NSE/BSE) analysis with technicals, news, and entry/exit levels | `/stock-analysis RELIANCE`                         |
| **dev-mentor**     | Interactive system design and CS mentor, teaches a topic then quizzes you interview-style  | `/dev-mentor consistent hashing`                   |

## Installation

Copy the `.claude/skills/` directory into your project:

```bash
cp -r .claude/skills/ /path/to/your/project/.claude/skills/
```

Or clone this repo and symlink individual skills:

```bash
ln -s /path/to/claude-personal-skills/.claude/skills/save-session /your/project/.claude/skills/save-session
```
