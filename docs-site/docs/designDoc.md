# Solution Design Document v1
**Team:** W18A-1  ·  **Seminar:** Wed 6 pm

**Members:** Kevin He, John Le, Ho Nam Wong, Sophia Lu, Lena Le

**Roles today:** Facilitators John Le and Lena Le, Timekeeper Sophia Lu, Scribe Kevin He, Challenger Ho Nam Wong

**Problem statement v1:** With ageing buildings and a lack of incentives to repair, property managers spend too much time coordinating severe maintenance repairs. This affects small and individual property managers, who spend most of their time on communication and maintenance admin. This bottleneck needs to change because it limits property managers' ability to keep residents satisfied and take on more properties.

**How Might We:** How might we build a product that makes communication about maintenance and tenant requests faster and easier to manage?

## 1. Concept and idea card
**Selected concept:** Dashboard and ticketing system with profiles  
**Why:** It covers all the issues in the problem statement while staying viable for user adoption.  
**Rejected, and why:** Automated Maintenance Assignment and Communication Forwarding and Rewording were rejected as standalone products. They were too wide in scope, and development could easily run into scope creep. Their strongest parts carry over as features of the selected concept: automated tradie recommendation and automated communication.

| Concept | Desirable | Feasible | Viable | Total |
|---|---|---|---|---|
| Dashboard and ticketing system with profiles | 3 | 3 | 2 | 8 |
| Automated Maintenance Assignment | 2 | 2 | 2 | 6 |
| Communication Forwarding and Rewording | 3 | 2 | 1 | 6 |

**Headline:** myEST8  
**Description:** A ticket ingestion and prioritisation platform for small property management agencies. Tenants report issues on the channels they already use: email, SMS or WhatsApp. An AI agent sorts each message, asks for photos through an upload link, checks them, and passes the issue to a property manager to confirm as a ticket. Tickets are ranked by urgency. For each job, myEST8 recommends tradies from their ratings and past quotes; the property manager requests quotes from 3–4 of them, compares at least two, and sends a work order. Property owners only approve when costs go over the property's limit, a shared calendar stops the agency double-booking tradies, and automated updates keep tenants, tradies and owners informed on their own channels.  
**Problem statement:** With ageing buildings and a lack of incentives to repair, property managers spend too much time coordinating severe maintenance repairs. This affects small and individual property managers, who spend most of their time on communication and maintenance admin. This bottleneck needs to change because it limits property managers' ability to keep residents satisfied and take on more properties.  
**Users and stakeholders:** Property managers (in agencies of 3–4), property owners, tradies, tenants, building managers (for common areas)  
**Value:** This solution matters because it frees up property managers' time to find and sign new property owners. It cuts the admin work that holds property management businesses back, and comparing at least two quotes per job helps keep maintenance costs down.  
**Next steps and questions:**
- Verify viability through additional interviews.
- Security and privacy review of the photo upload links and stored photo metadata (deferred for the MVP).
- Check the urgent repair flow against NSW rules: tenants may arrange urgent repairs themselves only if they can't reach the landlord or agent, or the repair isn't done in a reasonable time.
- Jev has been in limited early access since 15 September 2026. Confirm our access and usage limits will last the whole build.
- Test whether photos uploaded from phone browsers keep their metadata.

## 2. Persona and user stories
**Persona:** Mohammed, 42, property manager  
**Description:** Runs a small property management business (~3 employees) and oversees about 100 properties within one suburb.  
**Behaviours:**
- Uses AI a lot and often seeks guidance.
- Spends a lot of time on admin work.
- Communicates sporadically.
- Checks email in the morning, then only occasionally.

**Needs and goals:**
- Needs to grow the business by finding and talking to more clients.
- Needs to keep tenants and property owners happy so they stay.
- Goal: grow overall revenue and manage more properties.
- Goal: reduce costs and expenses.

**Evidence:** Questionnaire

**Epics:** Capturing the full process for tradie discovery and quoting; Understanding the communication pathways; Creating wireframes for the solution; Building the core maintenance request workflow (MVP)

| # | Epic | User story | Acceptance criteria | Size |
|---|---|---|---|---|
| 1 | Capturing the full process for tradie discovery and quoting | As a developer, I want a structured breakdown of each task and the endpoints connected to it, so that I can design an end-to-end process. | 1 BPMN diagram and 1 design document for the process. | M |
| 2 | Capturing the full process for tradie discovery and quoting | As the BA/Scrum Master, I want to take the requirements from this process and break them down into tickets, so that the developers have tasks that work towards impact. | 5 developer tickets created; 1 documentation submission. | M |
| 3 | Capturing the full process for tradie discovery and quoting | As the Product Owner, I want a tracker that maps tickets to user requirements, so that I can monitor progress towards business impact. | 1 ticket progress tracker the Product Owner can view. | L |
| 4 | Understanding the communication pathways | As a developer, I want a map of every channel tenants, tradies and property owners use to contact the property manager, so that I know which channels the intake agent must support. | 1 communication pathway map covering email, SMS and WhatsApp. | S |
| 5 | Creating wireframes for the solution | As a designer, I want wireframes of the main processes, so that we can check the flow with users before building it. | Process wireframes in Figma, reviewed by the team (scope TBD). | M |
| 6 | Building the core maintenance request workflow (MVP) | As a property manager, I want a guide to connect our email, SMS and WhatsApp, so that I can set myEST8 up without technical help. | The guide connects all 3 channels and explains how to create any the agency doesn't have. | M |
| 7 | Building the core maintenance request workflow (MVP) | As a tenant, I want to report an issue on the channel I already use and send photos through a link, so that I don't have to download an app or create an account. | Messages on email, SMS and WhatsApp reach the agent; a new issue gets an upload link; uploaded photos attach to the maintenance request. | L |
| 8 | Building the core maintenance request workflow (MVP) | As a property manager, I want each reported issue checked automatically before I confirm it, so that I only spend time on genuine issues. | Each maintenance request shows its photo metadata and image-match results; confirming creates a ticket; rejecting tells the tenant why. | M |
| 9 | Building the core maintenance request workflow (MVP) | As a property manager, I want tickets ranked by urgency, so that I deal with the most severe repairs first. | Every issue is ranked Urgent, High or Routine; urgent issues get the urgent repair reminder; the dashboard sorts tickets by urgency, then by submission time. | M |
| 10 | Building the core maintenance request workflow (MVP) | As a property manager, I want recommended tradies for each job and at least two quotes side by side, so that I can choose the best option in one place. | 3–4 recommended tradies per job; quote requests sent in one click; at least 2 quotes compared before a work order can be sent. | L |
| 11 | Building the core maintenance request workflow (MVP) | As a property owner, I want to approve repairs only when they go over my limit, so that I keep control of big costs without being asked about small jobs. | Approval is requested only when the repair cost would exceed the property's max repair cost; the YES/NO reply is recorded. | M |
| 12 | Building the core maintenance request workflow (MVP) | As a property manager, I want to see when tradies are already booked by my colleagues, so that we don't double-book the same tradie. | The shared calendar shows every scheduled job; new times are suggested when a tradie or tenant asks to reschedule. | L |
| 13 | Building the core maintenance request workflow (MVP) | As a property manager, I want automatic updates sent on each person's own channel, so that I don't have to relay messages myself. | Updates are sent at each status change, on the channel last used within 24 hours, otherwise by SMS or email. | M |
| 14 | Building the core maintenance request workflow (MVP) | As a property manager, I want to rate each tradie after a job, so that recommendations improve over time. | The property manager gives a 1–5 rating; the tenant is asked "Was it fixed properly?"; both show on the tradie's profile. | S |

## 3. User flow

**Tenant flow:**
1. Message the agency on the channel they already use: email, SMS or WhatsApp.
2. If the issue is urgent, receive a reminder that tenants arrange urgent repairs themselves, using the property's nominated tradies where possible, and can claim up to $1,000 back from the property owner. Send the receipt to the agency, which forwards it to the owner. The flow ends here for urgent repairs.
3. Otherwise, answer a few guided questions from the agent and upload photos through the link it sends.
4. Receive a confirmation when the property manager confirms the issue as a ticket, or the reason if it's rejected.
5. Follow progress through status updates and the progress tracker.
6. Confirm the visit time, or ask for another one.
7. Let the tradie in, then answer "Was it fixed properly?" (1–5).

**Property manager flow:**
1. Set up the agency in the onboarding guide: connect email, SMS and WhatsApp, then add properties (with max repair cost, tenants, owner and nominated tradies) and existing tradies (with a starting rating).
2. Review maintenance requests with their automatic check results, and confirm or reject each one. Confirming creates a ticket, ranked by urgency on the dashboard.
3. Check the trade proposed for the ticket's first job, and add any further jobs.
4. For each job, choose 3–4 recommended tradies and send them quote requests.
5. Confirm the price and time read from each quote, then choose one once at least two are in.
6. If the repair cost would go over the property's max repair cost, wait for the owner's approval.
7. Send the work order with a time slot when the tradie is free on the shared calendar, and accept a suggested new time if the tradie or tenant asks to move it.
8. Rate the tradie when the job is done, and close the ticket once all its jobs are done.

**Tradie flow:**
1. Receive a quote request by SMS, email or WhatsApp.
2. Reply with a price and proposed time, as a message or a PDF quote.
3. If chosen, receive a work order with the visit time, and reply to ask for a different time if needed.
4. Do the job and reply to say it's done.

**Property owner flow:**
1. Receive an approval request when a ticket's repair cost would go over the property's max repair cost. It shows the full repair cost.
2. Reply YES or NO. A reminder follows after 24 hours.
3. Receive updates as the work goes ahead.
4. For urgent repairs the tenant arranged, receive the tenant's receipt, forwarded with the property manager copied in, and reimburse the tenant up to $1,000.

## 4. Prioritised feature list

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

The MVP is a web app that also works in mobile browsers, built over 4 weeks.

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

## 5. Data and logic
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

## 6. Platform shortlist
| Platform | Handles well | Trade-off | Our skill (1–3) |
|---|---|---|---|
| GitHub | Repository hosting and change tracking | Merge conflicts, and a learning curve for anyone new to Git | 2 |
| VS Code | Code editing, extensions and built-in Git integration | Only an editor: hosting and deployment need other tools | _TBC_ |
| Figma | Process wireframes | Wireframes only; prototypes aren't functional | _TBC_ |
| Canva | Quick visuals and presentation slides | Limited for interactive wireframes | _TBC_ |
| Twilio | SMS, WhatsApp and (through SendGrid) inbound email from one provider | The WhatsApp sandbox only allows 3 preset templates for messages we send first; real WhatsApp numbers need Meta verification | _TBC_ |
| Jev (TypeSafe AI) | Fast, typed classification decisions: message type, urgency, trade | Text and JSON input only, can't write replies, limited early access | _TBC_ |

**Preferred:** One web app that works in mobile browsers, built with GitHub and VS Code, using Twilio for the messaging channels and Jev for classification. Figma is used only for process wireframes (TBD).  
**Reason:** Twilio covers all three channels with one integration. Jev returns typed decisions quickly and cheaply, which suits sorting every incoming message. A single app with internal modules keeps a 4-week build manageable for a team of 5.

## Peer check
**Strength we received:** _To be completed after the peer review session_  
**Question we received:** _To be completed after the peer review session_

