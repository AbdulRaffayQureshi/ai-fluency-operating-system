# How I Add the Next Case Study

**Shape (unchanged from Week 2):** Problem → What I Did (and Decided) → What Came of It. Three beats, no filler, every claim backed by a number or an artifact.

**Steps:**
1. Finish the work first. I don't draft the case study until the pipeline runs end-to-end and I have a real before/after number.
2. Pull the "Problem" paragraph from the original ask — the actual failure mode, not a generic pain point. (BACE1 case: a false positive ranking in the top 10. Churn API case: a notebook that crashed on schema drift.)
3. Write "What I Did" as decisions, not a tool list. Every sentence should answer "why this and not the obvious alternative" — e.g. "I used Pandera at the API boundary instead of validating downstream" is a decision; "I used Python" is not.
4. Close with "What Came of It" — one hard number (rank change, uptime, error caught, time saved) and one line proving reproducibility (repo link, locked dependencies, rerun instructions).
5. Append it to the Framed Cases doc under the existing two, keep the same Voice line at the top, and push it to the portfolio site.

**Named next piece of work:** FlyRank ML Internship capstone (Content Opportunity Scoring). Draft is in `case-study-3-flyrank-draft.md` in this folder — ready to append once the portfolio site is live.

**Reminder set:** see `reminder-evidence.png`.