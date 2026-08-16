# The kununu Check-in — directional prototype

**Not a shipped product.** This is a research and alignment instrument built for a
product-manager case-study interview: a clickable, front-end-only prototype of a
recurring "workplace check-in" loop, used to put a working loop in front of real
employees and to give Design/Engineering something concrete to react to — then
thrown away. Low-fidelity and directional on purpose: structure and flow, not
final visual design.

Background: the [PRD](docs/PRD.md), the original [design brief](docs/claude-design-task.md),
and the [case-study reasoning doc](docs/case-study.html) live in `docs/` for traceability.
None of them are required to run the prototype.

## What it is

A six-scene loop, shown inside a phone frame on a neutral desktop backdrop
(this is demoed on a shared screen, not a real phone):

1. **The nudge** — persona-specific prompt to start a 20-second check-in.
2. **Rate + optional "why"** — a single required rating plus a genuinely optional free-text reason.
3. **Where you stand** — a reactive comparison against role/industry, computed live from the rating just given; a colleague-comparison card that stays locked until an anonymity floor (N ≥ 5) is met.
4. **What's shared** — aggregate-only / never-individual confirmation.
5. **Reader / job-seeker view** — the same signal sliced by department and level.
6. **Exit capture** — reached via the "I don't work here anymore" tap, framed as a verified former-employee signal kept separate from current-employee signal.

Everything is deterministic and reactive: the "where you stand" percentiles are
computed from a small seeded synthetic cohort (a linear congruential generator
feeding a Box–Muller transform), not hard-coded strings — same rating, same
result, every time.

## How to run it

No build step, no dependencies, no network calls. Either:

- Double-click `index.html` to open it directly (`file://`), or
- Serve the repo root with any static file server, or
- Open it via GitHub Pages once published.

## Controls

| Key | Action |
|---|---|
| `←` / `→` | previous / next scene |
| `1`–`6` | jump directly to a scene |
| `r` | reset to scene 1 (clears rating/why, re-locks the colleague card) |
| `i` | toggle the instrumentation overlay |
| `p` | cycle the persona/segment |

All controls are also available as buttons in the desktop chrome above the
phone frame, in case keyboard isn't handy on a shared screen. Click the `?`
button to see the shortcut list on screen.

## Personas (segmentation, shown live)

A segmented control in the desktop chrome (never inside the phone) switches
between three personas, each changing the prompt copy and comparison scope:

- **New hire (2 months)** — onboarding-framed prompt; compared against a first-months cohort.
- **Settled employee (3 years)** — the default full loop; compared against role/industry/company.
- **Leaver** — the "I don't work here anymore" tap is prominent (not a quiet link) and routes to the exit/former-employee scene instead of the current-employee comparison.

## Instrumentation overlay

Off by default. Toggling it (the `Instrumentation` chrome button, or `i`) shows,
per scene, the **assumption** it tests, the **metric** you'd instrument, and the
**guardrail** in play — plus two demo-only affordances: a control to simulate
crossing the 5-response anonymity floor live, and a fake-door A/B toggle for the
nudge headline copy (illustrative only, no real traffic split behind it). When
the overlay is off, none of this scaffolding is visible.

## Design constraints honored

- All state lives in memory (a plain JS object + a `render()` function) — no
  `localStorage`/`sessionStorage`, so `r` always returns to a clean initial state
  and behaviour is identical locally and on GitHub Pages.
- No illustrations, stock photography, icon sets, gradients, or decorative
  shadows; a single ~140ms fade/slide is the only animation.
- The "Low-fidelity & directional — structure and flow, not final visual design"
  strip stays visible at the bottom of the phone frame at all times.
- No dead ends; every scene is reachable and the loop resets instantly.
