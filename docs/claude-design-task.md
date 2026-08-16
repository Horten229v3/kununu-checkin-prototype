# Claude Design task — "The kununu Check-in" interactive prototype

*Paste this whole document into Claude Design as the brief. It is self-contained; the companion PRD (`kununu-checkin-PRD.md`) is background, not required reading to build.*

---

## What to build

A single-file, front-end-only **clickable prototype** of a mobile "workplace check-in" loop, shown inside a **phone frame centred on a neutral desktop backdrop** (this is demoed on a shared screen, not a real phone). It is a **research and alignment instrument** for a product-manager case-study interview — a working loop to put in front of real employees, and something Design/Eng can react to. It is explicitly **not a shippable product.**

**Deliverable:** one self-contained `index.html` — HTML + CSS + vanilla JavaScript, **no build step, no external dependencies, no network calls, works fully offline** when opened as a local file. It will be committed to a public GitHub repo and served via GitHub Pages. Include a short `README.md` (what it is, how to run, keyboard controls, the one-line "not a shipped product" framing).

---

## The single most important constraint: deliberate restraint

This must look **middle-fidelity and lo-fi *on purpose*** — clean, legible, kununu-accented, but visibly a *directional flow, not final visual design*. **Do not over-polish it into something that looks shippable.** The whole point of the artifact is to prove a loop is *desirable* before craft is committed; a too-polished result actively undercuts that argument.

Concretely:
- Persistent, quiet **"Low-fidelity & directional — structure and flow, not final visual design"** label visible on screen (small, bottom of the phone frame or desktop chrome).
- Simple shapes, generous whitespace, one accent colour used sparingly. **No** illustrations, stock photos, custom iconography beyond simple line/emoji-free glyphs, gradients, shadows-as-decoration, or micro-animations beyond a basic fade/slide between screens.
- If in doubt, do **less**.

---

## Brand tokens (reuse exactly — these match the candidate's slide deck)

```css
:root{
  --ink:#16233F;      /* primary text (kununu navy-black) */
  --ink2:#414E63;     /* secondary text */
  --ink3:#737E92;     /* muted / captions */
  --navy:#102B69;     /* primary accent, buttons, headers */
  --navy-tint:#E9EEF8;
  --navy-line:#AEBBD8;
  --yellow:#FFC217;   /* kununu yellow — accent only, sparingly */
  --yellow-tint:#FFF3D1;
  --paper:#FFFFFF;    /* phone screen bg */
  --panel:#EEF2FA;    /* cards */
  --line:#E3E6EE;     /* borders */
  --danger:#A6402C;   /* used only for the locked/guardrail note if needed */
  --desk-bg:#E9ECF2;  /* desktop backdrop behind the phone */
}
```
System font stack (`-apple-system, "Segoe UI", Roboto, Helvetica, Arial, sans-serif`). Phone frame ~390×844, rounded corners, thin `--ink` bezel, a simple status bar reading **9:41**.

---

## State model — the loop must actually work (not static screenshots)

Keep **all state in-memory** (plain JS objects). **No `localStorage`/`sessionStorage`** — it must behave identically in a Claude Design preview and on GitHub Pages, and reset must return to the initial state cleanly.

**Seeded synthetic cohort.** Hard-code a small deterministic dataset: for each **role** (e.g. *Product Manager, Software Engineer, Warehouse Shift Lead*) and each **dimension** (*work-life balance, management, growth, workload, overall sentiment*), store a fixed distribution (an array of ~30–50 synthetic scores, or a mean+spread). Also store **industry** baselines. Use a fictional employer, e.g. **"Nordwind Logistics"**.

**Reactive payoff.** When the user picks a rating on a dimension, compute their **percentile against their role's distribution** and render it as e.g. *"Higher than 62% in your role."* Deterministic from the seeded data — same input, same result, every time. This reactivity is the P0 requirement; the "where you stand" screen feeling real is what the demo hinges on.

**N-floor logic.** Track `companyResponses` for the selected employer, seeded **below 5**. The **"Compare with colleagues"** card renders **locked**, stating the reason. Provide a demo-only affordance (behind the instrumentation toggle, see below) to **simulate crossing 5 responses** so the presenter can show the card **unlock live**. In the clean demo (overlay off) it simply shows locked with its reason.

---

## Personas (segmentation, shown live)

A small persona switcher (a segmented control in the desktop chrome, *not* inside the phone) with three options that visibly change the flow:

1. **New hire (2 months)** — prompt leans onboarding ("How's your start been?"); comparison vs. others' *first months*.
2. **Settled employee (3 years)** — the default full ladder; prompt "How's it going at work?"; comparison vs. role/industry/colleagues.
3. **Leaver** — the check-in lands, the **"I don't work here anymore"** tap appears prominently; taking it routes into the **exit / former-employee** framing (see scene 6) instead of the current-employee comparison.

Switching persona changes **at minimum the prompt copy and the comparison content**. Keep the mechanism to one obvious control.

---

## Scenes / flow

Build these as swappable "scenes" in one page. Provide **linear next/back** *and* **direct jump** to any scene (see controls). No dead ends.

**Scene 1 — The nudge**
> **How's it going at work?**
> A 20-second check-in. Anonymous, and you'll see how you compare.
> `verified · anonymous` · small "your team · this month" eyebrow.
> Primary button: **Start check-in** ("takes about 20 seconds").
*(Copy adapts to persona.)*

**Scene 2 — Rate, then an optional reason**
> `1 of 3` · **How's your work-life balance right now?** · "Tap how you feel."
> A simple scale from **Poor → Great** (5 taps or a 0–10 slider — keep it lo-fi).
> Then, **optional**: *"What's driving that? (optional)"* — a small row of **reason chips** whose options depend on the dimension (for work-life balance: `Workload` · `Flexibility` · `Team` · `Management` · `Pace`). Selecting one powers the reasons-comparison payoff (Scene 3.5).
> Below the chips, an optional free-text box *"Anything else? (optional)"* — this feeds the **private depth layer** and is **never shown to other contributors**.
> Two actions: **Continue** and **Skip.** The rating is required; the reason and the note are genuinely optional.

> **Why a chip, not just free text.** The reasons-comparison screen must *compute* from the input to feel real (the P0 reactivity rule), and vanilla JS can't parse free text. A structured tag is deterministic, keeps the reward tied to a *real* reason (so honesty still pays), and is anonymity-safe — unlike raw free text, which is often self-identifying and must never be surfaced to peers.

**Scene 3 — Where you stand (the payoff)**
> **Where you stand — from your check-in.** Show a few comparison rows, reactive to scene-2 input:
> - *Work-life balance* — e.g. "Higher than 62% in your role"
> - *Overall sentiment* — "Higher than 70% in your industry"
> - a "Management / About the same as your role" style row
> Then the **locked** card:
> **Compare with colleagues** — *"Unlocks once 5+ people at your company respond, so no single answer can be traced to you. Protects your anonymity."*
> Secondary: **See full breakdown.**

**Scene 3.5 — Where your reasons stand (conditional — only if a reason chip was selected)**
> Appears in the linear flow **after Scene 3, and only when the user picked a reason chip** in Scene 2. If they skipped, this screen is skipped too — which *is* the give-to-get working honestly: contribute a reason, see more; skip, see less; no one's raw text is ever unlocked.
> **Where your reasons stand.** One or two lines computed from the chip + rating, e.g.:
> *"You rated work-life balance low and cited **workload** — so did **3 in 5** others who rated it low."*
> *"You cited **workload**; the next most common reason in your role was **pace**."*
> Quiet reassurance: *"Reasons are shown as anonymous patterns — never anyone's individual words."*
> This is the reward for Step 2 of the ladder: **deeper personal insight, not access to others' content.**

**Scene 4 — What's shared**
> **You're in. Thanks!**
> *"Your answers were added anonymously to your company's public culture score."*
> Two reassurance ticks: **Only the aggregate is shown** · **Never your individual answers.**
> *"We'll check in again next month."* · link: **See your company's profile** → (goes to Scene 5).

**Scene 5 — Reader / job-seeker view (P1)**
The other side of the market: a job seeker looking at **Nordwind Logistics'** culture score. Show an **aggregate** score, then the same signal **sliced by role and level** (a short list/bar view: e.g. *Product · Engineering · Operations*, or *IC vs Management*). One line makes the point explicit: *"Light, structured check-ins become signal a job seeker can navigate by role and level — quality, not just volume."* Aggregate only; never an individual answer.

**Scene 6 — Exit capture (P1, reached via Leaver persona)**
The **"I don't work here anymore"** tap leads here.
> *"Now that you've moved on — how was it?"*
> A short note that they're now recorded as a **verified former employee**, kept **separate from current-employee signal**, and that the employer-verification path is gone (their history establishes they were there). One line: *"The most balanced review often comes from someone who just left."*

---

## Instrumentation overlay (P1) — off by default

A toggle (keyboard `i`, and a small button in the desktop chrome) that overlays, **per scene**, a compact card showing three lines — this is what turns the prototype from a mockup into evidence of method:

- **Assumption** this screen tests (e.g. Scene 1: *"'Where do I stand' pulls a first contribution"*; Scene 3: *"the comparison is a strong enough reward"*; Scene 4: *"aggregate-only reassurance preserves trust"*).
- **Metric** you'd instrument (e.g. nudge→start rate; start→step-1 completion; step-1→"why" rate; retention cohort).
- **Guardrail** in play (e.g. Scene 3: *N-floor ≥ 5*; Scene 4: *aggregate-only invariant*; cadence owned by kununu).

Also expose here (overlay-only): the **"simulate 5th response"** control that unlocks the colleague card, and an optional **fake-door A/B** toggle that swaps Scene 1's nudge headline between two variants (illustrative — label it as a fake-door test of copy).

When the overlay is **off**, none of this scaffolding shows — the clean demo is pristine.

---

## Presenter controls (build for a live 90–120s demo)

- **← / →** — previous / next scene.
- **1–6** — jump directly to any of the six primary scenes. (Scene **3.5** appears only in linear flow via **→** when a reason chip was given; it's not on a number key.)
- **r** — reset to Scene 1, initial state (clears any input, re-locks the N-floor card).
- **i** — toggle the instrumentation overlay.
- **p** — cycle persona (or use the on-screen control).
- All controls also available as small, unobtrusive buttons in the desktop chrome (in case keyboard isn't handy on the shared screen). Show a tiny "?" that lists the shortcuts.

---

## Acceptance criteria

- [ ] Opens as a single local `index.html` with **no build, no network, no dependencies**; identical behaviour in preview and on Pages.
- [ ] The **"where you stand" percentages react to the user's own rating** and are deterministic.
- [ ] The colleague card is **visibly locked and states the five-person reason**; the overlay control can **unlock it live**.
- [ ] If a reason chip was selected, **Scene 3.5 appears and its line reflects that chip + rating**; if the reason was skipped, Scene 3.5 is skipped.
- [ ] **Persona switch visibly changes** prompt and comparison; Leaver reaches the exit scene.
- [ ] Final screen states **aggregate-only / never individual**; no individual answer is ever shown as published.
- [ ] Reader scene shows signal **sliced by role/level**.
- [ ] Instrumentation overlay is **off by default** and, when on, shows assumption/metric/guardrail per scene.
- [ ] **No dead ends; `r` resets instantly; ←/→ and 1–6 navigate.**
- [ ] The **"low-fidelity & directional" label is always visible.**
- [ ] It looks **intentionally restrained** — not like a finished product.

## Design non-goals (do not do these)
- Do **not** add a real backend, auth, accounts, or any network request.
- Do **not** use `localStorage`/`sessionStorage` or any persistence.
- Do **not** add scenes, dimensions, or features beyond those listed ("while we're at it").
- Do **not** introduce photography, illustration, heavy iconography, gradients, or animation flourishes.
- Do **not** raise the fidelity toward "shippable." Restraint is the brief.
