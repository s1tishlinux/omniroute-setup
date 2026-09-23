# 🐋 Desktop Apps & IDE Extensions Compatibility Guide

## 📌 Overview

OmniRoute acts as a **Universal Gateway**, allowing any Desktop Application or IDE Extension that supports custom OpenAI or Anthropic API base URLs to connect directly to local Spark MLX, local Ollama, or multi-provider cloud fallback combos.

---

## 🌐 Compatibility Matrix

| App / Extension | Support Status | Base Endpoint | Model Input |
| :--- | :---: | :--- | :--- |
| **Codex Desktop App** | ✅ Supported | `http://localhost:20128/v1` | `combo-pro-coding` / `XHToken/Spark-X2.5-1.7B` |
| **Claude Desktop App** | ✅ Supported | `http://localhost:20128` | Custom Anthropic endpoint in config |
| **Jan AI / LM Studio** | ✅ Supported | `http://localhost:20128/v1` | Custom OpenAI model |
| **Open WebUI** | ✅ Supported | `http://localhost:20128/v1` | Add OpenAI Connection |
| **Chatbox / MindMac** | ✅ Supported | `http://localhost:20128/v1` | OpenAI API Compatible |
| **VS Code Copilot Chat** | ✅ Supported | Via `OmniCopilot` | Select combo from dropdown |
| **Continue.dev (VS Code/JetBrains)** | ✅ Supported | `http://localhost:20128/v1` | Configured in `~/.continue/config.yaml` |
| **Cursor IDE** | ✅ Supported | `http://localhost:20128/v1` | Cursor Settings ➔ Override OpenAI Base URL |
| **Cline / Roo Code / Kilo Code** | ✅ Supported | `http://localhost:20128/v1` | Provider: OpenAI Compatible |
| **Zed Editor** | ✅ Supported | `http://localhost:20128/v1` | `settings.json` ➔ `language_models` ➔ `openai` |

---

## ⚙️ App Configuration Walkthroughs

### 1. Codex Desktop App
1. Open **Codex Desktop** ➔ Settings ➔ Advanced / Custom Providers.
2. Base URL: `http://localhost:20128/v1`
3. API Key: `sk-omniroute-local`
4. Enter Model: `combo-pro-coding`

### 2. Claude Desktop App
Edit `~/Library/Application Support/Claude/claude_desktop_config.json`:
```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "http://localhost:20128"
  }
}
```

### 3. VS Code Copilot Chat (Native Integration)
Install **OmniCopilot** bridge extension:
```bash
code --install-extension diegosouzapw.omnicopilot
```
Populates all 6 combos directly in native VS Code Copilot Chat dropdown menu.

### 4. Cursor IDE
1. Open **Cursor Settings** ➔ **Models**.
2. Override OpenAI Base URL: `http://localhost:20128/v1`.
3. API Key: `sk-omniroute-local`.
4. Models: `combo-pro-coding`, `combo-deep-reasoning`, `combo-fast-chat`, `combo-vision-multimodal`, `combo-ultra-heavy`, `combo-zero-cost`.
