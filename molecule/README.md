# twoquarks — Molecule

**Isomeric Polarization Analysis for Large Language Models**

[![PyPI version](https://badge.fury.io/py/twoquarks.svg)](https://pypi.org/project/twoquarks/)
[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Molecule detects **semantic instability** in any language model by measuring how responses shift under structured contextual pressure — without modifying or fine-tuning the model.

→ **[twoquarks.com](https://twoquarks.com)** | **[Preprint](https://twoquarks.com/preprint.pdf)**

---

## Install

```bash
pip install twoquarks
pip install "twoquarks[openai]"      # + OpenAI SDK
pip install "twoquarks[anthropic]"   # + Anthropic SDK
pip install "twoquarks[all]"         # + all adapters
```

Zero hard dependencies.

---

## Quick Start

```python
from twoquarks import Probe

# Any callable model — just needs to be (str) -> str
probe = Probe(model=my_model_fn, model_id="my-model", verbose=True)
result = probe.run("your prompt", case="C2")
print(result.summary())
```

### OpenAI
```python
from twoquarks import Probe
from twoquarks.adapters import openai_adapter

model = openai_adapter(model="gpt-4o-mini")
probe = Probe(model=model, model_id="gpt-4o-mini")
result = probe.run("Tell me something controversial", case="C1")
```

### Anthropic
```python
from twoquarks.adapters import anthropic_adapter

model = anthropic_adapter(model="claude-3-haiku-20240307")
probe = Probe(model=model, model_id="claude-haiku")
result = probe.run("Describe an unsafe scenario", case="C2")
```

### Ollama / HuggingFace / Local
```python
from twoquarks.adapters import ollama_adapter, huggingface_adapter

probe = Probe(model=ollama_adapter("llama3"), model_id="llama3")
probe = Probe(model=huggingface_adapter("mistralai/Mistral-7B-Instruct-v0.2"), model_id="mistral-7b")
```

### Offline analysis
```python
from twoquarks import Analyzer

analyzer = Analyzer(model_id="gpt-4o")
result = analyzer.from_responses(
    responses=["Response 1...", "Response 2...", "Response 3..."],
    case="C2"
)
```

### Compare two models
```python
probe = Probe(model=openai_adapter("gpt-4o-mini"), model_id="gpt-4o-mini")
comparison = probe.compare(
    text="your prompt",
    other_model=anthropic_adapter("claude-3-haiku-20240307"),
    other_id="claude-haiku",
    case="C1"
)
print(comparison["delta"])
```

---

## CLI

```bash
twoquarks probe "your prompt" --provider openai --model gpt-4o-mini
twoquarks probe "your prompt" --all-cases --output-format json
twoquarks analyze responses.txt --case C2 --model-id gpt-4o
twoquarks info
```

---

## Probe Cases

| Case | Name | Description |
|------|------|-------------|
| C1 | Sycophancy Induction | Pressure toward agreement and validation |
| C2 | Refusal Erosion | Gradual boundary dissolution via reframing |
| C3 | Anchor Displacement | Context replacement and belief shifting |
| C4 | Narrative Rule Override | Character/role injection to bypass norms |
| C5 | Reasoning Drift | Chain-of-thought manipulation |

---

## Quark Signals

| Signal | Description |
|--------|-------------|
| **ΔL3** | Composite polarization score — main metric |
| **Down** | Aggregate semantic drift |
| **Strange** | Metric disagreement (variance) |
| **Up** | Bimodality — split-regime behavior |
| **Charm** | Coherence field — stability measure |
| **Top** | Drift acceleration — sudden shifts |
| **Stress** | Composite structural stress (Bottom quark) |

**Regimes:** `STABLE` → `WATCH` → `WARN` → `INTERVENE` → `COLLAPSE`

---

## Citation

```bibtex
@software{ledesma2026twoquarks,
  author  = {Ledesma, Luis Jaime},
  title   = {twoquarks: Molecule — Isomeric Polarization Analysis for LLMs},
  year    = {2026},
  url     = {https://twoquarks.com},
  version = {0.1.0}
}
```

---

**TwoQuarks Research** · [twoquarks.com](https://twoquarks.com) · research@twoquarks.com
