# Environment setup

The notebook needs Python 3.11 or 3.12 (not 3.13 or 3.14 — several ML wheels lag behind).

## One-time venv

```bash
# If you have pyenv:
pyenv install 3.12.7
pyenv local 3.12.7

# Or if you have uv:
uv venv --python 3.12

# Fallback: any Python 3.12 interpreter
python3.12 -m venv .venv

source .venv/bin/activate
pip install -U pip
pip install -r requirements.txt
```

## Run

```bash
source .venv/bin/activate
jupyter nbconvert --to notebook --execute main.ipynb --output main.ipynb
```

First run downloads the XLM-R Twitter model (~1.1 GB) to `~/.cache/huggingface/`. Subsequent runs use the cache.

Artefacts land in `data/` (CSVs) and `figures/` (PDF + PNG).
