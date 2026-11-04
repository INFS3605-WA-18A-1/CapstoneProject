# UAT Testing

User Acceptance Testing (UAT) checks with real Property Managers whether myEST8 supports how a small Sydney Agency actually handles maintenance, whether its wording makes sense, and whether anything important is missing before the final presentation.

!!! info "Status: planned. Sessions run 5–9 November 2026."
    The test plan and scenarios below are final. The **Results** section is filled in from the UAT pack (`myEST8_UAT_Pack.xlsx`) after the sessions.

## Test plan

| Item | Detail |
|---|---|
| **Objective** | Validate that a Property Manager at a small Agency can run the full maintenance workflow in myEST8, from a Tenant's first message to a finished Job, and that the product fits their day-to-day work. |
| **Where** | <https://myest8.vercel.app>, the current production build from `master`. Desktop browser (Chrome, Edge or Safari) at 1366–1440 px, plus the tester's own phone for one scenario. |
| **When** | Thursday 5 and Friday 6 November, the weekend of 7–8 November, and Monday 9 November as a fallback. In person or over Zoom/Teams, about 50 minutes each. |
| **Who** | **Tester:** a practising Property Manager at a small Agency. **Facilitator:** runs the session and plays the Tenant, Tradie and Property Owner from test phones and inboxes. **Note-taker:** fills in the UAT pack live. **Product owner:** triages defects and signs off. |
| **In scope** | Sign-up and onboarding; adding Properties, Tenants, Owners and Tradies (one by one and by import); Tenant reporting by SMS and email; follow-up questions and photo upload; the AI-filled Summary, Trade and Urgency; confirming a Maintenance Request; Urgent Repairs; Quote Requests, Estimates, Site Inspections and Firm Quotes; Owner approval; Work Orders and visit booking; the Tenant progress page; Dashboard, Dispatch, Tickets, Archive, Approvals, Schedule, Profiles and Settings. |
| **Out of scope** | Billing, trust accounting and rent; PropertyMe or Console Cloud integration; load and security testing; native apps; per-Agency WhatsApp numbers; Common Area repairs beyond passing them to the Building Manager. |
| **Test data** | Agency "Harbourside Rentals (UAT)". Properties 12 King St, Newtown (Max Repair Cost $500) and 4/18 Wattle St, Ultimo ($300). Tenant Priya Shah, Owner Daniel Ng, Tradies Drip Doctors, Flow Bros Plumbing (Nominated for King St) and Sparkright Electrical. **All made up.** No tester's own tenants, owners or tradies are ever contacted. |
| **Session outline** | 5 min welcome and consent → 35 min scenarios, thinking aloud → 5 min SUS questionnaire and open questions → 5 min wrap-up |

### Entry and exit criteria

**Entry:** the production deploy is green (CI `deploy-prod` and `smoke` passed); the test Agency, phones and inboxes are ready; the tester has read the information sheet and consented.

**Exit (all must be met to accept):**

1. Every High-priority scenario has been run by at least 3 testers.
2. No open Critical defects.
3. No more than 2 open High defects, each with a planned fix.
4. Average System Usability Scale (SUS) score of **68 or more**, the commonly cited industry average (Brooke 1996; Sauro 2011).
5. The team has reviewed all feedback.

### Severity and status definitions

| Severity | Meaning |
|---|---|
| Critical | Data loss, the wrong person contacted, or the flow can't continue |
| High | A main task is wrong or confusing with no workaround |
| Medium | Wrong or confusing, but there is a workaround |
| Low | Cosmetic, wording or layout |

Status values: **Pass** (behaved as expected), **Fail** (didn't; log a defect), **Blocked** (couldn't be run because of an earlier failure or missing setup), **Not run**.

**Defect handling:** each failure gets one row in the defect log, linked to its scenario. The team triages daily during the UAT window. Fixes go through the normal PR into `dev`, then a release to `master`, and are retested in the next session.

## Test scenarios

The scenarios run in order, and each builds on the one before. 32 scenarios: 19 High, 12 Medium and 1 Low priority.

| ID | Area | Scenario | Played by | Expected result | Priority |
|---|---|---|---|---|---|
| UAT-01 | Onboarding | Create an Agency account | Property Manager | The landing page explains myEST8 within seconds. Sign-up lands on "Welcome to myEST8" with the setup steps. | High |
| UAT-02 | Onboarding | Sign out and sign back in | Property Manager | Correct details reach the Dashboard. A wrong password shows a clear error without revealing whether the email exists. | High |
| UAT-03 | Onboarding | Invite a second Property Manager | Property Manager | The colleague appears in Your team, and Tradies are shared across the Agency. | Medium |
| UAT-04 | Onboarding | Set up the SMS and email channels | Property Manager | It's clear which number and email Tenants contact, and what each channel needs. | High |
| UAT-05 | Records | Add a Property with its Owner and Max Repair Cost | Property Manager | The address autocompletes and is pinned on the map. The Property shows its Owner and $500 limit. | High |
| UAT-06 | Records | Add a Tenant to the Property | Property Manager | Priya appears on the Property and in Profiles, and her mobile and email now reach the Agency as her. | High |
| UAT-07 | Records | Add Tradies and a Nominated Tradie | Property Manager | Each Tradie shows its Trade, and Flow Bros is the Nominated Tradie for King St. | High |
| UAT-08 | Records | Import Properties from a spreadsheet | Property Manager | Columns are matched, problems are flagged per row, and only the valid ticked rows are added. | Medium |
| UAT-09 | Intake | Tenant reports a leak by SMS | Tenant | Priya gets "A few quick questions" with no action from the Property Manager. | High |
| UAT-10 | Intake | Tenant answers and uploads a photo | Tenant | The upload link works on a phone, and the photo is checked and attached. | High |
| UAT-11 | Intake | Review the AI-filled Maintenance Request | Property Manager | Sensible Summary, Trade = Plumbing, Urgency = High, photo attached, marked "Filled in by AI". The tester rates whether it is trustworthy. | High |
| UAT-12 | Intake | Edit and confirm the Maintenance Request | Property Manager | It becomes a numbered Ticket, and Priya is texted a progress link. | High |
| UAT-13 | Intake | Tenant reports by email instead | Tenant | Trade = Carpentry, Urgency = Routine, and the follow-up comes back by email. | Medium |
| UAT-14 | Intake | Urgent Repair: no hot water | Tenant | Treated as an Urgent Repair: Priya is told to arrange it with Flow Bros, can claim up to $1,000, and gets a receipt link. No Ticket is created. | High |
| UAT-15 | Intake | Message from an unknown number | Tenant | The message waits for the Property Manager's review, and nothing goes to a Tradie or Owner. | Medium |
| UAT-16 | Quotes | Send Quote Requests to at least two Tradies | Property Manager | Up to four Plumbing Tradies are suggested, at least two must be chosen, and each Tradie gets a link. | High |
| UAT-17 | Quotes | Tradie gives an Estimate and asks for a Site Inspection | Tradie | The no-login quote page works on a phone, and the Ticket shows the $180 Estimate and the inspection request. | High |
| UAT-18 | Quotes | Book the Site Inspection | Property Manager | The visit is booked, and the Tenant can confirm or ask for another time. | Medium |
| UAT-19 | Quotes | Choose a Firm Quote under the Max Repair Cost | Property Manager | $180 and $240 are shown side by side. Choosing $180 needs no Owner approval, and the Work Order goes out. | High |
| UAT-20 | Quotes | Quote over the Max Repair Cost needs approval | PM / Owner | The Owner is asked by SMS. The Ticket shows "waiting on Owner", then approved after YES, and the Work Order goes out. | High |
| UAT-21 | Quotes | Owner declines | Owner | The Ticket shows the decline and what to do next. | Medium |
| UAT-22 | Quotes | Tradie who hasn't quoted gets a reminder | Property Manager | Reminded once after 48 h, and the Ticket says so. | Low |
| UAT-23 | Jobs | Tenant picks a visit time | Tenant | The visit is on the Schedule, and the Tradie and Tenant are both told. | High |
| UAT-24 | Jobs | Tradie marks the Job done; Tenant sees it | Tradie / Tenant | The Ticket moves to Done, the progress page shows the full timeline, and the Ticket moves to the Archive. | High |
| UAT-25 | Jobs | Tenant sends an Urgent Repair receipt | Tenant | A Reimbursement Request is created for the Property Manager to check and forward. | Medium |
| UAT-26 | Daily work | Dashboard: what needs my decision | Property Manager | Decisions are clearly marked, and the tester knows what to do first within a minute. | High |
| UAT-27 | Daily work | Dispatch map | Property Manager | Pins match Ticket addresses and Urgency, and the list and map agree. | Medium |
| UAT-28 | Daily work | Schedule for the week | Property Manager | Booked visits appear on the right day and time. | Medium |
| UAT-29 | Daily work | Approvals page | Property Manager | Everything waiting on an Owner is listed, with the amount and how long it has waited. | Medium |
| UAT-30 | Daily work | Find a Tenant's history in Profiles | Property Manager | Property, contacts and every Ticket are shown and editable in place. | Medium |
| UAT-31 | Daily work | Use myEST8 on a phone between inspections | Property Manager | Everything fits, nothing needs zooming, and confirming works with a thumb. | Medium |
| UAT-32 | Overall | "Which part of your repair process would this replace, and what would stop you using it?" | Property Manager | The answer is recorded word for word. | High |

## Feedback questionnaire

After the scenarios, each tester answers the 10 standard **SUS** statements (1 = strongly disagree … 5 = strongly agree), scored 0–100, and four open questions:

1. What did you like most about myEST8?
2. What was confusing, or what would you change first?
3. What is missing that your Agency would need before using it?
4. How do you handle repairs today, and how long does a typical repair take you?

Question 4 also gives the baseline for the [Impact Delivery Report](impact-report.md).

## Results

*To be completed after the sessions (5–9 November 2026).*

| Measure | Result |
|---|---|
| Testers who took part | — |
| Scenarios run / in scope | — / 32 |
| Pass rate (executed) | — |
| High-priority pass rate | — |
| Defects logged (Critical / High / Medium / Low) | — |
| Open defects at sign-off | — |
| Average SUS score (lowest / highest) | — |

### Exit criteria

| Criterion | Status |
|---|---|
| Every High-priority scenario run by ≥ 3 testers | Not yet met |
| No open Critical defects | — |
| ≤ 2 open High defects, each with a planned fix | — |
| Average SUS ≥ 68 | Not yet measured |
| All feedback reviewed | — |

### Defect log

| ID | Scenario | Summary | Severity | Status | Fix |
|---|---|---|---|---|---|
| — | — | — | — | — | — |

### Key feedback themes

*To be completed.*

### Sign-off

| | |
|---|---|
| Build tested | Production, <https://myest8.vercel.app>, commit ________ |
| Decision | ________ |
| Conditions / known issues accepted | ________ |
| Signed by (product owner) | ________ |

## Automated testing before UAT

UAT tests fit and usability. Correctness is covered first by the automated suite, which runs on every push and must pass before anything reaches production:

| Layer | What it proves |
|---|---|
| Domain and workflow (integration) | The whole maintenance workflow through the app on real Postgres with a fake clock: inbound messages, guided questions, Quotes, approvals, Work Orders and reminders, including two Property Managers acting at once |
| HTTP routes | Every page and webhook: status codes, redirects, sign-in, same-origin checks and provider signatures |
| Adapters (contract) | Each provider adapter sends the request the provider expects and reads its response |
| Browser end to end (Playwright) | Critical journeys in Chromium against the real server: sign-up, sign-in, a Tenant's text becoming a Maintenance Request, data surviving a restart, keyboard access |
| Production smoke test | After every deploy: the landing page answers, and the worker route rejects a missing secret and accepts the right one |

That is about 1,400 test cases plus 22 browser specs, all running on throwaway databases with mocked providers, so the tests need no secrets and never touch production data.
