# 🐋 OmniRoute Gateway Routing & Combos Guide

## 📌 Overview

OmniRoute runs locally as a **Universal OpenAI and Anthropic compatible Gateway** at:
- **Base Endpoint**: `http://localhost:20128/v1`
- **Dashboard**: `http://localhost:20128/home`

It manages 10 registered API key tiers and routes requests across 6 multi-provider fallback chains to eliminate single-provider overload errors (429/503/404).

---

## 🎯 6 Verified Multi-Provider Routing Combos

| Combo Name | Purpose | Model Fallback Chain | Verified Status |
| :--- | :--- | :--- | :---: |
| **`combo-pro-coding`** | Everyday Software Engineering | Gemini 3.6 Flash ➔ Mistral Codestral ➔ Groq Llama 3.3 70B | ✅ 200 OK |
| **`combo-deep-reasoning`** | Architecture, Complex Bugs & Math | Gemini 3.6 Flash ➔ Groq Llama 3.3 70B ➔ Mistral Codestral | ✅ 200 OK |
| **`combo-fast-chat`** | Sub-second Responses & Quick Q&A | Cerebras Llama 3.1 70B ➔ Gemini 3.1 Flash Lite ➔ Groq Llama 3.3 70B | ✅ 200 OK |
| **`combo-vision-multimodal`** | UI Mockups, Diagrams & Screenshots | Gemini 3.7 Flash ➔ Gemini 3.6 Flash | ✅ 200 OK |
| **`combo-ultra-heavy`** | Refactoring Huge Repos (2M Context) | Gemini 3.6 Flash ➔ Mistral Codestral ➔ Groq Llama 3.3 70B | ✅ 200 OK |
| **`combo-zero-cost`** | 100% Free Tiers Only | Gemini 3.6 Flash ➔ Groq Llama 3.3 70B ➔ Mistral Codestral | ✅ 200 OK |

---

## 🔑 Registered Provider API Keys

```text
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
```

---

## 🧪 Testing Combos via Terminal

### 1. Test via `omniroute chat`
```bash
omniroute chat --model combo-pro-coding "Write a binary search function"
```

### 2. Test via REST API `curl`
```bash
curl -s -X POST http://localhost:20128/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer sk-omniroute-local" \
  -d '{
    "model": "combo-pro-coding",
    "messages": [{"role": "user", "content": "Return string \"pro-coding OK\""}]
  }'
```

**Verified Response**:
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
        "content": "pro-coding OK"
      }
    }
  ]
}
```
