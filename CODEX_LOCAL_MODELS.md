# Codex Local And OmniRoute Setup

Date: 2026-09-21

## Current Default

Codex is configured to use the OmniRoute combo route by default:

```toml
model = "auto/best-coding"
model_provider = "omniroute"
model_reasoning_effort = "medium"
model_context_window = 2000000
model_auto_compact_token_limit = 1700000
tool_output_token_limit = 32768
```

Config file:

```sh
~/.codex/config.toml
```

OmniRoute provider block:

```toml
[model_providers.omniroute]
name = "OmniRoute"
base_url = "http://localhost:20128/v1"
supports_websockets = false
api_key = "<configured locally>"
```

Do not paste API keys into notes or chats. Keep the real key only in local config or environment variables.

## Verified

OmniRoute endpoint responded successfully for these combo routes:

```sh
auto/best-coding
auto/offline
best-combo
my-combo
```

`auto/best-coding` and `auto/offline` routed to:

```sh
claude-opus-4-6-thinking
```

`best-combo` routed to:

```sh
poolside/laguna-s-2.1:free
```

`my-combo` routed to:

```sh
nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free
```

Spark also responded successfully for:

```sh
codex/gpt-5.3-codex-spark
```

The Spark route reported the served backend model as:

```sh
claude-opus-4-6-thinking
```

Codex also accepted the config with:

```sh
CODEX_HOME=/Users/satishgundu/.codex codex --strict-config --help
```

There was one non-blocking warning about stale temp-dir cleanup permissions.

## Useful Profiles Already Present

These profiles already exist under `~/.codex`:

```sh
codex --profile codex-gpt-5-3-codex-spark
codex --profile codex-satish
codex --profile auto-offline
```

The important profile files are:

```sh
~/.codex/codex-gpt-5-3-codex-spark.config.toml
~/.codex/codex-satish.config.toml
~/.codex/auto-offline.config.toml
```

## Local Ollama

Ollama is reachable at:

```sh
http://127.0.0.1:11434
```

The Codex config also points to the local Codex-compatible Ollama URL:

```toml
openai_base_url = "http://127.0.0.1:11434/api/codex/v1"
model_catalog_json = "/Users/satishgundu/.codex/ollama-launch-models.json"
```

Installed local Ollama models seen on 2026-09-21:

```sh
gemma3:4b
qwen2.5-coder:7b
nomic-embed-text:latest
```

Only these two are chat/code completion models:

```sh
gemma3:4b
qwen2.5-coder:7b
```

`nomic-embed-text:latest` is embeddings-only.

## How To Launch

Default OmniRoute/Spark:

```sh
codex
```

Explicit OmniRoute combo profiles:

```sh
codex --profile auto-best-coding
codex --profile auto-offline
codex --profile best-combo
codex --profile my-combo
```

Explicit OmniRoute/Spark profile:

```sh
codex --profile codex-gpt-5-3-codex-spark
```

Local Ollama provider:

```sh
codex --oss --local-provider ollama
```

Specific local Ollama model:

```sh
codex --oss --local-provider ollama -m qwen2.5-coder:7b
codex --oss --local-provider ollama -m gemma3:4b
```

## Quick Health Checks

List Ollama OpenAI-compatible models:

```sh
curl -sS http://127.0.0.1:11434/v1/models
```

List Ollama native tags:

```sh
curl -sS http://127.0.0.1:11434/api/tags
```

List OmniRoute models:

```sh
curl -sS -H "Authorization: Bearer $OMNIROUTE_API_KEY" http://localhost:20128/v1/models
```

Test OmniRoute combo:

```sh
curl -sS \
  -H "Authorization: Bearer $OMNIROUTE_API_KEY" \
  -H "Content-Type: application/json" \
  http://localhost:20128/v1/chat/completions \
  -d '{"model":"auto/best-coding","messages":[{"role":"user","content":"Reply with exactly: ok"}],"max_tokens":8}'
```

Test OmniRoute/Spark:

```sh
curl -sS \
  -H "Authorization: Bearer $OMNIROUTE_API_KEY" \
  -H "Content-Type: application/json" \
  http://localhost:20128/v1/chat/completions \
  -d '{"model":"codex/gpt-5.3-codex-spark","messages":[{"role":"user","content":"Reply with exactly: ok"}],"max_tokens":8}'
```

## Notes

- A model literally named `spark` was not listed by local Ollama.
- Spark is available through OmniRoute as `codex/gpt-5.3-codex-spark`.
- Preferred default is now the OmniRoute combo route `auto/best-coding`.
- Restart Codex after changing `~/.codex/config.toml`.
- Official Codex config reference: [https://developers.openai.com/codex/config-file/config-advanced](https://developers.openai.com/codex/config-file/config-advanced)

====



(base) ➜ satishgundu@Mac \~/mlx-lm/Spark-MLX-LLM git:(fix/tokenizer-and-thinking) % claude

Error: claude native binary not installed.

Either postinstall did not run (--ignore-scripts, some pnpm configs)

or the platform-native optional dependency was not downloaded

(--omit=optional).

Run the postinstall manually (adjust path for local vs global install):

  node node\_modules/@anthropic-ai/claude-code/install.cjs

Or reinstall without --ignore-scripts / --omit=optional.

(base) ➜ satishgundu@Mac \~/mlx-lm/Spark-MLX-LLM git:(fix/tokenizer-and-thinking) %



=====



&nbsp;



"models": [
  {
    "title": "Spark MLX",
    "model": "XHToken/Spark-X2.5-1.7B",
    "apiBase": "http://localhost:8080/v1",
    "provider": "openai"
  },
  {
    "title": "Qwen Coder",
    "model": "qwen2.5-coder",
    "provider": "ollama"
  },
  {
    "title": "Omniroute Free",
    "model": "omniroute-model-name", 
    "apiBase": "http://YOUR_OMNIROUTE_URL/v1",
    "provider": "openai",
    "apiKey": "your-omniroute-key"
  }
]