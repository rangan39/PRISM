# PRISM — Results Analysis (AI Safety Framework Coding Study)

**Generated:** 2026-09-03  
**Source corpus:** `PRISM/run_config.json:2` — 19 documents, 4 labs (OpenAI, Google DeepMind, Anthropic, xAI), 2023-2026  
**Dataset:** `PRISM/results/coded_wide.csv:5649` rows, `PRISM/results/coded_long.csv:79087` rows, `PRISM/results/units.csv:4775` units  
**Model:** `claude-sonnet-5` (single model, single repeat, `run_config.json:17`), temp not set (model default)  
**Note:** No human adjudication. Reliability not computable by design (`PRISM/results/README.md:34`). Treat as triage, not findings.

---

## 1. Dataset Grain

*   **Units:** 4,775 verbatim excerpts (≤75 words, 1 exception `GDM-FSF-v3-0-0069-a` 85w). Split by lab: Anthropic 2,902 (60.8%), Google DeepMind 729, OpenAI 622, xAI 522 (`PRISM/results/units.csv`).
*   **Rows in analysis file:** 5,649 = 4,898 transition-coded rows + 751 `transition_id=NONE` (earliest version in chain, no prior to compare, so 6 change codes = `NA`) + removals. Endpoint pair `GDM-FSF-v3-1` (281 units) coded twice, so `transition_id` is part of key (`PRISM/results/README.md:66-77`).
*   **Transitions:** 18 (15 adjacent + 3 endpoint). Coverage: `ANT-RSP-v1-0_v2-0` 344 rows → `XAI-RMF-2025-08_FAIF-2025` 160 rows. Largest change rate 27.3% (`XAI-RMF-2025-02_FAIF-2026` 41/150), smallest 2.5% (`XAI-RMF-2025-08_FAIF-2025` 4/160).
*   **Included files:** `PRISM/corpus/` 20 PDFs, `PRISM/docs/codebook_v8.txt` (14 live codes, C02/C10 retired), `PRISM/study/prompts/` 3 prompts. Reproducible via `study/scripts/build_tables.py --labs OpenAI "Google DeepMind" Anthropic xAI` (`PRISM/results/README.md:8`).

---

## 2. Change vs. Content Prevalence

### Change codes (denominator = 4,898 transition rows)

| Code | Name | n | % | Direction split |
|------|------|---|---|-----------------|
| **C04** | Obligation & Threshold Strength Shift | 310 | **5.5%** | `tightened 156 / loosened 154` (50/50), facet `modality 184, bar 23, both 8, NA 95 (31%)` |
| **C03** | Threshold Existence Change | 144 | 2.5% | `architecture_replaced 61, removed 52, introduced 31` |
| **C06** | Definitional Drift | 112 | 2.0% | `broadened 75 / narrowed 37` (2:1) |
| **C05** | Adding/Dropping Risk Theme | 71 | 1.3% | `split 25, dropped 22, added 16, merged 8` |
| **C07** | Governance Tightening/Loosening | 33 | 0.6% | `tightened 26 / loosened 7` (79% tighten) |
| **C01** | Competitor/Conditionality Clause | 5 | 0.1% | `expanded 3 / introduced 2` |

*   **Any change per transition:** 2.6% (`ANT-RSP-v3-1_v3-2`) to 22.6% (`GDM-FSF-v3-0_v3-1`) and 27.3% endpoint (`XAI-RMF-2025-02_FAIF-2026`). Median ~14%.
*   **Removal rows:** 214 flagged `removal_candidate=true`. 167/214 (78%) carry **no change code** (`C01, C03-C07` all 0) - aligner flagged as removed but change coder saw no removal-type code. Inspect before counting.

### Content codes (denominator = 5,435 content-evaluated rows, i.e., `C08 != NA`; 214 removals excluded)

| Code | Theme | n (overall 45%) | Lab skew |
|------|-------|-----------------|----------|
| **C16** | Evaluation & Risk Management Practices | **2,540 (46.7%)** | GDM 59%, xAI 60%, OpenAI 48%, Anthropic 40% |
| **C14** | Governance, Transparency & Disclosure | 1,091 (20.1%) | Anthropic 27.6% vs GDM 6.4% |
| **C11** | Autonomy/Loss of Control | 476 (8.8%) | GDM 11.5%, OpenAI 11.2% |
| **C12** | Cyber | 419 (7.7%) | xAI 13.2%, GDM 10.6% |
| **C13** | CBRN | 302 (5.6%) | xAI 15.9% (3x Anthropic 3.3%) |
| **C09** | Risk Severity Levels & Thresholds | 422 (7.8%) | OpenAI 15.6% (2x others) |
| **C08** | Frontier Risk Definition & Framing | 213 (3.9%) | Even |
| **C15** | Persuasion/Influence | 110 (2.0%) | Anthropic 0.8% (near-absent post-v2) |

**Interpretation for engineer:** Frameworks are ~2/3 *how we test/govern* (`C16 + C14 = 66.8%`) and ~1/3 *what risks*. Risk framing itself is rare.

---

## 3. Per-Lab Trajectories

### OpenAI: Single disruptive redesign (379 rows, 1 transition)
*   **OAI-PF-2023_v2:** 73/379 with any change (**19.3%**). Highest `C03` rate **8.7% (33)** of any transition. Breakdown: `architecture_replaced 24` = post-mitigation Scorecard (`medium or below to deploy`) → per-category `High/Critical` thresholds. `C05 dropped` = `Persuasion` category removed (5 units 0079-0089) + `C15` drops from 28 to near-zero. Also `C01 introduced` (1 unit 0196: competitor without comparable safeguards clause), `C07 tightened` (SAG + Board oversight layer).
*   Content: `C09` 15.6% (most tier tables), `C16` 48%.

### Google DeepMind: Incremental broadening + definitional drift (937 rows, 5 transitions including endpoint)
*   **Per-transition any-change:** `v1→v2 18.1%, v2→v3-0 20.0%, v3-0→v3-1 22.6%` - steadily increasing churn.
*   **C06 is signature:** 62 total (6.6% vs 0.9% Anthropic), accelerating `v1→v2 5 broad/1 narrow → v2→v3-0 13/4 → v3-0→v3-1 22/0` (pure broadening in latest point-release). Example: CBRN uplift definition narrowing/broadening tests. `C05 split` dominant (single `Model Autonomy` → `AI Self-improvement / Long-range Autonomy / Autonomous Replication`).
*   **C04:** 60 cases (6.4%), facet `modality` 31 vs `bar` 4. Governance `C07` rare (5).
*   Content: Balanced, `C16` 59% highest.

### Anthropic: High-volume point releases, many micro-tweaks (2,968 rows, 9 transitions)
*   **Scale:** 2,902 units = 61% of corpus, 9 RSP versions (`v1-0` 355 → `v3-4` 334 units each, ~300 avg). Churn declines over time: `v1→v2 14.8% → v3-1→v3-2 2.6% (lowest corpus) → v3-3→v3-4 5.4%`.
*   **C04 dominates:** 170 cases (5.7%), `modality 101` (verb softening: `will pause → will evaluate appropriate mitigations`). `C03` 63 (2.1%) with `architecture_replaced 24, removed 21` - notably `Cyber Operations` drop in `v2-2→v3-0`.
*   **C01 competitor conditionality:** Only lab with `expanded` beyond introduction - `v1→v2` units 0249-0250 ("if another actor passes threshold we may lower Required Safeguards") and `v2-2→v3-0` 0224 ("meet or exceed competitor posture"). 3 total.
*   **C05 dropped:** 25 but small n; includes `Misuse` and `Autonomy and replication` framing buckets in `v1→v2`.
*   **Endpoint stability:** `v1-0→v3-4` any-change 18.1% (63/349) vs `v1→v2` 14.8% - suggests many mid-chain tweaks net to similar magnitude as single jump.
*   Content: Governance-heavy `C14 27.6%` (vs ~7% others) - RSP is governance doc. `C15` 0.8% (persuasion omitted).

### xAI: Two distinct instruments, punctuated tuning (614 rows, 4 transitions)
*   **RMF chain:** `2025-02→2025-08` 28/160 **17.5% any-change, 16.2% C04 (26)** - largest `C04` concentration corpus-wide. Pure modality tweaks. Then `2025-08→FAIF-2025` 4/160 **2.5%** - near-identical (copy-paste).
*   **FAIF chain:** `FAIF-2025→2026` 26/144 18.1% with `C03 removed 6` + `C06 7` (mostly `CBRN` bar shifts).
*   **Endpoint `RMF-2025-02→FAIF-2026`:** 41/150 **27.3% highest corpus**, `C04 24 (16%), C03 8 (5.3%), C06 7` - instrument switch inflates change rate.
*   Content: `C16` 60.5%, `C13` 15.9% (highest CBRN focus), `C12` 13.2% (cyber).

---

## 4. Cross-Lab Synthesis: What This Means for the AI Industry (Engineer Lens)

**a) Safety gating is converging on per-category thresholds, not aggregate scores.**
`C03 architecture_replaced = 61` (42% of C03) is the single largest direction. OpenAI is the cleanest example, but GDM and Anthropic footnotes show same migration. If you build eval infra, target modular gates (`CBRN-3`, `Cyber Ops`, `Autonomy L1`) rather than a single deploy block.

**b) Thresholds are calibrated bidirectionally.**
`C04` 50/50 tighten/loosen contradicts a "race to the bottom" narrative at code level. The facet is 89% `modality` (wording firmness) vs 11% `bar` (numeric value) and 31% `NA` (incomplete). Engineering takeaway: version-control both the *number* (e.g., `>2x → >5x`) and the *verb* (`will → will aim to`). Most drift is linguistic hedging, detectable only via C04 facet.

**c) Definitions are widening.**
`C06 broadened 75 vs narrowed 37`. GDM's latest `v3-0→v3-1` 22-0 broadening suggests scope creep (e.g., "all software projects" → "hardened real-world critical systems" - actually narrowed there, but overall trend is broaden). For eval suites, this means your threat model will expand without a version bump announcement - monitor definitional footnotes.

**d) Governance is layering, not thinning.**
`C07 tightened 26 vs loosened 7`. Additions: SAG approval, Board oversight, reporting lines. Anthropic drives 22/33 cases. Expect more sign-off gates in CI/CD for frontier releases.

**e) Competitor-conditional safety is codified but not yet norm.**
`5 cases total`. Industry rhetoric about "if rivals ship unsafely we adjust" is documented in only two labs' latest versions. For industry coordination, hedge is present at the margin, not systemic - but worth tracking as leading indicator of conditional commitment.

**f) Frameworks are test-harness docs.**
`C16 + C14 >> risk themes`. The corpus is 2x more about *how to evaluate/monitor/govern* than *what the risk is*. Aligns with industry focus on evals over taxonomy.

---

## 5. Known Limitations & Caveats (from `PRISM/results/README.md:212-276` and `PRISM/run_config.json:499-531`)

1.  **Single model, no agreement.** One `claude-sonnet-5` pass, no within/between-model agreement (`run_config.json:27`). 29/75 C04 facets missing on first half, 95/310 (31%) corpus-wide - model systematically omits required field. Second half warnings retained to keep halves comparable.
2.  **Non-deterministic sampling.** `temperature` rejected by API (`run_config.json:22`), sampling at model default - reruns will be similar, not identical. Budget $54.8 (intro) / $82.2 std.
3.  **Content exclusions (2):** `OAI-PF-2023` p6 scorecard caption (aggregate scoring rule) - makes `C03 architecture_replaced` absence on that transition artefactual; `ANT-RSP-v1-0` 394-word Task-2 LM worm passage (model refusal, 3 attempts, 0 tokens) - 15 units short, not framework property (`run_config.json:539-551`). Cross-lab coverage asymmetry.
4.  **Table transcription gaps:** Only `OAI-PF-2023 p15` and `XAI-RMF-2025-02 pp3-5` hand-transcribed; all other 23 Anthropic tables via `pdftotext` column layout - dropped cells not detectable (`README.md:224`).
5.  **Cosmetic splits:** 66 many-to-one alignments where target excerpts concatenate to exactly prior excerpt (text-identical, different cut points). Flagged `cosmetic_split=true` on 132 rows (`run_config.json:264-271`). Change rate on flagged rows 7.6% vs 8.8% baseline - no inflation, but n=10 firings small. Distribution uneven (4% many-to-one first half vs 29% second half, Anthropic point releases).
6.  **Removal signal diluted:** 78% of `removal_candidate` rows have no change code - presence of `removal_candidate` ≠ coded removal.
7.  **Unaligned rate varies:** 21% (`GDM v3-0→v3-1`) to 62% orphaned on `OAI 2023→v2` - "no counterpart" not evenly informative.

---

## 6. How to Use as Analyst / Engineer

*   **Start with `coded_wide.csv`** - filter `transition_id` you care about + `C0X==1` + read `C0X_direction`/`C04_facet`. Join to `units.csv` for `excerpt`/`section_heading`/`prior_counterpart_excerpt`.
*   **Verify with `coded_long.csv:evidence`** - every `1` has quoted span; empty or generic evidence = suspect. Check `flag==ambiguous` + `ambiguity_reason`.
*   **Respect NA vs 0:** `NA` = never evaluated (353 transition-less units + 50 removals for content); `0` = evaluated and did not fire. Conflating underestimates prevalence (`README.md:172-191`).
*   **For trend work:** Use `cosmetic_split` flag if you exclude many-to-one text-identical rows; report `C04` facet `NA` rate separately (39% first half, 28% second half).
*   **To update study:** Re-verify `checkpoint_c.frozen_files` sha256s in `run_config.json:372-403` and `checkpoint_c_anthropic_xai:592-657` before appending coders; rebuild tables via `build_tables.py`.

---

## 7. Open Questions / Next Analysis

*   Manual adjudication of 5 `C01` + 26 `C07 tightened` (high-impact, low-n) to confirm conditionality/governance claims.
*   Hand-review 95 `C04` NA facets - are they truly ambiguous or coder omission? Implications for threshold-tracking dashboards.
*   Diff the 61 `architecture_replaced` rows to distinguish true gating architecture vs. threshold rename.
*   Segment the excluded `ANT-RSP-v1-0` Task-2 passage with non-refusing model to close 15-unit gap and test refusal bias.
*   Time-series: correlate `C04 loosened` with external eval results / incident dates (outside corpus).

*Dataset primary record remains `PRISM/study/raw/` + `PRISM/run_log.jsonl`. CSVs are reproducible derivation - re-run to verify.*
