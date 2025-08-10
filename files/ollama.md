# Ollama Cheatsheet

## Installation

### macOS / Linux (Debian-based)
```bash
curl -fsSL https://ollama.com/install.sh | sh
````

### Windows (via MSI)

Download from: [https://ollama.com/download](https://ollama.com/download)

---

## Basic Commands

### List all local models

```bash
ollama list
```

### Pull (download) a model

```bash
ollama pull <model-name>
```

Example:

```bash
ollama pull llama3
```

### Run a model interactively

```bash
ollama run <model-name>
```

Example:

```bash
ollama run mistral
```

### Remove a model

```bash
ollama rm <model-name>
```

### Show system info (models, disk usage)

```bash
ollama info
```

---

## Custom Models

### Create a Modelfile

```bash
# Modelfile
FROM mistral
SYSTEM You are a helpful assistant.
```

### Build the model

```bash
ollama create mymodel -f Modelfile
```

### Run your custom model

```bash
ollama run mymodel
```

---

## API (Local REST API)

### Default endpoint

```
http://localhost:11434
```

### Example: Generate completion via curl

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "llama3",
  "prompt": "Explain quantum computing in simple terms.",
  "stream": false
}'
```

---

## Useful Models (as of August 2025, in alphabetical order)

| Model (exact name)                     | Type                          | Notes (usefulness for web app/API testing)                                                                            |
| -------------------------------------- | ----------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `codellama:7b-instruct`                | Code specialist               | One of the most reliable code models for Python, JS, and general API automation or testing.                           |
| `deepseek-coder:6.7b`                  | Code specialist               | Strong for code generation, PoCs, and API client development.                                                         |
| `gpt-oss:20b`                          | General / reasoning           | Excellent for complex reasoning, architecture analysis, or detailed testing plans; may be overkill for routine tasks. |
| `llama2:7b-chat`                       | General chat                  | Standard chat model; functional but outclassed by newer models for testing workflows.                                 |
| `llama3.1:8b`                          | General chat / reasoning      | High instruction-following quality and context handling; useful for complex testing scenarios.                        |
| `llava:7b`                             | Vision + text                 | Allows screenshot/UI analysis — useful in UI-assisted API testing or bug triage.                                      |
| `microsoft/phi-1_5`                    | Lightweight general           | Lightweight for quick checks, parsing, or embedding small reasoning in pipelines.                                     |
| `mistral:7b-instruct`                  | General / fast                | Fast, strong reasoning; ideal for rapid test iterations and automation scripts.                                       |
| `mistral:latest`                       | Duplicate (likely same as 7b) | Redundant variant; minimal extra value over instruct version.                                                         |
| `mistralai/Mixtral-8x7B-Instruct-v0.1` | General (Mixture-of-Experts)  | Good for multi-module reasoning and diverse API testing scenarios.                                                    |
| `qwen2.5:7b`                           | General purpose               | Strong all-rounder; excels at parsing prompts, API responses, and extracting structured data.                         |
| `qwen2.5-coder:7b`                     | Code specialist               | Better code generation than base Qwen; excellent for writing targeted test scripts.                                   |
| `TinyLlama/TinyLlama-1.1B-Chat-v1.0`   | Lightweight chat              | Minimal model for ultra-fast local testing, debugging, or quick response parsing.                                     |
| `wizardcoder:7b-python`                | Code specialist               | Python-focused, ideal for fuzzers, test frameworks, and exploit scripts.                                              |


---

## Notes

* Models are several GB in size. Download intentionally.
* `stream: false` gives whole output at once (useful for scripting).
* No GPU? Use 7B models or smaller for performance.
* Ollama stores models in: `~/.ollama`

---

## See Also

* Model Library: [https://ollama.com/library](https://ollama.com/library)
* GitHub Repo: [https://github.com/ollama/ollama](https://github.com/ollama/ollama)

Reach out: https://guns.lol/january1073
