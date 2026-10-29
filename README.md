# asms agent skills

[Agent skills](https://agentskills.io) that teach AI coding agents to use [asms.ai](https://asms.ai) phone numbers: Claude Code, Codex, Cursor, Gemini CLI, GitHub Copilot and any agent that reads `SKILL.md` files.

| Skill | What it does |
|---|---|
| [`sms-verification`](skills/sms-verification/SKILL.md) | Get a one-time number and the SMS verification code for a service |
| [`rent-phone-number`](skills/rent-phone-number/SKILL.md) | Rent a number for a day to a year and read the SMS it receives |

## Install

```bash
npx skills add asms-ai/agent-skills
```

Or copy a skill folder into your agent's skills directory (for Claude Code, `~/.claude/skills/` or `.claude/skills/` in a project).

The skills work best with the asms MCP server connected. Setup for each client: **[asms.ai/agents](https://asms.ai/agents)**. Without it, `sms-verification` falls back to the [REST API](https://asms.ai/api-docs) with an API key in `ASMS_API_KEY`.

The skills always ask the user before anything that spends from their wallet, and never create accounts or keys on their behalf.

## License

MIT
