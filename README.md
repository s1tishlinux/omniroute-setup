# 🚀 OmniRoute, Ollama & Multi-IDE AI Routing Setup Guide

A complete, production-ready guide for installing, configuring, and wiring **OmniRoute**, **Ollama**, and developer IDEs (**VS Code**, **Continue.dev**, **GitHub Copilot**, **Cursor**, **Claude Code**, **Codex CLI**) using segregated free LLM tiers, local fast autocomplete, and fallback model chains on macOS.

---

## 📋 Table of Contents
1. [Architecture & Topology](#-architecture--topography)
2. [Prerequisites](#-prerequisites)
3. [Step-by-Step Execution Guide](#-step-by-step-execution-guide)
   - [Step 1: Install & Verify Core Services](#step-1-install--verify-core-services)
   - [Step 2: Register Developer API Keys](#step-2-register-developer-api-keys)
   - [Step 3: Create Segregated Routing Combos](#step-3-create-segregated-routing-combos)
   - [Step 4: Wire Continue.dev Extension](#step-4-wire-continuedev-extension)
   - [Step 5: Wire GitHub Copilot Chat (Native VS Code Integration)](#step-5-wire-github-copilot-chat-native-vs-code-integration)
   - [Step 6: Create Ignore Filters (.continueignore & .cursorignore)](#step-6-create-ignore-filters-continueignore--cursorignore)
   - [Step 7: Verification & Testing](#step-7-verification--testing)
4. [IDE & Terminal Agent Integration Setup](#-ide--terminal-agent-integration-setup)
   - [VS Code & GitHub Copilot](#vs-code--github-copilot)
   - [Cursor IDE](#cursor-ide)
   - [Continue.dev Extension](#continuedev-extension)
   - [Claude Code & Codex CLI](#claude-code--codex-cli)
5. [OmniRoute Web Dashboard Overview](#-omniroute-web-dashboard-overview)
6. [CLI Command Reference](#-cli-command-reference)
7. [Troubleshooting](#-troubleshooting)

---

## 🏗️ Architecture & Topology

```
+-------------------------------------------------------------------------+
|                               Local Host                                |
|                                                                         |
|  +------------------------+             +----------------------------+  |
|  |   Ollama Local Server  |             |      OmniRoute Gateway     |  |
|  | http://localhost:11434  |             |   http://localhost:20128   |  |
|  +-----------+------------+             +--------------+-------------+  |
|              |                                         |                |
|    Local Tab Autocomplete                     Segregated Model Combos   |
|   (qwen2.5-coder:7b-base)                   RTK Token Compression      |
|              |                                         |                |
+--------------+-----------------------------------------+----------------+
               |                                         |
               v                                         v
   +-----------------------+                 +-----------------------+
   |  Continue.dev / IDE   |                 | Upstream API Providers|
   | (VS Code / Cursor)    |                 | (Gemini, Groq, GitHub)|
   +-----------------------+                 +-----------------------+
```

---

## 🛠️ Prerequisites

- **macOS** (Apple Silicon or Intel)
- **Homebrew** installed (`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`)
- **Node.js** `>= 20.0.0`
- Developer API keys:
  - Google Gemini API Key
  - Groq API Key
  - GitHub Personal Access Token (PAT) / GitHub Models

---

## 🚀 Step-by-Step Execution Guide

### Step 1: Install & Verify Core Services

1. **Verify Homebrew & Install Ollama**:
   ```bash
   brew install ollama
   brew services start ollama
   ```

2. **Pull the Local Autocomplete Model**:
   ```bash
   ollama pull qwen2.5-coder:7b-base
   ```

3. **Install OmniRoute Globally**:
   ```bash
   npm install -g omniroute@latest --include=optional
   ```

4. **Start OmniRoute Service**:
   ```bash
   omniroute start
   ```

---

### Step 2: Register Developer API Keys

Add your provider keys using the OmniRoute CLI:

```bash
# Google Gemini API Key
omniroute keys add gemini "YOUR_GEMINI_API_KEY"

# Groq API Key
omniroute keys add groq "YOUR_GROQ_API_KEY"

# GitHub Models PAT
omniroute keys add github "YOUR_GITHUB_PAT"
```

To list registered keys:
```bash
omniroute keys list
```

---

### Step 3: Create Segregated Routing Combos

Execute the following commands to create isolated, fallback-enabled model chains:

1. **Pro Coding Combo** (Primary: Gemini 3.6 Flash → Fallback: Groq Llama 3.3 70B):
   ```bash
   omniroute combo create combo-pro-coding --strategy priority --models "gemini/gemini-3.6-flash,groq/llama-3.3-70b-versatile"
   ```

2. **Deep Reasoning Combo** (Primary: DeepSeek-R1 via GitHub → Fallback: Gemini 3.6 Pro):
   ```bash
   omniroute combo create combo-deep-reasoning --strategy priority --models "github/deepseek-r1,gemini/gemini-3.6-pro"
   ```

3. **Fast Chat Combo** (Primary: Groq Llama 3.3 70B → Fallback: Codestral):
   ```bash
   omniroute combo create combo-fast-chat --strategy priority --models "groq/llama-3.3-70b-versatile,mistral/codestral-2501"
   ```

Verify created combos:
```bash
omniroute combo list
```

---

### Step 4: Wire Continue.dev Extension

Create or update `~/.continue/config.yaml`:

```yaml
models:
  - name: "Pro Coding (Gemini 3.6 Flash -> Groq)"
    provider: "openai"
    model: "combo-pro-coding"
    apiBase: "http://localhost:20128/v1"
    apiKey: "sk-omniroute-local"

  - name: "Deep Reasoning (DeepSeek-R1)"
    provider: "openai"
    model: "combo-deep-reasoning"
    apiBase: "http://localhost:20128/v1"
    apiKey: "sk-omniroute-local"

  - name: "Fast Iteration (Groq 70B)"
    provider: "openai"
    model: "combo-fast-chat"
    apiBase: "http://localhost:20128/v1"
    apiKey: "sk-omniroute-local"

  - name: "Spark MLX"
    provider: "openai"
    model: "XHToken/Spark-X2.5-1.7B"
    apiBase: "http://127.0.0.1:8080/v1"

tabAutocompleteModel:
  title: "Local Qwen 7B Autocomplete"
  provider: "ollama"
  model: "qwen2.5-coder:7b-base"
  apiBase: "http://localhost:11434"
```

---

### Step 5: Wire GitHub Copilot Chat (Native VS Code Integration)

Install the official **OmniCopilot** bridge extension:

```bash
code --install-extension diegosouzapw.omnicopilot
```

This populates your local OmniRoute combos directly in the native VS Code Copilot model dropdown menu without needing an active Copilot subscription.

---

### Step 6: Create Ignore Filters (.continueignore & .cursorignore)

Create ignore files in `~/.continue/.continueignore` and `~/.cursorignore` to prevent context bloating:

```plaintext
node_modules/
.git/
dist/
build/
coverage/
*.lock
package-lock.json
pnpm-lock.yaml
*.log
```

---

### Step 7: Verification & Testing

1. **Run OmniRoute Doctor**:
   ```bash
   omniroute doctor
   ```

2. **Simulate Combo Routing**:
   ```bash
   omniroute simulate --combo combo-pro-coding "Write a quicksort function"
   ```

3. **Test Chat Completion API**:
   ```bash
   curl -i -X POST http://localhost:20128/v1/chat/completions \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer sk-omniroute-local" \
     -d '{
       "model": "combo-pro-coding",
       "messages": [{"role": "user", "content": "Hello world"}]
     }'
   ```

---

## 🖥️ IDE & Terminal Agent Integration Setup

### Native VS Code & GitHub Copilot
1. Install extension: `code --install-extension diegosouzapw.omnicopilot`
2. Open Copilot Chat panel (`Cmd + Shift + I`).
3. Select `combo-pro-coding`, `combo-deep-reasoning`, or `combo-fast-chat` from the model picker dropdown.

### Cursor IDE
1. Go to **Cursor Settings** -> **Models**.
2. Enable **Override OpenAI Base URL** and set it to: `http://localhost:20128/v1`.
3. Set **API Key** to: `sk-omniroute-local`.
4. Click **Add Model** and add:
   - `combo-pro-coding`
   - `combo-deep-reasoning`
   - `combo-fast-chat`
5. Disable `gpt-4o` and `claude-3.5-sonnet` to ensure traffic flows strictly through your OmniRoute combos.

### Terminal Agents (Claude Code & Codex CLI)
Launch terminal agents pre-wired to OmniRoute combos:

```bash
# Launch Claude Code using Pro Coding combo:
omniroute run claude --model combo-pro-coding

# Launch Codex CLI using Deep Reasoning combo:
omniroute run codex --model combo-deep-reasoning
```

---

## 🌐 OmniRoute Web Dashboard Overview

Access the Web Dashboard at: **[http://localhost:20128/home](http://localhost:20128/home)**

- **`/home` / `/dashboard`**: Real-time traffic, latency, and token usage analytics.
- **`/dashboard/free-tiers`**: Monitor ~1.51B monthly free tokens across 40+ provider pools.
- **`/dashboard/combos`**: Visual combo builder (Priority, Round-Robin, Latency routing).
- **`/dashboard/compression`**: RTK (Real-Time Token Kernel) + Caveman token compression.
- **`/dashboard/logs`**: Real-time SSE request inspector.

---

## 💻 CLI Command Reference

| Action | Command |
| :--- | :--- |
| **System Check** | `omniroute doctor` |
| **List Combos** | `omniroute combo list` |
| **List Providers** | `omniroute providers` |
| **List API Keys** | `omniroute keys list` |
| **Open Dashboard** | `omniroute open dashboard` |
| **Dry-Run Route Simulation** | `omniroute simulate --combo <combo-name> "<prompt>"` |
| **Terminal REPL** | `omniroute repl` |
| **Terminal Chat** | `omniroute chat "<prompt>"` |

---

## 🔧 Troubleshooting

- **Error: `401 Unauthorized` / `Authentication required`**:
  Ensure your request includes `-H "Authorization: Bearer sk-omniroute-local"` or a key created in `/dashboard/api-keys`.
- **Error: `402 Payment Required`**:
  Occurs when an upstream provider key is unconfigured or out of quota. Add a direct provider key via `omniroute keys add <provider> <key>`.
- **Ollama connection issue**:
  Verify Ollama is running on port 11434 (`curl http://localhost:11434`). Start via `brew services start ollama`.
