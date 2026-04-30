---
name: pierced-tongue
description: "Transport layer and protocol orchestration for model-to-model communication. Required dependency for throw-rock skill. Use when: setting up multi-model delegation; configuring model transport protocols; need low-level message passing between models; debugging throw-rock communication"
version: 1.0.0
author: Wicak + Hermes
license: MIT
metadata:
  hermes:
    tags: [llm, transport, protocol, orchestration, multi-model]
    related_skills: [throw-rock]
  openclaw:
    tags: [llm, transport, multi-model]
    skill_type: model_management
  claw:
    standard_version: "1.0"
    required_tools: [delegate_task, model_call, http_request]
---

# Pierced Tongue 🗣️

**Transport layer and protocol orchestration for model-to-model communication.**

Required dependency for `throw-rock` skill. Handles low-level message passing between models using multiple transport protocols.

## When to Use

- Setting up multi-model delegation
- Configuring LLM transport protocols (HTTP, stdio, ACP)
- Need low-level message passing between models
- Debugging `throw-rock` communication issues
- Orchestrating helper models for batch processing

## Transport Protocols

### 1. HTTP Transport (Default)
Direct HTTP API calls between models:

```python
import requests

def http_transport(target_url, message, headers):
    """Send message via HTTP POST."""
    response = requests.post(
        f"{target_url}/v1/chat/completions",
        headers=headers,
        json={
            "model": "llama-3.3-70b-versatile",
            "messages": [{"role": "user", "content": message}],
            "max_tokens": 500
        }
    )
    return response.json()
```

### 2. stdio Transport
Subprocess-based communication via stdin/stdout:

```python
import subprocess

def stdio_transport(command, message):
    """Send message via subprocess stdin."""
    proc = subprocess.Popen(
        command,
        stdin=subprocess.PIPE,
        stdout=subprocess.PIPE,
        stderr=subprocess.PIPE,
        text=True
    )
    stdout, stderr = proc.communicate(input=message)
    return stdout
```

### 3. ACP (Agent Communication Protocol)
Structured protocol for agent-to-agent communication:

```python
# ACP format
acp_message = {
    "version": "1.0",
    "source": "hermes-agent",
    "target": "groq-helper",
    "message_id": "unique_id",
    "payload": {
        "format": "machine-json",
        "data": {"action": "read_file", "params": {...}}
    }
}
```

## Message Formats

Pierced Tongue supports multiple message formats (matches throw-rock tiers):

### Format 1: Standard JSON
```json
{"action": "read_file", "params": {"path": "main.py"}}
```

### Format 2: Compact Short-Key (Tier 1)
```json
{"a":"rf","p":{"pa":"main.py"}}
```

### Format 3: Symbolic Notation (Tier 2)
```
→rf(pa:main.py)∴
```

## Protocol Orchestration

### Setup Multi-Model Config

```yaml
# ~/.hermes/pierced-tongue-config.yaml
models:
  main:
    provider: openrouter
    model: tencent/hy3-preview
  helper:
    provider: groq
    model: llama-3.3-70b-versatile
    transport: http
    base_url: https://api.groq.com/openai/v1
    api_key_env: GROQ_API_KEY
  transport:
    timeout: 30
    retry_attempts: 3
    format: symbolic  # or compact, standard
```

### Orchestration Workflow

```python
class LLMOrchestrator:
    def __init__(self, config_path):
        self.config = load_config(config_path)
        self.transport = self._init_transport()
    
    def delegate(self, task, target_model="helper"):
        """Delegate task to target model."""
        # 1. Translate to compact format
        message = self._to_format(task, self.config['transport']['format'])
        
        # 2. Send via transport
        response = self.transport.send(
            target=self.config['models'][target_model],
            message=message
        )
        
        # 3. Parse response
        return self._parse_response(response)
    
    def _to_format(self, task, fmt):
        if fmt == "symbolic":
            return self._to_symbolic(task)
        elif fmt == "compact":
            return self._to_compact_json(task)
        return json.dumps(task)
```

## Integration with throw-rock

Pierced Tongue is the **required transport layer** for throw-rock:

```
throw-rock (protocol) 
    ↓ uses
pierced-tongue (transport)
    ↓ communicates via
HTTP / stdio / ACP
    ↓ reaches
Helper Model (Groq, etc.)
```

### Usage Together

```python
# throw-rock delegates task
task = {"action": "analyze", "params": {"path": "/app"}}

# pierced-tongue handles transport
orchestrator = LLMOrchestrator("pierced-tongue-config.yaml")
result = orchestrator.delegate(task)
```

## Hermes Agent Integration

### Configuration

```yaml
# ~/.hermes/config.yaml
delegation:
  provider: groq
  model: llama-3.3-70b-versatile
  transport: pierced-tongue  # Use pierced-tongue as transport
  base_url: https://api.groq.com/openai/v1
  api_key_env: GROQ_API_KEY
  message_format: symbolic  # Tier 2 format
```

### Using with delegate_task

```python
from hermes_tools import delegate_task

# delegate_task automatically uses pierced-tongue transport
result = delegate_task(
    goal='{"a":"tr","p":{"cmd":"ls -la"}}',  # Compact format
    toolsets=["terminal"],
    context="Machine-language agent. JSON only."
)
```

## OpenClaw Integration

```yaml
# ~/.openclaw/config.yaml
models:
  helper:
    transport: pierced-tongue
    format: symbolic
```

## Scripts

### `scripts/pierced-tongue.py`

Generic transport layer implementation:

```python
#!/usr/bin/env python3
"""Pierced Tongue - LLM Transport Layer."""
import json
import os
import requests
from typing import Dict, Any

class Transport:
    def __init__(self, config: Dict[str, Any]):
        self.config = config
    
    def send(self, target: Dict[str, Any], message: str) -> str:
        """Send message to target model."""
        transport_type = target.get('transport', 'http')
        
        if transport_type == 'http':
            return self._http_send(target, message)
        elif transport_type == 'stdio':
            return self._stdio_send(target, message)
        else:
            raise ValueError(f"Unknown transport: {transport_type}")
    
    def _http_send(self, target, message):
        headers = {
            'Authorization': f"Bearer {os.getenv(target['api_key_env'])}",
            'Content-Type': 'application/json'
        }
        payload = {
            'model': target['model'],
            'messages': [{'role': 'user', 'content': message}],
            'max_tokens': 500
        }
        resp = requests.post(
            f"{target['base_url']}/chat/completions",
            headers=headers,
            json=payload
        )
        return resp.json()['choices'][0]['message']['content']

def main():
    print("🗣️ Pierced Tongue - LLM Transport Layer")
    print("Required dependency for throw-rock skill")
    # Implementation here...

if __name__ == "__main__":
    main()
```

## Smoke Test

Run basic smoke test to verify transport works:

```bash
python scripts/pierced-tongue.py --test
```

Expected output:
```
🗣️ Pierced Tongue Smoke Test
✅ HTTP transport initialized
✅ stdio transport initialized
✅ Message format: symbolic
✅ Transport layer ready
```

## Pitfalls

- ❌ **Missing API keys** — Ensure `GROQ_API_KEY` or equivalent is set
- ❌ **Transport mismatch** — Main and helper must use compatible transports
- ❌ **Message format drift** — Both sides must agree on format (symbolic/compact/standard)
- ✅ **Timeout handling** — Set reasonable timeouts for HTTP transport
- ✅ **Retry logic** — Implement retries for transient failures

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

**Skills compound. Prompts don't.** Pierced Tongue provides the transport layer so throw-rock can focus on protocol. Together they enable efficient model-to-model communication.