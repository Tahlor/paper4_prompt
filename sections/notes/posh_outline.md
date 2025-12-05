# Paper Outline: Confidence Estimation for Document Transcription MLLMs

## Working Title
**"Beyond Accuracy: A Systematic Comparison of Confidence Proxies for Human-in-the-Loop Document Transcription"**

---

## Core Contributions (Ranked)

1. **First systematic comparison of confidence proxy families** (verbalized, justification-based, ensemble) for document transcription under a unified review-budget framework

2. **First systematic exploration of error modalities**: Characterizing *what kinds* of errors models make and how these depend on prompt design and visual test-time augmentation

3. **Practical improvement over logits**: Demonstrating that a simple GBT trained on features from 2 cheaper model runs can outperform raw logits for selective risk/coverage

4. **Prompt diversity contributes to ensemble effectiveness** (soft orthogonality claim): Different prompt variants contribute meaningful variation to ensemble-based confidence, beyond just visual degradation

---

## Story Arc

### The Problem
Practitioners deploying MLLMs for document transcription face a key operational question: *which predictions should I trust and which should I send for human review?* Traditional metrics (CER, WER) don't answer this. We need confidence signals that enable efficient human-in-the-loop workflows.

### The Gap
- Logprobs are the gold standard but often unavailable (API constraints)
- Self-reported confidence is convenient but poorly understood for transcription
- Ensemble methods are expensive but potentially powerful
- **Nobody has systematically compared these for document transcription**

### Our Approach
We evaluate confidence estimation through a *review-budget lens*: given a fixed budget for human review, what is the achievable final accuracy? We systematically compare:
- Logprob-derived confidence
- Self-reported verbalized confidence
- Justification-based signals
- Ensemble agreement across prompts/models

### Key Findings
1. Logprobs are robust but not always available or optimal
2. Self-reported confidence depends heavily on prompting (scale, framing)
3. Ensemble variance from *diverse prompts* provides useful signal
4. A simple GBT combining cheap signals can beat raw logits

### The Takeaway
Confidence estimation isn't one-size-fits-all. We provide a practical guide: use logprobs when available, but prompt diversity and learned combination can close the gap or exceed it.

---

## Section-by-Section Outline

### 1. Introduction (~1 page)

**Opening Hook**: The gap between benchmark accuracy and operational deployment. Models achieve impressive CER, but practitioners still need to know *which* predictions to trust.

**Problem Statement**: 
- Scalar error rates obscure practical tradeoffs
- Review-budget framework: accuracy as a function of human review proportion
- This directly models the operational constraint

**Why Now**: 
- Test-time compute is variable (reasoning models, ensembles)
- Training contamination makes static benchmarks unreliable
- We need evaluation that reflects real deployment

**Contributions** (bulleted):
1. First systematic comparison of confidence proxy families for document transcription
2. First systematic characterization of error modalities under different prompting strategies
3. Demonstration that simple learned models on cheap signals outperform raw logits
4. Evidence that prompt diversity provides complementary signal for ensemble confidence

**Figure 1** (Hero Figure): 
- **Concept**: Risk-coverage curves showing the landscape of methods
- **Content**: 4-panel or single panel showing representative risk-coverage curves for:
  - Raw logprobs (baseline)
  - Self-reported confidence (multiple scales)
  - Ensemble (prompt diversity)
  - GBT combination
- **File**: `[NEEDS CREATION: figures/hero_risk_coverage.png]`
- **Message**: Visual proof that methods differ substantially, and combination wins

---

### 2. Background & Related Work (~1 page)

*Note: Existing `2_background.tex` is well-structured. Keep the three-family structure.*

**2.1 Uncertainty Quantification in LLMs**
- Logprob-based methods (gold standard, calibration challenges)
- Brief: MLLMs inherit these issues

**2.2 Black-Box Confidence Proxies**
- Verbalized confidence (prompting for self-assessment)
- Justification-based (reasoning traces, length as signal)
- Ensemble-based (self-consistency, agreement)
- **Key gap**: These are studied for text QA, not document transcription

**2.3 Evaluation Beyond Scalar Metrics**
- Budget-aware evaluation
- Inference scaling laws
- Selective classification / abstention
- **Our framing**: Review-budget = human effort, not compute

**2.4 Document AI Context**
- MLLM for transcription/HTR
- Ensemble/voting history in OCR
- Calibration is known to be poor in this domain

---

### 3. Methodology (~1.5 pages)

#### 3.1 Review-Budget Framework

**Definition**: Formalize the review-budget evaluation

$$\text{ACC}(r) = \frac{\text{correct predictions in } (1-r)}{ (1-r) \cdot N} + r$$

Where $r$ is the review rate (proportion sent to human review, assumed corrected).

**Risk-Coverage Curve**: Plot accuracy vs. review rate. Area under this curve = efficiency of confidence signal.

**Key Metrics**:
- **AUROC**: Can the signal rank correct above incorrect?
- **AUC of Risk-Coverage Curve**: Overall efficiency
- **ECE**: Calibration quality
- **Brier Score**: Probability estimation quality

> **Table 1**: Summary of confidence metrics used
> | Metric | Measures | Higher/Lower Better |
> |--------|----------|---------------------|
> | AUROC | Ranking quality | Higher |
> | AUC-RC | Coverage efficiency | Higher |
> | ECE | Calibration | Lower |
> | Brier | Probability accuracy | Lower |

#### 3.2 Confidence Signal Families

**3.2.1 Logprob-Based Confidence**
- Token-level mean logprob
- Minimum token logprob
- Entropy-based measures

**3.2.2 Verbalized (Self-Reported) Confidence**
- Direct prompting: "Rate your confidence 1-10"
- Alternative: "Rate 0.0-1.0"
- Justification prompting: "Explain your uncertainty"

**3.2.3 Ensemble-Based Confidence**
- Agreement across multiple prompt variants
- Agreement across visual augmentations
- Cross-model agreement

**3.2.4 Learned Combination (GBT)**
- Features: confidence scores from multiple cheap runs
- Target: whether prediction matches ground truth
- Selective prediction: route to higher-confidence model

#### 3.3 Prompt Variants Tested

> **Table 2**: Prompt Variant Taxonomy
> | Category | Description | Example |
> |----------|-------------|---------|
> | BASELINE | Neutral instruction | "Transcribe this document" |
> | GRANULARITY_CHAR | Character-level output | "Output like: M,a,r,y" |
> | GRANULARITY_WORD | Word-level output | "Output word by word" |
> | SEMANTIC_LITERAL | Exact reproduction | "Transcribe exactly as written" |
> | SEMANTIC_CONTEXT | Allow modernization | "Transcribe, modernizing spelling" |
> | CONFIDENCE_VERBAL | Request self-assessment | "Rate confidence 1-10" |
> | REASONING | Chain-of-thought | "Explain your reasoning" |

#### 3.4 Visual Test-Time Augmentation

> **Table 3**: Visual Degradation Taxonomy
> | Type | Description | Parameters |
> |------|-------------|------------|
> | Gridwarp | Geometric distortion | σ ∈ {5, 10, 15, 20} |
> | Noise | Additive Gaussian | σ ∈ {10, 20, 30, 40} |
> | Blur | Gaussian blur | kernel ∈ {3, 5, 7, 9} |

#### 3.5 Experimental Setup

**Dataset**: [Specify your dataset - historical documents?]

**Models Evaluated**:
- Gemini 2.0 Flash
- Gemini 2.5 Flash  
- GPT-4o Mini (Low reasoning)
- [Others?]

**Experiment Matrix**: 
- ~X prompts × Y degradations × Z models = N experimental conditions
- Each produces predictions + confidence signals

---

### 4. Results (~2.5 pages)

#### 4.1 RQ1: How do confidence proxy families compare?

**Finding 1.1**: Logprobs provide robust ranking but are not always best
- AUROC is consistently good
- But calibration (ECE) can be poor, especially for reasoning models

**Finding 1.2**: Self-reported confidence depends critically on scale
- [0-1] float scale → overconfident distributions
- [1-10] integer scale → more conservative, better separation

> **Figure 2**: Confidence Distribution Histograms by Scale
> - **Existing**: `figures/self_confidence_score_scale/o1_histogram_uncalibrated_*.png`
> - 3-panel: Gemini 2.5, Gemini 2.0, GPT-4o Mini
> - Shows [0-1] vs [1-10] distributions

**Finding 1.3**: Logprobs outperform self-reported for correctness prediction
- Consistently higher AUROC across models
- But gap narrows with proper scale choice

> **Table 4**: Comparison of Confidence Families (Main Results)
> - **File**: `[NEEDS CREATION: tables/main_confidence_comparison.tex]`
> - **Columns**: Model | Prompt Category | CER | Logprob AUROC | Self-Conf AUROC | Ensemble AUROC | GBT AUROC
> - **Rows**: Grouped by model, then by prompt category
> - Highlight: Best AUROC per row in bold

#### 4.2 RQ2: How robust are confidence signals under degradation?

**Finding 2.1**: AUROC remains stable even as CER increases
- Monotonic degradation: CER rises, but both logprob and self-conf AUROC hold steady
- Models get worse but still know what they don't know

**Finding 2.2**: Logprobs degrade more gracefully than self-reported
- Under severe degradation, self-reported becomes less reliable

> **Figure 3**: Robustness Under Monotonic Degradation
> - **Existing**: `figures/monotonic_degradation/combined_master_sample_database_calibrated_panel_cer_auroc.png`
> - 3-panel: Gridwarp, Noise, Blur
> - Shows CER ↑ while AUROC stable

#### 4.3 RQ3: What error modalities do models exhibit?

**Finding 3.1**: Prompt design influences error types
- Literal prompts → more verbatim errors (frequency bias)
- Contextual prompts → more interpretation errors but lower overall CER
- Character-level prompts → different error distribution

**Finding 3.2**: Error type correlates with confidence signal quality
- [Specify which errors are easier/harder to detect with which signals]

> **Table 5**: Error Modality Breakdown by Prompt Category  
> - **File**: `[NEEDS CREATION: tables/error_modalities.tex]`
> - **Columns**: Prompt Category | Substitution % | Insertion % | Deletion % | Fabrication % | Mean CER
> - **Rows**: Each prompt category
> - Show how error profile changes with prompting

> **Figure 4**: Error Type × Confidence Calibration
> - **File**: `[NEEDS CREATION: figures/error_type_calibration.png]`
> - **Concept**: Heatmap or grouped bar showing which error types are well-detected by which signals

#### 4.4 RQ4: Can we do better than raw logits?

**Finding 4.1**: GBT on cheap signals outperforms raw logits
- Train GBT to predict correctness from multiple cheap model runs
- Selective routing: use prediction to choose which model to trust
- Better risk-coverage than any single signal

**Finding 4.2**: Two cheap runs can beat one expensive run
- Demonstrates practical value for cost-constrained deployment

> **Figure 5**: Risk-Coverage Curves - GBT vs Baselines
> - **File**: `[NEEDS CREATION: figures/gbt_risk_coverage.png]`
> - **Content**: 
>   - Multiple curves: Logprob, Self-Conf [1-10], Ensemble, GBT
>   - GBT achieves best AUC
> - This is a key result figure

> **Table 6**: GBT Feature Importance
> - **File**: `[NEEDS CREATION: tables/gbt_feature_importance.tex]`
> - **Columns**: Feature | Importance Score | Type (logprob/self-conf/ensemble)
> - Shows which signals contribute most to learned combination

#### 4.5 RQ5: Does prompt diversity contribute beyond visual augmentation?

**Finding 5.1**: Prompt-based variation provides complementary signal
- Best pairs are not always neutral + degradation
- Some prompt variants are occasionally best in selective risk curve
- Prompt diversity improves ensemble effectiveness

**Finding 5.2**: The effect is model-dependent (soft claim)
- Cannot say "use prompt X in situation Y"
- But can say: adding prompt diversity reliably improves worst-case coverage

> **Figure 6**: Ensemble Composition Analysis
> - **File**: `[NEEDS CREATION: figures/ensemble_composition.png]`
> - **Concept**: For each coverage level, show which prompt/augmentation contributed to selected prediction
> - Bar chart or stacked area showing prompt diversity is non-trivial contributor

> **Table 7**: Best Pair Analysis
> - **File**: `[NEEDS CREATION: tables/best_pairs.tex]`
> - **Columns**: Model | Coverage Level | Best Pair Type | % of Cases
> - **Rows**: Show that "prompt + prompt" or "prompt + aug" pairs frequently appear as best
> - Supports: prompt diversity matters, not just visual augmentation

---

### 5. Discussion (~0.75 page)

**Practical Guidelines**:
- If logprobs available: use them, but consider learned combination
- If API-only: use [1-10] scale, not [0-1]
- For high-stakes: ensemble with prompt diversity worth the cost
- Consider GBT routing if operating at scale

**Limitations**:
- Single dataset/domain (historical documents)
- Prompt effects are model-specific
- Didn't explore fine-tuning for calibration

**Future Work**:
- Performance-per-FLOP evaluation
- Cross-domain evaluation
- Active learning with confidence signals

---

### 6. Conclusion (~0.25 page)

We provide the first systematic comparison of confidence proxies for document transcription. Key findings:
1. Logprobs are robust but self-reported confidence with proper scaling can approach them
2. Prompt diversity provides meaningful signal for ensemble methods
3. Simple learned combinations can exceed single-signal baselines
4. The review-budget framework offers a practical lens for evaluation

---

## Figures & Tables Summary

### Existing (ready to use)
| ID | Description | Path |
|----|-------------|------|
| Fig 2 | Confidence histograms by scale | `figures/self_confidence_score_scale/*.png` |
| Fig 3 | Monotonic degradation panel | `figures/monotonic_degradation/combined_*.png` |

### Needs Creation
| ID | Description | Suggested Path | Priority |
|----|-------------|----------------|----------|
| Fig 1 | Hero risk-coverage curves | `figures/hero_risk_coverage.png` | HIGH |
| Fig 4 | Error type × calibration | `figures/error_type_calibration.png` | MED |
| Fig 5 | GBT risk-coverage comparison | `figures/gbt_risk_coverage.png` | HIGH |
| Fig 6 | Ensemble composition analysis | `figures/ensemble_composition.png` | MED |
| Tab 1 | Metrics summary | inline in text | LOW |
| Tab 2 | Prompt variant taxonomy | `tables/prompt_taxonomy.tex` | MED |
| Tab 3 | Visual degradation taxonomy | `tables/degradation_taxonomy.tex` | MED |
| Tab 4 | Main confidence comparison | `tables/main_confidence_comparison.tex` | HIGH |
| Tab 5 | Error modality breakdown | `tables/error_modalities.tex` | MED |
| Tab 6 | GBT feature importance | `tables/gbt_feature_importance.tex` | HIGH |
| Tab 7 | Best pair analysis | `tables/best_pairs.tex` | MED |

---

## Open Questions / Decisions Needed

1. **Dataset description**: What exactly is the transcription dataset? (Historical documents, size, characteristics)

2. **Model selection**: Final list of models to include? (Currently: Gemini 2.0/2.5, GPT-4o Mini)

3. **GBT details**: What features exactly go into the GBT? Need to specify for reproducibility.

4. **Soft orthogonality claim**: The current framing is "prompt diversity contributes to ensemble effectiveness" without claiming *which* prompts work *when*. Is this sufficiently strong?

5. **Page budget**: This outline is ~7-8 pages of content. CVPR is 8 pages. May need to compress Background or move some results to Supplemental.

---

## Supplemental Material (Already Drafted)

- Confidence scale analysis (detailed histograms)
- Monotonic degradation detailed analysis
- [Add: Full experiment matrix results]
- [Add: Per-prompt breakdown tables]
- [Add: Risk-coverage curves for all conditions]