<p align="center">
  <img src="https://em-content.zobj.net/source/apple/391/speaking-head_1f5e3.png" width="120" />
</p>

<h1 align="center">pierced-tongue</h1>

<p align="center">
  <strong>transport layer for model-to-model talk</strong>
</p>

<p align="center">
  <a href="https://github.com/wizzleweasel/pierced-tongue/stargazers"><img src="https://img.shields.io/github/stars/wizzleweasel/pierced-tongue?style=flat&color=yellow" alt="Stars"></a>
  <a href="https://github.com/wizzleweasel/pierced-tongue/commits/main"><img src="https://img.shields.io/github/last-commit/wizzleweasel/pierced-tongue?style=flat" alt="Last Commit"></a>
  <a href="LICENSE"><img src="https://img.shields.io/github/license/wizzleweasel/pierced-tongue?style=flat" alt="License"></a>
</p>

<p align="center">
  <a href="#before--after">Before/After</a> •
  <a href="#install">Install</a> •
  <a href="#transport-protocols">Protocols</a> •
  <a href="#required-skills">Dependencies</a> •
  <a href="#benchmarks">Benchmarks</a>
</p>

<p align="center">
  <strong>🗣️ Pierced Tongue Ecosystem</strong> &nbsp;·&nbsp;
  <strong>pierced-tongue</strong> <em>transport layer</em> <sub>(you are here)</sub> &nbsp;·&nbsp;
  <a href="https://github.com/wizzleweasel/throw-rock">throw-rock</a> <em>throw compressed</em> &nbsp;·&nbsp;
  <a href="https://github.com/juliusbrussee/caveman">caveman</a> <em>talk less</em>
</p>

---

Transport layer and protocol orchestration for **model-to-model communication**. Required dependency for `throw-rock` skill.

**Works with:** Hermes Agent ✅ | OpenClaw ✅ | Other Claw Agents ✅

Based on the observation that efficient transport = faster delegation. So we made it a one-line install.

## Before / After

<table>
<tr>
<td width="50%">

### 🗣️ Without Transport Layer (150+ tokens)

> "I need to send this task to the helper model using HTTP API, but I also need to handle timeouts, retries, and different message formats like JSON or symbolic notation..."

</td>
<td width="50%">

### 🗣️ With Pierced Tongue (30 tokens)

> `transport.send(target, msg)` → done. HTTP/stdio/ACP handled.

</td>
</tr>
<tr>
<td>

### 🗣️ Normal Multi-Model

> "Set up the helper model with Groq API, configure the base URL, handle authentication, then send the task in compact JSON format..."

</td>
<td>

### 🗣️ Pierced Tongue

> `orchestrator.delegate(task)` → auto transport. That it.

</td>
</tr>
</table>

**Same delegation. 80% less setup. Transport abstracted.**

## Install

Pick your agent. One command. Done.

| Agent | Install |
|-------|---------|
| **Hermes Agent** | `git clone https://github.com/wizzleweasel/pierced-tongue.git ~/.hermes/skills/pierced-tongue` |
| **OpenClaw** | `git clone https://github.com/wizzleweasel/pierced-tongue.git ~/.openclaw/skills/pierced-tongue` |
| **Claude Code** | `git clone https://github.com/wizzleweasel/pierced-tongue.git skills/` |
| **Any other** | `git clone https://github.com/wizzleweasel/pierced-tongue.git` |

Install once. Use in every delegation after that. One command. That it.

### What You Get

Auto-activation for transport setup. Pierced Tongue auto-starts when:
- Setting up multi-model delegation
- Configuring model transport protocols
- Need low-level message passing between models

## Transport Protocols

<table>
<tr>
<td width="33%">

#### 🌐 HTTP Transport

- Direct HTTP API calls
- REST-style communication
- Timeout + retry built-in
- Good for: cloud APIs, Groq, OpenAI

</td>
<td width="33%">

#### 💻 stdio Transport

- Subprocess communication
- stdin/stdout messaging
- Zero network overhead
- Good for: local models, CLI tools

</td>
<td width="33%">

#### 🔗 ACP (Agent Communication Protocol)

- Structured agent protocol
- Version negotiation
- Message IDs + delivery
- Good for: multi-agent systems

</td>
</tr>
</table>

## Message Formats

Pierced Tongue supports multiple formats (matches throw-rock tiers):

| Format | Example | Savings |
|--------|---------|---------|
| **Standard JSON** | `{"action":"rf","params":{"path":"main.py"}}` | 10% |
| **Compact (Tier1)** | `{"a":"rf","p":{"pa":"main.py"}}` | 27% |
| **Symbolic (Tier2)** | `→rf(pa:main.py)∴` | 56% ✅ |

## Required Skills

Pierced Tongue enables these skills:

### 🪨 throw-rock (Delegation Protocol)
**Enabled by:** Pierced Tongue transport  
**Install:** `git clone https://github.com/wizzleweasel/throw-rock.git`  
**Purpose:** Model-to-model delegation with compressed formats

### 🪨 caveman (Language Compression)
**Works with:** Pierced Tongue transport  
**Install:** `git clone https://github.com/juliusbrussee/caveman.git`  
**Purpose:** 75% token savings via caveman-speak

### Dependency Chain
```
throw-rock (protocol)
    ↓ uses
pierced-tongue (transport) ← YOU ARE HERE
    ↓ communicates via
HTTP / stdio / ACP
    ↓ reaches
Helper Model (Groq, etc.)
```

## Benchmarks

| Scenario | Without Transport | With Pierced Tongue | Savings |
|----------|-------------------|---------------------|---------|
| Setup delegation | ~150 tokens | ~30 tokens | **80%** |
| Send message (HTTP) | ~100 tokens | ~40 tokens | **60%** |
| Multi-model config | ~200 tokens | ~50 tokens | **75%** |

```
┌─────────────────────────────────────┐
│  SETUP TOKENS SAVED    ████████ 80% │
│  TRANSPORT ABSTRACTED  ████████ 100%│
│  SPEED INCREASE        ████████ ~3x │
│  VIBES                 ████████ OOG │
└─────────────────────────────────────┘
```

- **Faster setup** — transport abstracted = speed go brrr
- **Easier to use** — `transport.send()` not 50 lines of code
- **Same capability** — all transport methods supported
- **Save tokens** — 80% less setup cost
- **Fun** — every delegation become simple

## Integration

### Hermes Agent
```yaml
# ~/.hermes/config.yaml
delegation:
  transport: pierced-tongue  # Use pierced-tongue
  message_format: symbolic   # Tier2 format
```

### OpenClaw
```yaml
# ~/.openclaw/config.yaml
models:
  helper:
    transport: pierced-tongue
    format: symbolic
```

### Generic Usage
```python
from pierced_tongue import Transport

transport = Transport(config='config.yaml')
result = transport.send(
    target='groq-helper',
    message='→rf(pa:main.py)∴'
)
```

## Testing

```bash
# Smoke test
python scripts/pierced-tongue.py --test

# Hermes Agent
hermes --test-skill pierced-tongue

# OpenClaw
openclaw --test-skill pierced-tongue
```

---

**Skills compound. Prompts don't.** Pierced Tongue provides the transport so throw-rock can focus on protocol. 🗣️