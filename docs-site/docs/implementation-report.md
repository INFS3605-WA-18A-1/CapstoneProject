# Implementation Report

This report covers how the myEST8 build went against the plan: what was delivered, which decisions changed and why, what it took, and what is still open. For what the system is and how it works, see [Implementation](implementation.md).

## 1. Summary

- **Delivered:** all 12 core MVP features from the [Design Document](designDoc.md), plus five additions that came out of testing: AI follow-up drafts, the Approvals page, the Archive, team management and shared contact addresses.
- **Running:** in production at <https://myest8.vercel.app>, with real SMS, email and WhatsApp, real AI services, and automated deploys from `master`.
- **Build window:** 8 October – 4 November 2026 (4 weeks), as planned.
- **Next:** User Acceptance Testing with practising Property Managers, 5–9 November 2026 ([UAT Testing](testing.md)).

## 2. Build in numbers

All figures are from the myEST8 repository on 4 November 2026.

| Measure | Value |
|---|---|
| Commits | 612 |
| Architecture Decision Records | 19 (`docs/adr/`) |
| Database migrations | 17, all additive (so a rollback can run older code on the newer schema) |
| Server-rendered pages | 30 |
| External-service adapters | 27, each with an in-process mock |
| Automated test cases | ~1,400 (Vitest), plus 22 Playwright browser specs |
| CI checks on every push | typecheck, unit/integration tests, browser tests, production build check |

## 3. Timeline

| Week | Dates | Focus | Outcome |
|---|---|---|---|
| 1 | 8–14 Oct | Foundations | Repo, domain language (`CONTEXT.md`), ADRs 0001–0003, module layout, the demo with a phone simulator and movable clock |
| 2 | 15–20 Oct | Core workflow | Intake, guided questions, photo upload and checks, Maintenance Requests → Tickets, Quotes, Owner approval, Work Orders, reminders. Peak of 82 commits on 16 Oct. |
| 3 | 21–25 Oct | Real channels and hosting | Moved from SQLite to Postgres/Supabase, deployed on Vercel with CI/CD, connected real SMS, email and WhatsApp. The Twilio route failed and was replaced (see §4). Peak of 102 commits on 21 Oct. |
| 4 | 28 Oct – 4 Nov | AI, polish, UAT readiness | Haiku composing emails, Jev job fit and request judgments, visit offers, the Dispatch map, redesigned Dashboard, Profiles, Schedule and Approvals, motion system, landing page, the UAT pack |

## 4. Decisions that changed during the build

The plan left three decisions open: the web framework, database and hosting; the vision model; and the quote-reading model. Several other decisions had to be revisited once real providers were tried. Each change is recorded as an ADR in the repository.

| Area | Planned | Built | Why it changed |
|---|---|---|---|
| Framework, database, hosting | Open | Hono + Postgres (Supabase) on Vercel | Hono runs unchanged on Node and on serverless. Postgres gives the transactions and locking needed when several Property Managers act at once. Everything fits free tiers. |
| SMS | Twilio | textbee (Android phone gateway) | Twilio's trial can't send custom text, and Twilio **rejected our Australian regulatory bundle** (22 Oct), so we couldn't get a number at all. The per-Agency Twilio code is kept but switched off (ADR 0005 → 0013). |
| Email | Twilio SendGrid Inbound Parse | Resend with an inbound subdomain | SendGrid's free plan was retired in May 2025. Gmail OAuth was tried next, but its tokens expire about every 7 days in Testing mode (ADR 0004 → 0006). |
| WhatsApp | Twilio sandbox | Meta Cloud API, one server number | Twilio was only a paid layer over Meta's API. The first version sent a verification code to the server's own number, which Meta refuses, so it was redesigned to connect without a code (ADR 0019). |
| Classification | Jev | Jev, with Claude Haiku as fallback | Jev suits typed judgments with probabilities. Haiku covers outages (ADR 0011). |
| Writing messages | Fixed templates only | Haiku composes emails, with templates as fallback | Template emails read as robotic. Code-checked composition keeps every fact exact (ADR 0008 → 0011). |
| Guided questions | Four questions, one at a time | Haiku extracts what the Tenant already said, then asks once for the rest, at most twice | Tenants were asked things they had already answered ("last night the sink broke" already says where and when) (ADR 0014) |
| Quoting | One quote step | Estimate → optional Site Inspection → Firm Quote, per Tradie | Australian tradies quote in stages, and an estimate isn't binding. A Firm Quote is now always required (ADR 0007 → 0009 → 0016). |
| Scheduling | Property Manager picks the time | Tradie offers up to three times, Tenant picks | Removes a round of phone tag. The Property Manager can still override (ADR 0015). |
| Tradie ranking | Strict tie-breaks on ratings | Weighted blend including AI job fit | Decimal ratings rarely tie, so Google rating and price never counted, and nothing looked at the Job itself (ADR 0016). |
| Urgent Repair receipts | Auto-forwarded to the Owner | Property Manager checks, then forwards | Unchecked files would have gone to Owners under the Agency's name (ADR 0010) |
| Geocoding | Google | Nominatim + Photon | Google Geocoding needs billing enabled. The OpenStreetMap services are free and match the map tiles (ADR 0012). |

## 5. Challenges and how we handled them

1. **Australian SMS regulation.** This was the biggest risk. Real Australian SMS needs identity-verified numbers, and a student project couldn't get one through Twilio. textbee routes texts through a real SIM, at the cost of one shared number and the free-tier limits (50 texts a day, 300 a month). The Twilio code stays in place in case a bundle is approved later.
2. **Serverless background work.** Vercel's free plan has no long-running process and only daily cron. We run the worker after every webhook and ping a cron route every 5 minutes from GitHub Actions. Tasks are claimed atomically and retried after 10 minutes if a run dies.
3. **Latency.** Pages make several queries in a row. With the function in the default US region and the database in Seoul, every page took seconds. Moving the function to `icn1`, next to the database, fixed it.
4. **Keeping AI output trustworthy.** Free-form model output can drop or invent an amount or a link. Composition is therefore checked by code against the template's facts and falls back to the template. AI text is labelled ✦✦ in the interface. Profile wording is written by code, never by a model.
5. **Testing without real providers.** Every external service is behind a port with a mock, so the whole workflow (including two Property Managers acting at the same moment) is tested on real Postgres with a fake clock, with no secrets needed.
6. **Safe production releases.** A university project has no staging. We added a strict `dev` → `master` release flow, additive-only migrations, post-deploy smoke tests, a one-click rollback workflow and weekly encrypted backups.

## 6. Delivery process

- **Branching:** feature branch → PR into `dev`, reviewed with CI green → release PR `dev` → `master`, which deploys. CI rejects PRs into `master` unless they come from `dev` or a `hotfix/` branch.
- **Decisions** were recorded as ADRs when they were made, and domain terms were kept in `CONTEXT.md` so code, interface and documentation use the same words (Maintenance Request, Ticket, Job, Firm Quote, Max Repair Cost…).
- **Issues** were tracked as markdown files under `.scratch/<feature>/`, with triage labels.
- **AI-assisted development:** coding agents worked on separate git worktrees under the same review and CI rules as human contributors. Agents open PRs but never merge to `master`.

## 7. Known limitations

| Limitation | Impact | Planned response |
|---|---|---|
| One shared SMS number (textbee), 50 texts a day | Only one Agency can use SMS at a time, and it isn't enough for real volume | A paid provider with a number per Agency once regulatory approval is possible (the code already exists) |
| One shared WhatsApp number; Meta test-number limits | Same as above | Meta business verification and Embedded Signup per Agency |
| Vision photo-match and PDF quote-reading models not chosen | Photo match and PDF parsing run on the mock contract. The Property Manager confirms every result, and Tradies type their quotes on the quote page. | Choose and evaluate a model against the existing contracts |
| No independent security review of the no-login links | Tokens are unguessable and rate limited, but untested by a third party | Review before any real Tenant data is used |
| Rate limiter is per serverless instance | Burst limits are approximate | A Vercel Firewall rule on the no-login paths |
| Storage files are not in the weekly backup | Photos could be lost in a storage incident | Add a bucket sync to the backup job |
| No integration with property management software (PropertyMe, Console Cloud) | Records are entered by hand or by spreadsheet import | Post-MVP integration, prioritised by UAT feedback |
