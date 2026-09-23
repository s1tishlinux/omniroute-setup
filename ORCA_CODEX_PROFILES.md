# 🐋 Orca Codex CLI Profiles & Launcher Guide

## 📌 Overview

This document provides a dedicated guide for configuring and running **OpenAI Codex CLI** (`codex`) with Orca Spark MLX profiles, custom model catalogs, and OmniRoute fallback routing on macOS.

---

## ⚙️ Profile Files Location

Codex CLI profile configuration files are stored in `~/.codex/`:

| Profile File | Purpose | Target Provider / Server |
| :--- | :--- | :--- |
| `~/.codex/config.toml` | Base user configuration | OmniRoute default (`http://127.0.0.1:20128/v1`) |
| `~/.codex/orca-spark.config.toml` | Orca Spark MLX profile | OmniRoute gateway / Spark MLX |
| `~/.codex/ollama-launch-models.json` | Model selector catalog | Enables `/model` interactive picker |

---

## 📄 Profile 1: Orca Spark Profile (`~/.codex/orca-spark.config.toml`)

```toml
model = "XHToken/Spark-X2.5-1.7B"
model_provider = "omniroute"
model_reasoning_effort = "max"
model_context_window = 32768
tool_output_token_limit = 16384
skills_context_budget = 0.25

openai_base_url = "http://localhost:20128/v1"
api_key = "sk-omniroute-local"
```

---

## 📄 Profile 2: Model Selector Catalog (`~/.codex/ollama-launch-models.json`)

To enable custom models and combos in Codex CLI's interactive `/model` menu, add model definitions to `ollama-launch-models.json`:

```json
{
  "models": [
    {
      "slug": "combo-pro-coding",
      "display_name": "combo-pro-coding (OmniRoute)",
      "description": "OmniRoute Pro Coding Chain (Gemini 3.6 Flash -> Mistral -> Groq)",
      "context_window": 2000000,
      "max_context_window": 2000000,
      "auto_compact_token_limit": 1700000,
      "input_modalities": ["text", "image"],
      "priority": 100
    },
    {
      "slug": "XHToken/Spark-X2.5-1.7B",
      "display_name": "Spark-X2.5-1.7B (Local Spark MLX)",
      "description": "Local Spark MLX 1.7B Apple Silicon GPU Model (http://127.0.0.1:8080/v1)",
      "context_window": 32768,
      "max_context_window": 32768,
      "auto_compact_token_limit": 30000,
      "input_modalities": ["text"],
      "priority": 93
    }
  ]
}
```

---

## 🚀 Execution Commands

### 1. Launch Interactive Orca Session
```bash
cd /Users/satishgundu/orca
codex --profile orca-spark
```

### 2. Run Non-Interactive Task with Verified Output
```bash
cd /Users/satishgundu/orca
codex --profile orca-spark exec --skip-git-repo-check "Return text 'Orca Spark integration verified!'"
```

**Verified Log Output**:
```text
OpenAI Codex v0.155.1
--------
workdir: /Users/satishgundu/orca
model: XHToken/Spark-X2.5-1.7B
provider: omniroute
session id: 01a0cca5-299c-7863-a460-45b52322ab4f
--------
user: Return text 'Orca Spark integration verified!'
codex: Orca Spark integration verified!
tokens used: 6,075
```
