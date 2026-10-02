# INFS3605-W18A-1 Capstone Project
Designing an AI Solution for SMB founders decision making being distributed within the organisation or designing AI capabilities for new workflows

**Survey for Real Estate Agents: https://forms.gle/fd6GaQW8e9yanR2TA**

**Click here for the documentation page: https://infs3605-wa-18a-1.github.io/CapstoneProject/**

## Context and Research
Desk research narrowed the brief from Australian SMBs in general to small property management agencies:

- **SMB admin load:** Australian businesses spend about 81 days a year on admin ([Sage](https://sage-productivity.azurewebsites.net/assets/report-on-admin-burden.pdf)), and with most having few or no staff, the owner is usually the only decision-maker ([ABS](https://www.abs.gov.au/statistics/economy/business-indicators/counts-australian-businesses-including-entries-and-exits/jul2022-jun2026)).
- **AI adoption is shallow:** 69% of SMBs use AI regularly but only 12% call it core to operations ([Intuit QuickBooks](https://quickbooks.intuit.com/au/blog/news/ai-impact-report-australia-2026/)). Non-adopters mostly cite trust and relevance, not cost ([National AI Centre](https://www.ai.gov.au/news-and-insights/blog/ai-adoption-insights-december-2025-february-2026)).
- **Sector choice:** We compared construction, agriculture and real estate management, then chose real estate. 66% of property managers say their workload is busy or far too busy and 29% intend to leave ([MRI via Real Estate Business](https://www.realestatebusiness.com.au/industry/28712-property-manager-exodus-at-all-time-high-but-is-the-industry-turning-a-corner)). Only 18% of property organisations call their AI capability "stable and growing" ([Yardi & Property Council](https://www.realestatebusiness.com.au/tech/31513-ai-adoption-accelerates-but-confidence-lags-in-property-sector)).
- **Repairs are the bottleneck:** Half of renters live in homes needing repairs ([UNSW *Rights at Risk*](https://www.unsw.edu.au/newsroom/news/2025/06/seven-in-ten-renters-scared-to-ask-for-repairs-report)), construction trades are in national shortage ([Jobs and Skills Australia](https://www.jobsandskills.gov.au/publications/towards-national-jobs-and-skills-roadmap-summary/current-skills-shortages)), and properties per manager fell from 122 to 116 between 2019 and 2023 ([Macquarie](https://www.macquarie.com.au/assets/bfs/documents/business-banking/bb-real-estate-industry/macquarie-bank-real-estate-benchmarking-report-2023.pdf)).

The survey above gathers first-hand input from real estate agents. Full findings are on the [Market Research](https://infs3605-wa-18a-1.github.io/CapstoneProject/market-research/) page.

## Problem Statement V 1
**Reframed Problem Statement**
>With aging buildings and lacking incentives to repair, property managers spend too much time coordinating maintainence repairs for severe repairs. It affects the small or individual property managers who spend a majority of their time on communication and matianence admin work. This process bottleneck needs to change because it reduces property managers ability to satisfy residents and gain additional properties for management.

**HMW** help small property managers coordinate severe building repairs with less time spent on communication and admin, so they can keep residents satisfied and take on more properties?

1. Martin, C. et al. (June 2025). [_Rights at Risk: Rising Rents and Repercussions_](https://www.unsw.edu.au/newsroom/news/2025/06/seven-in-ten-renters-scared-to-ask-for-repairs-report). UNSW City Futures Research Centre with the ACOSS-UNSW Sydney Poverty and Inequality Partnership, National Shelter and NARO. It is a national survey of 1,019 private renters.
	- 50% of renters live in homes needing repairs, and 10% need urgent repairs.
	- 74% had some defect or issue. The most common were pests (31%), leaks or flooding (24%), hot water problems (21%) and mould in bathrooms (18%).
	- 68% worry that asking for repairs could trigger a rent increase, and 56% fear eviction. That tension makes repair coordination harder for managers.
	- It is not Sydney-specific, and it covers tenants rather than managers' time.
2. Aidan Devine (Sept 2025). [Dirty truth behind rise of horror living conditions in rental homes](https://www.realestate.com.au/news/dirty-truth-behind-rise-of-horror-living-conditions-in-rental-homes/). Survey of 148 landlords 
	- Finder survey of 148 landlords found that two in five (38 per cent) have had a tenant wait longer than is reasonable for a repair in the last year.