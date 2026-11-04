# Impact Delivery Report

This report sets out the business impact myEST8 is designed to deliver, how the delivered MVP maps to it, and how we will measure it. myEST8 hasn't been used by a real Agency yet, so the impact figures here are **projections built on stated assumptions**. They will be checked against what Property Managers tell us in [UAT](testing.md) (5–9 November 2026) and replaced with their numbers.

## 1. The problem we set out to solve

> With ageing buildings and a lack of incentives to repair, property managers spend too much time coordinating severe maintenance repairs. This affects small and individual property managers, who spend most of their time on communication and maintenance admin. This bottleneck needs to change because it limits property managers' ability to keep residents satisfied and take on more properties.

The evidence behind it (full detail on [Market Research](market-research.md) and [Problem Statement Refinement](prb-st-refinement.md)):

- **Workload:** 66% of property managers describe their workload as busy or far too busy, and 29% intend to leave the industry (MRI, via Real Estate Business).
- **Capacity is shrinking:** properties per manager fell from 122 to 116 between 2019 and 2023 (Macquarie Real Estate Benchmarking Report 2023).
- **Repairs are common and slow:** 50% of renters live in homes needing repairs (UNSW *Rights at Risk*, 2025), and 38% of landlords had a tenant wait longer than is reasonable for a repair in the past year (Finder, 2025).
- **Coordination is manual:** requests arrive through many channels and are tracked, prioritised and chased by hand, and tradie discovery relies on personal lists and word of mouth.

## 2. How the MVP delivers the impact

Each impact the design promised maps to features that are now built and running in production.

| Intended impact | Delivered by | Who benefits |
|---|---|---|
| **Less time on communication** | The AI intake agent asks the follow-up questions, collects photos and writes the Summary. Automatic updates go out at every status change on each person's own channel. | Property Manager |
| **Less time chasing** | Quote Requests in one click, automatic reminders (48 h for quotes, 24 h for Owner approvals, 24 h for visit offers) and alerts only when someone hasn't responded | Property Manager |
| **Decisions, not admin** | The Dashboard shows only what needs the Property Manager's decision: confirm a request, choose a Firm Quote, or handle a stalled item. Everything else moves on its own. | Property Manager, Agency principal |
| **Lower and more defensible repair costs** | At least two Firm Quotes compared side by side, recommendations weighted by rating, job fit and price, and Owner approval only above the Max Repair Cost | Property Owner |
| **Faster, more transparent repairs for Tenants** | Report on any channel with no app or account, a live progress tracker, and visit times the Tenant picks | Tenant |
| **Fewer wasted call-outs** | Photo and metadata checks before confirmation, and Site Inspections booked only when the Tradie asks for one | Owner, Agency |
| **Correct handling of urgent repairs** | Urgent issues go straight to the Tenant with the Nominated Tradie and the $1,000 reimbursement process, which the Property Manager checks before forwarding | Tenant, Owner, Agency (compliance) |
| **Capacity to grow** | Hours freed from admin can go to winning and serving more Owners (see §3) | Agency principal |

## 3. Projected time savings

**Approach.** For one repair, add up the Property Manager's coordination touches today and compare them with the touches left once myEST8 handles the routine ones. The minute values below are **assumptions for illustration**. UAT question 4 ("How do you handle repairs today, and how long does a typical repair take you?") will replace them.

| Coordination step | Today (manual), assumed | With myEST8 | What the Property Manager still does |
|---|---|---|---|
| Take the report, ask follow-up questions, get photos | 15 min | 2 min | Reads the Summary and confirms |
| Find tradies and request quotes | 15 min | 2 min | Ticks the recommended Tradies |
| Chase and compare quotes | 15 min | 3 min | Chooses a Firm Quote |
| Get Owner approval (when over the limit) | 10 min | 0–1 min | Nothing, unless the Owner doesn't reply |
| Book the visit between Tradie and Tenant | 15 min | 1 min | Nothing, unless asked to override |
| Status updates to Tenant and Owner, close-out | 10 min | 1 min | Rates the Tradie |
| **Total per repair** | **~80 min** | **~10 min** | |

**Illustrative Agency.** Take the persona's portfolio of about 100 Properties per Property Manager, and **assume** 2 non-urgent repairs per Property per year:

- 200 repairs × ~70 minutes saved ≈ **230 hours a year per Property Manager**, or about **4.5 hours a week**.
- At the benchmark of about 116 Properties per manager, that is roughly the time needed to take on more Properties, which is the growth the problem statement says is being blocked. The exact number is for UAT testers to confirm or correct.

!!! warning "Projection, not a measurement"
    These figures depend entirely on the assumed minutes and repair volume. We'll report the UAT-derived version in §6 and say plainly if it comes out lower.

## 4. Cost to run

| Item | MVP (now) | Real Agency (estimate) |
|---|---|---|
| Hosting (Vercel) and database (Supabase) | Free tiers | Paid tiers needed for commercial use (Vercel Hobby is non-commercial only) |
| SMS | textbee free tier (1 phone, 50 texts a day, 300 a month); texts cost what the phone plan charges | A dedicated number per Agency. Twilio quoted about US$8.25 a month per Australian mobile number plus a per-message charge (checked 21 Oct 2026; prices change). |
| Email (Resend) | Free tier | Low monthly cost at Agency volumes |
| AI (Jev, Claude Haiku) | Pay per call; small messages, and judgments cached so each is asked once | Scales with message volume; Haiku is the lowest-cost Claude tier |
| Maps and geocoding | Free (OpenStreetMap services, Google Places on its free allowance) | Unchanged at Agency scale |

Running costs are small next to the value of the Property Manager hours in §3. We'll set the exact per-Agency cost once a paid SMS route is confirmed.

## 5. How we will measure impact

| KPI | Baseline source | Target / signal | When |
|---|---|---|---|
| Property Manager minutes per repair | UAT Q4 (current process) | ≥ 50% reduction (projection: ~85%) | UAT, then the pilot |
| Time from report to confirmed Ticket | UAT Q4; myEST8 timestamps | Same day for non-urgent issues | Pilot |
| Time from report to booked visit | myEST8 timestamps | Track the trend; fewer stalled items | Pilot |
| Share of Jobs with ≥ 2 Firm Quotes | myEST8 data | ≥ 90% (the rest with a recorded reason) | Pilot |
| Share of AI-filled requests confirmed without edits | myEST8 data (edits to Summary, Trade, Urgency) | Rising over time; a measure of AI trust | Pilot |
| Usability | SUS questionnaire | Average ≥ 68 | UAT |
| Willingness to adopt | UAT-32 and open questions | Testers name a process step myEST8 would replace | UAT |

## 6. UAT validation of impact

*To be completed after the sessions (5–9 November 2026).*

| Question | Finding |
|---|---|
| Typical Property Manager time per repair today (UAT Q4) | — |
| Steps testers said myEST8 would replace (UAT-32) | — |
| What would stop them using it | — |
| Average SUS score | — |
| Revised time-saving estimate | — |

## 7. Risks to the impact

| Risk | Effect on impact | Mitigation |
|---|---|---|
| Change resistance: people keep the processes they know | Low adoption, so no time is saved | Tenants, Tradies and Owners need no new app or account. Onboarding is a guided wizard, and spreadsheet import brings in existing records. |
| AI misclassification (wrong Urgency or Trade) | Rework, or an urgent issue handled late | Low-confidence messages go to the Inbox. Every Maintenance Request is confirmed by a person. AI-written text is labelled. |
| SMS regulation and provider limits | The channel tenants use most is capped | Email and WhatsApp keep working. The paid per-Agency SMS code is ready once approved. |
| Privacy of Tenant data and photos | Loss of trust; legal exposure | Made-up data only for now, encrypted credentials and backups, and a security review planned before real data |
| Fit with existing property management software | Double entry reduces the time saved | Spreadsheet import now; PMS integration prioritised by UAT feedback |

## 8. Conclusion

The MVP delivers every capability the design said was needed to cut a Property Manager's maintenance admin: intake, checking, quoting, approval, booking and updates now run on their own, and the Property Manager makes the decisions. The projected saving is large, about 4.5 hours a week for a manager with 100 Properties. It is still a projection until UAT confirms the baseline. The next step is to replace the assumptions in §3 with testers' own numbers and report the result here.
