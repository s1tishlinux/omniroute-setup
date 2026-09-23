# 🐋 Orca Spark-MLX Apple Silicon GPU Setup & Usage Guide

## 📌 Overview

**Spark-MLX-LLM** is an Apple Silicon hardware-accelerated local inference engine designed specifically for running **Spark 2.5** models (such as `XHToken/Spark-X2.5-1.7B`) natively on M1/M2/M3/M4 Macs without requiring GGUF conversion.

---

## 🏗️ Architecture & Server Configuration

```
+-------------------------------------------------------------------------------+
|                             Local macOS Machine                               |
|                                                                               |
|   +-------------------------------------+   +------------------------------+  |
|   |          Spark MLX Engine           |   |       OmniRoute Gateway      |  |
|   |  http://127.0.0.1:8080/v1           |   |   http://localhost:20128/v1  |  |
|   |  Model: XHToken/Spark-X2.5-1.7B     |   |   Model: combo-pro-coding    |  |
|   +------------------+------------------+   +--------------+---------------+  |
|                      |                                     |                  |
+----------------------|-------------------------------------|------------------+
                       |                                     |
                       v                                     v
          +------------------------------------------------------+
          |           Codex CLI / IDE Tools / Desktop            |
          +------------------------------------------------------+
```

- **Server Endpoint**: `http://127.0.0.1:8080/v1`
- **Default Model**: `XHToken/Spark-X2.5-1.7B`
- **Backend**: MLX LM on Apple Silicon GPU (`Metal`)

---

## ⚡ Quick Start Commands

### 1. Check Server Liveness
```bash
curl -s http://127.0.0.1:8080/v1/models
```

**Output**:
```json
{
  "object": "list",
  "data": [
    {
      "id": "XHToken/Spark-X2.5-1.7B",
      "object": "model",
      "created": 1790139487
    }
  ]
}
```

### 2. Test Direct Chat Completion (REST API)
```bash
curl -s -X POST http://127.0.0.1:8080/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "XHToken/Spark-X2.5-1.7B",
    "messages": [
      {"role": "user", "content": "Hello Spark MLX! Explain recursion in one sentence."}
    ]
  }'
```

**Verified Output**:
```json
{
  "id": "chatcmpl-63c09dca-cc3b-4b51-9250-42332f0f820f",
  "system_fingerprint": "0.31.3-0.32.2-macOS-26.6.2-arm64-arm-64bit-Mach-O-applegpu_g15g",
  "object": "chat.completion",
  "model": "XHToken/Spark-X2.5-1.7B",
  "choices": [
    {
      "index": 0,
      "finish_reason": "stop",
      "message": {
        "role": "assistant",
        "content": "Recursion is a programming technique where a function calls itself to solve a problem by breaking it down into smaller, self-similar subproblems until a base condition is met."
      }
    }
  ],
  "usage": {
    "prompt_tokens": 21,
    "completion_tokens": 35,
    "total_tokens": 56
  }
}
```

---

## 🐍 Python API Usage

```python
from mlx_lm import generate
from spark_mlx_llm import load

# Load model and tokenizer directly on Apple Silicon GPU
model, tokenizer = load("XHToken/Spark-X2.5-1.7B", dtype="bfloat16")

prompt = tokenizer.apply_chat_template(
    [{"role": "user", "content": "Write a Python function to check prime numbers."}],
    tokenize=False,
    add_generation_prompt=True,
)

response = generate(model, tokenizer, prompt=prompt, max_tokens=128)
print(response)
```

---

## 🛠️ CLI Utilities in Workspace (`/Users/satishgundu/orca/workspaces/Spark-MLX-LLM`)

- **`spark-mlx-generate`**: Command-line prompt text generator.
- **`spark-mlx-chat`**: Interactive terminal REPL.
- **`spark-mlx-server`**: OpenAI-compatible server (`--host 127.0.0.1 --port 8080`).
- **`spark-mlx-convert`**: Quantize checkpoints to 8-bit or 4-bit.
