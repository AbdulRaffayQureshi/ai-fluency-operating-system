# Claude Pressure-Test Output

**Date:** 2026-07-25

---

Alright — playing this straight, no hedging.

**Does every page earn its place?**

**Hero** — Earns it, but the claim itself is doing a lot of unverified lifting. "Production-ready," "reproducible code," "rigorous data contracts," "clean UI architecture" — that's four distinct promises stacked into one sentence, and none of them are proven yet at the point the visitor reads them. A skeptical visitor (which, if you're targeting technical buyers or technical-adjacent decision-makers, you should assume) reads that line as _marketing until proven otherwise_. It's a strong claim, but strong claims create a debt that the rest of the site has to repay. Ask: does your Case Studies page actually repay all four promises, or just one or two?

**Work/Case Studies** — Earns it, unquestionably the load-bearing page. But 2-3 projects is thin for four distinct claims. If each case study only demonstrates "ML app that works," you've proven maybe 25% of your claim (production-readiness) and left "reproducible code," "data contracts," and "UI architecture" as assertions with no evidence. That's the biggest structural risk in this sitemap — not a missing page, but _insufficient proof density_ on the one page doing all the work.

**About** — This is the weakest link and you should defend its existence harder than you probably want to. A "short trust-building bio" is doing almost no work for a claim this technical. Trust for someone claiming "rigorous data contracts" isn't built by a bio — it's built by evidence of rigor: architecture decisions, testing philosophy, how you handle production failure, whether you write your own docs. Unless About contains something structurally different from a LinkedIn summary, it's a page a skeptical technical buyer will skip, and every skipped page is friction, not neutral. Cut it or radically repurpose it (see below).

**Contact** — Earns it. No notes, it's the terminus and it's correctly minimal (Calendly embed, low-friction).

---

**What's missing that would strengthen belief**

You're claiming three specific, checkable things beyond "I can build apps": reproducibility, data contracts, and clean architecture. None of those are self-evident from a portfolio screenshot or a metrics callout. A skeptical technical reviewer will specifically look for:

- **Reproducible code** → Is there a public repo link? Is there anything about CI/CD, testing, environment reproducibility (Docker, pinned deps, seed control) _per case study_? If not, "reproducible code" is an assertion, not a demonstrated trait.
- **Rigorous data contracts** → This is a specific, somewhat unusual term to lead with. Does any case study actually show a schema, a validation layer, versioning, or contract-testing approach? If this phrase isn't backed by at least one concrete artifact, cut it from the claim — an unsubstantiated specific claim is worse than a substantiated vague one, because specificity signals "check me" to a skeptical reader.
- **Clean UI architecture** → Screenshots don't prove architecture. Component structure, state management approach, or a brief "how I organized this" note would.

**The fix isn't a new page** — it's restructuring each case study to have a consistent proof template: Problem → Architecture decisions → Data contract/testing approach → Repro instructions or repo link → Outcome metrics. If every case study hits those five beats, your Hero claim stops being marketing copy and starts being a preview of evidence you're about to show.

---

**What to cut**

**About**, as currently scoped. A "short trust-building bio" for this audience is weak sauce — replace it with either (a) nothing, folding a 2-line credibility statement into the Hero or Contact page, or (b) something that actually does trust-building for _this specific claim_: your engineering philosophy, how you approach production risk, or a "how I work" section that's really just more proof, dressed as bio.

Nothing else needs cutting — this is already minimal. The risk here isn't page bloat, it's **evidence thinness on the one page that matters**.

---

**Is the flow frictionless from landing → believing → acting?**

Mechanically, yes — three clicks, no dead ends, CTA at both ends. But "frictionless" isn't just about click-count, it's about _belief velocity_. Right now the flow is:

Claim (unproven) → 2-3 examples (partial proof) → bio (near-zero proof) → ask for a booking

That's asking someone to commit to a _project consultation_ — a real-world, calendar-blocking action — on the strength of partial evidence for a fairly technical, specific claim. The gap between "what I proved" and "what I'm asking you to do" is the actual friction, and it's invisible in a sitemap diagram because sitemaps only show structure, not evidence load per page.

**One honest question to ask yourself:** if a skeptical CTO read your Hero claim and then your two case studies, would they be able to independently verify "reproducible" and "rigorous data contracts," or would they have to take your word for it? If it's the latter, that's the actual weakness — not the sitemap shape, which is fine.
