# 🖱️ Cursor IDE & Antigravity IDE Setup Guide for Orca & OmniRoute

## 📌 Overview

This step-by-step guide explains how to configure **Cursor IDE** and **Antigravity IDE** to route all chat queries, code edits, and multi-file reasoning through **Orca Spark MLX** (local Apple Silicon GPU) and **OmniRoute Gateway** (6 multi-provider fallback chains).

---

## 🏗️ Architecture

```
+-----------------------------------------------------------------------------------------+
|                                    Cursor / Antigravity IDE                             |
|                                                                                         |
|       Settings ➔ Override OpenAI Base URL: http://localhost:20128/v1                    |
|       API Key: sk-omniroute-local                                                       |
+--------------------------------------------+--------------------------------------------+
                                             |
                                             v
                           +------------------------------------+
                           |         OmniRoute Gateway          |
                           |       http://localhost:20128       |
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

## ⚙️ Step-by-Step Configuration in Cursor / Antigravity IDE

### Step 1: Open Settings
1. Open **Cursor IDE** or **Antigravity IDE**.
2. Press `Cmd + ,` (or click gear icon ⚙️ in the bottom left).
3. Select **Cursor Settings** ➔ **Models** (or **AI Settings**).

---

### Step 2: Override OpenAI Base URL
1. Find the section **OpenAI API Key / Base URL**.
2. Enable **Override OpenAI Base URL**.
3. Set Base URL:
   ```text
   http://localhost:20128/v1
   ```
4. Set API Key:
   ```text
   sk-omniroute-local
   ```

---

### Step 3: Add Orca & Combo Model Names
Click **+ Add Model** and type each of the following model names:

1. **`XHToken/Spark-X2.5-1.7B`** *(Local Orca Spark MLX GPU Model)*
2. **`combo-pro-coding`** *(Everyday Software Engineering Chain)*
3. **`combo-deep-reasoning`** *(Architecture, Complex Bugs & Math)*
4. **`combo-fast-chat`** *(Sub-second Responses & Quick Q&A)*
5. **`combo-vision-multimodal`** *(UI Mockups & Screenshots)*
6. **`combo-ultra-heavy`** *(2M Context Refactoring Chain)*
7. **`combo-zero-cost`** *(100% Free Tiers Chain)*

---

### Step 4: Test in Cursor Chat (`Cmd + L`)
1. Open the Chat Panel using `Cmd + L` (or `Cmd + I` for inline edit).
2. Open the model selector dropdown menu at the bottom of the prompt window.
3. Select **`XHToken/Spark-X2.5-1.7B`** or **`combo-pro-coding`**.
4. Type your prompt:
   ```text
   Hello! Write a Python function to check if a number is prime.
   ```

---

## 🧪 Terminal Verification Test

You can test Cursor IDE's exact payload structure via terminal `curl`:

```bash
curl -i -X POST http://localhost:20128/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer sk-omniroute-local" \
  -d '{
    "model": "XHToken/Spark-X2.5-1.7B",
    "messages": [
      {"role": "system", "content": "You are assisting inside Cursor IDE."},
      {"role": "user", "content": "Return string \"Cursor Orca OK\""}
    ]
  }'
```

**Expected Response**:
```json
{
  "id": "chatcmpl-63c09dca-cc3b-4b51-9250-42332f0f820f",
  "object": "chat.completion",
  "model": "XHToken/Spark-X2.5-1.7B",
  "choices": [
    {
      "index": 0,
      "finish_reason": "stop",
      "message": {
        "role": "assistant",
        "content": "Cursor Orca OK"
      }
    }
  ]
}
```
