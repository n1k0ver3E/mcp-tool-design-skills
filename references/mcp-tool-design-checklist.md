# MCP Tool Design Checklist

Use this checklist when creating or reviewing MCP/function-calling tools.

## Core Lens

- The model cannot see your implementation.
- The model mostly reasons from `name`, `description`, schema, and returned payloads.
- Tool design is an Agent UX problem, not just an API wrapping problem.

## 1. Tool Set Size

- Prefer roughly `5-15` tools per server when possible.
- Merge overlapping tools before adding new ones.
- Move long operational guidance into a Skill/reference instead of stuffing descriptions.
- If a tool is rarely needed, ask whether it should be hidden behind a different workflow.

## 2. Naming

- Use verb-first, task-specific names like `create_github_issue` instead of vague names like `handle_issue`.
- Make names distinguishable from neighboring tools.
- Encode the real object or destination in the name when ambiguity is likely.

## 3. Description

- State what the tool does, when to use it, important defaults, and notable limits.
- Explain parameter formats that humans would otherwise infer.
- Mention side effects and irreversibility directly in the description.
- A misleading description is worse than a short description.

## 4. Input Schema

- Keep required fields to the minimum that truly cannot be defaulted.
- Prefer flatter schemas over deep nesting.
- Use `enum`, `minimum`, `maximum`, and explicit format guidance to reduce hallucinated inputs.
- Avoid provider-fragile constructs such as complex `oneOf`/`anyOf`/`$ref` unless verified.
- Put defaults in both schema and natural-language descriptions when clients may ignore schema defaults.

## 5. Executor Behavior

- Keep the schema strict, but parse inputs defensively in the implementation.
- Accept common harmless variants when practical.
- Validate everything before executing side effects.
- For stdio MCP servers, never write operational logs to `stdout`.

## 6. Output Design

- Return stable, structured fields when follow-up actions depend on them.
- Offer a simple/summary mode only when verbosity regularly hurts downstream reasoning.
- Do not force the model to re-parse large prose blobs if structured fields are available.

## 7. Error Design

- Treat errors as feedback, not just failures.
- For recoverable errors, include:
- what failed
- why it failed
- how to retry or which tool to use next
- For unrecoverable or permission/configuration issues, state that user action is required.
- Prefer structured error payloads when the next step depends on machine-readable routing.

## 8. Side-Effect Safety

- Add explicit confirmation parameters for destructive actions.
- Split read and write tools when that improves decision quality.
- Make irreversible effects obvious before invocation.

## 9. Skill vs Tool

- Prefer a Skill when the model needs long instructions, sequencing guidance, heuristics, or domain playbooks.
- Prefer a tool when the model needs deterministic execution against code, APIs, files, or services.
- Use both when the Skill should teach the model how and when to call the tools.

## Review Prompts

- Can the model tell when this tool should be used instead of adjacent tools?
- Which parameters are most likely to be guessed wrong?
- If the call fails, does the response help the model recover in one more step?
- Is this really one tool, or a thin wrapper around internal implementation details?
