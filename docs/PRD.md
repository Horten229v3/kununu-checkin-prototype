# PRD — The kununu Check-in

**A recurring, low-friction self-comparison loop that grows review volume while trust, representativeness and usefulness rise with it.**

| | |
|---|---|
| **Author** | Eugene Borisenko |
| **Status** | Draft — validation phase (pre-engineering) |
| **Audience** | Shared context for Design and Engineering |
| **Companion artifact** | Interactive prototype (see Appendix A) + Claude Design task |
| **Case reference** | kununu Product Manager B2C — "B2C Engagement & User Growth" |

> **How to read this document.** This is a *product* PRD, but the product is not yet greenlit. The header outcome is the destination; the near-term deliverable is a **go/no-go decision backed by evidence**, not a shipped feature. The prototype in Appendix A is a research instrument that lives inside the validation phase — it is not v1. Everything here is written to keep those two things from being confused.

---

## 1. Problem statement

kununu's value to a job seeker depends on the review corpus being *representative*, not merely large. But writing behaviour is asymmetric: unhappy employees self-select into reviewing (loss aversion pulls them in), while the satisfied majority stays silent — and negative entries also travel further once written, so a reader is most likely to land on the loudest voice. Left alone, the corpus tilts negative and the platform's core currency, **trust**, quietly erodes.

**So the problem is not "too few reviews." It is a skewed sample.** The target is a *better-sampled* corpus, achieved by activating the people who currently stay silent — not by extracting more from the people who already speak.

*Evidence status:* the negative-skew premise is treated as load-bearing but **unvalidated against kununu's own data**. It is the first thing the validation plan tests (§8). Supporting-but-not-proof: the depth-skew is measured externally (Höllig et al., ~298k kununu reviews — negative reviews are more elaborated); the attention-skew is observed in consumer-review feeds, not on kununu.

---

## 2. Goals

Outcomes, not outputs. The whole point is that these move **together**; any solution that buys one by spending another is rejected.

1. **Volume** — more, and *fresher*, contributions per company per month (recency matters: a stale review of a since-reorganised company is worth little).
2. **Representativeness** — shift the contributor mix toward the satisfied, still-employed majority, so the corpus reflects the base rate, not just the tail.
3. **Trust** — grow it, not merely protect it: verification, aggregation, and structural anonymity make the signal *more* credible as volume rises.
4. **Usefulness** — a reader can slice signal by role, level, department and tenure and judge whether a given complaint is the rule or the exception.

**Non-goal masquerading as a goal (explicitly rejected):** raw submission count. Volume without trust is worthless; a funnel-optimised corpus flooded with low-effort or AI-generated entries has negative value.

---

## 3. Non-goals

As important as the goals — these prevent the idea (and the prototype) from drifting.

- **Not shortening the existing review form as the primary lever.** It buys volume and spends trust: same skew, added quality risk. Deferred, not chosen.
- **Not gamification** — no leaderboards, points, or badges. You honestly review only a handful of employers in a career, so a volume leaderboard invites gaming and rewards the wrong behaviour. Every reward in this design is *personal insight*, never status.
- **Not a give-to-get access wall.** Coercive, and it produces low-effort reviews written only to pass the gate. Deferred.
- **Not a standalone engagement-survey product.** kununu already tried that (kununu engage, ~2018) into a crowded category with negligible footprint. This is a carrot *bundled to drive public contribution* — different aim, different buyer, different place in the funnel.
- **Not depth from the satisfied majority.** They will not write essays. Take *calibration* from them; leave *depth* to the motivated minority as a separate layer.
- **Not scraping LinkedIn or any external surveillance** to detect the exit moment. Breaks terms, no lawful GDPR basis, re-identifies anonymous users — and is exactly the posture the platform sells against.
- **(For the prototype specifically) Not a polished, shippable app.** See Appendix A. Restraint is a requirement, not a limitation.

---

## 4. Users

kununu is a **three-sided market**, and trust is the single asset all three transact on.

| Side | Supplies | Gets | Tension |
|---|---|---|---|
| **Reviewers** (employees) | the honest signal | to be heard; to learn where they stand | — |
| **Job seekers** (readers) | nothing | insight an interview can't give — *if they trust it* | the reason the corpus has value |
| **Companies** (payers) | employer branding spend | a reputation on the line; a workforce temperature read | strongest motive to tilt public content → trust must be defended *structurally* |

**Contributor segments** (the segmentation the whole design turns on):

- **Silent satisfied** — the reach problem. Value is *calibration* (the base rate), not depth. The only segment that needs the expensive distribution lever.
- **Disgruntled minority** — arrive on their own; high depth, low volume. Their reviews are often the most useful thing on the page. Not to be silenced — to be *contextualised*.
- **Job-seeking** — arrive via search; readers who can be flipped to writers (they land on their own employer).
- **Leavers** — the fullest, least-fearful, best-informed picture. A second, high-value capture moment (§6).

---

## 5. The product

### 5.1 The spine
A structured, low-friction self-comparison loop: satisfied and unsatisfied alike contribute light, calibrating signal in order to see **where they stand** — by role, level, industry, and (above an anonymity floor) their own company. The **aggregate, never the individual**, becomes the public trend a reader can slice. Deep prose reviews remain a separate layer from the motivated minority.

> Volume from the light loop · Trust from calibration, aggregation and verification · Relevance from sliceable structured signal · Depth from the motivated minority.

### 5.2 The check-in ladder
kununu owns the cadence and periodically nudges current employees (app or email) to answer a couple of light, structured questions. Each step is **optional** and rewards with **personal insight**, not status:

- **Step 1 — the rating.** One quick scale. Earns the first comparison; costs seconds. Solves cold start because the reward lands on contribution #1, no history needed.
- **Step 2 — the optional "why".** A sentence behind the rating unlocks *others' reasons*, not just their numbers. Self-selecting depth: only people with something to say add it, so you get elaboration without a mandatory field's low-effort text.
- **Step 3 — more dimensions.** Rate management, growth, workload; the personal profile sharpens.
- **Step 4 — the dormant asset.** "Companies that share what you value / how your exact role and level feel elsewhere" — framed as a profile building quietly, ready *if* you ever need it. Bankable now, valuable later.

**Why the ladder beats a one-shot:** it rewards sustained, repeated contribution, which structurally favours the satisfied, still-employed majority — the people around long enough to climb it. The angry reviewer is a one-shot; the ladder selects *for* the balanced voices the corpus is missing. It works against the skew by design, not just by adding volume.

### 5.3 Personalisation & segmentation (the brief's explicit ask)
Personalisation is what makes a *light* contribution useful, and it does two different jobs:

- **For the reader — relevance.** Filter and rank so a job-seeking PM sees PM signal at their level and department. Improves *consumption*, not contribution.
- **For the contributor — participation & quality.** This is where the lever actually is:
  - *Segmented triggering* — new hire, one-year, settled long-tenured, and leaver each warrant a different prompt at a different moment. Using the segment to pick the moment is what lifts participation.
  - *Segmented reward* — "where do I stand" only motivates because it is personalised to *your* role, level, industry. A generic company average pulls no one.
  - *Segmented questions* — what's worth asking a PM differs from a warehouse shift lead. Raises quality, lowers friction.

**The connective claim:** a light rating tagged with role, level, tenure and department becomes *sliceable* signal — which is how you get quality without demanding depth.

### 5.4 Second capture moment — the exit
The most balanced contributor is often someone who just left (fullest picture, least fear, no worry about a current manager). Captured **by consent, never by surveillance**:

- **Exit-lapse capture (cheap, first).** When a check-in lands on someone who quietly left, the *"I don't work here anymore"* tap becomes the exit signal — doubles as data hygiene, and opens the best review-ask there is: "now that you've moved on, how was it?"
- **HR-offboarding integration (scaled, later).** Employer system fires a neutral, kununu-owned prompt on the real exit date, firewalled like the check-in.
- **Verification flips at exit.** No more company email or employer invite; their history establishes they were there, so they're routed as **verified former-employee signal, kept separate from current-employee signal**.

---

## 6. Trust guardrails (hard constraints, not features)

Trust is a **veto**, not a tradeable RICE factor. Anything that risks it is deprioritised regardless of reach or effort.

- **kununu owns the cadence** (randomised or lifecycle-fixed) — never employer-discretionary, so prompts can't be timed to fire after good news.
- **Aggregate-only publication.** The individual profile stays private. This is simultaneously the privacy fix *and* the trust fix.
- **N-floor on colleague comparison.** No within-company/team comparison below **N = 5** respondents; benchmark against role and industry until the pool is dense. *State the number in-product.*
- **Verification binds to the review, not the person.** The employer invite/work email proves "works here" for that one review, attaches a verified flag, and is discarded as an anchor — it must not become the login or link reviews under an employer-visible key. (German rulings can compel disclosure of an author's real name; this is the landmine this closes.)
- **Reward attaches to helpfulness-to-a-decision, not raw engagement** — or the reward re-amplifies dramatic negative reviews and fights the de-skewing thesis.
- **The employer lends the channel but never touches content, timing, or who-gets-asked.**

---

## 7. Requirements

Scoped for the **validation phase**. The "product" columns describe the eventual build so Design/Eng don't make choices that foreclose it — but only P0 is in the current cycle.

**P0 — must exist to validate the core question**
- [ ] A working, front-end-only **check-in loop** a real employee can complete in ~20 seconds: nudge → rate → (optional why) → personalised "where you stand" → aggregate-only confirmation.
- [ ] The "where you stand" payoff must be **reactive** to the user's own input (percentile vs a role/industry baseline), not a static screenshot — desirability rides on this feeling real.
- [ ] **N-floor made visible**: colleague comparison shows as *locked with its reason stated* ("unlocks once 5+ people at your company respond, so no single answer can be traced to you").
- [ ] **Segmentation made visible**: at least the prompt and the comparison change by contributor segment.
- [ ] **Aggregate-only** publication reassurance on the final screen.

**P1 — strengthens the argument if time allows**
- [ ] **Reader/job-seeker screen** — company culture score sliced by role and level, proving usefulness ("quality, not just volume").
- [ ] **Exit tap** — "I don't work here anymore" routes to former-employee framing; verification-flip shown.
- [ ] **Instrumentation overlay** — per-screen assumption / metric / guardrail, to make the *method* legible.
- [ ] A **fake-door A/B** of two nudge copies (illustrative).

**P2 — deliberately deferred (design shouldn't block them, shouldn't build them)**
- The recurring-cadence engine · aggregation/benchmarking pipeline at scale · identity-vs-verification architecture · HRIS/employer integration · full multi-question instrument · multi-market/multi-language rollout · colleague comparison in production (needs density + real privacy engineering).

**Acceptance criteria (P0 loop)**
- Given a user on the nudge screen, when they start and choose a rating, then they reach a "where you stand" screen whose percentiles reflect *that* rating.
- Given the colleague-comparison card below the N-floor, when the user views it, then it is visibly locked *and states the five-person reason*.
- Given a different segment is selected, when the user runs the loop, then the prompt and/or comparison differ.
- Given the user finishes, then they see aggregate-only confirmation and never see their individual answer published.
- The loop has **no dead ends** and can be **reset to first screen** instantly.

---

## 8. Validation plan — the escalator

The organising logic is **riskiest-assumption-first**: rank by what could kill the idea, test the cheapest killers first, fake the expensive-to-build parts, never go dark. Engineers enter *last*, on purpose — so that when they build, they build something already shown to be wanted.

**Load-bearing assumptions (tested first):**
1. Trust is the currency and the goal is to grow it (not raw volume).
2. The corpus is negatively skewed by self-selection — *tested against kununu's own data.*
3. A reachable, silent, satisfied population exists.
4. Employers will cooperate on firewalled terms.
5. "Where do I stand" pulls a first contribution.
6. A meaningful share climbs past step 1.
7. Enough people volunteer a "why" to produce depth.

**The first 4–6 weeks buy down risk; they do not ship product.**

| Weeks | Focus | Output |
|---|---|---|
| 1–2 | Pull kununu's own data; lock north star + guardrails | Does the skew premise hold on real numbers? |
| 2–4 | Qualitative research with real current employees | Does "where do I stand" genuinely pull a first contribution? |
| 4–6 | Fake-doors + Wizard-of-Oz cohort | Behavioural signal on intent and the full value loop, no backend built |
| 1–6 (parallel) | Gate conversations — Data, Trust & Safety, Privacy/DPO, B2B PMs, CS | Clear the idea or surface a blocker while it's cheap to change course |

**Green-light bar at week 6:** premise holds · employees want the comparison · no gatekeeper sees a blocker.

**What needs engineering vs. not:**
- *No engineering:* fake-door for intent; Wizard-of-Oz loop (send check-in via existing email/form tool, compute each person's comparison by hand in a spreadsheet, send it back); a throwaway clickable prototype (Appendix A); interviews, concept tests, existing-data analysis.
- *Needs engineering:* cadence engine, aggregation/benchmarking pipeline, identity-vs-verification architecture, employer/HRIS integration.
- *Deliberately deferred:* colleague comparison, HRIS import, full instrument, multi-market rollout.

**Who's involved, in order** (talk first to the people who could kill it): Gate 1 Data → Gate 2 Trust&Safety + Privacy/DPO → Gate 3 B2B PMs + CS → then Research → Design → Engineering → Launch (Sales/Marketing/Legal). Throughout: anyone with kununu-engage memory; Betriebsrat awareness.

---

## 9. Success metrics

**North star:** fresh, *representative* contributions per company per month — a volume metric weighted by how much it closes the sample gap, not raw count.

**Leading indicators (days–weeks):** nudge → start rate; start → step-1 completion; step-1 → step-2 ("why") rate; ladder climb rate (share returning for more dimensions); segment mix of contributors vs. baseline.

**Lagging indicators (weeks–months):** representativeness of the corpus (contributor mix vs. workforce base rate); recency distribution; reader-side "helpful to a decision"; retention *cohorts* (not the launch bump).

**Guardrails (veto thresholds, pre-registered):** no drop in per-review quality/helpfulness; no de-anonymisation incident; no rise in fraud/moderation load per submission beyond capacity; aggregate-only invariant never violated.

**The novelty-effect trap:** a new check-in shows a spike that decays. The measure is **retention cohorts over time, with thresholds pre-registered so there's no peeking and rationalising later.**

---

## 10. Continuous learning during a 3-month build

Failure mode: a team goes quiet for three months and discovers at launch that the reward doesn't retain. Avoided structurally:
- **Instrument before you build** — metrics/guardrails from weeks 1–2 wired into the first increment; every slice reports.
- **Ship the thinnest slice to a tiny cohort** (dogfood + one design-partner employer), not a big-bang at month three.
- **Keep the manual Wizard-of-Oz loop running in parallel**, so qualitative signal never stops; the throwaway prototype stays cheap to iterate ahead of the real build.
- **Break the build into increments that each produce a learning**, not just a shippable component.

---

## 11. Open questions

| Question | Owner | Blocking? |
|---|---|---|
| Does the negative-skew premise hold on kununu's real distribution? | Data | **Blocking** |
| Is the reviewable population mostly leavers, or is there a reachable satisfied base? | Data | **Blocking** |
| Will employers accept a firewalled channel (no control of content/timing/who)? | CS / B2B PM | **Blocking** |
| Can verification bind to the review without de-anonymising the person, under DACH law? | Privacy/DPO + Legal | **Blocking** |
| Does "where do I stand" motivate a first contribution with real employees? | Research | **Blocking** |
| Current live status of kununu engage; exact current signup/verification flow; ownership situation | PM (confirm) | Non-blocking |
| Is a meaningful logged-in base addressable given anonymous-without-account reviewing? | Data | Non-blocking |
| Betriebsrat / employee-representation implications of employer-pushed prompts | Legal / People | Non-blocking |

---

## 12. Timeline

- **Now:** validation phase — prototype (Appendix A) as the artifact for weeks 4–6 research.
- **Weeks 1–6:** risk buy-down → go/no-go (§8).
- **~3 months** to a first real build *if* greenlit, with continuous learning throughout (§10).
- **Hard external deadline for the artifact:** case-study interview demo (single session, prototype on screen ~90–120s).

---

## Appendix A — The prototype (what it is, and what it is *not*)

**What it is:** a self-built, clickable, front-end-only prototype of the check-in loop, built to (1) put a working loop in front of real employees to test the "where do I stand" pull, and (2) give Design and Engineering something concrete to argue with. In the case narrative it is the physical answer to *"what could you test without engineering support?"* — the artifact of weeks 4–6.

**What it is *not*:** it is **not v1**, not a step toward shipping, and **not where visual craft is judged** — that is the designer's work, once the core question is de-risked. It is a **research and alignment instrument, and then it is thrown away.**

**Why this framing must be defended in the build.** A working, reactive, kununu-accented app with a reader screen and a persona switcher is *more* than a throwaway sketch. The resolution is deliberate: **middle fidelity, kununu colour accents, and the lo-fi line stated out loud in-product** ("Low-fidelity and directional: structure and flow, not final visual design"). If Design over-polishes this into a shippable-looking product, it *undercuts the argument* that the PM de-risks cheaply before committing craft. **Restraint here is a requirement, not a limitation.**

**Scope of the artifact:** P0 loop + P1 reader screen, exit tap, and instrumentation overlay (§7). Reactive synthetic data, in-memory only. Single self-contained `index.html`, no build step, served from GitHub Pages, works offline. Full build spec lives in the companion **Claude Design task**.
