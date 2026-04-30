# Pierced Tongue 🗣️

**Transport layer and protocol orchestration for model-to-model communication.** Free. Open source. MIT licensed.

Required dependency for `throw-rock` skill. Handles low-level message passing between models.

Skills compound. Prompts don't.

## Install

### Hermes Agent
```bash
git clone https://github.com/wizzleweasel/pierced-tongue.git ~/.hermes/skills/pierced-tongue
```

### OpenClaw
```bash
git clone https://github.com/wizzleweasel/pierced-tongue.git ~/.openclaw/skills/pierced-tongue
```

### Other Claw Agents
```bash
git clone https://github.com/wizzleweasel/pierced-tongue.git
# Copy skills/ directory to your agent's skills path
```

## What It Does

| Layer | Purpose | Protocols |
|-------|---------|-----------|
| Transport | HTTP, stdio, ACP | Low-level message passing |
| Protocol | Message formats (JSON, Compact, Symbolic) | Standard, Tier1, Tier2 |
| Orchestration | Multi-model coordination | Helper model management |

## When to Use

Required for `throw-rock` skill. Use when:
- Setting up multi-model delegation
- Configuring model transport protocols
- Need low-level message passing between models
- Debugging throw-rock communication

## How It Works

Pierced Tongue is the **transport layer** that throw-rock uses for model-to-model communication:

```
throw-rock (protocol) 
    ↓ uses
pierced-tongue (transport)
    ↓ communicates via
HTTP / stdio / ACP
    ↓ reaches
Helper Model (Groq, etc.)
```

Each skill is a `SKILL.md` file following the [Agent Skills](https://agentskills.io) open standard:

```yaml
---
name: pierced-tongue
description: "Transport layer for model-to-model communication. Required for throw-rock. Use when: setting up multi-model delegation; configuring transport protocols"
---
```

The description tells agents **when** to use the skill. The body provides full transport protocols, message formats, and orchestration workflow.

Works with **Hermes Agent, OpenClaw, Claude, ChatGPT, Copilot, Cursor, Windsurf** — any tool that supports the Agent Skills standard or MCP.

## Quality

Reviewed against agent skill best practices:

- **Clear triggers** — "Use when:" terms for reliable discovery
- **Agent-first** — Transport protocols only, no explanations agents already know
- **Concise** — Protocol details only, no fluff
- **Output contracts** — Declared formats for composability

## Catalog

1 skill for model transport:

| Skill | Description |
|-------|-------------|
| pierced-tongue | Transport layer and protocol orchestration (HTTP/stdio/ACP) |

## Test

```bash
# Hermes Agent
hermes --test-skill pierced-tongue

# OpenClaw
openclaw --test-skill pierced-tongue

# Generic smoke test
python scripts/pierced-tongue.py --test
```

---

**Skills compound. Prompts don't.** Pierced Tongue provides the transport layer so throw-rock can focus on protocol.
