---
name: mcp-tool-design-skills
description: Design and review MCP or function-calling tools for Agent reliability. Use when creating or auditing tool names, descriptions, input schemas, defaults, error payloads, destructive-operation safeguards, tool count, or when deciding whether a Skill should replace more tools.
---

# MCP Tool Design Skills

Use this skill when an Agent tool works technically but is hard for the model to choose or call correctly.

## Workflow

1. Treat the tool surface as Agent UI.
- Review only what the model can see: `name`, `description`, input schema, and returned payloads.

2. Reduce selection and input burden first.
- Check whether the server exposes too many overlapping tools.
- Prefer fewer, clearer tools over thin wrappers around every internal API.
- Keep required parameters minimal and state defaults explicitly.

3. Make schemas LLM-friendly.
- Use semantic tool names and concrete parameter descriptions.
- Prefer flat, predictable schemas over deeply nested, highly dynamic ones.
- Use enums, ranges, and examples to constrain ambiguous fields.
- Avoid fragile schema features unless you have verified the target client/provider supports them.

4. Design outputs and errors for recovery.
- Return structured success payloads when downstream steps need stable fields.
- Make recoverable errors actionable: explain what failed, why, and how the Agent should retry.
- Mark user-action/configuration blockers clearly so the Agent does not loop.

5. Add safeguards for side effects.
- For destructive or external-write operations, require explicit confirmation-style parameters.
- Separate read-only discovery tools from mutating tools when possible.

6. Decide whether this should stay a tool.
- If the problem is mostly guidance, sequencing, best practices, or long instructions, prefer a Skill or reference doc instead of pushing more text into tool descriptions.

## References

- `references/mcp-tool-design-checklist.md`
