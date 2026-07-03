# Governed Agent — a Squid AI demonstration

The **fourth** Squid AI build, and deliberately a different animal from the first three.

- First Rung → *argues* the compounding-integration motion.
- Agent Blueprint Builder → *scopes* an agent from a workflow.
- Sephora Agents in Compounding Order → *a mock case* for Eyal.

All three argue **how to think**. This one lets the viewer **feel the product work**.

## What it is
A standalone, shippable single-screen web demo. One file, no dependencies, no build
step — open `index.html` in any browser, drag onto Vercel, or plug into a page.

The visitor talks to a live-feeling AI agent sitting on top of three fictional legacy
systems and watches it answer. It proves Squid AI's core claim without a single slide:
**one governed agent layer over existing systems, no migration, permissions inherited
from the source systems.**

## The one screen
- **World picker** — three fictional companies, three legacy systems each:
  - **Ridgeline Fire District** — DispatchCAD · CrewRoster · FleetOps
  - **Maison Lumière** (beauty retail) — RegisterPOS · LoyaltyLedger · PayLink
  - **Meridian Mutual** (insurance) — PolicyCore (1998) · ServiceDesk · FinanceDB
- **Left / chat** — three suggested questions per world plus free type.
- **Right / "Receipts"** — the trace. Each step lights up tagged with the source
  system and its color. This *is* the schema, shown only as evidence for an answer,
  never as a static architecture diagram.
- **Persona control** — signed-in role. Each world has one question marked
  **TESTS ACCESS**: it hits a permission wall. Switch to the cleared role and the same
  question opens; a red lock marks the exact trace step where a lower role is refused.
  Permissions inherit from the source systems, live, in front of the buyer.
- **Caption** — *"The agent inherits your permissions. It never knows more than you're
  allowed to."*
- **Footer** — *"Built on the pattern Squid AI deploys in four weeks."*

## The permission walls (the risk this build spends)
| World | Wall question | Refused role → cleared role |
|---|---|---|
| Fire | injury names from last night's fire | Shift Captain → Records Division (sealed PII) |
| Retail | full card number for a refund | Store Associate → Payments (PCI-scoped PAN) |
| Insurance | claimant SSN & medical records | Adjuster → Auditor (PHI / Special Investigations) |

A demo where the AI visibly **can't** do something because governance is working is more
convincing to an enterprise buyer than ten questions answered perfectly.

## How the "live" part works (and why it's safe to ship)
- The **suggested questions** are pre-scripted with streaming animation: zero latency,
  zero hallucination risk, forwardable link.
- **Free type** matches a known flow loosely; anything outside the seeded systems gets an
  honest *"I searched these three systems and don't have a record — I won't guess."*
  The demo never invents data.
- No API key in the browser, no external calls — so it also runs as a claude.ai Artifact
  and can be dropped into any site or an `<iframe>`.

## How Dan uses it (the sell-side job)
This is a **forwardable follow-up / leave-behind**, not a self-serve toy. It does Dan's
arguing when he's not in the room — specifically it disarms the security/IT reviewer who
isn't on the first call but vetoes the deal later. That skeptic is exactly who the
permission wall is built for.

The UI is shaped for that job:
- **Felt "before"** — the trace starts by showing the three systems as three separate
  logins with no shared language ("you'd open each by hand, or ask once"). The pain is
  shown, not asserted (no fake click-counter).
- **"This is you," not a sample** — the picker leads with the **stack** (Public safety /
  Retail & loyalty / Insurance), company name as subtitle: *pick the one closest to yours.*
- **A next step** — one quiet CTA ("See it on your stack →"). Set it in the `CTA` const at
  the top of the `<script>` in `index.html`:
  ```js
  const CTA = { label: "See it on your stack →", href: "https://cal.com/squid-ai/intro" };
  ```
  Leave `href` as `"#"` and the button stays inert (still visible). Point it at a calendar
  or a `mailto:` and it becomes live pipeline.

**v2 power move (structured for it, not built):** swap the three worlds' system names for a
real prospect's stack — it's a data-only edit in the `WORLDS` object.

## Deploy
```
vercel        # then vercel --prod
```
Or drag this folder onto vercel.com/new. Static site, `index.html` at root.

## Before it goes out
- Everything is fictional: no real company names, no Sephora program details, no real
  fire-district or insurer data. The footer says so.
- "Squid AI" is always written in full, never "Squid."
- Voice: no em dashes, no "AI magic"; the vocabulary is trust, governance,
  permissions, receipts.

## Explicitly NOT built (per spec — v2 only)
Mock swivel-chair task with clickable legacy UIs · live stack-guessing from free industry
input · per-deal config mode (Dan seeds a prospect's systems via JSON) · lead capture /
CTA walls / logo carousels.
