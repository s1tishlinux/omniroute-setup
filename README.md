# 🚀 OmniRoute, Ollama & Multi-IDE AI Routing Setup Guide

A complete, production-ready guide for installing, configuring, and wiring **OmniRoute**, **Ollama**, and developer IDEs & CLI agents (**VS Code**, **Continue.dev**, **GitHub Copilot**, **Cursor**, **Claude Code**, **Codex CLI**, **Qwen Code**, **Orca**, **Aider**) using 10 verified API provider tiers, local GPU autocomplete, and 6 multi-provider fallback chains on macOS.

---

## 📋 Table of Contents
1. [Architecture & Topology](#-architecture--topology)
2. [Prerequisites](#-prerequisites)
3. [6 Multi-Provider Routing Combos Overview](#-6-multi-provider-routing-combos-overview)
4. [Step-by-Step Execution Guide](#-step-by-step-execution-guide)
   - [Step 1: Install & Verify Core Services](#step-1-install--verify-core-services)
   - [Step 2: Register & Verify 10 Developer API Keys](#step-2-register--verify-10-developer-api-keys)
   - [Step 3: Create 6 Multi-Provider Routing Combos](#step-3-create-6-multi-provider-routing-combos)
   - [Step 4: Wire Continue.dev Extension](#step-4-wire-continuedev-extension)
   - [Step 5: Wire GitHub Copilot Chat (Native VS Code Integration)](#step-5-wire-github-copilot-chat-native-vs-code-integration)
   - [Step 6: Configure Terminal Coding Agents (Codex, Claude, Qwen, Orca)](#step-6-configure-terminal-coding-agents-codex-claude-qwen-orca)
   - [Step 7: Create Ignore Filters (.continueignore & .cursorignore)](#step-7-create-ignore-filters-continueignore--cursorignore)
   - [Step 8: Verification & Overload Failover Testing](#step-8-verification--overload-failover-testing)
5. [Live Execution & Doctor Diagnostic Outputs](#-live-execution--doctor-diagnostic-outputs)
   - [API Keys Status (`omniroute keys list`)](#api-keys-status-omniroute-keys-list)
   - [Combos Status (`omniroute combo list`)](#combos-status-omniroute-combo-list)
   - [Doctor Verification (`omniroute doctor`)](#doctor-verification-omniroute-doctor)
   - [Live API Test Output (`curl` execution)](#live-api-test-output-curl-execution)
6. [IDE & Tool Setup Reference](#-ide--tool-setup-reference)
7. [OmniRoute Web Dashboard Overview](#-omniroute-web-dashboard-overview)
8. [CLI Command Reference](#-cli-command-reference)
9. [Troubleshooting & FAQs](#-troubleshooting--faqs)

---

## 🏗️ Architecture & Topology

```
+-----------------------------------------------------------------------------------+
|                                     Local Host                                    |
|                                                                                   |
|  +------------------------+  +-------------------+  +--------------------------+  |
|  |   Ollama Local Server  |  | Spark MLX Server  |  |    OmniRoute Gateway     |  |
|  | http://localhost:11434  |  | 127.0.0.1:8080    |  |  http://localhost:20128  |  |
|  +-----------+------------+  +---------+---------+  +------------+-------------+  |
|              |                         |                       |                  |
|    Local Tab Autocomplete           Local Spark MLX      6 Multi-Provider Combos |
|   (qwen2.5-coder:7b-base)        (Spark-X2.5-1.7B)     RTK Token Compression  |
|              |                         |                       |                  |
+--------------+-------------------------+-----------------------+------------------+
               |                         |                       |
               v                         v                       v
   +-----------------------+ +-----------------------+ +----------------------------+
   | Continue.dev / Cursor | | VS Code / Copilot Chat| | 10 Verified Cloud Providers|
   | (VS Code / Cursor IDE)| | (OmniCopilot Extension)| | (Gemini, Groq, NVIDIA, GH)|
   +-----------------------+ +-----------------------+ +----------------------------+
```

---

## 🛠️ Prerequisites

- **macOS** (Apple Silicon or Intel)
- **Homebrew** installed (`brew`)
- **Node.js** `>= 20.0.0`
- Developer API keys for providers:
  - Google Gemini API Key
  - Groq API Key
  - Mistral AI API Key
  - Cerebras API Key
  - SambaNova API Key
  - NVIDIA NIM API Key
  - Cohere API Key
  - HuggingFace API Key
  - OpenRouter API Key
  - GitHub Personal Access Token (PAT)

---

## 🎯 6 Multi-Provider Routing Combos Overview

| Combo Name | Best Used For | Fallback Chain |
| :--- | :--- | :--- |
| **`combo-pro-coding`** | Everyday Software Engineering | Gemini 3.6 Flash ➔ Mistral Codestral ➔ NVIDIA Llama 3.3 70B ➔ Groq Llama 3.3 70B |
| **`combo-deep-reasoning`** | Architecture, Complex Bugs & Math | GitHub DeepSeek-R1 ➔ SambaNova DeepSeek-R1 ➔ Gemini 3.1 Pro |
| **`combo-fast-chat`** | Sub-second Responses & Quick Q&A | Cerebras Llama 3.1 70B ➔ Groq Llama 3.3 70B ➔ Gemini 3.1 Flash Lite |
| **`combo-vision-multimodal`** | UI Mockups, Diagrams & Screenshots | Gemini 3.7 Flash ➔ NVIDIA Llama 3.2 90B Vision |
| **`combo-ultra-heavy`** | Refactoring Huge Repos (2M Context) | Gemini 3.6 Flash ➔ Gemini 3.1 Pro |
| **`combo-zero-cost`** | 100% Free Tiers Only | Gemini 3.6 Flash ➔ GitHub DeepSeek-R1 ➔ NVIDIA Llama 3.3 70B |

---

## 🚀 Step-by-Step Execution Guide

### Step 1: Install & Verify Core Services

1. **Install & Start Ollama**:
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

4. **Start OmniRoute Daemon**:
   ```bash
   omniroute start
   ```

---

### Step 2: Register & Verify 10 Developer API Keys

Add your provider keys using the OmniRoute CLI:

```bash
# 1. Google Gemini API Key
omniroute keys add gemini "YOUR_GEMINI_API_KEY"

# 2. Groq API Key
omniroute keys add groq "YOUR_GROQ_API_KEY"

# 3. Mistral AI API Key
omniroute keys add mistral "YOUR_MISTRAL_API_KEY"

# 4. Cerebras API Key
omniroute keys add cerebras "YOUR_CEREBRAS_API_KEY"

# 5. SambaNova API Key
omniroute keys add sambanova "YOUR_SAMBANOVA_API_KEY"

# 6. NVIDIA NIM API Key
omniroute keys add nvidia "YOUR_NVIDIA_API_KEY"

# 7. Cohere API Key
omniroute keys add cohere "cohere_i8dBub6Zf3IHkeUudOMybWudNR9o1dy6KTUNY1qE4a8PSV"

# 8. HuggingFace API Key
omniroute keys add huggingface "YOUR_HUGGINGFACE_API_KEY"

# 9. OpenRouter API Key
omniroute keys add openrouter "YOUR_OPENROUTER_API_KEY"

# 10. GitHub Models PAT
omniroute keys add github "YOUR_GITHUB_PAT"
```

Verify active keys:
```bash
omniroute keys list
```

---

### Step 3: Create 6 Multi-Provider Routing Combos

Create 6 specialized fallback chains so your tools never hit API limits or overload errors:

```bash
# 1. Pro Coding Combo (Everyday Software Engineering)
omniroute combo create combo-pro-coding --strategy priority \
  --models "gemini/gemini-3.6-flash,mistral/codestral-2501,nvidia/llama-3.3-70b-instruct,groq/llama-3.3-70b-versatile"

# 2. Deep Reasoning Combo (Architecture, Complex Bugs & Math)
omniroute combo create combo-deep-reasoning --strategy priority \
  --models "github/deepseek-r1,sambanova/DeepSeek-R1,gemini/gemini-3.1-pro-preview"

# 3. Fast Chat Combo (Sub-second Responses & Quick Q&A)
omniroute combo create combo-fast-chat --strategy priority \
  --models "cerebras/llama-3.1-70b,groq/llama-3.3-70b-versatile,gemini/gemini-3.1-flash-lite"

# 4. Vision Multimodal Combo (UI Mockups & Diagrams)
omniroute combo create combo-vision-multimodal --strategy priority \
  --models "gemini/gemini-3.7-flash,nvidia/llama-3.2-90b-vision-instruct"

# 5. Ultra Heavy Combo (Refactoring Huge Repos - 2M Context)
omniroute combo create combo-ultra-heavy --strategy priority \
  --models "gemini/gemini-3.6-flash,gemini/gemini-3.1-pro-preview"

# 6. Zero Cost Free Tiers Combo (100% Free Tiers Only)
omniroute combo create combo-zero-cost --strategy priority \
  --models "gemini/gemini-3.6-flash,github/deepseek-r1,nvidia/llama-3.3-70b-instruct"
```

Verify created combos:
```bash
omniroute combo list
```

---

### Step 4: Wire Continue.dev Extension

Update `~/.continue/config.yaml`:

```yaml
models:
  - name: "Pro Coding (Gemini 3.6 Flash -> Antigravity -> NVIDIA)"
    provider: "openai"
    model: "combo-pro-coding"
    apiBase: "http://localhost:20128/v1"
    apiKey: "sk-omniroute-local"

  - name: "Deep Reasoning (DeepSeek-R1 -> Gemini 3.1 Pro)"
    provider: "openai"
    model: "combo-deep-reasoning"
    apiBase: "http://localhost:20128/v1"
    apiKey: "sk-omniroute-local"

  - name: "Fast Iteration (Flash Lite -> Gemma 3 -> Llama 3.3)"
    provider: "openai"
    model: "combo-fast-chat"
    apiBase: "http://localhost:20128/v1"
    apiKey: "sk-omniroute-local"

  - name: "Vision Multimodal (Gemini 3.7 Vision -> NVIDIA Vision)"
    provider: "openai"
    model: "combo-vision-multimodal"
    apiBase: "http://localhost:20128/v1"
    apiKey: "sk-omniroute-local"

  - name: "Ultra Heavy Refactoring (Gemini 3.6 -> Gemini 3.1 Pro)"
    provider: "openai"
    model: "combo-ultra-heavy"
    apiBase: "http://localhost:20128/v1"
    apiKey: "sk-omniroute-local"

  - name: "Zero Cost Free Tiers (Gemini -> GitHub -> NVIDIA)"
    provider: "openai"
    model: "combo-zero-cost"
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

This populates all 6 OmniRoute combos directly into the native VS Code Copilot model dropdown menu without requiring a paid Copilot subscription.

---

### Step 6: Configure Terminal Coding Agents (Codex, Claude, Qwen, Orca)

1. **OpenAI Codex CLI (`codex`)**:
   - Run setup to generate profiles:
     ```bash
     omniroute setup-codex
     ```
   - Update `~/.codex/config.toml`:
     ```toml
     model = "combo-pro-coding"
     openai_base_url = "http://127.0.0.1:20128/v1"
     skills_context_budget = 0.25
     ```
   - Populate `~/.codex/ollama-launch-models.json` so all combos & local models appear in the Codex interactive `/model` selector:
     ```json
     {
       "models": [
         "combo-pro-coding",
         "combo-deep-reasoning",
         "combo-fast-chat",
         "combo-vision-multimodal",
         "combo-ultra-heavy",
         "combo-zero-cost",
         "qwen2.5-coder:7b",
         "gemma3:4b",
         "Spark-X2.5-1.7B"
       ]
     }
     ```

2. **Claude Code CLI (`claude`)**:
   ```bash
   omniroute setup-claude
   ```

3. **Qwen Code CLI (`qwen`)**:
   If running inside a virtual environment (e.g. `s1tishlinux-stunning-spoon`):
   ```bash
   source ~/orca/projects/copilot-worktrees/learn-and-use-genai/s1tishlinux-stunning-spoon/venv/bin/activate
   omniroute setup-qwen --model combo-pro-coding --yes
   ```

4. **Orca / Aider / Goose / Open Interpreter**:
   ```bash
   export OPENAI_API_BASE="http://localhost:20128/v1"
   export OPENAI_API_KEY="sk-omniroute-local"
   ```

---

### Step 7: Create Ignore Filters (.continueignore & .cursorignore)

Create ignore files in `~/.continue/.continueignore` and `~/.cursorignore`:

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

### Step 8: Verification & Overload Failover Testing

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

## 📊 Live Execution & Doctor Diagnostic Outputs

### API Keys Status (`omniroute keys list`)

```text
📋 Loaded env from /Users/satishgundu/.omniroute/.env

API Keys

  cerebras             csk-tm***mnt9          ● enabled
  cohere               cohere***8PSV          ● enabled
  gemini               AQ.Ab8***DDlg          ● enabled
  groq                 gsk_Sd***1Xmi          ● enabled
  huggingface          hf_BXk***fzNV          ● enabled
  mistral              mstrl_***yxYS          ● enabled
  nvidia               nvapi-***D0db          ● enabled
  openrouter           sk-or-***2d11          ● enabled
  sambanova            4809df***a2fe          ● enabled
  together             key_Cd***WBLR          ● enabled

10 key(s).
```

---

### Combos Status (`omniroute combo list`)

```text
📋 Loaded env from /Users/satishgundu/.omniroute/.env

Combos

  ○ combo-vision-multimodal   [priority    ] enabled
  ○ combo-ultra-heavy         [priority    ] enabled
  ○ combo-zero-cost           [priority    ] enabled
  ○ combo-pro-coding          [priority    ] enabled
  ○ combo-deep-reasoning      [priority    ] enabled
  ○ combo-fast-chat           [priority    ] enabled
```

---

### Doctor Verification (`omniroute doctor`)

```text
📋 Loaded env from /Users/satishgundu/.omniroute/.env

OmniRoute Doctor

Data dir: /Users/satishgundu/.omniroute
Database: /Users/satishgundu/.omniroute/storage.sqlite

OK   Config: .env found at /Users/satishgundu/.omniroute/.env
OK   Database: SQLite quick_check passed and migrations look current
OK   Storage/encryption: Encrypted credential samples decrypt successfully
OK   Port availability: Configured port(s) are available
OK   Node runtime: v24.18.0 is supported
OK   Native binary: better-sqlite3 native binary is compatible
OK   Memory: 512 MB limit configured; 0.1 GB free
OK   Server liveness: Server reachable (health endpoint returned 401, likely requires MANAGEMENT_TOKEN)
OK   CLI machine token: Server accepted the local machine token
OK   CLI: Claude Code: Claude Code configured
OK   CLI: Codex CLI: Codex CLI configured

Summary: 11 ok, 38 warning(s), 0 failure(s)
```

---

### Live API Test Output (`curl` execution)

```json
curl -s -X POST http://localhost:20128/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer sk-omniroute-local" \
  -d '{"model": "combo-pro-coding", "messages": [{"role": "user", "content": "Return the string \"OmniRoute operational!\" only."}]}'

{
  "id": "758bc3bfa8a2423a9f62a7413f892883",
  "object": "chat.completion",
  "created": 1790138502,
  "model": "codestral-2501",
  "choices": [
    {
      "index": 0,
      "finish_reason": "stop",
      "message": {
        "role": "assistant",
        "content": "\"OmniRoute operational!\""
      }
    }
  ],
  "usage": {
    "prompt_tokens": 14,
    "completion_tokens": 7,
    "total_tokens": 21
  }
}
```

---

## 🖥️ IDE & Tool Setup Reference

### Native VS Code & GitHub Copilot
1. Install extension: `code --install-extension diegosouzapw.omnicopilot`
2. Open Copilot Chat (`Cmd + Shift + I`).
3. Select any combo (`combo-pro-coding`, `combo-deep-reasoning`, `combo-vision-multimodal`) from the dropdown.

### Cursor IDE
1. Go to **Cursor Settings** -> **Models**.
2. Enable **Override OpenAI Base URL**: `http://localhost:20128/v1`.
3. Set **API Key**: `sk-omniroute-local`.
4. Add model names: `combo-pro-coding`, `combo-deep-reasoning`, `combo-fast-chat`, `combo-vision-multimodal`, `combo-ultra-heavy`, `combo-zero-cost`.

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

## 🔧 Troubleshooting & FAQs

- **Model API Overloaded Error**:
  OmniRoute's Priority fallback strategy automatically catches 429/503/404 overload errors and reroutes your prompt to the next provider (e.g. Google -> Mistral -> NVIDIA -> Groq) within milliseconds.
- **`401 Unauthorized`**:
  Ensure your request includes `-H "Authorization: Bearer sk-omniroute-local"`.
- **Codex CLI Metadata Warning**:
  The `Model metadata for combo-pro-coding not found` notice in Codex CLI is purely informational when using custom OpenAI-compatible proxies. It defaults safely to a 2M token context window.
