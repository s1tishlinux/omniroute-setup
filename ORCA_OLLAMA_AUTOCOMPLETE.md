# 🐋 Local Ollama & Tab Autocomplete Guide

## 📌 Overview

This document details the configuration for running **local Ollama** on Apple Silicon (`http://localhost:11434`) for sub-50ms tab autocompletion and offline coding.

---

## 🛠️ Installation & Service Daemon

### 1. Install & Start Ollama via Homebrew
```bash
brew install ollama
brew services start ollama
```

### 2. Pull Autocomplete & Vision Models
```bash
# Code autocomplete model (7B)
ollama pull qwen2.5-coder:7b-base

# Lightweight vision & chat model (4B)
ollama pull gemma3:4b
```

---

## ⚡ Continue.dev Tab Autocomplete Setup

In `~/.continue/config.yaml`:

```yaml
tabAutocompleteModel:
  title: "Local Qwen 7B Autocomplete"
  provider: "ollama"
  model: "qwen2.5-coder:7b-base"
  apiBase: "http://localhost:11434"

tabAutocompleteOptions:
  useCopyBuffer: true
  maxPromptTokens: 2048
  debounceDelay: 150
```

---

## 🧪 Health Check & Model Inspection

### List Local Models
```bash
curl -s http://localhost:11434/api/tags
```

**Output**:
```json
{
  "models": [
    {
      "name": "qwen2.5-coder:7b-base",
      "modified_at": "2026-09-22T19:00:00Z",
      "size": 4683074048
    },
    {
      "name": "gemma3:4b",
      "modified_at": "2026-09-22T19:15:00Z",
      "size": 2840000000
    }
  ]
}
```

### Test Autocomplete FIM (Fill-in-the-Middle) Completion
```bash
curl -s -X POST http://localhost:11434/api/generate \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen2.5-coder:7b-base",
    "prompt": "<|fim_prefix|>def fibonacci(n):\n    <|fim_suffix|>\n    return a<|fim_middle|>"
  }'
```
