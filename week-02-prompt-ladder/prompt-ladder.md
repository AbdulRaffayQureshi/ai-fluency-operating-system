# Prompt Ladder: Drug Candidate Ranking

**Task:** Prioritize BACE1 inhibitor candidates for synthesis.  
**Date:** 2026-07-26  
**Intern:** Abdul Raffay Qureshi

---

## Baseline (Run 0): The Weak Prompt

**Prompt:**
&gt; Help me analyze drug discovery data.

**Representative Output Excerpt:**
&gt; Drug discovery is a complex, multi-stage process that involves identifying potential therapeutic compounds, testing them for efficacy and safety, and optimizing them for clinical use. Modern drug discovery leverages computational methods such as molecular docking, molecular dynamics simulations, and machine learning to accelerate the identification of promising candidates. Data analysis in this context typically involves processing large datasets from high-throughput screening campaigns, cheminformatics databases, and clinical trial results. Key considerations include...

**Notes:**

| Question                     | Answer                                                                                            |
| ---------------------------- | ------------------------------------------------------------------------------------------------- |
| What changed in the prompt?  | Nothing — this is the baseline.                                                                   |
| What improved in the output? | Nothing. It gave a generic essay defining drug discovery.                                         |
| What still failed?           | No ranking, no specific compounds, no actionable next step. Useless to a chemist with a deadline. |
| What would I try next?       | Narrow the goal from "analyze" to a specific decision: rank candidates.                           |

---

## Version 1: Clearer Goal

**Prompt:**
&gt; Rank these 50 BACE1 inhibitor candidates by predicted binding strength.

**Representative Output Excerpt:**
&gt; Based on typical BACE1 inhibitor scaffolds, here is a hypothetical ranking of candidate compounds by predicted binding affinity:
&gt;
&gt; 1. Compound A — Strong hydrogen bonding with catalytic dyad
&gt; 2. Compound B — Good hydrophobic complementarity
&gt; 3. Compound C — Moderate affinity, potential for optimization
&gt;
&gt; Note: Without access to your specific dataset, these are illustrative examples...

**Notes:**

| Question                     | Answer                                                                                                                                    |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| What changed in the prompt?  | Changed "analyze drug discovery data" to "rank 50 BACE1 inhibitors by binding strength."                                                  |
| What improved in the output? | It stopped explaining what drug discovery is. It attempted a ranking and used domain terms (catalytic dyad, hydrophobic complementarity). |
| What still failed?           | It hallucinated compounds ("Compound A," "Compound B") because I gave it no actual data.                                                  |
| What would I try next?       | Give it the real dataset and scores so it ranks actual molecules.                                                                         |

---

## Version 2: Defined Audience

**Prompt:**
&gt; Rank these 50 BACE1 inhibitor candidates by predicted binding strength for a medicinal chemist who needs to pick 5 compounds for synthesis next week.

**Representative Output Excerpt:**
&gt; For a medicinal chemist selecting leads for synthesis, here is a prioritized list considering both predicted affinity and synthetic accessibility:
&gt;
&gt; 1. Compound A — Predicted pIC50 ~8.5; route: Suzuki coupling (2 steps)
&gt; 2. Compound B — Predicted pIC50 ~8.2; route: amide formation (1 step)
&gt; 3. Compound C — Predicted pIC50 ~7.9; route: reductive amination (3 steps)
&gt;
&gt; _Disclaimer: Compound identifiers and scores are illustrative..._

**Notes:**

| Question                     | Answer                                                                                                                                                                                                      |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| What changed in the prompt?  | Added audience: medicinal chemist with a synthesis deadline next week.                                                                                                                                      |
| What improved in the output? | It dropped beginner explanations. Started using synthesis-aware language (Suzuki coupling, amide formation, step counts). It implicitly considered synthetic accessibility, which matters to this audience. |
| What still failed?           | Still hallucinating compounds and scores. The "illustrative" disclaimer means it's making things up.                                                                                                        |
| What would I try next?       | Attach the actual docking scores and assay data so the ranking is grounded in real numbers.                                                                                                                 |

---

## Version 3: Real Context

**Prompt:**
&gt; Rank these 50 BACE1 inhibitor candidates by predicted binding strength for a medicinal chemist who needs to pick 5 compounds for synthesis next week. Use the attached AutoDock Vina docking scores (kcal/mol) and experimental pIC50 values from ChEMBL assay 1798. Compound IDs are BACE_001 through BACE_050.

**Representative Output Excerpt:**
&gt; Based on the provided Vina scores and ChEMBL pIC50 data:
&gt;
&gt; BACE_017: Vina -10.4, pIC50 8.7 — Strong agreement
&gt; BACE_031: Vina -9.8, pIC50 8.5 — Strong agreement
&gt; BACE_008: Vina -9.2, pIC50 7.1 — Docking score overestimates affinity
&gt; BACE_044: Vina -8.9, pIC50 8.3 — Experimental data better than predicted
&gt; ...
&gt;
&gt; Top 5 for synthesis: BACE_017, BACE_031, BACE_044, BACE_012, BACE_029.

**Notes:**

| Question                     | Answer                                                                                                                                                                         |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| What changed in the prompt?  | Added real context: AutoDock Vina scores, ChEMBL assay 1798, compound ID range BACE_001–050.                                                                                   |
| What improved in the output? | It referenced actual numeric data. Rankings were grounded in real docking scores and experimental values instead of hallucinated structures. It caught BACE_008 as an outlier. |
| What still failed?           | The output was a wall of text. A chemist under deadline pressure would need to reformat this into a scannable table.                                                           |
| What would I try next?       | Specify the output format as a table so it's immediately usable.                                                                                                               |

---

## Version 4: Specified Output Format

**Prompt:**
&gt; Rank these 50 BACE1 inhibitor candidates by predicted binding strength for a medicinal chemist who needs to pick 5 compounds for synthesis next week. Use the attached AutoDock Vina docking scores (kcal/mol) and experimental pIC50 values from ChEMBL assay 1798. Present results as a markdown table with columns: Rank, Compound ID, Vina Score, pIC50, Synthesis Priority.

**Representative Output Excerpt:**
&gt; | Rank | Compound ID | Vina Score | pIC50 | Synthesis Priority |
&gt; |------|-------------|------------|-------|-------------------|
&gt; | 1 | BACE_017 | -10.4 | 8.7 | High |
&gt; | 2 | BACE_031 | -9.8 | 8.5 | High |
&gt; | 3 | BACE_044 | -8.9 | 8.3 | High |
&gt; | 4 | BACE_012 | -9.1 | 7.9 | Medium |
&gt; | 5 | BACE_029 | -8.7 | 7.8 | Medium |
&gt; | ... | ... | ... | ... | ... |

**Notes:**

| Question                     | Answer                                                                                                                                                                                                                                                                                                   |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| What changed in the prompt?  | Added output format: markdown table with specific columns.                                                                                                                                                                                                                                               |
| What improved in the output? | Structure is scannable. Compound IDs and scores line up cleanly. A chemist can scan it in 10 seconds.                                                                                                                                                                                                    |
| What still failed?           | **This didn't help as much as I expected.** The "Synthesis Priority" column was just High/Medium/Low based on rank order — no real filtering. Three compounds in the top 10 had PAINS alerts, but the table flagged them as "High" priority because they docked well. Pretty table, bad recommendations. |
| What would I try next?       | Add hard constraints and quality criteria so the ranking filters out toxic or problematic scaffolds automatically.                                                                                                                                                                                       |

---

## Version 5: Constraints & Quality Criteria

**Prompt:**
&gt; Rank these 50 BACE1 inhibitor candidates by predicted binding strength for a medicinal chemist who needs to pick 5 compounds for synthesis next week. Use the attached AutoDock Vina docking scores (kcal/mol) and experimental pIC50 values from ChEMBL assay 1798. Present results as a markdown table with columns: Rank, Compound ID, Vina Score, pIC50, Synthesis Priority. Flag any compound with PAINS alerts, MW &gt;500, or logP &gt;5 as "Deprioritize." Highlight compounds where Vina score and pIC50 disagree by &gt;1 log unit. Explain each flag in one sentence.

**Representative Output Excerpt:**
&gt; | Rank | Compound ID | Vina Score | pIC50 | Priority | Flags |
&gt; |------|-------------|------------|-------|----------|-------|
&gt; | 1 | BACE_017 | -10.4 | 8.7 | High | — |
&gt; | 2 | BACE_031 | -9.8 | 8.5 | High | — |
&gt; | — | BACE_003 | -10.1 | 8.6 | **Deprioritize** | PAINS alert: rhodanine scaffold (frequent hitter). |
&gt; | — | BACE_021 | -9.9 | 8.4 | **Deprioritize** | MW 512; logP 5.3 (beyond Rule of 5). |
&gt; | 3 | BACE_044 | -8.9 | 8.3 | High | — |
&gt; | 4 | BACE_012 | -9.1 | 7.9 | Medium | — |
&gt; | ⚠️ | BACE_008 | -9.2 | 7.1 | Review | Discrepancy: Vina overestimates by 1.6 log units. Rescore with explicit waters? |
&gt; | 5 | BACE_029 | -8.7 | 7.8 | Medium | — |

**Notes:**

| Question                     | Answer                                                                                                                                                                                                     |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| What changed in the prompt?  | Added constraints (PAINS, MW, logP filters) and verification requirements (flag docking-experimental discrepancies &gt;1 log unit).                                                                        |
| What improved in the output? | It caught false positives. BACE_003 and BACE_021 were top-scoring by affinity alone but got flagged for real medicinal-chemistry red flags. BACE_008's discrepancy was surfaced with a concrete next step. |
| What still failed?           | For BACE_008, it suggested "rescore with explicit waters" but didn't explain how. The one-sentence flag is good, but a junior chemist might not know what "explicit waters" means in this context.         |
| What would I try next?       | Add a final instruction to recommend the single most important next experimental step for each flagged compound.                                                                                           |

---

## Final Reusable Prompt

Cleaned up so someone else on my track could use it without me in the room.
