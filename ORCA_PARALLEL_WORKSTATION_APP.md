# 🐋 Orca Parallel Agent Workstation App Setup & Usage Guide

## 📌 Overview

**Orca Parallel Agent Workstation** (`/Applications/Orca.app`) is a multi-agent desktop application for macOS. It orchestrates parallel autonomous coding agents (**Codex**, **Claude**, **Claude Agent Teams**, **OpenClaude**, **GitHub Copilot**, **OpenClaw**, **Qwen Code**, **Aider**) across multiple Git worktrees and repositories simultaneously.

---

## 🏗️ Architecture: Orca Parallel Agents + OmniRoute Gateway

```
+-------------------------------------------------------------------------------------------------+
|                                 ORCA PARALLEL AGENT WORKSTATION                                 |
|                                    (/Applications/Orca.app)                                     |
|                                                                                                 |
|   +-------------------+  +-------------------+  +-------------------+  +---------------------+  |
|   |  Parallel Agent 1 |  |  Parallel Agent 2 |  |  Parallel Agent 3 |  |   Parallel Agent 4  |  |
|   |  (Codex Worker 1) |  |  (Codex Worker 2) |  |  (Claude Worker)  |  |  (OpenClaude Worker)|  |
|   |   Worktree: feature  |   Worktree: bugfix   |   Worktree: refactor |   Worktree: docs        |  |
|   +---------+---------+  +---------+---------+  +---------+---------+  +----------+----------+  |
+-------------|----------------------|----------------------|-----------------------|-------------+
              |                      |                      |                       |
              +----------------------+----------------------+-----------------------+
                                             |
                                             v
                           +------------------------------------+
                           |         OmniRoute Gateway          |
                           |  http://localhost:20128/v1 (OpenAI)|
                           |  http://localhost:20128 (Anthropic)|
                           +-----------------+------------------+
                                             |
                   +-------------------------+-------------------------+
                   |                                                   |
                   v                                                   v
  +---------------------------------+                 +---------------------------------+
  |        Orca Spark MLX           |                 |  6 Multi-Provider Fallbacks     |
  |    http://127.0.0.1:8080/v1     |                 |  • combo-pro-coding             |
  |    Model: XHToken/Spark-X2.5    |                 |  • combo-deep-reasoning         |
  +---------------------------------+                 +---------------------------------+
```

---

## ⚙️ How Orca Parallel Agents Connect to OmniRoute

Orca's internal agent runtime homes (e.g. `/Users/satishgundu/Library/Application Support/orca/codex-runtime-home/home/config.toml`) are configured to point all parallel worker instances directly to **OmniRoute Gateway**:

- **OpenAI Base URL**: `http://127.0.0.1:20128/v1`
- **Default Model**: `combo-pro-coding`
- **Fallback Chains**: Gemini 3.6 Flash ➔ Mistral Codestral ➔ Groq Llama 3.3 70B
- **Local Apple Silicon Model**: `XHToken/Spark-X2.5-1.7B` (`http://127.0.0.1:8080/v1`)

---

## 🎯 Getting Started Checklist in Orca.app

1. **Open Orca.app**:
   Launch Orca from `/Applications/Orca.app` or via spotlight.
2. **Select Default Agent**:
   In Orca's onboarding screen:
   - Select **Codex** (or **Claude**, **OpenClaude**, **orca claude-teams**).
3. **Turn on Notifications**:
   Enable desktop notifications for completed turn events.
4. **Choose Execution Mode**:
   - **YOLO / Dangerously Skip Permissions**: For autonomous parallel multi-repo work without prompt approvals.
5. **Start Parallel Work**:
   Create multiple parallel tasks across different Git branches or worktrees. All token requests will automatically route through OmniRoute with zero API costs!

---

## 📄 Orca Internal Agent Runtime Config (`config.toml`)

Stored at `/Users/satishgundu/Library/Application Support/orca/codex-runtime-home/home/config.toml`:

```toml
model = "combo-pro-coding"
model_provider = "omniroute"
model_reasoning_effort = "max"
model_context_window = 2000000
model_auto_compact_token_limit = 1700000
tool_output_token_limit = 32768
skills_context_budget = 0.25
personality = "pragmatic"
approvals_reviewer = "user"

model_catalog_json = "/Users/satishgundu/.codex/ollama-launch-models.json"

openai_base_url = "http://127.0.0.1:20128/v1"
sandbox_mode = "danger-full-access"

[model_providers.omniroute]
name = "OmniRoute"
base_url = "http://localhost:20128/v1"
supports_websockets = false
api_key = "sk-omniroute-local"
```

---

## 🧪 Terminal Verification for Orca App Workers

Verify that Orca parallel agent workers can send requests to OmniRoute:

```bash
curl -i -X POST http://localhost:20128/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer sk-omniroute-local" \
  -d '{
    "model": "combo-pro-coding",
    "messages": [
      {"role": "system", "content": "You are a parallel agent worker running inside Orca.app."},
      {"role": "user", "content": "Return string \"Orca App Agent Worker OK\""}
    ]
  }'
```

**Response**:
```json
{
  "id": "chatcmpl-pVuzauinO-miqfkP6M2-uQ4",
  "object": "chat.completion",
  "created": 1790139305,
  "model": "gemini-3.6-flash",
  "choices": [
    {
      "index": 0,
      "finish_reason": "stop",
      "message": {
        "role": "assistant",
        "content": "Orca App Agent Worker OK"
      }
    }
  ]
}
```
