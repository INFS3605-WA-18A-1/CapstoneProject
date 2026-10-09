# Implementation Document V1

## Feature Description and Technology Usage
This section covers the core and secondary features and the technologies used to implement them. The MVP is a single web app that also works in mobile browsers, built over 4 weeks. Domain terms such as maintenance request, ticket, job and max repair cost are defined in CONTEXT.md, and the key decisions are recorded in `docs/adr/`.

### Feature List
**Core features (MVP):**
1. **Channel connections and onboarding guide:** the agency connects its email, SMS and WhatsApp in an onboarding guide. If it doesn't have one of these channels, the guide walks the property manager through creating it.
2. **AI intake agent:** reads every message on the connected channels, works out who sent it, and sorts it into a new issue, an update on an existing ticket, a common area issue, or other.
3. **Photo upload and verification:** for a new issue, the agent sends the tenant an upload link by SMS or WhatsApp. Photos are checked automatically (photo metadata, and whether the image matches the description) before a property manager confirms the maintenance request, which creates the ticket.
4. **Urgency ranking:** every issue is ranked Urgent, High or Routine. Urgent repairs go back to the tenant to arrange; tickets are sorted so the most severe repairs are handled first.
5. **Profiles:** for properties (max repair cost, tenants, owner, nominated tradies, frequent issues), tenants and tradies (trades, ratings, quote history).
6. **Tradie recommendation:** for each job, myEST8 recommends tradies based on their myEST8 and Google ratings and their previous quotes.
7. **Quote requests and comparison:** the property manager sends quote requests to 3–4 recommended tradies and needs at least 2 quotes, compared side by side, before sending a work order.
8. **Owner approval:** the property owner approves when a ticket's repair cost would go over the property's max repair cost.
9. **Shared calendar and scheduling:** every scheduled job across the agency in one calendar, showing when each tradie is free, with suggested times when a visit needs to move.
10. **Automated updates:** status updates go to tenants, tradies and owners on the right channel at each step.
11. **Ratings:** after each job, the property manager rates the tradie and the tenant answers one question, which feeds the tradie's profile.
12. **Progress tracker:** a visual timeline of each ticket for the tenant and the property manager.

**Secondary features (after the 4-week MVP):**

| # | Feature | Notes |
|---|---|---|
| 13 | FAQ page | Answers common tenant questions. |
| 14 | FAQ-powered chatbot | Adapts its answers from the FAQ content, cutting repeat questions to the property manager. |
| 15 | Tradie sign-up | Tradie accounts. Valuable for many features, but the MVP keeps every tradie on their existing channels. |
| 16 | Tradie calendar | Tradies share their own availability. Requires tradie sign-up; the shared agency calendar covers part of this in the MVP. |
| 17 | Direct tenant–tradie connection | Tenants and tradies coordinate visits directly. |
| 18 | Tenant check-ins | The platform checks in on tenants periodically. |
| 19 | Regular tenant checks | Tenants upload images and do regular checks to catch issues early. |
| 20 | Weekly quests and tenant score | Small weekly tasks for tenants. The tenant score counts only verified tenant-caused damage and recorded lease breaches, never message volume, and stays inside the agency. |
| 21 | Voice communication | Tenants can report and discuss issues by voice. |

***
### Data and logic
**Collected:**
- From tenants: messages on email, SMS or WhatsApp, photos through the upload link (with any metadata), answers to the agent's guided questions, and a 1–5 answer to "Was it fixed properly?" after each job.
- From tradies: quotes (price and proposed time) as free-text replies or PDFs, requests to change a visit time, and confirmation that a job is done.
- From property owners: YES/NO replies to approval requests.
- From property managers: channel connections, profile details, confirmations of maintenance requests, quote and work order decisions, and a 1–5 tradie rating after each job.
- From Google: each tradie's overall Google rating, where they have a listing.

**Stored:**
- **Property profiles:** address, property owner, current tenants, max repair cost, nominated tradies, building manager contact (strata properties), frequent issues and ticket history.
- **Tenant profiles:** contact details, channels used, ticket history.
- **Tradie profiles:** contact details, trades, myEST8 rating, Google rating, quote history, scheduled jobs.
- **Maintenance requests and tickets:** description, photos, check results, urgency, status, jobs, and a log of every message.
- **Jobs:** trade, quote requests, quotes, work order, scheduled time.

**Presented:**
- **Property manager dashboard:** maintenance requests and their check results waiting for confirmation; tickets ranked by urgency; recommended tradies for each job; a side-by-side quote comparison; the shared calendar; the progress tracker; and an inbox for "other" and unknown-sender messages.
- **Tenant:** replies and status updates on their own channel, the photo upload page and the progress tracker.
- **Tradie:** quote requests, work orders and visit times on their own channel.
- **Property owner:** approval requests showing the full repair cost, and updates on their own channel.

**Logic rules:**
1. **Message handling:** the agent matches each sender to a stored tenant, tradie or owner by phone number or email. Unknown senders go to the property manager's inbox; linking one to a person saves the number for next time. The message is then sorted:
    - **New issue:** goes through verification (rule 2).
    - **Update on an existing ticket:** attached to that ticket, including duplicate reports from housemates.
    - **Common area issue:** forwarded to the building manager, and the tenant is told it has been passed on.
    - **Other:** goes to the property manager's inbox untouched.
2. **Verification:** the agent asks a few guided questions and sends an upload link by SMS or WhatsApp. Each photo gets two automatic checks: its metadata, which is flagged only if it contradicts the report (taken before the tenancy began or well before the report, or far from the property; missing metadata doesn't count against it), and whether the image matches the description. The issue is then a maintenance request. A property manager confirms it, which creates the ticket, or rejects it, and the tenant is told why.
3. **Urgency:** each issue is ranked Urgent, High or Routine. Anything threatening safety, the structure, security or an essential service (e.g. gas leak, flooding, dangerous electrical fault, no hot water) is Urgent: the agent reminds the tenant that they arrange urgent repairs themselves, using the property's nominated tradies where possible, and can claim up to $1,000 back from the property owner. When the tenant sends the receipt, it's forwarded to the owner with the property manager copied in as a third party. Damage that will worsen if left (e.g. a slow leak) is High; everything else is Routine. Tickets are sorted by urgency, then by submission time.
4. **Trades:** the trade for the ticket's first job is proposed automatically from a fixed list (plumbing, electrical, carpentry, roofing, appliances, locksmith, pest control, general handyman). The property manager can change it and add more jobs.
5. **Tradie recommendation:** for each job, recommend tradies with that trade, ranked by their myEST8 rating, Google rating and previous quotes. The two ratings are shown side by side, not combined. A tradie who isn't on Google keeps the starting rating the property manager gave them.
6. **Quotes:** the property manager sends quote requests to 3–4 recommended tradies. The agent reads the price and proposed time from each reply, and the property manager confirms them. Tradies who don't reply get a reminder after 48 hours; after 72 hours the property manager is prompted to invite more. At least 2 quotes are needed before a work order, unless the property manager records a reason for going ahead with one.
7. **Owner approval:** if approving a quote would take the ticket's repair cost over the property's max repair cost, the property owner must approve that quote. The request shows the full repair cost, and the owner replies YES or NO. The owner gets a reminder after 24 hours, and the property manager is alerted after 48. Quotes already approved stay approved.
8. **Work orders and scheduling:** the property manager sends a work order with a time slot (2 hours by default, adjustable) when the tradie is free in the shared calendar. The agent sends the slot to the tradie and the tenant, and either can ask for another time. Suggested new times are free for the tradie and fit the availability the tenant gave.
9. **Choosing the channel:** within 24 hours of a person's last message, reply on the channel they used. Otherwise send by SMS, or by email if there's no mobile number.
10. **Ratings:** when a job is done, the property manager rates the tradie 1–5 and the tenant is asked "Was it fixed properly?" (1–5). Both feed the tradie's myEST8 rating used in rule 5.

***
### Technology Implementation
The product has four main areas: communication management, classification, tradie recommendation, and scheduling and approvals. They're built as one app with internal modules (intake agent, tickets, quotes, scheduling, notifications), with the AI agent running as a background worker inside it, rather than as separate microservices. A team of 5 can't deploy and connect several services in 4 weeks, and module boundaries give the same separation (ADR-0001).

Only property managers have accounts. Tenants, tradies and property owners use their existing channels, apart from the no-login photo upload page (ADR-0002).

#### Communication Management Solution Design
Messages arrive from the agency's email, SMS and WhatsApp. Photos need verification before a ticket is created and sent to the property manager.

|Feature Name|Technology Name|Usage & Cohesion|
|---|---|---|
|Connection to multiple channels|Twilio Programmable Messaging (SMS, WhatsApp); Twilio SendGrid Inbound Parse (email)|Every inbound message reaches the app as a webhook from one provider. The agency forwards its inbox for email. The demo uses Twilio's WhatsApp sandbox (ADR-0003).|
|Sending updates|Twilio Programmable Messaging; Twilio SendGrid|Replies go on the channel the person last used within 24 hours, otherwise by SMS or email. WhatsApp messages sent first need pre-approved templates (only 3 preset ones in the sandbox).|
|Agent replies|Fixed message templates|Predictable and cheap. Jev can't write text, so every reply the agent sends comes from a template.|
|Sender matching|App database|Phone numbers and email addresses are matched to stored tenants, tradies and owners. Unknown senders go to the property manager's inbox.|
|Photo upload|Upload page in the web app, linked by SMS or WhatsApp|No login. Gets photos past WhatsApp and MMS compression, which strips metadata.|
|Photo metadata check|EXIF reader library (chosen with the stack)|Flags only contradictions: taken before the tenancy began or well before the report, or far from the property.|
|Image matches description|Vision-capable LLM (TBD)|Checks that the photo shows the reported issue. The result is shown to the property manager.|
|Channel onboarding guide|Web app pages|Connects each channel and guides the property manager through creating any the agency doesn't have.|

#### Classification Solution Design
Our app structures each request to Jev: the message (with the ticket's context) as the state, and each decision as a typed question with a fixed set of answers. Jev answers all questions in one pass, with confidence scores.

|Feature Name|Technology Name|Usage & Cohesion|
|---|---|---|
|Message class|Jev (TypeSafe AI)|New issue / Update on an existing ticket / Common area issue / Other.|
|Urgency|Jev (TypeSafe AI)|Urgent / High / Routine. Urgent sends the urgent repair reminder instead of starting verification.|
|Trade|Jev (TypeSafe AI)|One of the fixed trades, proposed for the ticket's first job. The property manager can change it.|
|Owner approval replies|Jev (TypeSafe AI)|Reads a property owner's reply as YES or NO.|

#### Tradie Recommendation Solution Design
|Feature Name|Technology Name|Usage & Cohesion|
|---|---|---|
|Tradie ranking|App logic over stored ratings and quote history|Ranks tradies with the job's trade by myEST8 rating, Google rating and previous quotes.|
|Google ratings|Google Places API|Overall rating for tradies with a Google listing, shown next to the myEST8 rating rather than combined.|
|Quote reading|TBD (Jev or the vision-capable LLM)|Reads the price and proposed time from tradie replies and PDF quotes. The property manager confirms them.|

#### Scheduling and Approvals Solution Design
|Feature Name|Technology Name|Usage & Cohesion|
|---|---|---|
|Shared calendar|Web app|Shows every scheduled job across the agency and when each tradie is free. Slots are 2 hours by default.|
|Reschedule suggestions|App logic|Suggests times that are free for the tradie and fit the tenant's stated availability.|
|Reminders and deadlines|Background worker|Quote reminders (48 h, then a prompt to invite more at 72 h) and owner approval reminders (24 h, then a property manager alert at 48 h).|
|Progress tracker|Web app|A timeline built from ticket and job statuses, for the property manager and, through a link, the tenant.|

#### Open decisions
- Web framework, database and hosting.
- Which vision-capable LLM checks photos.
- Which model reads prices and times from tradie replies and PDF quotes.

#### Notes for later
- Security review of the no-login photo upload links.
- Privacy of stored photos and photo location metadata.
- Production WhatsApp setup: each agency registers its own WhatsApp sender and templates for quote requests, work orders, approval requests and reminders.
