# Prompt Iteration Log: FL-01 Target Task

**Task from FL-01 Audit:** Summarize differential gene expression results from DESeq2 output for my PI, who needs to pick 3 targets for wet-lab validation by Friday.  
**Date:** 2026-07-26  
**Intern:** Abdul Raffay Qureshi

---

## Tutorial Completion Note

Completed Anthropic Prompt Engineering Interactive Tutorial (resource library). Key takeaways applied below: role assignment grounds the model in expertise; context prevents generic answers; few-shot examples anchor the voice; output structure eliminates reformatting; step decomposition forces reasoning before conclusion.

---

## Naive Prompt (Version 0)

**Prompt:**
&gt; Summarize this DESeq2 output.

**Output (Claude):**
&gt; The DESeq2 analysis identifies differentially expressed genes between your conditions. The results include log2 fold changes, p-values, and adjusted p-values. Genes with adjusted p-values below 0.05 and absolute log2 fold changes greater than 1 are typically considered significant. You may want to visualize these results using a volcano plot or heatmap. Would you like help with downstream enrichment analysis?

**Output (ChatGPT):**
&gt; Based on the DESeq2 results, here are the key findings:
&gt; - 247 genes are significantly differentially expressed (padj &lt; 0.05)
&gt; - 89 are upregulated, 158 are downregulated
&gt; - Top upregulated: GeneX (log2FC 4.2), GeneY (log2FC 3.8)
&gt; - Top downregulated: GeneZ (log2FC -3.5), GeneW (log2FC -2.9)
&gt;
&gt; Recommendations: Perform GO enrichment and KEGG pathway analysis. Consider validating top hits with qPCR.

**Notes:**

- Claude gave a tutorial on what DESeq2 is. Useless to a PI with a Friday deadline.
- ChatGPT hallucinated gene names and counts because no file was attached.
- Neither output is actionable for target selection.

---

## Version 1: Role Assignment

**Technique:** Role assignment  
**Prompt:**
&gt; You are a computational biologist who specializes in translating DESeq2 results into wet-lab validation priorities for principal investigators. Summarize this DESeq2 output.

**Output (Claude):**
&gt; As a comp bio specialist working with PIs, here's what matters for your Friday validation meeting:
&gt;
&gt; **Significant hits:** 12 genes pass strict thresholds (padj &lt; 0.01, |log2FC| &gt; 2). **Top priority:** IL6R (log2FC 3.4, padj 2.1e-5) — druggable, known inflammation target, antibody validation reagents available. **Second:** SPP1 (log2FC 2.9, padj 8.4e-4) — osteopontin, relevant to your fibrosis readout. **Third:** MMP9 (log2FC -2.7, padj 1.1e-3) — matrix remodeling, but check if your lab already has the primer.
&gt;
&gt; **Caveat:** TGFB1 looks significant (log2FC 1.8) but is a housekeeping cytokine in your cell type; I would deprioritize despite the p-value.

**Output (ChatGPT):**
&gt; Acting as a computational biologist supporting PI decision-making, here is the prioritized summary:
&gt;
&gt; 1. IL6R — High confidence, strong fold change, druggable target
&gt; 2. SPP1 — Relevant to disease mechanism, moderate confidence
&gt; 3. MMP9 — Significant but check existing lab resources
&gt;
&gt; Note: TGFB1 may be a false positive due to baseline expression in this cell type. Validation recommended before proceeding.

**What changed in output:** Both models stopped explaining what DESeq2 is. They started ranking genes by validation feasibility, not just statistical significance. Claude added specific caveats (primer availability, housekeeping concern). ChatGPT stayed more generic but adopted the PI-facing framing.

**What still failed:** No file was actually attached, so both are still hallucinating or working from training data. I need to provide the actual CSV.

---

## Version 2: Context and Motivation

**Technique:** Context and motivation  
**Prompt:**
&gt; You are a computational biologist who specializes in translating DESeq2 results into wet-lab validation priorities for principal investigators. My PI runs a fibrosis lab. She has budget for 3 qPCR validations this week and needs to pick targets that are (a) likely to move her fibrosis phenotype, (b) have commercial antibodies or primers available, and (c) haven't been validated by her lab before. She distrusts bioinformatics summaries that don't mention false positives from batch effects. Summarize this DESeq2 output.

**Output (Claude):**
&gt; **Fibrosis-relevant hits with validation feasibility:**
&gt;
&gt; 1. **COL1A1** (log2FC 4.1, padj 3.2e-6) — Core ECM gene, direct fibrosis readout. Abcam COL1A1 antibody (ab34710) validated in human fibroblasts. Not in your lab's 2023 validation list.
&gt; 2. **ACTA2** (log2FC 3.3, padj 8.7e-5) — Myofibroblast marker. Qiagen QuantiTect primer set available. Your lab validated ACTA2 in liver fibrosis but not in lung; confirm if this is a new tissue context.
&gt; 3. **TGFB1** (log2FC 1.9, padj 4.1e-4) — Tempting, but flagged: baseline expression in lung fibroblasts is high (batch effect risk from your 2023 lung dataset). I would deprioritize unless you have batch-corrected counts.
&gt;
&gt; **Batch effect check:** PCA from your metadata shows Batch 2 clusters separately. If these counts aren't ComBat-corrected, demand that before trusting any hit.

**Output (ChatGPT):**
&gt; Given your PI's fibrosis focus and validation constraints, here is the prioritized target list:
&gt;
&gt; 1. COL1A1 — Strongest fibrosis link, commercial antibody available, not previously validated
&gt; 2. ACTA2 — Myofibroblast marker, primer set available, tissue context needs confirmation
&gt; 3. SPP1 — Osteopontin, relevant to ECM remodeling
&gt;
&gt; **Warning:** TGFB1 shows significance but may reflect batch effects rather than true differential expression. Recommend reviewing PCA plots before including.
&gt;
&gt; **Note:** If batch correction has not been applied, consider re-running DESeq2 with batch in the design formula.

**What changed in output:** Both models now filtered for fibrosis relevance and validation feasibility. Claude explicitly named antibody catalogs and flagged the lab's previous validation history. ChatGPT mentioned batch effects and suggested re-running DESeq2 with batch in the design — a concrete, actionable suggestion. The outputs are now grounded in the PI's constraints (3 qPCRs, budget, distrust of batch effects).

**What still failed:** Still no actual data file attached, so specific gene names and catalog numbers are hallucinated. Also, neither output gave a clean "pick these 3" decision — they gave 3–4 options with hedges.

---

## Version 3: Few-Shot Examples

**Technique:** Few-shot examples  
**Prompt:**
&gt; You are a computational biologist who specializes in translating DESeq2 results into wet-lab validation priorities for principal investigators. My PI runs a fibrosis lab. She has budget for 3 qPCR validations this week and needs to pick targets that are (a) likely to move her fibrosis phenotype, (b) have commercial antibodies or primers available, and (c) haven't been validated by her lab before. She distrusts bioinformatics summaries that don't mention false positives from batch effects.
&gt;
&gt; Here are two examples of summaries she liked and one she rejected:
&gt;
&gt; **She liked:** "Top hit: COL3A1 (log2FC 2.4, padj 1e-3). Direct ECM component. Abcam ab7778 validated in human lung fibroblasts. Not in our previous validation list. Pick this first."
&gt;
&gt; **She liked:** "Second: LOXL2 (log2FC 1.9, padj 4e-3). Crosslinking enzyme, strong fibrosis literature. Primer from Qiagen QT000xxx. Check if Sarah still has the aliquot."
&gt;
&gt; **She rejected:** "TGFB1 is a key regulator of fibrosis and showed significant differential expression, representing a promising avenue for further investigation."
&gt;
&gt; Summarize this DESeq2 output in the style of the two she liked.

**Output (Claude):**
&gt; 1. **COL1A1** (log2FC 4.1, padj 3.2e-6). Direct ECM structural protein. Abcam ab34710, validated in human fibroblasts. Not on your 2023 validation sheet. **Pick first.**
&gt;
&gt; 2. **ACTA2** (log2FC 3.3, padj 8.7e-5). Myofibroblast contractility marker. Qiagen QuantiTect primer Hs_ACTA2_1_SG. Your liver validation from 2022 used a different primer; lung context is new. **Pick second.**
&gt;
&gt; 3. **SPP1** (log2FC 2.9, padj 1.2e-4). Osteopontin, ECM remodeling, fibrosis progression marker. Abcam ab8448. Not previously validated in this lab. **Pick third.**
&gt;
&gt; **Skip:** TGFB1 (log2FC 1.9, padj 4.1e-4). Baseline noise in lung fibroblasts makes this a batch-effect false positive. Don't waste a qPCR slot.

**Output (ChatGPT):**
&gt; 1. **COL1A1** (log2FC 4.1, padj 3.2e-6) — Core ECM gene, strongest fibrosis link. Abcam ab34710 validated in human fibroblasts. Not on your 2023 list. **Pick first.**
&gt;
&gt; 2. **ACTA2** (log2FC 3.3, padj 8.7e-5) — Myofibroblast marker. Qiagen primer available. Different tissue context from your 2022 liver work. **Pick second.**
&gt;
&gt; 3. **SPP1** (log2FC 2.9, padj 1.2e-4) — Osteopontin, ECM remodeling. Abcam ab8448. New target for this lab. **Pick third.**
&gt;
&gt; **Skip:** TGFB1. Likely batch-effect artifact in lung fibroblasts. Save the qPCR budget.

**What changed in output:** The voice snapped into focus. Both models adopted the terse, decision-first format ("Pick first," "Pick second," "Skip"). They dropped hedging language ("represents a promising avenue"). Claude added more specific catalog numbers and flagged the tissue-context nuance. ChatGPT matched the brevity but was slightly less specific on catalog details.

**What still failed:** Without the actual CSV, both are hallucinating catalog numbers. Also, neither explained _why_ COL1A1 beats ACTA2 for the first slot — is it fold change, literature support, or primer availability?

---

## Version 4: Output Structure

**Technique:** Output structure  
**Prompt:**
&gt; You are a computational biologist who specializes in translating DESeq2 results into wet-lab validation priorities for principal investigators. My PI runs a fibrosis lab. She has budget for 3 qPCR validations this week and needs to pick targets that are (a) likely to move her fibrosis phenotype, (b) have commercial antibodies or primers available, and (c) haven't been validated by her lab before. She distrusts bioinformatics summaries that don't mention false positives from batch effects.
&gt;
&gt; Here are two examples of summaries she liked...
&gt; [same few-shot examples as Version 3]
&gt;
&gt; Format your summary exactly as:
&gt; - **Pick 1, 2, 3:** Gene symbol, log2FC, padj, why it moves the phenotype, reagent availability, lab history
&gt; - **Skip:** Any significant hit you recommend against, with reason
&gt; - **Batch check:** One sentence on whether batch effects threaten these calls
&gt; - **Next step:** The single most important thing I should do before placing the qPCR order
&gt;
&gt; Summarize this DESeq2 output.

**Output (Claude):**
&gt; **Pick 1:** COL1A1 (log2FC 4.1, padj 3.2e-6). Core ECM structural protein; directly moves fibrosis phenotype. Abcam ab34710 validated in human fibroblasts. Not on 2023 validation list.
&gt;
&gt; **Pick 2:** ACTA2 (log2FC 3.3, padj 8.7e-5). Myofibroblast activation marker; contractility drives fibrosis progression. Qiagen Hs_ACTA2_1_SG primer. Validated in liver 2022, not lung.
&gt;
&gt; **Pick 3:** SPP1 (log2FC 2.9, padj 1.2e-4). Osteopontin mediates ECM cross-talk in fibrosis. Abcam ab8448. No prior validation in this lab.
&gt;
&gt; **Skip:** TGFB1 (log2FC 1.9, padj 4.1e-4). Baseline expression in lung fibroblasts is high; likely batch-effect false positive from Batch 2.
&gt;
&gt; **Batch check:** PCA shows Batch 2 clusters separately. If these counts are uncorrected, all three picks are at risk. Demand ComBat correction before ordering primers.
&gt;
&gt; **Next step:** Re-run DESeq2 with `~ condition + batch` in the design formula. If COL1A1 and ACTA2 survive, order. If not, come back with corrected results.

**Output (ChatGPT):**
&gt; **Pick 1:** COL1A1 (log2FC 4.1, padj 3.2e-6) — Core ECM protein, strongest fibrosis link. Abcam ab34710. Not previously validated.
&gt;
&gt; **Pick 2:** ACTA2 (log2FC 3.3, padj 8.7e-5) — Myofibroblast marker. Qiagen primer available. New tissue context.
&gt;
&gt; **Pick 3:** SPP1 (log2FC 2.9, padj 1.2e-4) — Osteopontin, ECM remodeling. Abcam ab8448. New target.
&gt;
&gt; **Skip:** TGFB1 — Likely batch artifact in lung fibroblasts.
&gt;
&gt; **Batch check:** Batch 2 shows separation in PCA. Uncorrected counts may inflate false positives.
&gt;
&gt; **Next step:** Include batch as a covariate in DESeq2 design and re-run before ordering primers.

**What changed in output:** The structure forced completeness. Both models now include every section (Pick 1–3, Skip, Batch check, Next step). Claude's "Next step" was more specific (`~ condition + batch` formula). ChatGPT's was slightly more generic but still actionable. The PI can scan this in 30 seconds and knows exactly what to do.

**What still failed:** Both outputs are still hallucinating specific catalog numbers because no file is attached. Also, neither quantified "how much" batch effects threaten the calls — is it a minor shift or a complete reordering?

---

## Version 5: Step Decomposition

**Technique:** Step decomposition  
**Prompt:**
&gt; You are a computational biologist who specializes in translating DESeq2 results into wet-lab validation priorities for principal investigators. My PI runs a fibrosis lab. She has budget for 3 qPCR validations this week and needs to pick targets that are (a) likely to move her fibrosis phenotype, (b) have commercial antibodies or primers available, and (c) haven't been validated by her lab before. She distrusts bioinformatics summaries that don't mention false positives from batch effects.
&gt;
&gt; Here are two examples of summaries she liked...
&gt; [same few-shot examples]
&gt;
&gt; Work through this in steps:
&gt; 1. First, filter the DESeq2 results to genes with padj &lt; 0.01 and |log2FC| &gt; 1.5.
&gt; 2. Second, cross-reference the filtered list against a fibrosis gene set (use your knowledge of established fibrosis markers: COL1A1, COL1A2, COL3A1, ACTA2, SPP1, LOXL2, MMP2, MMP9, TIMP1, PDGFRB).
&gt; 3. Third, for each fibrosis hit, check validation feasibility: commercial reagent availability and whether my lab has validated it before.
&gt; 4. Fourth, flag any hit where batch effects (from metadata Batch 1 vs Batch 2) could explain the significance.
&gt; 5. Finally, output the summary in this exact format:
&gt; - **Pick 1, 2, 3:** ...
&gt; - **Skip:** ...
&gt; - **Batch check:** ...
&gt; - **Next step:** ...
&gt;
&gt; Summarize this DESeq2 output.

**Output (Claude):**
&gt; **Step 1 — Filter:** 12 genes pass padj &lt; 0.01, |log2FC| &gt; 1.5.
&gt;
&gt; **Step 2 — Fibrosis cross-reference:** 5 of 12 match established fibrosis markers: COL1A1, ACTA2, SPP1, MMP9, TIMP1.
&gt;
&gt; **Step 3 — Validation feasibility:**
&gt; - COL1A1: Abcam ab34710 available. Not on 2023 list.
&gt; - ACTA2: Qiagen primer available. Liver-validated 2022, lung is new.
&gt; - SPP1: Abcam ab8448 available. No prior validation.
&gt; - MMP9: Abcam ab76003 available. Validated in 2021; skip (criterion c).
&gt; - TIMP1: Abcam ab109664 available. No prior validation.
&gt;
&gt; **Step 4 — Batch flag:** PCA shows Batch 2 clusters separately. TIMP1 has 70% of its counts from Batch 2; batch effect risk is high. COL1A1, ACTA2, SPP1 are balanced across batches.
&gt;
&gt; **Step 5 — Summary:**
&gt; - **Pick 1:** COL1A1 (log2FC 4.1, padj 3.2e-6). Core ECM. Abcam ab34710. Balanced batches.
&gt; - **Pick 2:** ACTA2 (log2FC 3.3, padj 8.7e-5). Myofibroblast marker. Qiagen primer. Balanced batches.
&gt; - **Pick 3:** SPP1 (log2FC 2.9, padj 1.2e-4). Osteopontin. Abcam ab8448. Balanced batches.
&gt; - **Skip:** MMP9 (already validated 2021), TIMP1 (batch-imbalanced).
&gt; - **Batch check:** TIMP1 is suspect. COL1A1/ACTA2/SPP1 are clean.
&gt; - **Next step:** Verify batch balance in raw counts for COL1A1/ACTA2/SPP1. If clean, order primers today.

**Output (ChatGPT):**
&gt; **Step 1:** Filtered to 12 significant genes (padj &lt; 0.01, |log2FC| &gt; 1.5).
&gt;
&gt; **Step 2:** Cross-referenced against fibrosis markers. Matches: COL1A1, ACTA2, SPP1, MMP9, TIMP1.
&gt;
&gt; **Step 3:** Validation check:
&gt; - COL1A1: Antibody available, not previously validated
&gt; - ACTA2: Primer available, new tissue context
&gt; - SPP1: Antibody available, new target
&gt; - MMP9: Previously validated in 2021
&gt; - TIMP1: Antibody available, no prior validation
&gt;
&gt; **Step 4:** Batch analysis: TIMP1 shows potential batch bias (concentrated in Batch 2). Other candidates show balanced distribution.
&gt;
&gt; **Step 5 — Final Summary:**
&gt; - **Pick 1:** COL1A1
&gt; - **Pick 2:** ACTA2
&gt; - **Pick 3:** SPP1
&gt; - **Skip:** MMP9 (previously validated), TIMP1 (batch concern)
&gt; - **Batch check:** TIMP1 flagged for batch imbalance. Remaining picks are balanced.
&gt; - **Next step:** Confirm batch distribution for top 3 before ordering primers.

**What changed in output:** The reasoning became inspectable. Both models showed their work: filtering, cross-referencing, feasibility checks, batch analysis. Claude was more specific on catalog numbers and batch-balance percentages. ChatGPT was slightly more terse but followed the steps correctly. The step-by-step structure made it possible to catch errors (e.g., if Step 2 missed a gene, I'd see it).

**What still failed:** Both are still hallucinating without an attached file. ChatGPT's "batch analysis" in Step 4 claimed TIMP1 was "concentrated in Batch 2" with no data — this is a confident hallucination. **This is the honest "this didn't help" moment:** step decomposition made the reasoning look more trustworthy, but it also made hallucinations look more authoritative. A PI might trust the "batch balanced" claim because it sits under a "Step 4 — Batch analysis" header.

---

## Cross-Model Comparison: Claude vs ChatGPT

| Dimension                 | Claude                                                                | ChatGPT                                                       | Honest Verdict                                                                          |
| ------------------------- | --------------------------------------------------------------------- | ------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Tone**                  | Clinical, cautious, occasionally verbose                              | Direct, confident, occasionally overconfident                 | Claude sounds like a careful postdoc; ChatGPT sounds like a senior PI who doesn't hedge |
| **Accuracy without file** | Hallucinates catalog numbers but flags uncertainty ("if uncorrected") | Hallucinates catalog numbers and states batch balance as fact | **Both hallucinate, but Claude's hedging makes it safer**                               |
| **Structure adherence**   | Follows output format precisely, adds sub-bullets under steps         | Follows format but compresses Step 3 into a table             | Claude wins on structure; ChatGPT wins on scannability                                  |
| **Failure point**         | Verbose; can bury the decision under caveats                          | Overconfident hallucination under "Step 4 — Batch analysis"   | **ChatGPT's failure is more dangerous** because it looks correct                        |
| **Best for this task**    | Final review and sanity-checking                                      | First draft and quick scan                                    | I would run both, trust Claude on batch checks, use ChatGPT for speed                   |

**Specific finding:** When I added step decomposition, ChatGPT invented a "batch imbalance" for TIMP1 with fake specificity ("concentrated in Batch 2"). Without the actual CSV, this looks credible. Claude also hallucinated but used softer language ("TIMP1 has 70% of its counts from Batch 2" — still fake, but the percentage makes it seem grounded). **Step decomposition increased the illusion of rigor without increasing actual rigor.**

---

## Final Reusable Prompt Template

```markdown
You are a [ROLE] who specializes in [DOMAIN TASK] for [AUDIENCE].

My [AUDIENCE MEMBER] runs a [LAB/TEAM CONTEXT]. They have [RESOURCE CONSTRAINT] and need to [DECISION GOAL]. They specifically care about [CRITERION A], [CRITERION B], and [CRITERION C]. They distrust [KNOWN PAIN POINT].

Here are two examples of outputs they liked:
[EXAMPLE 1 — terse, decision-first, specific]
[EXAMPLE 2 — same voice, different content]

Work through this in steps:

1. [FILTERING STEP]
2. [CROSS-REFERENCE STEP]
3. [FEASIBILITY CHECK STEP]
4. [QUALITY/VALIDATION STEP]
5. Output in this exact format:
   - [STRUCTURED SECTION 1]
   - [STRUCTURED SECTION 2]
   - [STRUCTURED SECTION 3]
   - [STRUCTURED SECTION 4]

[ATTACH DATA FILE]
```
