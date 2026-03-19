# Cookiy MCP Tool Design

`cookiy-mcp-tool-design` is a reusable Codex/Claude skill for designing and reviewing MCP or function-calling tools so agents can choose them correctly, pass the right parameters, recover from errors, and avoid unsafe side effects.

It turns a practical MCP design checklist into a lightweight skill you can invoke during tool design or review work.

## What This Skill Helps With

- Reviewing tool `name`, `description`, and `inputSchema`
- Reducing tool overlap and tool-count bloat
- Improving defaults, required fields, and enum/range constraints
- Designing agent-friendly output payloads and error messages
- Adding safeguards for destructive or side-effecting actions
- Deciding whether something should be a tool, a skill, or both

## Repository Structure

```text
.
├── README.md
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── mcp-tool-design-checklist.md
```

## Install

### Codex

Clone the repo and symlink it into your Codex skills directory:

```bash
git clone https://github.com/<your-org>/cookiy-mcp-tool-design.git
mkdir -p ~/.codex/skills
ln -s /absolute/path/to/cookiy-mcp-tool-design ~/.codex/skills/cookiy-mcp-tool-design
```

If you prefer copying instead of symlinking:

```bash
cp -R /absolute/path/to/cookiy-mcp-tool-design ~/.codex/skills/cookiy-mcp-tool-design
```

### Claude Code

Clone the repo and place it under your project's Claude skills directory:

```bash
git clone https://github.com/<your-org>/cookiy-mcp-tool-design.git
mkdir -p .claude/skills
ln -s /absolute/path/to/cookiy-mcp-tool-design .claude/skills/cookiy-mcp-tool-design
```

## How To Use

Invoke the skill explicitly when you are designing or reviewing MCP/function-calling tools.

Example prompts:

- `Use $cookiy-mcp-tool-design to review this MCP tool schema.`
- `Use $cookiy-mcp-tool-design to redesign these 12 tools into a smaller set.`
- `Use $cookiy-mcp-tool-design to make this destructive tool safer.`
- `Use $cookiy-mcp-tool-design to decide whether this should be a tool or a skill.`

You can also ask for focused reviews:

- naming clarity
- schema simplification
- defaults and required fields
- output/error design
- side-effect safety
- tool count and overlap

## Typical Review Flow

The skill follows this order:

1. Treat the tool surface as agent UI.
2. Reduce tool count and overlap before polishing schema details.
3. Make names, descriptions, and input schema easy for the model to distinguish.
4. Make outputs and errors useful for the model's next step.
5. Add guardrails for side effects.
6. Decide whether long guidance belongs in a skill instead of a tool description.

## Best Used For

- MCP server design
- Function-calling schema review
- Agent integration debugging
- Tool-surface simplification
- Safety review for write/delete/send operations

## Files

- `SKILL.md`: main workflow and trigger description
- `references/mcp-tool-design-checklist.md`: condensed review checklist
- `agents/openai.yaml`: UI metadata for skill discovery

## License

Add your preferred license before publishing.
