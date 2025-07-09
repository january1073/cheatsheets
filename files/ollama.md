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

## Useful Models (as of 2025)

| Model Name        | Type            | Notes                        |
| ----------------- | --------------- | ---------------------------- |
| llama3            | General Chat    | High quality, open, 8B/70B   |
| mistral / mixtral | General Chat    | Small, fast, performant      |
| codellama         | Coding          | Good for Python, JS, etc.    |
| dolphin-mixtral   | Uncensored Chat | Looser filters, strong model |
| phi / tinyllama   | Lightweight     | Very small, basic use only   |
| wizardcoder       | Coding          | Strong in code-only tasks    |
| llava             | Vision + Text   | Accepts images (WIP)         |

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
