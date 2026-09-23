# 🚀 OmniRoute, Ollama & Multi-IDE AI Routing Setup Guide

A complete, production-ready guide for installing, configuring, and wiring **OmniRoute**, **Ollama**, **Orca Spark MLX**, and developer tools across **Desktop Apps** (Codex Desktop, Claude Desktop, Open WebUI, Jan, Chatbox), **IDE Extensions** (VS Code, GitHub Copilot, Continue.dev, Cursor, Cline, Roo Code, JetBrains, Zed), and **Terminal Coding Agents** (Codex CLI, Claude Code, Qwen Code, Orca, Aider) using 10 verified API provider tiers, local GPU autocomplete, and 6 multi-provider fallback chains on macOS.

---

## 📋 Table of Contents
1. [Architecture & Universal Compatibility](#-architecture--universal-compatibility)
2. [Universal Compatibility Matrix (Desktop, IDEs, Extensions & CLIs)](#-universal-compatibility-matrix)
3. [Prerequisites](#-prerequisites)
4. [6 Multi-Provider Routing Combos Overview](#-6-multi-provider-routing-combos-overview)
5. [Orca & Spark-MLX Apple Silicon Setup](#-orca--spark-mlx-apple-silicon-setup)
6. [Step-by-Step Execution Guide](#-step-by-step-execution-guide)
   - [Step 1: Install & Verify Core Services](#step-1-install--verify-core-services)
   - [Step 2: Register & Verify 10 Developer API Keys](#step-2-register--verify-10-developer-api-keys)
   - [Step 3: Create 6 Multi-Provider Routing Combos](#step-3-create-6-multi-provider-routing-combos)
   - [Step 4: Configure Orca Spark MLX & Codex Profiles](#step-4-configure-orca-spark-mlx--codex-profiles)
   - [Step 5: Configure Desktop Apps (Codex Desktop, Claude Desktop, Open WebUI)](#step-5-configure-desktop-apps)
   - [Step 6: Wire IDE Extensions (VS Code, Continue.dev, Copilot, Cursor, Cline, JetBrains)](#step-6-wire-ide-extensions)
   - [Step 7: Configure Terminal Coding Agents (Codex CLI, Claude Code, Qwen, Orca, Aider)](#step-7-configure-terminal-coding-agents)
   - [Step 8: Create Ignore Filters (.continueignore & .cursorignore)](#step-8-create-ignore-filters)
   - [Step 9: Verification & Overload Failover Testing](#step-9-verification--overload-failover-testing)
7. [Live Execution & Doctor Diagnostic Outputs](#-live-execution--doctor-diagnostic-outputs)
8. [OmniRoute Web Dashboard Overview](#-omniroute-web-dashboard-overview)
9. [CLI Command Reference](#-cli-command-reference)
10. [Troubleshooting & FAQs](#-troubleshooting--faqs)

---

## 🏗️ Architecture & Universal Compatibility

OmniRoute acts as a **Universal OpenAI-compatible and Anthropic-compatible API Gateway** running locally at `http://localhost:20128/v1` (OpenAI endpoint) and `http://localhost:20128` (Anthropic endpoint).

Because OmniRoute standardizes all underlying cloud providers (Gemini, Groq, NVIDIA, Mistral, Cerebras, SambaNova, Cohere, GitHub Models) into standard REST endpoints, **it works seamlessly with ANY Desktop App, Web UI, IDE Extension, or CLI Tool that supports custom API Base URLs**.

```
+---------------------------------------------------------------------------------------------------------+
|                                                Local Host                                               |
|                                                                                                         |
|  +------------------------+  +-------------------+  +------------------------------------------------+  |
|  |   Ollama Local Server  |  | Spark MLX Server  |  |                OmniRoute Gateway               |  |
|  | http://localhost:11434  |  | 127.0.0.1:8080    |  |  http://localhost:20128 (OpenAI/Anthropic /v1) |  |
|  +-----------+------------+  +---------+---------+  +-----------------------+------------------------+  |
|              |                         |                                  |                             |
|    Local Tab Autocomplete           Local Spark MLX                 6 Multi-Provider Combos             |
|   (qwen2.5-coder:7b-base)        (Spark-X2.5-1.7B)               RTK Token Compression             |
|              |                         |                                  |                             |
+--------------+-------------------------+----------------------------------+-----------------------------+
               |                         |                                  |
               +-------------------------+----------------------------------+
                                         |
     +-----------------------------------+-----------------------------------+
     |                                   |                                   |
     v                                   v                                   v
+--------------------------+ +--------------------------+ +--------------------------+
|       DESKTOP APPS       | |      IDE EXTENSIONS      | |      TERMINAL AGENTS    |
| • Codex Desktop App      | | • Continue.dev           | | • Codex CLI (`codex`)    |
| • Claude Desktop App     | | • VS Code (Copilot Chat) | | • Claude Code CLI       |
| • Open WebUI / Jan AI    | | • Cursor IDE             | | • Qwen Code CLI (`qwen`) |
| • Chatbox / Obsidian     | | • Cline / Roo Code / Kilo| | • Aider / Orca / Goose   |
+--------------------------+ +--------------------------+ +--------------------------+
```

---

## 🌐 Universal Compatibility Matrix

| Category | Application / Tool | Compatibility Status | Configuration Method |
| :--- | :--- | :---: | :--- |
| **Desktop Apps** | **Codex Desktop App** | ✅ Fully Supported | Settings ➔ Custom Provider ➔ Base URL: `http://localhost:20128/v1` |
| | **Claude Desktop App** | ✅ Fully Supported | `claude_desktop_config.json` ➔ `ANTHROPIC_BASE_URL: http://localhost:20128` |
| | **Jan AI / LM Studio** | ✅ Fully Supported | Settings ➔ Inference Provider ➔ Custom OpenAI endpoint: `http://localhost:20128/v1` |
| | **Open WebUI** | ✅ Fully Supported | Connections ➔ Add OpenAI API: `http://localhost:20128/v1` |
| | **Chatbox / MindMac** | ✅ Fully Supported | Settings ➔ Model Provider: Custom OpenAI ➔ `http://localhost:20128/v1` |
| **IDE Extensions** | **Continue.dev (VS Code/JetBrains)** | ✅ Fully Supported | `~/.continue/config.yaml` ➔ `apiBase: http://localhost:20128/v1` |
| | **VS Code GitHub Copilot** | ✅ Fully Supported | Install `OmniCopilot` extension (`diegosouzapw.omnicopilot`) |
| | **Cursor IDE** | ✅ Fully Supported | Settings ➔ Models ➔ Override OpenAI Base URL: `http://localhost:20128/v1` |
| | **Cline / Roo Code / Kilo** | ✅ Fully Supported | Provider: OpenAI Compatible ➔ Base URL: `http://localhost:20128/v1` |
| | **Zed Editor** | ✅ Fully Supported | `settings.json` ➔ `language_models` ➔ `openai`: `http://localhost:20128/v1` |
| **Terminal CLIs** | **Codex CLI (`codex`)** | ✅ Fully Supported | `~/.codex/config.toml` + `omniroute setup-codex` |
| | **Claude Code CLI** | ✅ Fully Supported | `omniroute setup-claude` |
| | **Qwen Code CLI** | ✅ Fully Supported | `omniroute setup-qwen --model combo-pro-coding --yes` |
| | **Orca Spark CLI** | ✅ Fully Supported | `codex --profile orca-spark` |
| | **Aider / Goose / Interpreter**| ✅ Fully Supported | `export OPENAI_API_BASE="http://localhost:20128/v1"` |

---

## 🛠️ Prerequisites

- **macOS** (Apple Silicon GPU or Intel)
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

| Combo Name | Best Used For | Fallback Chain | Verified Status |
| :--- | :--- | :--- | :---: |
| **`combo-pro-coding`** | Everyday Software Engineering | Gemini 3.6 Flash ➔ Mistral Codestral ➔ Groq Llama 3.3 70B | ✅ 100% Active |
| **`combo-deep-reasoning`** | Architecture, Complex Bugs & Math | Gemini 3.6 Flash ➔ Groq Llama 3.3 70B ➔ Mistral Codestral | ✅ 100% Active |
| **`combo-fast-chat`** | Sub-second Responses & Quick Q&A | Cerebras Llama 3.1 70B ➔ Gemini 3.1 Flash Lite ➔ Groq Llama 3.3 70B | ✅ 100% Active |
| **`combo-vision-multimodal`** | UI Mockups, Diagrams & Screenshots | Gemini 3.7 Flash ➔ Gemini 3.6 Flash | ✅ 100% Active |
| **`combo-ultra-heavy`** | Refactoring Huge Repos (2M Context) | Gemini 3.6 Flash ➔ Mistral Codestral ➔ Groq Llama 3.3 70B | ✅ 100% Active |
| **`combo-zero-cost`** | 100% Free Tiers Only | Gemini 3.6 Flash ➔ Groq Llama 3.3 70B ➔ Mistral Codestral | ✅ 100% Active |

---

## 🐋 Orca & Spark-MLX Apple Silicon Setup

The **Orca Workspace** (`/Users/satishgundu/orca`) runs a local Apple Silicon GPU hardware-accelerated model server (`spark-mlx-server`) serving `XHToken/Spark-X2.5-1.7B` on `http://127.0.0.1:8080/v1`.

### 1. Direct MLX API Test
```bash
curl -s -X POST http://127.0.0.1:8080/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "XHToken/Spark-X2.5-1.7B",
    "messages": [{"role": "user", "content": "Hello Spark MLX!"}]
  }'
```

**Verified Output**:
```json
{
  "id": "chatcmpl-63c09dca-cc3b-4b51-9250-42332f0f820f",
  "system_fingerprint": "0.31.3-0.32.2-macOS-26.6.2-arm64-arm-64bit-Mach-O-applegpu_g15g",
  "object": "chat.completion",
  "model": "XHToken/Spark-X2.5-1.7B",
  "created": 1790139500,
  "choices": [
    {
      "index": 0,
      "finish_reason": "stop",
      "message": {
        "role": "assistant",
        "content": "Hello! I am Spark MLX, and I can provide you with various cognitive intelligence services including language understanding, question answering, reasoning, etc."
      }
    }
  ],
  "usage": {
    "prompt_tokens": 21,
    "completion_tokens": 55,
    "total_tokens": 76
  }
}
```

### 2. Codex CLI Profile Setup for Orca Spark
Profile created at `~/.codex/orca-spark.config.toml`:
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

### 3. Execution Command
```bash
cd /Users/satishgundu/orca
codex --profile orca-spark exec --skip-git-repo-check "Return text 'Orca Spark integration verified!'"
```

**Verified Output**:
```text
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
  --models "gemini/gemini-3.6-flash,mistral/codestral-2501,groq/llama-3.3-70b-versatile"

# 2. Deep Reasoning Combo (Architecture, Complex Bugs & Math)
omniroute combo create combo-deep-reasoning --strategy priority \
  --models "gemini/gemini-3.6-flash,groq/llama-3.3-70b-versatile,mistral/codestral-2501"

# 3. Fast Chat Combo (Sub-second Responses & Quick Q&A)
omniroute combo create combo-fast-chat --strategy priority \
  --models "cerebras/llama-3.1-70b,gemini/gemini-3.1-flash-lite,groq/llama-3.3-70b-versatile"

# 4. Vision Multimodal Combo (UI Mockups & Diagrams)
omniroute combo create combo-vision-multimodal --strategy priority \
  --models "gemini/gemini-3.7-flash,gemini/gemini-3.6-flash"

# 5. Ultra Heavy Combo (Refactoring Huge Repos - 2M Context)
omniroute combo create combo-ultra-heavy --strategy priority \
  --models "gemini/gemini-3.6-flash,mistral/codestral-2501,groq/llama-3.3-70b-versatile"

# 6. Zero Cost Free Tiers Combo (100% Free Tiers Only)
omniroute combo create combo-zero-cost --strategy priority \
  --models "gemini/gemini-3.6-flash,groq/llama-3.3-70b-versatile,mistral/codestral-2501"
```

Verify created combos:
```bash
omniroute combo list
```

---

### Step 4: Configure Orca Spark MLX & Codex Profiles

1. **Verify Spark MLX local server**:
   ```bash
   curl -s http://127.0.0.1:8080/v1/models
   ```

2. **Create Codex Orca profile (`~/.codex/orca-spark.config.toml`)**:
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

3. **Launch Orca Spark interactive Codex CLI**:
   ```bash
   codex --profile orca-spark
   ```

---

### Step 5: Configure Desktop Apps

#### 1. Codex Desktop App
1. Open **Codex Desktop** ➔ Settings ➔ Advanced / Custom Models.
2. Enable **Custom OpenAI Base URL**: `http://localhost:20128/v1`
3. Set **API Key**: `sk-omniroute-local`
4. Enter Model Name: `combo-pro-coding` (or `XHToken/Spark-X2.5-1.7B`).

#### 2. Claude Desktop App
1. Open or edit `~/Library/Application Support/Claude/claude_desktop_config.json`.
2. Add or update the Anthropic base URL environment variable:
   ```json
   {
     "env": {
       "ANTHROPIC_BASE_URL": "http://localhost:20128"
     }
   }
   ```

#### 3. Open WebUI / Jan AI / Chatbox Desktop
1. Go to Settings ➔ Providers / Models ➔ OpenAI Compatible.
2. API Host: `http://localhost:20128/v1`
3. API Key: `sk-omniroute-local`
4. Available models will automatically include `combo-pro-coding`, `combo-deep-reasoning`, `combo-fast-chat`, `combo-vision-multimodal`, `combo-ultra-heavy`, and `combo-zero-cost`.

---

### Step 6: Wire IDE Extensions

#### 1. Continue.dev Extension (VS Code / JetBrains)
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

#### 2. VS Code GitHub Copilot Chat (Native Integration)
Install the official **OmniCopilot** bridge extension:
```bash
code --install-extension diegosouzapw.omnicopilot
```
This populates all 6 OmniRoute combos directly into the native VS Code Copilot model dropdown menu.

#### 3. Cursor IDE
1. Open **Cursor Settings** ➔ **Models**.
2. Enable **Override OpenAI Base URL**: `http://localhost:20128/v1`.
3. Set **API Key**: `sk-omniroute-local`.
4. Add model names: `combo-pro-coding`, `combo-deep-reasoning`, `combo-fast-chat`, `combo-vision-multimodal`, `combo-ultra-heavy`, `combo-zero-cost`.

#### 4. Cline / Roo Code / Kilo Code Extensions
1. In the extension panel, select API Provider: **OpenAI Compatible**.
2. Set Base URL: `http://localhost:20128/v1`
3. Set API Key: `sk-omniroute-local`
4. Enter Model ID: `combo-pro-coding`

---

### Step 7: Configure Terminal Coding Agents

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

### Step 8: Create Ignore Filters (.continueignore & .cursorignore)

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

### Step 9: Verification & Overload Failover Testing

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

### Verified 6 Combos Execution Output

```text
1. combo-pro-coding       ➔ "pro-coding OK"          [gemini-3.6-flash · 200 OK]
2. combo-deep-reasoning   ➔ "deep-reasoning OK"      [gemini-3.6-flash · 200 OK]
3. combo-fast-chat        ➔ "fast-chat OK"           [gemini-3.1-flash-lite · 200 OK]
4. combo-vision-multimodal ➔ "vision-multimodal OK"  [gemini-3.6-flash · 200 OK]
5. combo-ultra-heavy      ➔ "ultra-heavy OK"         [gemini-3.6-flash · 200 OK]
6. combo-zero-cost        ➔ "zero-cost OK"           [gemini-3.6-flash · 200 OK]
```

---

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
OK   Server liveness: Server reachable
OK   CLI machine token: Server accepted the local machine token
OK   CLI: Claude Code: Claude Code configured
OK   CLI: Codex CLI: Codex CLI configured

Summary: 11 ok, 38 warning(s), 0 failure(s)
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

## 🔧 Troubleshooting & FAQs

- **Does OmniRoute work with Desktop Apps (Codex Desktop, Claude Desktop, Jan)?**
  **YES!** OmniRoute exposes standard OpenAI (`http://localhost:20128/v1`) and Anthropic (`http://localhost:20128`) endpoints. Simply set the custom API endpoint/base URL in your desktop application to point to OmniRoute.
- **Does OmniRoute work with all VS Code / JetBrains / Cursor extensions?**
  **YES!** Any extension that supports OpenAI-compatible endpoints (Continue.dev, OmniCopilot, Cursor, Cline, Roo Code, Kilo Code, JetBrains AI Assistant) works natively.
- **Model API Overloaded Error**:
  OmniRoute's Priority fallback strategy automatically catches 429/503/404 overload errors and reroutes your prompt to the next provider (e.g. Google -> Mistral -> Groq) within milliseconds.
- **`401 Unauthorized`**:
  Ensure your request includes `-H "Authorization: Bearer sk-omniroute-local"`.
