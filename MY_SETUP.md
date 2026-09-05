# PR Agent — My Setup & Integration

## Original Project

**Original Repository:** https://github.com/Codium-ai/pr-agent *(listed as godo-ai/pr-agent → corrected via GitHub redirect to Codium-ai/qodo-ai)*

**Original Authors / Organization:** Codium AI / Qodo AI

**License:** Apache License 2.0 (also MIT per some redirects — Apache-2.0 per package)

**My Fork:** https://github.com/gajbhiyes774/pr-agent

> **Open Source Project — Setup & Integration** — Original code, license, attribution preserved.

---

## What This Project Does

AI-assisted pull request review across GitHub/GitLab/Bitbucket/Azure etc.:
- Commands `/review`, `/describe`, `/improve`, `/ask` via `pr_agent`
- Agent → tool orchestration via `pr_agent/agent/pr_agent.py`
- Dynaconf settings (`pr_agent/settings/`), Jinja2 prompts (StrictUndefined)

---

## Technologies

Python 3.12+ • uv • Dynaconf • LiteLLM • Starlette • ai handlers

---

## How I Configured It

### 1. Fork & Clone
```bash
gh repo fork Codium-ai/pr-agent --clone=false
# fork: https://github.com/gajbhiyes774/pr-agent
git clone --depth 1 https://github.com/Codium-ai/pr-agent.git
git remote add fork https://github.com/gajbhiyes774/pr-agent.git
```

### 2. Install
```bash
pip install pr-agent  # → attempts but fails dependency resolution: litellm==1.43.13 not found (available 1.99.0)
pip install pr-agent --no-deps
# → pr-agent 0.2.4 installed (bypasses litellm pin)
pip install starlette-context  # missing dep for cli import
```

Per `AGENTS.md`, official install is:
```bash
uv sync  # uses uv.lock
# then: PYTHONPATH=. uv run pr-agent --pr_url https://... review
# or: docker build -f docker/Dockerfile --target test .
```

---

## How I Ran It

```bash
python -c "import pr_agent; print('import ok')"
# → import ok

pr-agent --help
# → ModuleNotFoundError: atlassian (Bitbucket), plus many version mismatches
#   Full CLI requires: uv sync with full deps (atlassian, starlette_context, etc.)
```

---

## Problems Encountered & Solutions

| Problem | Diagnosis | Solution |
|---------|-----------|----------|
| `pip install pr-agent` → `ResolutionImpossible: litellm==1.43.13` (not found, now 1.99.0) | Pin is outdated | Use `--no-deps` or `uv sync` which respects lockfile |
| `ModuleNotFoundError: starlette_context` | Missing dep when using `--no-deps` | `pip install starlette-context` |
| `ModuleNotFoundError: atlassian` | Many provider deps | `uv sync` installs all — recommended per `AGENTS.md` |
| Tenacity/tiktoken/ujson/uvicorn version mismatches | Pin drift | Use `uv sync` (locked) instead of bare `pip` |

**Changes made:** None — only this `MY_SETUP.md`.

---

## My Contribution

- [x] Forked via GitHub fork (preserved attribution & Apache-2.0)
- [x] Cloned original
- [x] Installed `pr-agent 0.2.4` via `--no-deps` (verified import)
- [x] Documented correct `uv sync` path for full install

---

## Test Report

| Test | Result |
|------|--------|
| `pip install pr-agent --no-deps` | ✅ 0.2.4 |
| `import pr_agent` | ✅ |
| `pr-agent --help` | ⚠️ Needs full deps (`uv sync` + `atlassian`, etc.) — documented |

Full test per `AGENTS.md`:
```bash
uv sync
PYTHONPATH=. uv run pytest tests/unittest/test_fix_json_escape_char.py -q
```

---

## License Preservation

Apache-2.0 `LICENSE` retained.

---

## Portfolio Card

**PR Agent — Open Source Project — Setup & Integration**

- Original: https://github.com/Codium-ai/pr-agent
- My Fork: https://github.com/gajbhiyes774/pr-agent
- Tech: Python • PR Review • LLM
- My Work: Fork, pip install (no-deps), import verification, documented uv path
- License: Apache-2.0

