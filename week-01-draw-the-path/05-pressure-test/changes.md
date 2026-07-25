# Changes Based on Pressure-Test

**Date:** 2026-07-25

---

## Change #1: Repurpose About Page into "How I Work" (Engineering Philosophy)

**Finding:** Claude called the About page the weakest link. A "short trust-building bio" does almost no work for a technical claim about reproducible code and data contracts. A skeptical technical buyer will skip it, and every skipped page is friction.

**Change:** Replace the generic bio with a "How I Work" section that functions as *more proof, dressed as bio*. Include:
- My approach to production risk (testing philosophy, CI/CD)
- How I enforce data contracts (schema validation, versioning)
- Link to a public repo or Docker setup showing reproducibility

**Impact:** The About page stops being a skipped page and starts repaying the Hero's claim debt. It turns friction into evidence.

---

## Change #2: Add a Proof Template to Every Case Study

**Finding:** Claude identified "insufficient proof density" as the biggest structural risk. With 4 distinct promises in the Hero, 2-3 projects proving only "it works" leaves 75% of the claim unsubstantiated.

**Change:** Restructure each case study with a consistent 5-beat proof template:
1. Problem
2. Architecture decisions
3. Data contract / testing approach
4. Repro instructions or repo link
5. Outcome metrics

**Impact:** The Hero claim stops being marketing copy and becomes a preview of evidence. A skeptical CTO can independently verify "reproducible" and "rigorous data contracts" without taking my word for it.