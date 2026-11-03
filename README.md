# AI Agents & Customer Support Analytics

A Power BI report that compares **AI-only**, **AI-to-human** and **human-only** customer support on cost, resolution, SLA compliance, repeat contacts and customer satisfaction, and shows where handoffs and routing break down.

Built for the **FP20 Analytics Challenge: AI Agents & Customer Support** (October 2026).

> **Report preview:** _add screenshots to `/images` and link them here_
> **Published report:** _add the Publish-to-web / novyPro link here_

---

## Business questions

The report answers the 15 challenge questions, grouped as:

1. **Trends and movers**: how key metrics change over time, which measures improve or decline, and where there are outliers.
2. **Segments and drivers**: which segments, channels, issue types and customer groups explain performance differences.
3. **AI vs human**: how the three support paths compare on cost, resolution time, CSAT and SLA, including at the same complexity level.
4. **Handoffs**: how escalation reasons and handoff timing relate to queue time, resolution and satisfaction.
5. **Repeat contacts**: where repeat contacts within 7 days add the most cost.
6. **Customers**: how tiers, lifecycle stages and languages differ.
7. **Recommendations**: what leaders should prioritise.

## Dataset

- **Source:** FP20 Analytics Challenge 42 dataset, `AI_Agents_Customer_Support_Challenge_clean.xlsx` (download it from the [challenge page](https://fp20analytics.com/live-challenge/)).
- **Size:** 12,000 tickets, created 1 Jan 2025 to 30 Sep 2026, one row per ticket.
- **Costs are synthetic** (per the data dictionary), so treat cost figures as indicative.


## Data model

A star schema with one fact table and five dimensions, all one-to-many with single-direction filters.

| Table | Role | Key |
| --- | --- | --- |
| `FactSupportTickets` | Ticket-level facts (resolution, SLA, CSAT, costs, queue and handling minutes, escalation reason, handoff band) | `TicketID` |
| `DimDate` | Calendar (marked as date table) | `Date` |
| `DimIssue` | Issue category, subcategory, support team | `IssueKey` |
| `DimProduct` | Product family and name | `ProductKey` |
| `DimCustomerProfile` | Region, country, language, tier, lifecycle (a segment combination, not a unique customer) | `ProfileKey` |
| `DimSupportRoute` | Channel and support path (AI only, AI to human, Human only) | `RouteKey` |

Helper tables hold measures, dynamic card text and field parameters. See [docs/data-dictionary.md](docs/data-dictionary.md) and [dax/measures.md](dax/measures.md).

## Key metrics and definitions

| Metric | Definition |
| --- | --- |
| Initial resolution rate | Tickets initially resolved ÷ all tickets |
| SLA attainment | Tickets that met the SLA target **and** were resolved at first contact ÷ all tickets |
| Repeat-contact rate | Share of tickets with at least one repeat contact within 7 days |
| Total repeat contacts | Count of all repeat contacts within 7 days (a ticket can have up to 2) |
| Average CSAT | Average of recorded scores only; missing scores are survey non-response and are not imputed |
| CSAT response rate | Tickets with a CSAT score ÷ all tickets |
| Total support cost | AI cost + human cost + repeat cost |
| Cost per ticket | Total support cost ÷ tickets |
| AI handoff rate | AI-to-human tickets ÷ all AI-handled tickets (AI only + AI to human) |

## Headline findings

All figures were recomputed from the source workbook and cross-checked against the Power BI model.

| KPI | Value |
| --- | --- |
| Tickets | 12,000 |
| Total support cost | €162.7K (human €128.5K, repeat €28.7K, AI €5.6K) |
| Cost per ticket | €13.56 |
| Initial resolution | 95.0% |
| Repeat-contact rate | 16.3% (2,360 repeat contacts) |
| SLA attainment | 82.9% |
| Average CSAT | 4.05 (41.6% response rate) |
| Average resolution | 6.0 hours |

**AI vs human**

| Path | Tickets | Cost per ticket | CSAT | SLA | Avg resolution | Repeat rate |
| --- | --- | --- | --- | --- | --- | --- |
| AI only | 4,724 | €1.46 | 4.40 | 95.6% | 0.7 h | 10.9% |
| AI to human | 3,456 | €27.09 | 3.52 | 83.3% | 6.7 h | 26.0% |
| Human only | 3,820 | €16.28 | 4.08 | 66.7% | 12.0 h | 14.2% |

- AI-only is cheapest and best rated at every complexity level. Handoffs are the costliest path at every complexity level, though they also take the hardest tickets (33% high complexity, against 11% for AI-only).
- **Handoff timing matters more than queue time.** Handoffs after more than 12 minutes of AI time have CSAT 2.92 and 10.1-hour resolution, against about 3.7 and 5.3 to 5.8 hours for earlier handoffs.
- **Email is the SLA gap.** 61.4% SLA, against about 92% for chat and messaging. Business-tier customers using email reach only 24.0%.
- **Repeats cost €28.7K** (17.6% of total cost); more than half comes from the AI-to-human path.
- **AI-only share rose** from 36.4% to 42.6% (Jan to Sep, 2025 vs 2026) while cost per ticket and CSAT stayed flat.
- **Best fit for AI-only:** order tracking, password reset, invoice copy. **Most need humans:** billing disputes and integration errors.

These are associations in a single dataset, not tested interventions.

## Recommendations

1. Hand off within 5 minutes for missing-technical-knowledge and policy-restriction cases, and cap AI time at about 12 minutes.
2. Route billing disputes and integration errors straight to a human.
3. Fix email service for Business customers with a dedicated queue, or shift them to chat and messaging.
4. Reduce repeats after AI-to-human handoffs (better handoff summaries, follow-up checks).
5. Keep growing AI-only for simple, high-volume issues.
6. Raise AI-only use for new customers, who cost more and repeat more.
7. Investigate the Aug 2026 cost spike and plan for the Nov peak.

The full written summary is in [docs/key-insights.md](docs/key-insights.md).

## Tools

Power BI Desktop, DAX, Power Query, ZoomCharts Drill Down visuals.

## Repository structure

```
.
├── README.md
├── docs/
│   ├── data-dictionary.md   # fields, definitions, data notes
│   └── key-insights.md      # answers to the 15 challenge questions
├── dax/
│   └── measures.md          # measure definitions, grouped by table
├── report/                  # place the .pbix here
└── images/                  # report screenshots
```

## Using this project

1. Download the dataset from the challenge page.
2. Open the `.pbix` from `/report` in Power BI Desktop and point the data source at your local copy of the workbook.
3. Refresh.

## Known limitations

- CSAT comes from about 42% of tickets, and some groups have few scores (for example identity-verification handoffs, 41 scores).
- Costs are synthetic.
- Support-path comparisons are not adjusted for factors the data does not record.
- The date table runs to Dec 2026 but tickets end Sep 2026, so filter trends to the data range.
- Categories such as handoff band, complexity and priority need an explicit sort column (built in Power Query) to display in logical order.

## Author

**Amit Bahedia**

Data & Research Professional

https://www.linkedin.com/in/amitbahedia/

## Acknowledgements

Dataset and challenge by [FP20 Analytics](https://fp20analytics.com/) in collaboration with ZoomCharts.

