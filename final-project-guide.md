# COMP 550 Final Project Report Guide

This guide encodes the complete approach for writing the team's final project paper. It is based on the assignment requirements, the approved proposal, TA feedback, the high-scoring reference paper (mBERT/UCCA), and Nabil's writing style.

---

## 1. Project overview

**Paper**: "Cross-Lingual Sentiment Disparity toward Geopolitical Adversaries"
**Team**: Nabil, Maximillian, Sebastien. All members contribute to design, implementation, analysis, and writing.
**Format**: ACL style file (submission version with ruler), PDF only
**Length**: 4.5-5 pages of content. References do NOT count toward this limit. Figures, tables, appendices, and equations DO count.
**Due**: April 13, 2026 (automatic extension to April 23)
**Also submit**: All code written for the project

### Repo structure

```
├── Association_for_Computational_Linguistics__ACL__conference/
│   ├── formatting.md
│   ├── README.md
│   └── latex/
│       ├── acl.sty              ← ACL style file
│       ├── acl_latex.tex        ← use this as the paper base
│       ├── acl_lualatex.tex
│       ├── acl_natbib.bst
│       └── custom.bib
├── final project/
│   ├── comp550 proposal.pdf
│   ├── Project Description.pdf
│   └── sample_paper.md
├── final-project-guide.md       ← this file (repo root, not inside final project/)
├── main.ipynb                   ← inference + analysis code
└── README.md
```

### Grading criteria (5 dimensions)
1. **Depth of content**: substantive question, thorough exploration
2. **Justification of contribution vs prior work**: clear positioning in the literature
3. **Correctness and experimental design**: sound methodology, valid analysis
4. **Quality of report**: clarity, conciseness, replicability
5. **Attribution**: proper citation of sources, datasets, and toolkits

---

## 2. Paper structure with page budget

Total budget: ~4.75 pages of content (aim for this, not 4.5 or 5.0 exactly).

| Section | Budget | Purpose |
|---------|--------|---------|
| Abstract | ~150-200 words | Problem, method, key finding, implication |
| Introduction | ~1 page | Motivation, research question, hypothesis, contribution summary |
| Related Work | ~0.5-0.75 page | 3-5 papers, explicit differentiation from each |
| Method | ~1-1.5 pages | Model, data, templates, translation, ANOVA design |
| Results | ~1 page | Tables, heatmaps, ANOVA interaction, interpretation |
| Discussion & Conclusion | ~0.5-0.75 page | Hypothesis verification, limitations, future work |
| Statement of Contributions | ~2-3 lines | Who did what |

---

## 3. Section-by-section guidance

### Abstract
Write this last. One sentence each for: problem, method, key result, implication. No hedging. Direct and confident.

**Pattern from reference paper:**
> This study explores the capabilities of mBERT in encoding semantic categories... We examine the model's performance... we observe that the intermediate to deeper layers... are generally the best-performing...

**For our paper, roughly:**
> We test whether multilingual sentiment models exhibit a "Geopolitical Language Gap" [problem]. We use XLM-RoBERTa with synthetic neutral templates across 4 languages and 30 countries [method]. We find that... [key result]. This suggests that... [implication].

### Introduction (~1 page)

**Must contain:**
- Opening: why cross-lingual sentiment bias matters (1-2 sentences, not a grand claim)
- Research question stated explicitly: "We test whether countries labeled as geopolitical adversaries receive more negative sentiment predictions in English than in three other major world languages (Chinese, Russian, Arabic)."
- Hypothesis stated explicitly: "We expect this English-vs-non-English gap to be larger in magnitude for adversary nations than for neutral or allied ones."
- Why these three non-English languages rather than each country's native language: the choice is justified briefly in Method (typological coverage, team verification capacity, balanced design), and the limitation that the aggregate does not correspond to any country's spoken language is named in the Discussion. Keeping the hypothesis at the level of "EN vs the three-language aggregate" prevents a mismatch between stated hypothesis and tested measure.
- Brief method summary (1-2 sentences)
- Contribution summary (what this paper adds that prior work does not)

**What the grader looks for**: Clear motivation, explicit hypothesis, positioning relative to prior work. The reference paper does this well by embedding related work comparisons directly in the introduction.

**Common mistakes**: Starting too broad ("NLP has made great strides..."), not stating the hypothesis, burying the research question.

### Related Work (~0.5-0.75 page)

**Must contain:**
- 3-5 papers cited and discussed
- For EACH paper: what they did, and how our work differs
- Organized by theme, not by paper

**Differentiation language (from reference paper):**
- "Our work shifts the focus from X to Y"
- "We build on their research by focusing on..."
- "Though X also performs Y, our paper specifically focuses on..."
- "Their study evaluates A, while ours evaluates B"

**Current papers from proposal (3, need 1-2 more):**
1. Barbieri et al. (2022) - XLM-T model family (we use their model)
2. Faisal & Anastasopoulos (2023) - geographic and geopolitical biases of language models (MRL Workshop)
3. Li et al. (2024) - language-dependent inconsistencies on geopolitical questions

**Two additional papers (confirmed):**

4. Câmara et al. (2022) — "Mapping the Multilingual Margins: Intersectional Biases of Sentiment Analysis Systems in English, Spanish, and Arabic." ACL Workshop on Language Technology for Equality, Diversity and Inclusion. Measures gender, racial, and ethnic biases in multilingual sentiment systems across English, Spanish, and Arabic. Differentiation: they study demographic group biases using equity evaluation corpora; we study geopolitical bias by country category using neutral synthetic templates. Their Arabic findings ground our language choice.

5. Goldfarb-Tarrant, Ross & Lopez (2023) — "Cross-lingual Transfer Can Worsen Bias in Sentiment Analysis." EMNLP 2023, pp. 5691–5704. Shows that cross-lingual transfer in sentiment models amplifies gender and racial bias compared to monolingual baselines, across five languages. Differentiation: they examine demographic bias amplification via transfer; we examine whether the bias manifests as a geopolitical language gap by comparing English to native-language scores for specific country categories. Their finding that transfer introduces directional bias supports the theoretical premise of our hypothesis.

### Method (~1-1.5 pages)

**Must contain (replicability is a grading criterion):**

1. **Model**: XLM-RoBERTa (twitter-xlm-roberta-base-sentiment), trained on ~198M tweets, fine-tuned on UMSAB. Justification to include in the Method:

   > We use twitter-xlm-roberta-base-sentiment (Barbieri et al., 2022) because it is the only widely available multilingual model that is both pretrained on large-scale social media data across 100+ languages and fine-tuned for three-class sentiment classification with a calibrated softmax output. mBERT (Devlin et al., 2019) provides multilingual representations but has no sentiment head, which would require us to fine-tune and introduce training artifacts that would confound the cross-lingual comparison. mT5 (Xue et al., 2021) is generative rather than a classifier, so extracting calibrated sentiment probabilities is non-trivial. Monolingual sentiment models would not allow the cross-lingual comparison our hypothesis requires.

2. **Data / Templates**: 30 neutral English templates, each containing a `[Country]` slot. Templates are organized into five structural families of six sentences each, giving syntactic variety while keeping every template free of sentiment-bearing vocabulary. Templates were written in English first, then translated into Chinese, Russian, and Arabic (validation procedure in subsection 4).

   **Design process (include in Method):**
   > Templates were written to avoid mental-state verbs (believe, think), evaluative adjectives, and constructions that presuppose a valence (e.g., "unfortunately"). Candidate templates that evoked a specific news event during informal review were rewritten. The five structural families (news, education, travel, conversation, descriptive) were chosen to produce syntactic variety across the [Country] slot's grammatical position (object of preposition, subject, object of verb) without introducing sentiment.

   **Template fixation (include in Method only if true at submission time):**
   > The 30 English templates and their translations are frozen in the submitted `main.ipynb` and were not adjusted in response to model outputs after inference was run. The submitted notebook reflects the templates used to produce the reported numbers.

   *Note: do not call this "pre-registration" unless templates were genuinely committed to a public repo before inference. The weaker phrasing above is accurate without overclaiming.*

   **Family A — News / media**
   1. I saw a news report about [Country].
   2. There was an article about [Country] in the newspaper.
   3. A podcast discussed [Country] this week.
   4. The documentary focused on [Country].
   5. I watched a segment about [Country] on TV.
   6. The magazine published a story about [Country].

   **Family B — Education / reference**
   1. We studied [Country] in class.
   2. I read a book about [Country].
   3. The textbook had a chapter on [Country].
   4. A lecture covered the history of [Country].
   5. My professor mentioned [Country] in her talk.
   6. The encyclopedia has an entry on [Country].

   **Family C — Travel / location**
   1. My friend traveled to [Country] last summer.
   2. A colleague of mine is from [Country].
   3. Someone I know lives in [Country].
   4. The flight from [Country] landed this morning.
   5. My neighbor visited [Country] last year.
   6. A student in my class came from [Country].

   **Family D — Conversation / mention**
   1. Someone mentioned [Country] yesterday.
   2. [Country] came up in our conversation.
   3. We talked about [Country] over dinner.
   4. A friend brought up [Country] at the meeting.
   5. I was reading something about [Country].
   6. [Country] was part of the discussion today.

   **Family E — Descriptive / geographical**
   1. [Country] shares borders with other nations.
   2. [Country] has several major cities.
   3. [Country] has a capital.
   4. [Country] is a member of the United Nations.
   5. [Country] issues its own passports.
   6. [Country] is shown on world maps.

3. **Country selection**: 30 countries, balanced across three categories of 10, operationalized as follows.

   **Adversaries (10)** — countries named as strategic competitors in the U.S. National Security Strategy (2022), appearing on the U.S. State Sponsors of Terrorism list, or under comprehensive U.S. sanctions regimes:
   China, Russia, Iran, North Korea, Cuba, Syria, Venezuela, Belarus, Nicaragua, Myanmar.

   **Allies (10)** — current NATO member states, selected for geographic spread across Western, Central, and Southern Europe plus North America:
   United Kingdom, France, Germany, Canada, Italy, Spain, Netherlands, Poland, Turkey, Norway.

   **Neutral (10)** — countries that have declared military neutrality, constitutional neutrality, or are prominent Non-Aligned Movement members:
   Switzerland, Austria, Ireland, Serbia, India, Indonesia, Brazil, South Africa, Mexico, Argentina. Sweden and Finland are deliberately excluded because they joined NATO in 2023–2024.

   **Address TA feedback** ("adversary with respect to whom?"): acknowledge explicitly in the Method that the adversary/ally framing is U.S.-centric. Justify this choice: the model under study (XLM-RoBERTa-Twitter) is trained predominantly on English-language social media, which over-represents U.S. and allied media discourse. Using a U.S.-centric geopolitical frame therefore tests whether the model's sentiment predictions align with the framing of its dominant training distribution. This asymmetry is also named in the Discussion as a limitation.

4. **Languages**: English, Chinese, Russian, Arabic. Translation validation uses a two-track procedure because the team has native/fluent coverage for Chinese and Arabic but not Russian.

   **Track 1 — fluent-speaker review (Chinese, Arabic):**
   > Chinese and Arabic translations were reviewed by the team member fluent in that language (Sebastien for Chinese, Nabil for Arabic). Each reviewer inspected translations for three criteria: (a) semantic fidelity to the English original, (b) absence of sentiment-bearing vocabulary introduced in translation, and (c) grammatical naturalness in the target language. Review also checked verb and adjective morphology that agrees with the `{country}` slot, to ensure strings remain grammatical across the full country set regardless of the country name's grammatical gender. Translations failing any criterion were revised until all three were satisfied.

   **Track 2 — MT consensus and back-translation (Russian):**
   > No team member is a native Russian speaker, so Russian translations were validated through a two-step automated procedure. Each template was translated from English into Russian using two independent machine translation systems (Google Translate and DeepL). A template was accepted only if both systems produced semantically equivalent Russian output, and if a back-translation of each Russian version into English preserved the meaning of the original template. Back-translations were additionally checked for gender-agreement failures in predicate position, since these are not caught by meaning-preservation alone. Templates that failed either check were rewritten in simpler English and reprocessed until agreement was reached. Because the templates are short, neutral, and syntactically plain, this procedure is appropriate for the register used in the study.

   **Gender-agreement confound identified and corrected:**
   > During validation, both tracks identified a shared bug class in six templates (Arabic E1, E5, E6 and Russian D2, D6, E6). Each of these templates contained a verb or participle whose morphology agrees with the `{country}` slot (e.g., Arabic `تظهر`, Russian `отображена`, both feminine-inflected). Because country names in the study vary in grammatical gender — masculine for e.g. العراق, لبنان, Иран, Китай and neuter for e.g. Перу, Чили — these templates produced grammatical output for some countries and ungrammatical output for others, introducing a grammaticality gradient correlated with country-name gender. This is qualitatively different from the uniform case-mismatch noise affecting other Russian templates: case-mismatch is constant across countries, while verb-gender mismatch is non-uniform and could bias XLM-RoBERTa sentiment downward for countries whose names trigger the disagreement. All six templates were rewritten to gender-invariant constructions — nominal sentences (`لـ {country} حدود مشتركة...`), impersonal plurals (`{country} обсуждали сегодня`), present-tense verbs without gender morphology (`{country} присутствует...`), or reframing so the grammatical subject is a gender-invariant noun (`Тема {country} всплыла...`, `اسم {country} مذكور...`, `تُصدر حكومة {country}...`). Post-rewrite back-translation confirmed semantic preservation against the English source.

   **Discussion implication:** name the asymmetry as a limitation. Russian validation is methodologically weaker than Chinese and Arabic validation, and any language-specific effect observed in Russian results should be interpreted with this asymmetry in mind.

5. **Measurement**: The model returns a three-class softmax over {Negative, Neutral, Positive}. Class probabilities are obtained by reading `model.config.id2label`, running `softmax` on the logits, and indexing by label (no hardcoded class index). The primary sentiment score is bipolar valence:

   > S(c, L) = P_pos(c, L) − P_neg(c, L),
   > averaged across all 30 templates for country *c* in language *L*. S ranges in [−1, 1]: positive values mean the model assigns a net-positive reading, negative values a net-negative reading.

   Bipolar valence is used rather than P(neg) alone because cross-lingual shifts can move probability mass between any two of the three classes, and P(neg) alone would miss shifts between Neutral and Positive. P(neg) is retained as a secondary measure and reported in a sensitivity analysis to confirm that conclusions do not depend on the choice.

   **This is a non-standard measure, and the assignment explicitly requires non-standard measures be defined.** The Method must open its measurement subsection with the formal definition:

   > For each country *c*, we define the Cross-Lingual Sentiment Gap as
   > Gap(c) = S_EN(c) − mean(S_ZH(c), S_RU(c), S_AR(c)),
   > where S_L(c) is the bipolar valence score (P_pos − P_neg) averaged across the 30 templates for country *c* in language *L*. A negative Gap indicates the model assigns a more negative sentiment in English than in the aggregate of the three non-English languages tested.

   Rationale for aggregating across ZH, RU, AR: the four languages in the study are fixed (EN, ZH, RU, AR), and tying "native" to each country's official language would leave most allied and neutral countries without a comparable non-English score, breaking the balanced design. Averaging across the three non-English languages gives every country a comparable Gap value and operationalizes "cross-lingual disparity" uniformly across all 30 countries. The limitation that the non-English aggregate does not correspond to each country's actual spoken language is named explicitly in the Discussion.

   **Per-language supplementary gaps:** because the aggregate Gap averages over ZH, RU, and AR, it could mask opposite-sign effects. Per-language gaps Gap_L(c) = S_EN(c) − S_L(c) for L ∈ {ZH, RU, AR} are also reported, and Results states whether the aggregate Gap is driven consistently by all three languages or dominated by one.

6. **Statistical analysis**: The primary analysis is a one-way ANOVA on the 30 per-country Gap values across the three country categories (Adversary, Ally, Neutral), combined with a planned directional contrast. The two-way ANOVA (language × category) on raw scores is reported as a supplementary analysis. Significance level α = .05. Full code in `main.ipynb`; the Method describes the design in prose.

   **Why the one-way-on-Gap design is primary:** the hypothesis is about country-category differences in the English-vs-non-English disparity. Computing Gap per country and testing whether the three categories differ on that measure puts the hypothesis directly into the test. It also makes the unit of inference (the country) transparent and avoids the pseudo-replication risk of treating 30 shared templates as independent observations. The supplementary 4 × 3 ANOVA on raw scores checks whether the pattern also appears in the decomposed language × category interaction.

   **Unit of analysis (include in Method):**
   > Each of the 30 countries has one Gap value, defined as S_EN − mean(S_ZH, S_RU, S_AR) with each S_L computed as the bipolar valence score averaged across the 30 templates. The 30 Gap values are partitioned into three balanced groups of 10 (Adversary, Ally, Neutral) for the primary one-way ANOVA. For the supplementary two-way ANOVA, country-language means across templates yield n = 120 observations in a balanced 30 × 4 layout that collapses to 4 × 3 at the category level.

   **ANOVA type justification (include in Method):**
   > The primary one-way ANOVA has a single factor (category) so ANOVA-type choice does not apply. The supplementary two-way design is fully balanced (10 countries per category, 4 languages per country), so Type I, II, and III sums of squares produce identical F-statistics. We report Type II, the default in `statsmodels.anova_lm`. Assumption checks (Levene's test for variance homogeneity across groups, Shapiro–Wilk on residuals) are reported alongside the ANOVA table.

   **Bounded-response acknowledgment:** the bipolar valence score is bounded in [−1, 1]. The OLS ANOVA normality assumption is therefore approximate. A Kruskal–Wallis rank-based sensitivity check is run in `main.ipynb`. The P(neg)-only sensitivity analysis is also reported to confirm that conclusions do not depend on the choice of sentiment measure.

   **Hypothesis operationalization (state this in the Method, before Results):**
   > The hypothesis is considered supported if both conditions hold: (a) the one-way ANOVA on per-country Gap values across the three categories is significant at p < .05, and (b) the planned directional contrast with weights [+2, −1, −1] applied to [Adversary, Ally, Neutral] Gap means is significant at p < .05 (one-tailed) in the predicted direction (adversaries more negative in EN than in the non-English aggregate relative to allies and neutrals). The one-tailed framing matches the directional prediction pre-specified in the proposal. A significant ANOVA alone is insufficient because the omnibus F does not identify direction.

   **Effect size reporting:** η² for the one-way ANOVA on Gap; Cohen's d for the planned contrast. **Post-hoc tests** (only if the ANOVA is significant): three pairwise comparisons of category Gap means with Holm–Bonferroni correction. The planned contrast is pre-specified and is not corrected.

   **Contrast computation (prose summary; full code in `main.ipynb`):** the planned contrast is computed on the 30 per-country Gap values. Standard error uses the pooled residual MSE from the one-way ANOVA on Gap (pooled_sd = √MSE); SE(contrast) = pooled_sd · √(Σ wᵢ²/nᵢ) = pooled_sd · √(6/10); residual df = 30 − 3 = 27. If Levene's test rejects equal variances, a Welch-corrected variant is reported alongside. Cohen's d for the contrast uses pooled_sd from the same MSE. Record `statsmodels`, `scipy`, `numpy`, `pandas`, `torch`, `transformers` versions, the model revision SHA, and the random seed in the notebook header for provenance.

7. **Nonsense-country baseline probe (root-cause sub-experiment).** The TA asked us to analyze why any observed disparity happens. We include a baseline experiment: run all 30 templates × 4 languages with the `[Country]` slot replaced by five fictitious country names (Zorbia, Kelandor, Traviska, Nuberia, Sambalos), chosen to span the token-length distribution of real country names. Five names rather than one provides a small distribution rather than a single point, reducing the chance that an idiosyncratic subword produces the baseline.

   **Interpretation rule (qualitative, not inferential):** the per-language mean and range of the nonsense-country Gaps are reported in Figure 1 as a reference band next to the real-country Gaps. If the real-country Gap for Adversaries sits well outside the nonsense-country range while Ally and Neutral Gaps sit within it, the effect is plausibly country-identity-driven. No hypothesis test is run on n = 5 vs n = 10 per category because power would be too low to be informative either way; the baseline is descriptive and is framed that way in the Results.

   **Limitation:** fictitious names still tokenize to unfamiliar subwords, so the probe isolates country-identity signal imperfectly. The small baseline sample also limits the strength of any distributional claim. Named in Discussion.

8. **Attribution checklist** (criterion 5 of the grading rubric — attribution of sources, datasets, and toolkits). Every item below must be cited in the paper, either inline in Method or as part of References:
   - Model: `cardiffnlp/twitter-xlm-roberta-base-sentiment` → Barbieri et al. (2022)
   - Training data for the fine-tuning: UMSAB → Barbieri et al. (2022)
   - Transformers library: HuggingFace `transformers` (cite the Wolf et al. 2020 paper if referenced, else cite the library version in a footnote)
   - Statistical library: whatever Sebastien used (scipy.stats / statsmodels / R) — cite with version number
   - Plotting library: matplotlib / seaborn — cite with version number
   - Translation method: if machine-translated, name the service (Google Translate, DeepL, etc.) and cite; if human-translated, state who verified each language
   - Geopolitical framework sources: U.S. National Security Strategy (2022), NATO membership list, non-aligned movement reference

**What the grader looks for**: Could another researcher replicate this from the description alone?

### Results (~1 page)

**Must contain:**
- Table(s) with actual numbers (category-level averages per language)
- Heatmap(s) showing sentiment scores by country and language
- ANOVA results: F-statistic, p-value, effect size
- Interpretation of each result tied back to the hypothesis

**Interpretation pattern (from reference paper):**
> For English (Train: En, Test: En), the peak accuracy of ~0.760 occurs with layer 7... This result matches that of Tenney et al. (2019) which state that...

**For our paper:**
- State the finding with numbers
- Connect to the hypothesis ("This supports/does not support our hypothesis that...")
- Connect to prior work where relevant
- Use "This means that" to explain implications

**Address TA feedback on root cause**: If a disparity is found, include analysis of whether it comes from (a) the training data distribution in each language, or (b) the sentiment classifier itself. Even a brief discussion (e.g., comparing the model's behavior on country-neutral vs country-specific templates) addresses this.

### Discussion & Conclusion (~0.5-0.75 page)

**Must contain:**
1. **Hypothesis verification**: explicitly state whether results support or refute the hypothesis, with the specific numbers. Use one of the two templates below based on outcome.

   **If hypothesis is supported (one-way ANOVA on Gap significant AND planned contrast significant in the predicted direction):**
   > Our results support the hypothesis of a Cross-Lingual Sentiment Gap. The one-way ANOVA across country categories on the 30 per-country Gap values is significant (F(2, 27) = Z.ZZ, p = .XXX, η² = .XX), and the planned contrast confirms that the adversary Gap (M = X.XX) differs from the mean of ally and neutral Gaps (M = X.XX) in the predicted direction, t(27) = Z.ZZ, p = .XXX (one-tailed), Cohen's d = X.XX. The nonsense-country baseline falls within [range], meaning adversary Gaps sit outside the baseline range while ally and neutral Gaps sit within it, consistent with a country-identity-driven effect rather than a template artifact. This suggests that XLM-RoBERTa-Twitter encodes geopolitically-aligned sentiment differences that vary systematically with the language of the prompt.

   **If hypothesis is not supported (ANOVA non-significant OR contrast non-significant or in the wrong direction):**
   > Our results do not support the hypothesis of a Cross-Lingual Sentiment Gap. The one-way ANOVA on per-country Gap values was [significant / non-significant] (F(2, 27) = Z.ZZ, p = .XXX, η² = .XX), and the planned contrast comparing the adversary Gap against the mean of ally and neutral Gaps was [non-significant / in the opposite direction], t(27) = Z.ZZ, p = .XXX (one-tailed), Cohen's d = X.XX. Real-country Gaps [fall within / diverge from] the nonsense-country baseline range, consistent with the observed cross-lingual variance being [template-driven / small and category-invariant]. With the current template set and language coverage, XLM-RoBERTa-Twitter does not exhibit the geopolitically-patterned cross-lingual sentiment bias we hypothesized. A null finding at this granularity is informative given the sample is small: it places an upper bound on effect size rather than establishing the absence of bias.

   Fill in the bracketed values from the actual `main.ipynb` output. Do not adjust the hypothesis after seeing results. A null finding is still a contribution.
2. **Limitations** (address all of these):
   - Synthetic templates may not reflect real-world discourse
   - Language ≠ country (TA feedback): Arabic is spoken in allied, neutral, and adversary states. The mapping of language to geopolitical position is an oversimplification.
   - Western-centric categorization (TA feedback): adversary/ally is defined relative to U.S. interests
   - No control for training data volume per language
   - Translation validation is asymmetric: Chinese and Arabic were reviewed by fluent team members, while Russian relied on MT consensus and back-translation because no team member is a native Russian speaker. No formal inter-rater reliability was computed.
3. **Future work**: 2-3 concrete directions (real tweets, more languages, different models, root cause analysis with probing)

**What the grader looks for**: Honest assessment, not overclaiming. Specific limitations, not "more work is needed."

### Statement of Contributions

Keep it brief and factual (2-3 lines). The reference paper does this well:
> The entire group contributed to researching topics for the paper. Initially, we worked individually... We collaborated on all aspects of the project...

For our paper (written in the sample paper's voice, adapted to our pipeline):
> The entire group contributed to researching topics for the paper. Initially, we worked individually to explore the literature on multilingual sentiment bias and to draft candidate templates and country categorizations, before agreeing on the final design. We collaborated on all aspects of the project, in the code, specifically, template translation and verification, the inference pipeline with XLM-RoBERTa, statistical analysis with two-way ANOVA and the planned contrast, visualizations, as well as the final write-up.

---

## 4. Writing voice

This is a team paper. Use **"we/our"** throughout (ACL convention). Otherwise, apply Nabil's full style.

### Patterns to use

| Pattern | Example |
|---------|---------|
| Confident "will" | "This approach will capture sentiment differences across languages" |
| "This means that" | "Accuracy drops from 71% to 58% in cross-lingual settings. This means that the model encodes language-specific features." |
| "This suggests that" | "The gap is largest for adversary nations. This suggests that training data imbalance plays a role." |
| "likely because" | "French scores higher than Arabic, likely because the model saw more French tweets during pretraining." |
| "rather than" | "We extract softmax probabilities rather than argmax labels, because continuous scores reveal finer differences." |
| Comma-joined clauses | "We use synthetic templates to control for content, and we translate each template into four languages to isolate the effect of language." |

### Patterns to NEVER use

**Punctuation blacklist:**
- Em dashes: never. Use commas or periods.
- Semicolons: never. Split into two sentences or use a comma.

**Vocabulary blacklist:**
- crucial, pivotal, key (adj), landscape, delve, tapestry, interplay, intricate, vibrant, testament, valuable, enhance, foster, showcase, highlight, underscore
- Additionally, Furthermore, Moreover (use "also" or start a new sentence)
- "It is important to note that" (just say it)
- "serves as" / "stands as" (use "is")

**Structural blacklist:**
- Rule of three
- Negative parallelisms ("It's not just X, it's Y")
- Tacked-on -ing phrases ("highlighting the importance of...")
- Formulaic section openers ("In this section, we describe...")
- Parallel definitions ("X is A. Y is B. Z is C." with identical structure)
- Every evidence sentence starting with "Table X shows..."

### Team consistency
All three authors must match this voice. Before submission, one person (Nabil) should do a full pass to harmonize tone across sections written by different people. Check for:
- Consistent "we/our" (no "I" or "the authors")
- Same terminology throughout (pick one term and stick with it)
- Sentence length variation in every section
- No AI vocabulary in any section

---

## 5. Humanization for longer-form writing

The same AI-avoidance rules from 1-page summaries apply, but longer papers have additional risks:

1. **Vary evidence introduction across the paper**: If the Results section has 5 findings, don't start all 5 with "Table X shows." Mix in "The ANOVA reveals...", "Across all four languages,...", "When we compare...", etc.

2. **No formulaic paragraph structure**: Don't make every paragraph follow setup-evidence-interpretation in the same order. Some paragraphs can lead with the finding, some with context.

3. **Section transitions should be natural**: Don't use "Having described our method, we now turn to results." Just start the Results section with the results.

4. **Passive voice is fine in moderation**: "The model was trained on 198M tweets" is natural. But don't write an entire paragraph in passive.

5. **Read each section aloud independently**: Does it sound like a researcher explaining their work at a lab meeting? If it sounds like a Wikipedia article, rewrite.

---

## 6. Formatting

- **Template**: ACL style files are already in the repo at `Association_for_Computational_Linguistics__ACL__conference/latex/`
  - Use `acl_latex.tex` as the paper base (copy it into a new working file, e.g., `paper.tex`)
  - `acl.sty` and `acl_natbib.bst` must be in the same directory as `paper.tex` when compiling
  - The document class line must be `\documentclass[review]{acl}` — the `review` option enables the submission ruler
  - To use Overleaf: upload `acl_latex.tex`, `acl.sty`, `acl_natbib.bst`, and `custom.bib` together
- **Output**: PDF compiled from LaTeX
- **Page count**: 4.5-5 pages of content (text + figures + tables + equations + appendices). References are excluded.
- **Figures/tables**: Heatmap (countries × languages, color = negative score) and category-level summary table are the minimum. An ANOVA interaction plot (language × category) is a strong addition if space allows.
- **Code**: `main.ipynb` is the submission artifact. Before submitting:
  - Clear all outputs, re-run top-to-bottom from a clean kernel, verify it completes without errors
  - Add a markdown cell at the top explaining: dependencies and how to install them (`pip install ...` or a `requirements.txt`), the order of cells, and how to reproduce the results table
  - Set a fixed random seed if any stochastic step exists (tokenization/generation should be deterministic for this model, but state it)
  - Save the raw output data (per-country per-language per-template scores) to a CSV or JSON file that is submitted alongside the notebook, so the grader can verify the numbers in the paper without re-running inference

---

## 8. Execution plan (5 days: April 18–23)

### Phase 1 — Setup + run experiments (April 18, blocking everything else)

- [ ] Set up shared Overleaf project (or local LaTeX) using `acl_latex.tex` from `Association_for_Computational_Linguistics__ACL__conference/latex/` as the base. Upload `acl.sty`, `acl_natbib.bst`, and `custom.bib` alongside it. Add BibTeX entries for all 5 papers.
- [ ] Run `main.ipynb` inference pipeline end-to-end — all 30 templates × 4 languages × 30 countries → raw softmax scores saved to CSV/JSON
- [ ] Run the nonsense-country baseline probe (Zorbia) across all 30 templates × 4 languages
- [ ] Run one-way ANOVA on the 30 per-country Gap values across Adversary/Ally/Neutral (primary), record F, df, p, η²; run Levene and Shapiro
- [ ] Run planned directional contrast [+2, −1, −1] on category Gap means; record t, df=27, one-tailed p, Cohen's d
- [ ] Run supplementary two-way ANOVA (language × category) on raw bipolar scores; record interaction F, p, partial η²
- [ ] Run P(neg)-only and Kruskal–Wallis sensitivity checks
- [ ] Produce heatmap (countries × languages, with nonsense-country reference band) and category-level summary table
- [ ] Confirm the exact statistical library/function calls in `main.ipynb` so the Method section matches reality

### Phase 2 — Draft core sections (April 19)

- [ ] Method section drafted (model, data pipeline, templates with examples, country list, languages, Gap definition, baseline probe, statistical design)
- [ ] Results section drafted once numbers are in — fill table, write interpretation for each finding, include baseline probe comparison

### Phase 3 — Draft framing sections (April 20)

- [ ] Introduction (motivation, RQ, hypothesis, method summary, contribution)
- [ ] Related Work (5 papers, explicit differentiation for each)

### Phase 4 — Draft closing sections (April 21)

- [ ] Discussion & Conclusion (hypothesis verdict with numbers, 4+ specific limitations, 2-3 future directions)
- [ ] Statement of Contributions (final version)
- [ ] Abstract — write last, after all sections are drafted

### Phase 5 — Review and submit (April 22–23)

- [ ] Full voice-harmonization pass across the paper (em dashes, AI vocabulary, we/our consistency, sentence length variation)
- [ ] Run pre-submission checklist (Section 11 of this guide) item by item
- [ ] Format check: ACL style file with ruler, 4.5–5 pages, all figures captioned, references complete
- [ ] Code (`main.ipynb`): clear outputs, re-run from clean kernel top-to-bottom, add a markdown cell at top explaining how to run. Verify it completes without errors.
- [ ] Submit PDF + code on myCourses before April 23

---

## 9. TA feedback (must be addressed)

The TA raised four points. Each must be addressed somewhere in the paper:

| TA concern | Where to address | How |
|------------|-----------------|-----|
| "Adversary with respect to whom?" | Method section | State framework explicitly: U.S. NSS 2022, NATO membership. Acknowledge U.S.-centric framing. |
| "Languages are spoken in many countries" | Discussion/Limitations | Name this as a limitation: Arabic spans allied (Jordan), neutral, and adversary states. Language is a proxy, not a direct mapping. |
| "Analyze why the disparity happens" | Results | Nonsense-country baseline probe (Method subsection 7): compare real-country Gaps against a "Zorbia" placeholder baseline to isolate whether the effect is country-identity-driven or a template/language artifact. |
| "Define neutral/allied explicitly" | Method section | List operationalization criteria with specific examples. |

---

## 10. Planning status

Three states: **Drafted** = fully written into the guide and ready to use as-is. **Planned** = decision made, approach committed, but execution still required. **Open** = unresolved, needs action.

| Item | Status | Where |
|------|--------|-------|
| Related work (5 papers) | Drafted | Method RelWork section, Section 13 BibTeX |
| Model justification (XLM-R vs mBERT vs mT5) | Drafted | Method subsection 1 |
| 30 English templates (5 families × 6) | Drafted | Method subsection 2 |
| 30-country list with operationalization | Drafted | Method subsection 3 |
| Translation protocol (two-track) | Drafted | Method subsection 4 |
| Bipolar score and Gap definition | Drafted | Method subsection 5 |
| One-way ANOVA on Gap + planned contrast | Drafted | Method subsection 6 |
| Supplementary two-way ANOVA | Drafted | Method subsection 6 |
| Kruskal–Wallis and P(neg) sensitivity | Drafted | Method subsection 6, Section 16 cell 16 |
| Hypothesis operationalization | Drafted | Method subsection 6 |
| Nonsense-country baseline (descriptive) | Drafted | Method subsection 7 |
| Discussion verdict templates | Drafted | Section 3 Discussion |
| Figure and table specs | Drafted | Section 14 |
| Page-budget compression levers | Drafted | Section 15 |
| Notebook cell-by-cell scaffold | Drafted | Section 16 |
| Template neutrality sanity rating | Drafted | Section 17 |
| BibTeX entries (5 core + 4 toolkits) | Drafted (pending final check) | Section 13 |
| 90 non-English translations | Drafted | `main.ipynb` cell 4, all three languages reviewed |
| ZH translation review (Sebastien) | Drafted | Fluent-speaker check passed against guide §4 Track 1 criteria |
| AR translation review (Nabil) | Drafted | Fluent-speaker check passed against guide §4 Track 1 criteria |
| RU MT consensus + back-translation check | Drafted | Guide §4 Track 2; gender-agreement fixes merged (PR #2) |
| Notebook build (cells 1–17) | Drafted | `main.ipynb` populated from Section 16 scaffold |
| `requirements.txt` + `SETUP.md` | Drafted | Pinned deps for Python 3.11/3.12; venv instructions in `SETUP.md` |
| Smoke-test inference run | Drafted (2026-04-19) | Pipeline runs clean; candidate-translation numbers show hypothesis supported (one-way F(2,27)=8.115, p=.0017, η²=.375; contrast t=−4.02, p=.0002, d=−2.20) |
| Final inference run and CSV artefacts | Done (2026-04-22) | `data/raw_scores.csv`, `country_language_means.csv`, `gap_per_country.csv` |
| Team neutrality ratings (30 templates × 3 raters) | Drafted | Guide §17; human raters passed; independent LLM rater mean 3.07 (C1/C5 flagged mild +) |
| One-way ANOVA, contrast, post-hocs | Done (2026-04-22) | Final: F(2,27)=8.747, p=.0012, η²=.393; contrast t=−4.163, p=.0001, d=−2.28 |
| Figure 1 heatmap, Figure 2 interaction plot | Done (2026-04-22) | `figures/fig1_heatmap.{png,pdf}`, `figures/fig2_interaction.{png,pdf}` |
| Paper drafting (Abstract → Conclusion) | Drafted (2026-04-22) | `final project/paper/paper.tex`; Overleaf zip at `final project/paper.zip` |
| Author block (names + student IDs) | Drafted (2026-04-22) | `paper.tex` author block: Nabil Bin Muzafar Shah (261153850), Maximillian Fong (261120319), Sebastien Chow (261044349) |
| Notebook hygiene (cells, seeds, paths, outputs) | Verified (2026-04-22) | Audited by subagent against §16 scaffold: all 17 cells in order, seeds set, `model.config.id2label` used, batched softmax (no pipeline), all CSVs + figure PDFs/PNGs produced, no hardcoded paths or debug output |
| Paper numbers cross-checked against data | Verified (2026-04-22) | Audited by subagent: only rounding deltas <0.005; substantive values all match `data/*.csv` and notebook cell outputs |
| Template neutrality self-rating | Removed (2026-04-22) | Dropped from paper and from §17; nonsense-country baseline is the primary defence against template-driven artefacts |
| BibTeX final verification against ACL Anthology | Open | 5 core entries (Barbieri, Faisal, Li, Câmara, Goldfarb-Tarrant) copied verbatim from §13 (verified 2026-04-19). 4 toolkit entries in `paper/custom.bib` (Wolf, Seabold, Hunter, Waskom) still need a DOI/ACL-Anthology sanity check |
| Overleaf submission bundle | Drafted (2026-04-22) | `final project/paper.zip` contains `paper.tex`, `custom.bib`, `acl.sty`, `acl_natbib.bst`, `figures/fig1_heatmap.pdf`, `figures/fig2_interaction.pdf`. Compile on Overleaf with pdfLaTeX |
| Page-count check on compiled PDF | Open | Target 4.5–5 pages content (refs excluded, appendices + figures included). Measure on Overleaf first; appendix Table 2 (per-country Gap) can be dropped to save ~0.4 page if over |
| Fresh-venv install smoke-test | Open | Subagent flagged potential `sentencepiece==0.2.0` × `protobuf==5.27.2` conflict in `requirements.txt`. Current pins work on the machine that produced the notebook outputs, but a clean `pip install -r requirements.txt` on a fresh venv should be run before submission to confirm |
| Code submission bundle (notebook + CSVs + figures + setup) | Open | Required by assignment alongside PDF: `main.ipynb`, `data/*.csv`, `figures/*.pdf`, `requirements.txt`, `SETUP.md` |

**TA feedback:** the adversary framing, language ≠ country, root-cause analysis, and neutral/allied definitions are all addressed in the Method and Discussion as shown in Section 9 — Drafted, not yet written into the final paper.

### Status-update discipline

Section 10 is updated whenever an Open item moves to Planned/Drafted/Done, whenever a new dependency is discovered, or at the end of any session that changed artefact state. The rule is: if the repo state no longer matches what this table says, this table is wrong and must be edited before ending the session. Status drift makes the guide lie to future sessions, so the guide loses its "single source of truth" role. Statuses are **Drafted** (written into guide or artefact, ready to use), **Planned** (decision locked, execution pending), **Open** (unresolved).

---

## 11. Pre-submission checklist

### Content
- [ ] Abstract covers problem, method, result, implication in ~150-200 words
- [ ] Introduction states research question and hypothesis explicitly
- [ ] Related work cites 3-5 papers with explicit differentiation for each
- [ ] Method is replicable: model name, dataset, template examples, country list, statistical tests, significance thresholds
- [ ] Non-standard evaluation measure (Cross-Lingual Sentiment Gap, based on bipolar valence) is formally defined before it is used
- [ ] All toolkits and datasets cited: model (Barbieri et al.), UMSAB, transformers library, stats library, plotting library, translation tool, geopolitical framework sources
- [ ] Method addresses TA feedback: geopolitical mapping justification, language ≠ country acknowledgment
- [ ] Results include actual numbers: means, p-values, effect sizes
- [ ] Results connect findings to hypothesis ("This supports/does not support...")
- [ ] Results include root cause analysis attempt (training data vs classifier)
- [ ] Discussion explicitly verifies or refutes the hypothesis
- [ ] Discussion lists 4+ specific limitations (synthetic data, language-country mapping, Western framing, translation quality)
- [ ] Discussion has 2-3 concrete future work directions
- [ ] Statement of contributions is present and shows balanced workload
- [ ] All sources, datasets, and toolkits are cited

### Style
- [ ] "We/our" used throughout (no "I", no "the authors")
- [ ] Zero em dashes
- [ ] Zero semicolons
- [ ] No AI vocabulary (crucial, pivotal, key, landscape, delve, etc.)
- [ ] No rule of three, no negative parallelisms, no tacked-on -ing phrases
- [ ] Confident "will" statements for findings
- [ ] "This means that" / "This suggests that" / "likely because" for explanations
- [ ] Sentence length varies within every section
- [ ] Evidence introduced in varied ways across the paper
- [ ] Read aloud: sounds like a researcher, not a Wikipedia article
- [ ] One person (Nabil) did a final voice-harmonization pass

### Formatting
- [ ] ACL style file with ruler
- [ ] 4.5-5 pages of content (not counting references)
- [ ] All figures and tables have captions
- [ ] References are complete (authors, title, venue, year)
- [ ] PDF is the submitted format
- [ ] Code is submitted alongside the report

---

## 12. Reference patterns from the high-scoring paper

### Introduction hypothesis statement
> We hypothesize that mBERT's embeddings will most effectively encode semantic roles in monolingual settings within deeper layers... For cross-lingual settings, we predict that the deeper layers will also be most effective, but the overall classifier accuracy will be lower.

Pattern: state the hypothesis, then state what you expect to see if it is true.

### Related work differentiation
> Our work shifts the focus of mBERT's performance within their layers to cross-lingual semantic abilities...
> Our study builds off of their research by emphasizing on the encoding of UCCA semantic roles...
> Though our paper also performs an evaluation on mBERT's layers, it specifically focuses on...

Pattern: acknowledge what prior work did, then pivot to what you do differently.

### Method detail level
> The data was split into training (80%) and test (20%) sets... The balanced_weight parameter in LogisticRegression is also set to True to account for the imbalance...

Pattern: specific enough to replicate. Name the library, the parameters, the split ratio.

### Results interpretation
> For English (Train: En, Test: En), the peak accuracy of ~0.760 occurs with embeddings from layer 7... This result matches that of Tenney et al. (2019)...

Pattern: number first, then interpretation, then connection to prior work.

### Honest limitations
> While this study provides insight... one limitation of the experimentation is the dataset used in this study. The French-English parallel corpora used in this study to train and test the models are relatively small, as well as limited to only 2 languages.

Pattern: name the specific limitation, explain its impact, suggest how to address it.

### Conclusion framing
> Results of the experimentation conducted show that mBERT effectively captures semantic role information, with the middle layers performing the best...

Pattern: restate key finding with the specific evidence, don't overclaim.

---

## 13. BibTeX entries (paste into `custom.bib`)

All five entries below have been verified against the ACL Anthology (Apr 19, 2026).

```bibtex
@inproceedings{barbieri-etal-2022-xlm,
    title = "{XLM}-{T}: Multilingual Language Models in {T}witter for Sentiment Analysis and Beyond",
    author = "Barbieri, Francesco  and
      Espinosa Anke, Luis  and
      Camacho-Collados, Jose",
    booktitle = "Proceedings of the Thirteenth Language Resources and Evaluation Conference",
    month = jun,
    year = "2022",
    address = "Marseille, France",
    publisher = "European Language Resources Association",
    url = "https://aclanthology.org/2022.lrec-1.27/",
    pages = "258--266"
}

@inproceedings{faisal-anastasopoulos-2023-geographic,
    title = "Geographic and Geopolitical Biases of Language Models",
    author = "Faisal, Fahim  and
      Anastasopoulos, Antonios",
    editor = "Ataman, Duygu",
    booktitle = "Proceedings of the 3rd Workshop on Multi-lingual Representation Learning (MRL)",
    month = dec,
    year = "2023",
    address = "Singapore",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2023.mrl-1.12/",
    pages = "139--163",
    doi = "10.18653/v1/2023.mrl-1.12"
}

@inproceedings{li-etal-2024-land,
    title = "This Land is {Your, My} Land: Evaluating Geopolitical Bias in Language Models through Territorial Disputes",
    author = "Li, Bryan  and
      Haider, Samar  and
      Callison-Burch, Chris",
    editor = "Duh, Kevin  and
      Gomez, Helena  and
      Bethard, Steven",
    booktitle = "Proceedings of the 2024 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies (Volume 1: Long Papers)",
    month = jun,
    year = "2024",
    address = "Mexico City, Mexico",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2024.naacl-long.213/",
    pages = "3855--3871",
    doi = "10.18653/v1/2024.naacl-long.213"
}

@inproceedings{camara-etal-2022-mapping,
    title = "Mapping the Multilingual Margins: Intersectional Biases of Sentiment Analysis Systems in {E}nglish, {S}panish, and {A}rabic",
    author = "C{\^a}mara, Ant{\'o}nio  and
      Taneja, Nina  and
      Azad, Tamjeed  and
      Allaway, Emily  and
      Zemel, Richard",
    editor = "Chakravarthi, Bharathi Raja  and
      Bharathi, B  and
      McCrae, John P  and
      Zarrouk, Manel  and
      Bali, Kalika  and
      Buitelaar, Paul",
    booktitle = "Proceedings of the Second Workshop on Language Technology for Equality, Diversity and Inclusion",
    month = may,
    year = "2022",
    address = "Dublin, Ireland",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2022.ltedi-1.11/",
    pages = "90--106",
    doi = "10.18653/v1/2022.ltedi-1.11"
}

@inproceedings{goldfarb-tarrant-etal-2023-cross,
    title = "Cross-lingual Transfer Can Worsen Bias in Sentiment Analysis",
    author = "Goldfarb-Tarrant, Seraphina  and
      Ross, Bj{\"o}rn  and
      Lopez, Adam",
    editor = "Bouamor, Houda  and
      Pino, Juan  and
      Bali, Kalika",
    booktitle = "Proceedings of the 2023 Conference on Empirical Methods in Natural Language Processing",
    month = dec,
    year = "2023",
    address = "Singapore",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2023.emnlp-main.346/",
    pages = "5691--5704",
    doi = "10.18653/v1/2023.emnlp-main.346"
}
```

**Also cite (toolkits, not in the Related Work section but in References):**
- `transformers` library: Wolf et al. (2020), "Transformers: State-of-the-Art Natural Language Processing," EMNLP System Demonstrations.
- `statsmodels`: Seabold & Perktold (2010), "Statsmodels: Econometric and Statistical Modeling with Python," SciPy 2010.
- `matplotlib`: Hunter (2007), "Matplotlib: A 2D Graphics Environment," Computing in Science & Engineering.
- `seaborn`: Waskom (2021), "seaborn: statistical data visualization," Journal of Open Source Software.

---

## 14. Figure and table specifications

These are the team's committed choices so the Results section doesn't end up with inconsistent formats across figures made by different people. Adjustable if the team wants, but commit once.

### Table 1 — Category-level summary (required)

- **Structure**: 3 rows (Adversary, Ally, Neutral) × 5 columns (Category label, EN, ZH, RU, AR)
- **Cell format**: mean ± SD of the bipolar valence score S = P(pos) − P(neg) across countries in that cell, reported to 3 decimal places (e.g., `-0.102 ± 0.087`)
- **Optional 6th column**: Gap — mean Gap value per category, 3 decimal places
- **Caption**: "Mean (±SD) bipolar sentiment score S = P(pos) − P(neg) by geopolitical category and language, and Cross-Lingual Sentiment Gap per category."
- **LaTeX**: plain `tabular`, use `\toprule \midrule \bottomrule` from `booktabs` for ACL style

### Figure 1 — Heatmap (required)

- **Structure**: rows = 30 countries, columns = 4 languages (EN, ZH, RU, AR)
- **Row order**: grouped by category (Adversary block, then Ally block, then Neutral block), alphabetical within each category. A thin horizontal line separates the three blocks.
- **Cell values**: mean bipolar valence score S = P(pos) − P(neg) across the 30 templates, 2 decimal places if annotated
- **Library**: `seaborn.heatmap`. The colormap is diverging (centered at 0) because S ∈ [−1, 1]. A short reference band showing the nonsense-country Gap range is appended below the 30-country block.
- **Caption**: "Mean bipolar sentiment score S = P(pos) − P(neg) assigned by XLM-RoBERTa-Twitter to each country across four languages. Countries are grouped by geopolitical category; the bottom band shows the nonsense-country reference."

### Figure 2 — ANOVA interaction plot (include if space allows)

- **Structure**: x-axis = language (EN, ZH, RU, AR); y-axis = mean negative score; one line per category (Adversary, Ally, Neutral)
- **Error bars**: ±1 standard error across the 10 countries in each cell
- **Library**: `matplotlib` or `seaborn.pointplot`. Dimensions are Sebastien's call.
- **Caption**: "Interaction plot of mean bipolar sentiment score by language and geopolitical category. Non-parallel lines indicate an interaction effect. Error bars show ±1 SE across countries."

### Consistency rules across all figures

- Language ordering: always EN, ZH, RU, AR (left-to-right or top-to-bottom)
- Category ordering: always Adversary, Ally, Neutral
- Numeric precision in captions: 3 decimals for means, 3 decimals for p-values (or `p < .001`), 2 decimals for F-statistics and Cohen's d
- Font size in figures: readable when the PDF is at 100% zoom; no default matplotlib 10-point labels on a figure shrunk to a 3-inch column

---

## 15. Page-budget compression plan

The Method as currently planned contains: model justification, 30-template listing, 30-country listing with operationalization, two-track translation protocol, Gap formula and rationale, ANOVA design and type justification, hypothesis operationalization, baseline probe, design process note, template-fixation statement, attribution references. This is realistically 2+ pages of content, not the 1.5 budgeted. Compression levers are committed in advance rather than applied ad hoc on April 22.

**Compression levers in priority order (cheapest loss of information first):**

1. **List templates by family header + 1 example each**, with a pointer: *"Full template set in `main.ipynb` and supplementary materials."* Saves ~0.4 page.
2. **List countries inline as running prose** rather than indented bulleted blocks: *"Adversaries: China, Russia, Iran, North Korea, Cuba, Syria, Venezuela, Belarus, Nicaragua, Myanmar. Allies (NATO members): ..."* Saves ~0.2 page.
3. **Move toolkit citations to the References section** (where they don't count against page budget) rather than an inline Method checklist. Saves ~0.1 page.
4. **Drop the template-fixation sentence** from the Method. The notebook itself is the artefact. Saves ~0.05 page.
5. **Compress the statistical code description** to a one-line prose sentence: *"All ANOVA and contrast computations use `statsmodels` (OLS + `anova_lm(typ=2)`) with full code in `main.ipynb`."* Saves ~0.3 page.
6. **Merge the design-process note into a single sentence** instead of a paragraph. Saves ~0.1 page.

If all six levers are applied, Method compresses from ~2+ pages to ~1.2 pages. Only apply what's needed — don't over-compress.

**Do not compress:**
- Gap formal definition (required by the assignment for non-standard measures)
- Hypothesis operationalization (required for rigor)
- Baseline probe description (addresses a specific TA concern)
- Model justification (required for methodological clarity)

---

## 16. Notebook scaffold (`main.ipynb`)

The notebook is the executed companion to the Method section. It must run top-to-bottom from a clean kernel and produce every number, table, and figure referenced in the paper. Structure below is prescriptive so that it matches the committed Method prose exactly.

**Do not use `transformers.pipeline("text-classification", ...)`.** The pipeline API returns only the top-1 label and score and hides the full softmax distribution. The study needs all three class probabilities per cell.

### Cell-by-cell structure (17 cells)

1. **Header markdown** — title, run instructions, dependency list, how to reproduce. List exact `transformers`, `torch`, `statsmodels`, `scipy`, `numpy`, `pandas`, `seaborn`, `matplotlib` versions.
2. **Imports and seeds** — `import torch, numpy as np, pandas as pd`, set `torch.manual_seed(42)`, `np.random.seed(42)`, pick device (`cuda` > `mps` > `cpu`).
3. **Model and tokenizer load** —
   ```
   MODEL_ID = "cardiffnlp/twitter-xlm-roberta-base-sentiment"
   tokenizer = AutoTokenizer.from_pretrained(MODEL_ID, use_fast=True)
   model = AutoModelForSequenceClassification.from_pretrained(MODEL_ID).to(device).eval()
   id2label = model.config.id2label            # no hardcoded indices
   label2id = {v: k for k, v in id2label.items()}
   ```
   Log the model revision SHA returned by `from_pretrained`.
4. **Template dictionaries** — four dicts keyed by language code, each mapping template ID (e.g. `"A1"`..`"E6"`) to the template string with a `{country}` placeholder. Loaded from `templates/` or inlined.
5. **Country lists** — three Python lists (adversaries, allies, neutrals) of 10 names each. A `category_of` dict maps each country to its category. Also define `nonsense = ["Zorbia", "Kelandor", "Traviska", "Nuberia", "Sambalos"]`.
6. **Per-language country-name lookup** — a dict mapping (country_en, language) → rendered name (e.g., `("China", "zh") → "中国"`). For the four languages, render each of the 30 + 5 names in the target language so the substituted template reads naturally. English uses itself.
7. **Inference helper** —
   ```
   def score_batch(texts, batch_size=32):
       probs = []
       for i in range(0, len(texts), batch_size):
           batch = texts[i:i+batch_size]
           enc = tokenizer(batch, padding=True, truncation=True, return_tensors="pt").to(device)
           with torch.no_grad():
               logits = model(**enc).logits
           probs.append(torch.softmax(logits, dim=-1).cpu().numpy())
       return np.concatenate(probs)  # shape (N, 3)
   ```
8. **Generate the full cell list** — loop over (country, language, template_id) for all 30 countries × 4 languages × 30 templates = 3,600 rows, plus 5 nonsense × 4 × 30 = 600 baseline rows. Store (country, category, language, template_id, rendered_text) rows in a pandas DataFrame.
9. **Run inference** — call `score_batch` on the `rendered_text` column, attach three columns `p_neg`, `p_neu`, `p_pos` using `id2label` for indexing. Save the raw DataFrame to `data/raw_scores.csv`.
10. **Derive bipolar valence** — `df["S"] = df["p_pos"] - df["p_neg"]`. Save.
11. **Aggregate to country × language means** — group by (country, language), mean over templates, producing a (30 real + 5 nonsense) × 4 table. Save to `data/country_language_means.csv`.
12. **Compute Gap per country** — `Gap(c) = S_EN(c) − mean(S_ZH(c), S_RU(c), S_AR(c))`. Also compute per-language Gap_L(c) for the supplementary figure. Save `data/gap_per_country.csv`.
13. **One-way ANOVA on Gap (primary)** — `statsmodels.formula.api.ols("gap ~ C(category)", data=real_only).fit()` and `anova_lm(typ=2)`. Report F, df, p, η². Run Levene and Shapiro on residuals.
14. **Planned contrast** — weights = [+2, −1, −1] for (Adversary, Ally, Neutral) on category Gap means. Compute t, df=27, one-tailed p, Cohen's d using pooled SD from MSE. Include Welch variant if Levene rejects.
15. **Supplementary two-way ANOVA** — `ols("S ~ C(language) * C(category)", data=real_country_lang_means).fit()` with `anova_lm(typ=2)`. Report as confirmation, not as primary.
16. **P(neg)-only sensitivity + Kruskal–Wallis** — rerun the one-way ANOVA and contrast using `-p_neg` in place of `S`, and run `scipy.stats.kruskal` across the three category Gap distributions. Reported in a short sensitivity paragraph in Results.
17. **Figures** — Figure 1 heatmap (30 countries × 4 languages, value = S) with adversary/ally/neutral block separation and a nonsense-country reference band; Figure 2 interaction plot (language × category with ±1 SE bars). Save to `figures/`.

### Execution notes
- Estimated runtime: 2–5 minutes batched on CPU, under a minute on GPU. Runtime is not a blocker.
- All CSVs are submission artifacts so the grader can inspect numbers without re-running inference.
- Random seed affects nothing deterministic for this classification model but is set as a provenance signal.

---

## 17. Template neutrality sanity check

Removed (2026-04-22). The nonsense-country baseline is the primary defence against template-driven artifacts and is sufficient on its own. Team self-rating was dropped because it is not independent of the study and added no information the baseline does not already provide.
