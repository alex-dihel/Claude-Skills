# CRO Audit Framework — Extraction Doc

**Source:** Exposure Ninja CRO training video (full transcript, ~2h30m) + partial transcript cross-reference.
**Status:** Draft 1. Synthesized in original wording from source material, not transcript text. This is a working doc, expected to be revised as gaps get filled.

---

## 1. Core premise

Traffic-focused marketing (ads, SEO, social) gets people to a site. Almost nobody audits what happens after they land. A site converting at 0.5% vs. 3% doesn't just get more leads, it changes the economics of every other channel: the same ad spend produces roughly a third of the cost-per-lead once conversion rate triples. This is the argument for treating CRO as a prerequisite to scaling traffic spend, not an afterthought.

Framing for the skill: buttons and forms are implementation. The actual work is the psychological case for why a visitor should trust this business and act now, built around a form or CTA, not the form itself. A well-placed form on a page that hasn't built trust or handled doubt converts poorly regardless of button color or placement.

---

## 2. The six-part audit framework

Six elements to check for, in this order, both above and below the fold, on both the homepage and the specific landing page a visitor would actually arrive on (which is often not the homepage — check what ranks / what paid ads point to).

### 2.1 Credibility
**Definition:** Does the page give the visitor a concrete reason to believe this business is good at what it does, rather than assuming trust is automatic just because a website exists.

**Audit questions:**
- Is there any evidence of competence visible above the fold (not just a logo)?
- Are there before/afters, case studies, results, credentials, awards, or media mentions?
- Would a stranger with zero context be able to say *why* this business is credible, specifically?

**Notes:** The "before and after" format doesn't require a visual transformation product. A case study, a results summary, or a short video walkthrough of a past outcome can serve the same function for a B2B or service business.

### 2.2 Lure
**Definition:** The specific thing being offered to move a visitor from "just browsing" to "lead." For e-commerce this is usually just the product. For lead-gen/service businesses, this is the weak point most sites skip entirely.

**Audit questions:**
- What is the visitor told they'll get if they act (fill in the form, request a quote, sign up)?
- Is that outcome visible and tangible, or just a generic ask ("subscribe," "inquire," "get in touch")?
- Does the offer describe a next step *the visitor* perceives as valuable, not just a next step in the business's own sales process?
- Can you, in one sentence, describe how this specific offer moves the visitor closer to their goal? If not, the lure isn't strong enough.

**Notes:** The reframe that matters here: most businesses already have a version of this in their sales process (a scoping call, a free mockup, a consultation) but fail to name or sell it as a distinct, valuable step on the website. The fix is usually positioning existing steps more clearly, not adding new ones.

### 2.3 Objection handling
**Definition:** Addressing the doubts that create inertia before a first contact, distinct from objections raised later in an active sales conversation (price, timeline, etc.). This is about why someone hesitates to take the very first step at all.

**Common first-step objections to check for:**
- Do you even serve someone like me? (location, industry, situation fit)
- What actually happens after I submit this? (call? sales pitch? house visit?)
- Is this going to cost me anything or commit me to anything?
- Do I even have a legitimate reason to be here? (e.g., "do I actually have a claim" before "start my claim")

**Audit questions:**
- For the primary CTA, list every reasonable doubt a first-time visitor would have before clicking it.
- Is each one addressed on the page, near the CTA, not buried elsewhere?
- Is the CTA appropriately low-commitment for how much trust has been built so far? ("start my claim" vs. "find out if you could claim")

**Notes:** Uncertainty and fear of wasted time/embarrassment suppress action even when the underlying interest is genuine. This is a distinct failure mode from price objections and needs to be solved on-page, before the CTA is clicked, not after.

**Technique worth flagging — progressive disclosure in multi-step forms:** ordering form steps so the visitor answers questions about their *situation* first (e.g. injury details) before being asked for personal contact information. This reframes the form as a diagnostic tool rather than a lead-capture mechanism, and the visitor has already invested effort by the time contact fields appear, lowering the perceived commitment of providing them.

### 2.4 Social proof
**Definition:** Evidence that other people, similar to the visitor, have already made this same decision and were satisfied. Function is to remove the feeling of being the first/only person taking this risk.

**Audit questions:**
- Are there reviews, testimonials, review-platform ratings (Trustpilot etc.), client counts, or awards visible near the point of decision, not just buried on a separate page?
- Does social proof appear on every page a visitor might actually land on (landing pages, category pages), not just the homepage?
- Do testimonials/reviews look genuine and recent, or dated/clustered in a way that raises doubt?

**Notes:** Site audits should check whether credibility/social proof carries through to deep-linked or paid-traffic landing pages. A page that ranks or receives ad traffic directly needs to work as a cold-traffic entry point, it can't rely on trust built on the homepage a visitor never saw.

### 2.5 Ease of use
**Definition:** Removing friction ("conversion blockers") between deciding to act and completing the action.

**Audit questions:**
- Is the primary CTA visible and reachable without hunting?
- How many form fields are required? Is each one actually used downstream, or just collected out of habit?
- How many free-text fields vs. multiple-choice/dropdown fields? (free-text fields measurably suppress completion more than selectable fields)
- Is anything asked for that the business won't actually act on (e.g., phone number with no one calling)?

**Convention over novelty:** visitors are distracted and unfamiliar with any given site, so unconventional navigation or interaction patterns (a menu icon that doesn't read as a menu, an unusual page structure) increase bounce even when they look impressive or win design awards. A site that uses familiar, expected patterns for its core conversion path will generally outperform a more original one, even a visually strong one. One cited case: replacing a highly original but hard-to-navigate site with a conventional layout took conversion from roughly 2.5% to over 9% in the first month, on the same underlying business and offer.

**Redundant CTAs:** repeating the same call to action in multiple formats and locations on a single page (header button, banner, sticky footer bar, phone number, chat widget) measurably helps rather than looks excessive, provided each one leads to the same clear action.

**Findability / menu structure:** worth checking separately from the on-page CTA audit — can a visitor tell what the business does and find the relevant service from the main navigation? Watch for: too many menu options (recall drops sharply past ~5-6 choices), alphabetically-ordered menus that bury high-value items under a late letter, and internal/jargon labels (e.g. a label that only makes sense to the business, not the visitor) used in navigation.

**Notes — form field data point worth keeping for the skill:**
- Conversion rate trends downward as field count rises from roughly 3 to 10.
- But going too low (1–2 fields, e.g. phone number only) can also hurt conversion: too little being asked signals "you're about to get a hard sales pitch" and lowers perceived value/legitimacy of the offer, especially for anything positioned as a consultation or diagnosis.
- Deliberately asking a *few more* qualifying questions can be a legitimate choice, not just friction, if it filters for serious leads and a business has limited capacity to service low-intent leads. This is a trade-off to flag, not a universal "fewer fields always wins" rule.
- Rule of thumb to carry into the audit: only ask for information that will actually be used (to contact, qualify, or disqualify). Don't ask for a phone number if no one calls back.

### 2.6 Result
**Definition:** What actually happens once someone converts — the concrete next step from becoming a lead to closing. This determines how well the lure and CTA can be positioned.

**Format options a business might use, each with different friction/scalability trade-offs:**
- Phone/video call — good early on for testing/refining an offer, gets live feedback, doesn't scale well.
- In-person meeting — highest friction, best suited to high-ticket sales, least scalable.
- Emailed deliverable (proposal, mockup, recorded review/presentation) — lowest perceived risk for the visitor since it puts the effort on the business and gives the visitor an easy, low-embarrassment way to disengage if uninterested.
- Free trial of the product — works well for low-setup products; risky for anything with a heavy onboarding burden, since visitors can be scared off by the perceived effort rather than the product itself.

**Audit questions:**
- Does the page tell the visitor clearly what happens immediately after conversion?
- Is the format of the "result" appropriate to the complexity/price point of what's being sold?
- If a trial or self-serve product is offered, has onboarding friction been addressed separately (this can undo an otherwise good conversion path)?

**Post-conversion experience (beyond the page itself):** conversion doesn't end at form submission. Two follow-up principles worth carrying into a full audit even though they sit past the website itself:
- The first message/call after someone converts should address their most likely fear directly and immediately (e.g. "this isn't a sales pitch" if the visitor's top fear is being pitched). The fear doesn't disappear just because they converted.
- If a business is offering something as the "result" (a report, review, consultation), it should be established early, reinforced with proof partway through, and referenced again at the end, rather than only be mentioned once. Otherwise recipients can fail to realize the business sells the related service at all.

---

## 3. Applied audit methodology (from the worked examples)

Pattern used consistently across the walked-through site examples:

1. Identify the actual entry page a visitor would land on (not always the homepage — check what ranks in search and what paid ads point to specifically).
2. Score each of the six elements independently rather than an overall gut reaction — a site can be strong on one or two elements and fail on others.
3. Note where a page performs well on *design* but poorly on *conversion mechanics*, and call that distinction out explicitly. Site spend and visual polish are not a proxy for conversion performance.
4. It's useful to check competitors' **paid search ads specifically**, since businesses spending real money on traffic tend to have tested and optimized that landing page more than an organic/homepage.
5. A single missing element (e.g., no stated result) can undercut an otherwise strong page — score holistically but call out the single weakest link as the priority fix.
6. **Interpretive caveat:** a rising lead-conversion rate on the website will naturally bring in a higher share of unqualified leads and can lower the *downstream* sales close rate. That's an expected trade-off, not a failure. Treat a near-100% sales close rate as a red flag that the funnel is too narrow (only self-qualified visitors are getting through), not as a sign of a healthy site.

---

## 4. Known gaps (to scope in step 2)

- No dedicated coverage of technical/performance CRO (page speed, mobile rendering, Core Web Vitals).
- No A/B testing or experimentation methodology (how to test a hypothesis derived from this framework, sample size, etc.).
- No quantitative benchmarks by industry/vertical beyond the single form-field dataset above.
- Only two fully worked example types in the sources so far: service/lead-gen (legal claims, solicitors) and a single e-commerce product page. No SaaS/subscription, marketplace, or B2B software examples.
- No guidance on prioritizing fixes when multiple elements are weak (effort vs. impact framing not covered).
