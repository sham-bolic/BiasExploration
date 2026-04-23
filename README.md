# BiasExploration

Tests whether **XLM-RoBERTa-Twitter** (`cardiffnlp/twitter-xlm-roberta-base-sentiment`) assigns more negative sentiment to U.S.-designated adversary countries in English than in Chinese, Russian, or Arabic. 30 neutral templates × 30 countries (adversary / ally / neutral) + 5 nonsense baselines.

## Layout

```
main.ipynb              Full pipeline: inference → Gap → stats → figures
data/                   Generated CSVs
figures/                fig1_heatmap.pdf, fig2_interaction.pdf
final project/paper.pdf Compiled paper
final project/paper/    LaTeX source (paper.tex, custom.bib, acl.sty)
final-project-guide.md  Methodology / writing guide
requirements.txt
```

## Run

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
```

Open `main.ipynb` and run top-to-bottom. Model downloads on first run (~1.1 GB). Uses `mps` on Apple Silicon, `cuda` if available, else CPU. Runtime ~5–10 min.

## Build paper

```bash
cd "final project/paper"
pdflatex paper.tex && bibtex paper && pdflatex paper.tex && pdflatex paper.tex
```

## Key numbers

- ANOVA: F(2, 27) = 8.75, p = .001, η² = .39
- Adversary-vs-rest contrast: t(27) = −4.16, one-tailed p = .0001, d = −1.63
- Ally vs neutral: null (Holm p = .625)
- Nonsense baseline: Gap ∈ [−0.068, −0.015], mean −0.042

## Reproducibility

Seed 42. Tokenization `max_length=128, truncation=True`. Inference `batch_size=64`, fp32. Stats via `statsmodels` (Type II SS) and `scipy`. Model revision not pinned.
