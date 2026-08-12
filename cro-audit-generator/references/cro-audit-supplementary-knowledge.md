# Supplementary CRO Audit Knowledge Base: Six Gap Domains

*Compiled August 2026. This document supplements a qualitative six-element persuasion framework (credibility, lure, objection handling, social proof, ease of use, result/next-step clarity) with technical, analytical, statistical, benchmark, business-model, and prioritization knowledge. Every data point is attributed to a named source with a date so it can be freshness-checked. **Note on source quality:** many CRO statistics circulate as un-sourced folklore; wherever possible, primary studies are cited, and single-agency or vendor-marketing figures are flagged as such.*

## TL;DR
- Page speed, Core Web Vitals, mobile friction, and baseline accessibility have documented, quantified links to conversion — a CRO audit that ignores the technical layer is incomplete; the best-sourced datapoint is Google/Deloitte's 2020 *Milliseconds Make Millions* finding that a 0.1s mobile speed gain was associated with an 8.4% lift in retail conversions.
- Before recommending anything, an auditor should pull funnel/drop-off data, session recordings/heatmaps, form-field analytics, and traffic-source segmentation; findings should be converted into falsifiable hypotheses and validated with properly-powered A/B tests (95% confidence, 80% power, pre-set MDE, no peeking).
- Conversion benchmarks vary enormously by industry, channel, and business model; the "lure" and "result" elements play out very differently for SaaS trials, subscriptions, marketplaces, and high-ticket B2B, and fixes should be sequenced with an established scoring framework (ICE, PIE, or PXL).

---

## 1. Technical / Performance CRO

### Page load speed and conversion
- **Google/Deloitte, *Milliseconds Make Millions* (2020)** is the most rigorous published study on speed-to-revenue. Deloitte Digital and Fifty-five, commissioned by Google, analyzed mobile-site data from 37 retail, travel, luxury and lead-gen brands across Europe and the US over four weeks (roughly 30 million sessions), at a 90% confidence level. A **0.1-second improvement** across four speed metrics was associated with: **+8.4% retail conversions and +9.2% retail average order value; +10.1% travel conversions; and an 8.3% improvement (lower bounce) on lead-generation informational pages.** Because the original metrics (First Meaningful Paint, Estimated Input Latency) predate Core Web Vitals, treat the mechanism as durable but the specific metric names as superseded by LCP/INP/CLS (per Deloitte Ireland's own page and web.dev's case study).
- **Portent, "Site Speed is (Still) Impacting Your Conversion Rate" (Michael Wiegand, April 20, 2022; ~100M+ page views across 20 B2B/B2C sites, page-speed sample from 5.6M sessions over 30 days):** each additional second of load time between 0 and 5 seconds reduces conversion by an average of **4.42%**, with e-commerce conversion falling from **3.05% at 1 second to 1.08% at 5 seconds.** A site loading in 1 second converted about **3× higher** than a 5-second site overall (and roughly **2.5× higher for e-commerce specifically**), and about **5× higher** than a 10-second site. Returns are front-loaded — gains flatten after the first few seconds.
- **Google/SOASTA (2017), machine-learning analysis of ~11 million mobile landing pages ("The Need for Mobile Speed" / Think with Google):** **53% of mobile visits are abandoned if a page takes longer than 3 seconds to load**; bounce probability rises **32%** as load time goes from 1s to 3s, **90%** from 1s to 5s, and **123%** from 1s to 10s. Widely cited and still directionally referenced in 2025-26, but note the 2017 vintage.
- **Amazon (internal, 2006, frequently re-cited):** engineers injected 100 milliseconds of artificial latency and found every 100ms of added latency cost ~1% in sales — "amounting to billions of dollars annually." Old and internal, but a useful order-of-magnitude anchor.
- **Cloudflare (learning center):** a two-second delay in page rendering is associated with roughly a 4% loss in revenue per visitor. Vendor educational content — use as illustrative, not primary.

### Core Web Vitals thresholds and conversion
- **Google's "good" thresholds (documented on web.dev, unchanged in their "good" cutoffs since the INP transition):** **LCP ≤ 2.5s** (loading), **INP ≤ 200ms** (responsiveness), **CLS ≤ 0.1** (visual stability). Each is measured at the **75th percentile of real Chrome user data (CrUX)** over a rolling 28-day window; a page "passes" only when ≥75% of real visits hit "good." "Poor" begins at LCP >4s, INP >500ms, CLS >0.25. Thresholds are identical on mobile and desktop, but mobile is harder to pass.
- **INP replaced FID as a Core Web Vital on March 12, 2024.** INP measures responsiveness across all interactions in a session (reporting close to the worst), not just the first — so many sites that passed on FID now fail INP. INP is currently the most commonly failed metric (roughly 43% of sites fail the 200ms threshold, per multiple 2025-26 secondary trackers citing CrUX/HTTP Archive).
- **Pass rates (HTTP Archive Web Almanac, 2024/2025 data):** only about **43-48% of mobile origins and 54-56% of desktop origins** pass all three Core Web Vitals. So merely passing puts a site ahead of roughly half the web on mobile.
- **CWV → conversion (not just SEO):** the cleanest causal evidence is A/B-test case studies where only performance differed. **Rakuten 24's Core Web Vitals A/B test (web.dev, 2023): the faster variant produced 53.37% more revenue per visitor.** Google's web.dev also documents Yelp, Vodafone and others with conversion/revenue uplifts tied to LCP/CLS improvements. Treat published case studies as real but self-selected (companies publish wins).

### Mobile-specific conversion friction (distinct from desktop)
- **Mobile converts materially lower than desktop despite carrying more traffic.** Dynamic Yield's benchmark panel (trailing-12-month, accessed 2026) shows desktop converting ~2.5-4% versus mobile ~2.5-3.5%; and cart abandonment is markedly higher on mobile (~80.45% mobile vs 68.62% desktop, Dynamic Yield XP2). Baymard also reports mobile cart abandonment around 77% vs ~70% desktop.
- **Baymard usability testing (2024)** documents mobile-specific drivers distinct from desktop: touchscreen form-entry friction (typing 16-digit card numbers has a high error rate, and each error adds time and abandonment risk); small tap targets causing mis-taps into the wrong field; limited screen real estate forcing many scrolls through a checkout form; and trust signals (badges, security seals, social proof) getting pushed below the fold on mobile layouts. Enabling mobile wallets (Apple Pay/Google Pay) measurably cuts mobile checkout time and abandonment.
- **Practical audit implication:** mobile and desktop conversion should always be segmented separately; a blended figure hides mobile-specific friction.

### Baseline accessibility issues that suppress conversion (CRO-relevant subset)
- **Tap target size — WCAG 2.5.8 Target Size (Minimum), Level AA (introduced in WCAG 2.2):** interactive targets should be **≥24×24 CSS pixels**, or have ≥24px spacing. Apple Human Interface Guidelines and Android Material Design recommend a larger ~44×44px, which aligns with WCAG 2.5.5 (Level AAA). Undersized targets cause mis-taps, errors, and abandonment, especially on mobile checkout. (Sources: W3C WCAG 2.2; corroborated by Siteimprove, Smashing Magazine.)
- **The European Accessibility Act came into force June 28, 2025**, and Section 508/EN 301 549 reference WCAG 2.2 AA — so accessibility is increasingly a legal as well as a conversion issue.
- **Form labels:** missing/implicit labels break screen readers and also hurt everyone (autofill fails, unclear fields), raising form abandonment. Persistent visible labels (not placeholder-only) are a documented usability best practice (Nielsen Norman Group / Baymard checkout research).
- **Contrast:** WCAG AA requires a **4.5:1** contrast ratio for normal text (3:1 for large text). Low-contrast CTAs and body copy reduce legibility and, for CTAs specifically, reduce the likelihood the primary action is seen and clicked.
- **Framing for the Skill:** this is *not* a full WCAG audit — the CRO-relevant subset is tap-target size/spacing, real form labels + inline error clarity, and text/CTA contrast, because each has a direct, measurable path to completed conversions.

---

## 2. Analytics & Data Requirements (what to request before recommending)

A CRO auditor should insist on four layers of data before making recommendations; recommendations made without them are guesses.

### a) Funnel / drop-off analysis
- Map the real conversion journey as discrete steps and measure completion and drop-off between each. In **GA4, Funnel Exploration** supports up to 10 steps and distinguishes **closed funnels** (users must enter at step 1 — cleaner for diagnosing a linear checkout) from **open funnels** (entry at any step — better for messy, non-linear journeys). Enable "elapsed time" between steps: a long gap can signal hesitation/friction.
- **Method:** find the single biggest percentage drop between two adjacent steps and start there. Example benchmark reference: e-commerce add-to-cart → begin-checkout below ~25% is a red flag. Then **segment** the funnel (device, source/medium, new vs returning) — a blended funnel hides device- or channel-specific leaks.
- **Data-quality caveat:** verify each event fires exactly once with correct value/currency parameters (GA4 DebugView) before trusting the numbers.

### b) Session recordings & heatmaps
- **What they reveal that analytics cannot:** the *why* behind drop-off — rage clicks, repeated form retries, rapid backtracking, stalled decision moments, scroll depth, and whether users even see the CTA or form. Heatmaps (click, scroll, move) show aggregate attention; session replays show individual journeys.
- **Common tools (2025-26):** **Microsoft Clarity** (free, common starting point), **Hotjar** (heatmaps + recordings + a dedicated Form Analysis report), **Lucky Orange**, **FullStory** and **Contentsquare** (enterprise/digital-experience), and **PostHog** (developer/usage-priced). Segment heatmaps by device, traffic source, and landing-page variant or the aggregate becomes a blurred average.
- **Privacy:** session capture must respect GDPR/consent and mask PII.

### c) Form-field-level abandonment analytics
- Standard web analytics rarely captures field-level interaction, so this deserves its own tool layer. Form analytics measure **which field causes the most abandonment, time spent per field, which fields trigger corrections/re-entries, and where users give up entirely.**
- **Tools:** **Zuko** (dedicated form/checkout analytics, benchmarks against industry averages, attention heatmaps), **Hotjar Form Analysis**, **Lucky Orange**, **FullStory**.
- **Why it matters:** a real audit example — session recordings + form analytics showed users stalling at "phone number" and "budget" fields; cutting an 11-field form to 4 (first name, work email, company, team size) reportedly lifted demo requests 38% in 30 days. Note some form drop-off is structural (not everyone who sees a form intends to complete it), so calibrate against benchmarks before declaring a form "broken."

### d) Traffic-source segmentation (why it matters for interpreting CR)
- **A single blended conversion rate is nearly meaningless** because channels carry different intent. **Branded vs non-branded search is the clearest example: branded search typically converts 2-3× (some sources 2-5×) higher than non-branded**, because the visitor has already chosen you. A conversion-rate "improvement" can simply be a shift in traffic mix (more branded/direct), not a site improvement — and vice-versa.
- **Segment at minimum by:** channel (organic, paid search, paid social, organic social, email, direct, referral), device, new vs returning, geo, and campaign. Email and direct usually show high conversion (existing relationship/intent); cold social usually lowest.
- **Practical:** GA4 default channel groupings + branded/non-branded segmentation (Google Search Console now offers a native branded/non-branded split); SEO platforms (Ahrefs, Semrush) estimate branded vs non-branded automatically.

---

## 3. A/B Testing & Validation Methodology

### Turning an audit finding into a testable hypothesis
- The industry-standard structure is a **"Because we observed X, we believe changing Y for Z audience will result in [measurable outcome]; we will measure it by [metric]"** statement. Variants of this appear across CRO practitioners (Lead Forensics, Kraken Data, Mouseflow, Omniconvert).
- A strong hypothesis has, per multiple practitioner sources, **five components: Observation (data/insight that triggered it), Change (specific modification), Mechanism (why it should work), Prediction (which metric moves and direction), and Falsifiability (what result disproves it).** Weak: "a new checkout design will increase conversion." Strong: "Because our funnel shows the largest drop at the shipping-address step, we will test whether removing redundant fields improves checkout completion." Grounding each element in real data (recordings/heatmaps/analytics) is what separates a hypothesis from a guess and is what raises "confidence" in prioritization scoring.

### Statistical concepts a non-statistician needs (to avoid false positives)
- **Four inputs determine required sample size:** baseline conversion rate; **Minimum Detectable Effect (MDE)** — the smallest lift you want to reliably detect; **confidence level** (typically 95%, i.e. α=0.05); and **statistical power** (typically 80%, i.e. the chance of detecting a real effect). (Sources: Kissmetrics, Convert.com, AB Tasty, StatsTest.)
- **Sample size scales with the square of precision:** detecting half the effect needs ~4× the sample. Kissmetrics' worked example: detecting a 20% relative lift on a 5% baseline needs ~15,000 visitors per variation; detecting a 5% relative lift needs ~240,000 per variation. So small effects on low-traffic sites are often untestable — better to test for a bigger effect than run an invalid test.
- **Choosing MDE:** Convert.com's rule of thumb — aim ~5%, down to 1-2% for very high-traffic sites, up to 10% for low-traffic; target the lowest MDE you can hit within a **2-8 week** window.
- **Test duration:** run **full weeks** (ideally 2-4, up to 8) to capture weekly cycles; never stop the instant significance appears.
- **The #1 pitfall — peeking/early stopping:** repeatedly checking a test and stopping the moment p<0.05 dramatically inflates false-positive rate (Kissmetrics: can push the ~5% false-positive rate as high as ~30%). Significance "bounces around" early in a test; a day-3 winner is often noise. Fixes: pre-commit to sample size and duration, or use methods designed for continuous monitoring (sequential/"always-valid" or Bayesian engines).
- **Other pitfalls:** confusing statistical with practical significance (a "significant" 0.1% lift may not be worth shipping); running underpowered tests and treating a null result as "no effect"; setting an impossibly low MDE that never concludes.

### Commonly used A/B testing tools/platforms (2025-2026)
- **Market context:** Google Optimize shut down September 30, 2023, with no direct replacement — pushing users to third-party tools (VWO absorbed many). Consolidation is ongoing: VWO and AB Tasty announced a merger (early 2026); OpenAI acquired Statsig; Webflow absorbed Intellimize.
- **Client-side / marketing-led (visual editors):** **VWO** (bundles testing + heatmaps + recordings + surveys, Bayesian "SmartStats"), **Optimizely** (enterprise DXP; sequential "always-valid" Stats Engine designed to be peeking-safe), **AB Tasty**, **Convert** (privacy-focused).
- **Server-side / warehouse-native / developer-led:** **GrowthBook** and **Statsig** (open-source options), **Eppo**, **Kameleoon**, **LaunchDarkly** and **Split.io** (feature-flag platforms).
- **Selection guidance:** marketing teams that want no-code + behavior analytics → VWO/Optimizely/AB Tasty; engineering-led/product teams → GrowthBook/Statsig/LaunchDarkly; the discipline applied matters far more than the tool.

---

## 4. Industry Benchmark Data (with study + year)

**General caveat:** benchmarks go stale and definitions differ (visitor-to-lead vs landing-page-arrival vs add-to-cart), so always cite the study, year, and what's being counted.

### By business model / overall
- **"Most sites convert 1-4%."** E-commerce global average is often cited around **1.8-3%** (Dynamic Yield: ~3%+ Americas/EMEA, ~2.5% APAC, 2025). SaaS/B2B lead-gen commonly **2-5%**.
- **Landing pages (Unbounce Conversion Benchmark Report):** median around **6.6%** across industries, top performers above 11%. Note the widely-cited industry table dates to 2021 and hasn't been fully refreshed.

### By industry vertical (e-commerce)
- **Triple Whale 2025 Ad Performance Benchmarks (33,000+ brands, $18.4B ad spend):** conversion rates range from ~0.9% (Luxury & Jewelry) to ~6.22% (Food & Beverage); top converters Food & Beverage 2.73%, Health & Beauty 2.49%, Pet Supplies 2.45% (need-based/replenishment categories convert highest).
- **Dynamic Yield benchmark panel (trailing-12-mo, 2026):** Food & Beverage leads verticals at ~6.22%.

### By traffic channel (paid search)
- **WordStream 2025 Google Ads Benchmarks (16,000+ campaigns, Apr 2024-Mar 2025):** average Google Ads conversion rate across all industries **7.52%** (up 6.84% YoY). Best verticals: Automotive Repair/Services/Parts 14.67%, Animals & Pets 13.07%, Physicians & Surgeons 11.62%. Lowest: Finance & Insurance 2.55%, Furniture 2.73%, Real Estate 3.28%. Paid search converts higher than organic because searchers are further down-funnel.

### By traffic channel (full-funnel visitor-to-lead) — First Page Sage
- **First Page Sage, "Conversion Rate Benchmarks" (updated Dec 2025) and "Conversion Rate by Channel" (updated Mar 28, 2025):** GA-traffic-source conversion (B2C / B2B): **SEO/Organic 2.1% / 2.6%; Paid search (PPC/SEM) 1.2% / 1.5%; Email 2.8% / 2.4%; Direct 1.6% / 1.9%; Referral 1.8% / 1.1%; Organic Social 2.4% / 1.7%; Paid Social 2.1% / 0.9%; Display 0.7% / 0.3%; ABM (B2B) 3.8%.**
- **Critical caveat:** all First Page Sage figures are **one agency's B2B-skewed client data** (~70% B2B), measuring full-channel visitor-to-lead conversion — NOT a broad independent multi-company study, and NOT landing-page-moment conversion (which is why they're far lower than Unbounce's ~19% for email at the landing-page moment). Ruler Analytics (2026) reports a broader multi-company mean of ~5.13% counting forms, calls and chat — a different, higher figure due to a broader conversion definition.

### By industry vertical (B2B lead-gen) — First Page Sage
- **First Page Sage, "B2B Conversion Rates by Industry – 2026" (updated Sept 18, 2025; data Jan 2022-Aug 2025, 25 industries, visitor-to-lead/MQL):** Legal Services 7.4% (highest); HVAC 3.1%; Staffing & Recruiting 2.9%; Higher Education 2.8%; Real Estate 2.7%; Manufacturing 2.2%; Financial Services 1.9%; IT & Managed Services 1.5%; **B2B SaaS / Software Development 1.1% (lowest)**; median across industries ~1.9%. Same single-agency caveat applies.

### Cart abandonment (e-commerce)
- **Baymard Institute:** the canonical average cart abandonment rate is **~70.19%**, aggregated from ~49 studies (2006-2023), remarkably stable near 70% for over a decade. Of shoppers who abandon **for a fixable reason** (excluding "just browsing"), Baymard's Feb 2024 survey (n≈4,329-4,560 US adults): **extra costs (shipping/tax/fees) ~39-48% (top reason); mandatory account creation ~19-24%; checkout too long/complicated ~18%.** Baymard estimates best-practice checkout design can lift conversion ~**35.26%**.

### SaaS free-trial conversion (see also §5)
- **First Page Sage (2025, 86 SaaS companies, 71% B2B):** organic free-trial-to-paid ~**18.2% opt-in / 48.8% opt-out (credit-card)**. **ChartMogul/ProductLed 2026 (200 products):** ~**8.9% opt-in / 31.4% opt-out.** Freemium free-to-paid typically **2-5%**. Median B2B trial-to-paid ~**18.5%**, top quartile 35-45% (Baremetrics/Flint). Wide variance underscores: always name the study and trial model.

---

## 5. Additional Business-Model Coverage — how "Lure" and "Result" differ

The framework's **"lure"** (the offer/hook that pulls a visitor in) and **"result"** (the conversion outcome / next step) behave very differently across these models than for a simple contact form or product purchase.

### SaaS / free-trial signup flows
- **Lure:** the offer is a *trial of the product itself*, and the biggest lever is the **friction of the offer**: opt-in (no card) maximizes signups but converts low (~8-18% to paid); opt-out (card required) suppresses signup volume ~60-70% but converts far higher (~31-60%) and yields ~3× more paying customers per visitor. Freemium is the lowest-friction lure (highest visitor-to-signup, ~13-16%) but lowest free-to-paid (~2-5%).
- **Result:** the "conversion" is **not** the signup — it's reaching the **"aha"/activation moment** and then the paid upgrade. The critical structural difference vs. lead-gen/e-commerce is that success depends on **time-to-value inside the product**, not just the landing page. Data point: 7-day trials often out-convert 14/30-day trials (24% vs 19% vs 14% median, Flint), because urgency + fast activation matter more than length; most conversions happen around trial expiry, and after ~day 14 conversion drops toward ~1%. So SaaS CRO must optimize onboarding, not just the signup page.

### Subscription / recurring-billing businesses
- **Lure:** the offer is an ongoing relationship, so the hook centers on **plan structure and billing terms**. Monthly billing lowers the entry barrier (Baremetrics: monthly can lift initial conversion ~50%; Recurly: B2B monthly plans converted ~8.9% higher than annual); annual billing raises commitment, LTV and cuts churn (Recurly 2025: annual plans generate ~50-60% more revenue per user; annual retention 44.1% vs 17.5% monthly at 12 months, per RevenueCat).
- **Result:** the meaningful outcome is **not the first signup but retained recurring revenue** — so the "conversion" event is multi-stage (trial → first payment → renewal). Best practice: show both monthly and annual with annual pre-selected and savings shown in dollar terms; measure conversion value by lifecycle stage, not a single event. Structurally different from a one-time purchase because churn and renewal are part of the conversion definition.

### Two-sided marketplaces
- **Lure & Result are doubled:** every metric has a buyer version and a seller version, and value only exists when both sides are present. The core structural difference is **liquidity** — the probability that a buyer finds what they need and a seller gets a transaction (Sharetribe, Stripe, Point Nine). Buyer liquidity = search-to-fill rate; supplier liquidity = utilization rate.
- **Optimization implications:** single-variable optimization is often counterproductive (cutting seller fees may help seller retention but wreck unit economics). You must **balance supply and demand at a granular level** (geography/category/time), usually **seed supply first**, and start in a **narrow wedge** (one city/category) to reach density before expanding. Track match-quality metrics (search-to-booking rate, response time, listings per search) not just user counts. Trust infrastructure (verification, ratings, dispute resolution, payment protection) is a retention/conversion lever unique to marketplaces, and disintermediation risk (parties taking the relationship off-platform) is a structural threat.

### High-ticket B2B sales with long consideration cycles
- **Lure:** a single "buy/contact" CTA is too big an ask; the hook is a **graduated series of micro-conversions / lead magnets** matched to funnel stage (checklist/quiz top-of-funnel; webinar/ROI calculator mid-funnel; case study/demo bottom-of-funnel). **Demand Gen Report's 2024 Content Preferences Benchmark Survey:** a plurality of B2B buyers consume **3-5 pieces of content before engaging with a salesperson**, and **3 in 10 consume more than 5**; no single asset closes a deal — each earns the next step. Content investment should scale to deal size (custom research for $200K deals vs templates for $5K).
- **Result:** the on-site "conversion" (a demo request or download) is only the *start* of a weeks-to-months, multi-stakeholder (3-7 decision-makers) process; the real outcome is pipeline/closed revenue. Structurally, CRO must optimize for **lead quality over volume**, use progressive profiling, and align to MQL→SQL→opportunity stages — very different from an e-commerce checkout where the conversion and the revenue are the same instant. Healthcare/MedTech and enterprise (ACV >$25-50K) especially require 90-day+ nurture, not top-of-funnel volume.

---

## 6. Prioritization Frameworks

When an audit surfaces many issues (a typical conversion project finds "15-30 pages full of issues," per CXL), a scoring framework replaces "whoever argues loudest" with a consistent triage.

### ICE (Impact, Confidence, Ease)
- Created by **Sean Ellis** (GrowthHackers; used at Dropbox/LogMeIn). Score each factor 1-10; **ICE = (Impact + Confidence + Ease) / 3** (some practitioners multiply). A score ≥7 is generally "high priority." Fast and simple — best for early/high-velocity teams running few tests. Weakness: only three broad factors, highly subjective, and no built-in accounting for how many visitors a test reaches (a niche page and the homepage can look equal). "Confidence" is where research-backing (heatmaps, recordings, user testing) enters the score.

### PIE (Potential, Importance, Ease)
- Developed by **Chris Goward / WiderFunnel**, specifically for CRO. Score 1-10 on **Potential** (how much room to improve — low-performing pages score higher), **Importance** (how much qualified traffic/revenue the page carries), and **Ease** (implementation difficulty); average the three. Its distinctive strength is **Importance**, which forces weighting by page value — it stops teams testing clever ideas on pages nobody visits, and it forces you to pull actual traffic/conversion data before scoring. Many programs use **PIE to choose which pages to test, then ICE to prioritize individual tests within a page.**

### PXL (CXL / ConversionXL)
- Developed by **Peep Laja and the CXL team** to reduce ICE/PIE subjectivity. Instead of 1-10 gut scores, PXL asks ~10 concrete, mostly **binary yes/no questions** scored as points — e.g., is the change above the fold? noticeable within 5 seconds? on a high-traffic page? does it add/remove an element? is it backed by user testing / qualitative feedback / heatmaps / analytics? The more objective, research-backed the idea, the higher it scores; it "rewards the right behavior." Documented weakness (Conversion.com, GoodUI): it accounts for page *traffic* but not page *value* (a high-traffic zero-revenue page can outrank a high-value one), and its binary traffic flag is crude.

### Other named frameworks
- **RICE** (Reach, Impact, Confidence, Effort — Intercom): adds Reach, better when audience sizes vary widely. **BRASS, HIPE, DICET** exist but are less common in CRO. CXL also publishes a broader **ResearchXL** model for finding/bucketing issues (into "just do it," "test," "hypothesize," "instrument"), then using PXL to prioritize the test hypotheses.

### How practitioners typically sequence fixes
1. **Just-do-it fixes first:** obvious, low-effort, low-risk bugs and best-practice violations (broken forms, missing labels, slow LCP, undersized tap targets) don't need a test — ship them.
2. **High-impact × low-effort next** (the classic effort-vs-impact quadrant): prioritize quick wins on high-traffic/high-value pages (checkout, pricing, primary lead form).
3. **Score the remaining test backlog** with ICE/PIE/PXL, rank, and run highest-scored tests that are *feasible given traffic* (a top-ranked test you can't power is not runnable — the caveat most frameworks omit).
4. **Re-score periodically** as you learn; feed test results back into "Confidence." Sequence fixes down the funnel where the money is (checkout/pricing before a blog CTA), and weight issues affecting a large share of visitors.

---

## Recommendations (how to wire this into the Claude Skill)

1. **Make the technical layer a mandatory first pass, not optional.** Have the Skill request/estimate LCP, INP, CLS (at the 75th percentile, mobile and desktop separately) and mobile vs desktop conversion before scoring persuasion elements. Threshold that changes the recommendation: if any Core Web Vital is "poor" (LCP >4s, INP >500ms, CLS >0.25) or mobile CR is <50% of desktop CR, elevate a technical fix to the top of the backlog ahead of copy/persuasion tweaks.
2. **Gate persuasion recommendations on data availability.** Before the Skill asserts "add social proof" or "reduce objections," have it ask whether funnel/heatmap/form/segmentation data exists. If not, its output should be framed as *hypotheses to validate*, not conclusions — this is the single biggest guardrail against confident-but-wrong advice.
3. **Force every finding into the five-part hypothesis template** (Observation → Change → Mechanism → Prediction → Falsifiability) and attach an ICE or PIE score plus a **feasibility check**: compute rough required sample size from the site's baseline CR and traffic, and flag any recommendation that can't reach significance within 8 weeks as "ship-without-testing" or "not testable at this traffic."
4. **Always contextualize a conversion rate against the right benchmark.** Have the Skill pick the benchmark by business model + channel + device (e.g., compare a B2B SaaS lead form to First Page Sage's ~1.1%, not to WordStream's 7.52% paid-search figure), and state the source and year inline.
5. **Branch the "lure/result" analysis by business model** using §5: detect SaaS-trial vs subscription vs marketplace vs high-ticket-B2B vs simple e-commerce/lead-gen, and apply the model-specific definition of "conversion" (activation, retained MRR, liquidity/match, or pipeline) rather than a single generic "conversion."
6. **Sequence output as: (a) just-do-it technical/accessibility fixes, (b) high-impact/low-effort quick wins on money pages, (c) scored test backlog.** Re-run the benchmark and threshold checks quarterly.

---

## Caveats

- **Correlation vs causation:** most speed/CWV "impact on conversion" figures (Deloitte, Portent, SOASTA) are correlational or from self-selected published case studies; the cleanest causal evidence is controlled A/B tests like Rakuten 24. Present them as strong associations, not guarantees, and expect diminishing returns past the first few seconds.
- **Benchmark heterogeneity:** figures across sources disagree because they measure different things (landing-page-arrival CR vs full-funnel visitor-to-lead vs add-to-cart) and use different populations. First Page Sage's channel/industry numbers are **one agency's B2B-skewed client data**, not an independent multi-company study; WordStream is paid-search only; Unbounce's industry table is largely 2021. Never mix them without noting the definition.
- **Vintage flags:** Google/SOASTA 3-second stat is 2017 (11M mobile pages); Amazon 100ms stat is 2006; *Milliseconds Make Millions* is 2020 (mechanism durable, but its metric names — FMP, EIL — are superseded by LCP/INP/CLS).
- **Freshness cadence:** re-verify all §4 benchmarks and §5 trial figures **annually** (WordStream refreshes yearly, First Page Sage several times a year, ChartMogul/ProductLed annually). Core Web Vitals "good" thresholds have been stable since the INP launch (March 12, 2024) but watch for a possible new Core Web Vital and for WCAG 3.0 developments. Baymard's ~70% cart figure is stable, but its reason-percentages come from dated annual surveys.
- **Vendor bias:** "best A/B testing tools" and "best CRO software" lists are frequently vendor marketing (the ranking site often ranks itself #1). Treat tool lists as a market map, not an endorsement; the market is actively consolidating (Optimize dead, VWO+AB Tasty merging, Statsig→OpenAI), so re-check tool status before recommending.
- **Statistics are the guardrail, not decoration:** the most common way CRO recommendations produce false wins is peeking/early stopping and underpowered tests. Any Skill that recommends "test this" must also enforce pre-set sample size, full-week durations, and a defined MDE, or it will manufacture false positives.
