# Governed Agent — a working demo of permission-inherited AI

**What this is, plainly:** I built this on my own time while interviewing for a PM
role at Squid AI. It is not their code and it is not work I did there. Every
company, system and record in it is invented. I wrote it to answer a question I
kept hearing asked badly — *can an AI agent read across our systems without
becoming the thing that leaks them* — by showing it instead of arguing it.

It was the fourth of four builds I made during that process, and the only one that
lets you feel the product work rather than reading an argument about it. The other
three are live too: [First Rung](https://first-rung.vercel.app) argues why build
order is the strategy, [Agent Blueprint Builder](https://squid-ai-build.vercel.app)
scopes an agent out of a workflow, and [The three agents, in the order that makes
them compound](https://compounding-order.vercel.app) runs that argument as a case
study.

## What it is
A standalone, shippable single-screen web demo. One file, no dependencies, no build
step — open `index.html` in any browser, drag onto Vercel, or plug into a page.

The visitor talks to a live-feeling AI agent sitting on top of three fictional legacy
systems and watches it answer. It makes one claim without a single slide:
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
  Permissions inherit from the source systems, live, in front of whoever is watching.
- **Caption** — *"The agent inherits your permissions. It never knows more than you're
  allowed to."*
- **Footer** — the standing note that every company and record on screen is invented.

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

## The job I designed it for
I built it as a **forwardable leave-behind**, not a self-serve toy — something that
does the arguing when nobody is in the room. The reader I aimed at is the security
or IT reviewer who is never on the first call and vetoes the deal three weeks later.
That skeptic is exactly who the permission wall is built for.

Every UI decision below follows from that one reader:
- **Felt "before"** — the trace starts by showing the three systems as three separate
  logins with no shared language ("you'd open each by hand, or ask once"). The pain is
  shown, not asserted (no fake click-counter).
- **"This is you," not a sample** — the picker leads with the **stack** (Public safety /
  Retail & loyalty / Insurance), company name as subtitle: *pick the one closest to yours.*
- **A next step** — one quiet CTA, configurable rather than hardcoded, in the `CTA`
  const at the top of the `<script>` in `index.html`:
  ```js
  const CTA = { label: "See it on your stack →", href: "#" };
  ```
  Left inert by default. Point `href` at a calendar or a `mailto:` and the button
  goes live.

**Structured for, not built:** swapping the three worlds' system names for a real
stack is a data-only edit in the `WORLDS` object. I shaped the data that way on
purpose and stopped there.

## Deploy
```
vercel        # then vercel --prod
```
Or drag this folder onto vercel.com/new. Static site, `index.html` at root.

## The line I held
Everything on screen is fictional. No real company names, no client program details,
no real fire-district or insurer data, and nothing from any employer of mine. The
footer says so where a visitor can see it, not just here.

## Deliberately not built
Mock swivel-chair task with clickable legacy UIs · live stack-guessing from free industry
input · per-deal config: seed a set of systems from JSON · lead capture /
CTA walls / logo carousels.
