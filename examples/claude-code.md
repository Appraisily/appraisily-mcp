# Claude Code Setup

Add the hosted read-only Appraisily MCP server in one command:

```bash
claude mcp add --transport http appraisily https://mcp.appraisily.com/mcp
```

Install the agent skill:

```bash
mkdir -p ~/.claude/skills/appraisily-mcp
curl -fsSL https://raw.githubusercontent.com/Appraisily/appraisily-mcp/main/skills/appraisily-mcp/SKILL.md -o ~/.claude/skills/appraisily-mcp/SKILL.md
```
