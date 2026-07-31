## Case Study 3: FlyRank ML Internship — Content Decay Prioritization

**Problem**
FlyRank's editorial team was flagging decaying content with static age/volume thresholds. The rule caught roughly half of true declines and wasted editor hours rewriting pages that were already dead-ends — and standalone keyword volume showed near-zero correlation (r = 0.001) with actual page impressions, so the team's core sorting signal was misleading them.

**What I Did (and Decided)**
I trained a Random Forest classifier on 30,000 anonymized content records to replace the heuristic. Two decisions mattered most: (1) I split validation by client_id with GroupShuffleSplit instead of a random split — with only 32 clients in the data, a random split lets the model memorize client-specific baselines instead of learning real decay patterns. (2) I ran a deliberate leakage stress test by injecting a target-derived feature, watched precision jump to an artificial 99.8%, then audited the real feature set and caught a second, subtler leak (a 30-day impression window overlapping the label window) — removed it and retrained rather than reporting the inflated number.

**What Came of It**
The corrected, leak-free model hit 0.739 ROC-AUC and 66.52% precision against a 0.584 AUC / 52.41% precision rule-based baseline — a real, defensible lift, reported honestly after fixing what I broke first. Full leakage audit, notebooks, and the deployed research paper are in the repo; the pipeline reruns end-to-end from a single script.