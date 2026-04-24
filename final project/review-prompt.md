# Comprehensive Review Prompt — COMP 550 Final Paper (BiasExploration)

You are a harsh but fair COMP 550 (McGill, Winter 2026) grader. Your job is to audit this final paper end-to-end and produce a prioritized punch list of everything that would cost marks. Do not praise. Do not hedge. Do not soften. If something is fine, stay silent on it; only surface problems.

The project tests whether XLM-RoBERTa-Twitter assigns more negative sentiment to U.S.-designated adversary countries in English than in Chinese, Russian, or Arabic, using 30 neutral templates × 30 countries. Final project is worth 35% of the course grade.

## Files you must read (absolute paths)

**Paper + exemplar:**
- `/Users/nabilmuzafar/Documents/GitHub/BiasExploration/final project/paper/paper.tex` — the paper source under review
- `/Users/nabilmuzafar/Documents/GitHub/BiasExploration/final project/paper/paper.pdf` (or project root `paper.pdf`) — compiled output, for page-count and figure-rendering checks
- `/Users/nabilmuzafar/Documents/GitHub/BiasExploration/final project/sample_paper.md` — **exemplar paper.** Treat this as the calibration anchor for "what a strong submission looks like." Compare our paper against it throughout.

**Rubric + guidelines:**
- `/Users/nabilmuzafar/Documents/GitHub/BiasExploration/final-project-guide.md` — internal rubric, section budgets, compression levers, pre-submission checklist
- `/Users/nabilmuzafar/Documents/GitHub/BiasExploration/COMP 550 Course Outline Winter 2026.pdf` — official course grading context
- `/Users/nabilmuzafar/Documents/GitHub/BiasExploration/nabil-style.md` — voice/style guardrails

**Source of truth for numeric claims:**
- `/Users/nabilmuzafar/Documents/GitHub/BiasExploration/main.ipynb` — notebook (read outputs; do NOT re-execute)
- `/Users/nabilmuzafar/Documents/GitHub/BiasExploration/data/` — CSVs backing every number in the paper
- `/Users/nabilmuzafar/Documents/GitHub/BiasExploration/figures/` — heatmap + interaction plot
- `/Users/nabilmuzafar/Documents/GitHub/BiasExploration/requirements.txt` — replicability claim

**Context:**
- `git log --oneline -30` in the repo — recent revision history

---

## Review workflow

Execute these steps in order. Do not skip any.

### Step 0 — Exemplar calibration (read `sample_paper.md` first)

Before judging our paper, read `final project/sample_paper.md` in full and record internally:
- Section-length proportions (how many pages/words each section gets)
- Tone and register (formal, confident, concise)
- Depth of method detail (how much replicability information is given)
- Style of the contributions statement (the sample uses a brief collaborative statement without per-author attribution — note this)
- Figure/table density and caption style
- How limitations and future work are framed
- Related Work per-paper differentiation style

Every rubric verdict below must cite a specific point of comparison to the exemplar (e.g., "our Related Work differentiates papers individually, matching exemplar §2", or "our Method is shallower than exemplar §3 on data construction").

### Step 1 — Rubric audit (5 graded dimensions from `final-project-guide.md`)

For each dimension, render a **verdict** (Strong / Adequate / Weak) with specific line/section citations from `paper.tex` and a comparison to the exemplar:

1. **Depth of content** — Is the research question substantive? Is the exploration thorough? Are claims backed by numbers?
2. **Justification of contribution vs. prior work** — Are all 3–5 related papers differentiated *individually* (not lumped as a theme)? Is the two-feature contribution claim (cross-lingual gap design + nonsense-country baseline) defensible?
3. **Correctness & experimental design** — Does the bipolar-valence definition S = P_pos − P_neg in the paper match `main.ipynb`? Are ANOVA assumptions acknowledged? Are p-values, η², Cohen's d reported with df and sign conventions? Does the nonsense-country baseline do what the paper claims it does?
4. **Quality of report** — Clarity, conciseness, replicability. Could a stranger re-run the experiment from Method alone?
5. **Attribution** — Every dataset, toolkit, model, and idea cited? BibTeX entries for the five core references (Barbieri et al., Faisal & Anastasopoulos, Li et al., Câmara et al., Goldfarb-Tarrant et al.) sanity-checkable against ACL Anthology? Any uncited claims?

### Step 2 — Required-section coverage

Confirm each section is present and meets its budget/content per `final-project-guide.md`:
- **Abstract** (~150–200 words; problem/method/key result/implication)
- **Introduction** (~1 page; opening, explicit RQ, explicit hypothesis, method summary, contribution)
- **Related Work** (~0.5–0.75 page; 3–5 papers, per-paper differentiation)
- **Method** (~1–1.5 pages; model, data/templates, countries, languages, translation validation, Gap definition, stats, hypothesis operationalization, baseline probe, replicability detail)
- **Results** (~1 page; actual numbers, tables/heatmaps, interpretation tied to hypothesis)
- **Discussion & Conclusion** (~0.5–0.75 page; hypothesis verdict with numbers, ≥4 specific limitations, 2–3 future directions)
- **Statement of Contributions** (~2–3 lines, balanced workload statement)

Flag any section that is missing, bloated, starved, or off-budget.

### Step 3 — TA feedback coverage

Verify each TA concern is explicitly addressed in the text (cite line numbers):
- U.S.-centric adversary framing is justified in Method (not just Limitations)
- Languages ≠ countries conflation is acknowledged in Limitations (Arabic spans allied/neutral/adversary states, etc.)
- Root-cause analysis via nonsense-country baseline is present in Results
- Neutral and allied categories are explicitly defined with operationalization criteria

### Step 4 — Numerical audit

Spot-check every number in Abstract, Results, and Discussion against `data/*.csv` and `main.ipynb` outputs. Produce a table (claim → source → match / mismatch / unverifiable). Flag:
- Any mismatch or rounding inconsistency
- Any stat reported without its companions (F without df, t without df, Cohen's d without sign convention, p-values without test name)
- Any number in prose that cannot be traced to a CSV/notebook cell

Do NOT re-execute the notebook. Read outputs only.

### Step 5 — Style & voice compliance (first-class)

Audit `paper.tex` against `nabil-style.md`. Flag each occurrence with line number and severity:

**Blockers** (will read as AI-written):
- Em dashes (`---` or `—`)
- Semicolons
- Banned AI vocabulary: *crucial, pivotal, key* (as adjective), *landscape, delve, tapestry, interplay, intricate, vibrant, testament, valuable, enhance, foster, showcase, highlight, underscore*

**Majors:**
- Filler phrases ("It is important to note that", "It should be noted that")
- Copula avoidance ("serves as", "stands as" where "is" would work)
- Formulaic section openers ("In this section, we describe...")
- Tacked-on participial phrases ("..., highlighting the importance of ...")
- Forced rule-of-three constructions
- Negative parallelisms ("not just X, it's Y")

**Minors:**
- Voice inconsistency across author sections (team-voice harmonization pass) — does Method read like the same author as Discussion? Any abrupt register shifts?

### Step 6 — Submission-risk checklist

Check each open item from `final-project-guide.md` §§10–11:
- Compiled PDF ≤ 5 pages (measure the actual `paper.pdf`)
- ACL style ruler visible, author block anonymized (review submission mode)
- BibTeX entries verifiable against ACL Anthology (spot-check the five core refs)
- `requirements.txt` — flag the known `sentencepiece==0.2.0` × `protobuf==5.27.2` conflict risk
- AI-use-policy disclosure inserted before `\bibliography` (present? missing?)
- Figures render correctly in compiled PDF, captions self-contained
- Code artifacts (`main.ipynb`, CSVs) present and clean for submission alongside the PDF

---

## Output format

Return a **single markdown report**, structured exactly as below. No preamble, no closing remarks.

```
# Paper Review — BiasExploration

**Verdict:** <ready | needs-revision | not-ready>

## Blockers
- [line X] <issue> — <fix>
- ...

## Majors
- [line X] <issue> — <fix>
- ...

## Minors
- [line X] <issue> — <fix>
- ...

## Nits
- [line X] <issue> — <fix>
- ...

## Numerical audit
| Claim (paper §/line) | Paper value | Source (file/cell) | Source value | Match? |
|---|---|---|---|---|
| ... | ... | ... | ... | ✓ / ✗ |

## Exemplar comparison
- Depth: <how we compare to sample_paper.md>
- Structure: <...>
- Voice: <...>
- Contributions statement: <...>
- Figures/tables: <...>

## Rubric scores (1–5 each, with one-sentence justification)
- Depth of content: <n> — <why>
- Contribution vs. prior work: <n> — <why>
- Correctness & experimental design: <n> — <why>
- Quality of report: <n> — <why>
- Attribution: <n> — <why>

## Submission-risk checklist
- [ ] PDF ≤ 5 pages — <actual pages>
- [ ] ACL ruler visible, author anonymized
- [ ] BibTeX verified
- [ ] requirements.txt clean
- [ ] AI-use disclosure present
- [ ] Figures render
- [ ] Code artifacts ready
```

Rules:
- Every finding must cite a line number from `paper.tex` (or a specific file path for non-paper issues).
- Be specific. "Method is unclear" is useless; "line 147: bipolar-valence definition omits the P_neu term implicit in the notebook's softmax — either document exclusion or reconcile" is useful.
- Fixes must be actionable in one sentence.
- Do not invent issues. If the paper is solid on a dimension, do not fabricate a problem to fill space.
- Do not praise. Silence on a point means "acceptable."
