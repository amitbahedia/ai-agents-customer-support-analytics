# Key insights: answers to the 15 challenge questions

All figures come from the 12,000-ticket dataset (Jan 2025 to Sep 2026), recomputed from the source workbook and cross-checked against the Power BI model. They show associations, not proven causes. See [data-dictionary.md](data-dictionary.md) for definitions.

## Headline

AI-only support resolves 39% of tickets at €1.46 each with the highest satisfaction (CSAT 4.40). The cost and quality problems sit in AI-to-human handoffs, email service levels, Business-tier customers and repeat contacts.

## Q1, Q3, Q7: trends, movers, outliers

Same months compared (Jan to Sep):

| Metric | 2025 | 2026 |
| --- | --- | --- |
| Tickets | 4,673 | 5,185 |
| AI-only share | 36.4% | 42.6% |
| AI-to-human share | 28.6% | 29.8% |
| Cost per ticket | €13.62 | €13.57 |
| Average CSAT | 4.05 | 4.05 |
| SLA attainment | 83.5% | 82.8% |
| Repeat-contact rate | 15.8% | 17.0% |
| Average resolution (h) | 6.0 | 5.8 |

- AI-only share is the clear improvement. Cost per ticket did not fall because handoffs also rose slightly and the repeat rate rose 1.2 points.
- **Nov 2025:** 840 tickets against about 550 typically, with 78.8% SLA and 8.1-hour resolution, driven by Orders & delivery (358 tickets against 191 in Oct). Looks seasonal; the data gives no cause.
- **Aug 2026:** €17.76 cost per ticket on normal volume (627 tickets) and the highest monthly repeat rate (19.3%). Not one day or ticket; needs a closer look.

## Q2, Q4, Q5, Q6, Q8: segments, gaps, drivers

| Dimension | Best | Worst |
| --- | --- | --- |
| Support path, cost per ticket | AI only €1.46 | AI to human €27.09 |
| Channel, SLA | Live chat 92.2% | Email 61.4% |
| Customer tier, SLA | Standard 93.0% | Business 55.2% |
| Issue category, cost per ticket | Account access €5.35 | Technical support €23.96 |
| Issue category, SLA | Account access 86.5% | Billing & refunds 73.9% |
| Priority, SLA | Normal 85.9% | Urgent 64.1% |
| Complexity, CSAT | Low 4.2 to 4.3 | High 3.4 to 3.6 |
| Region, SLA | Southern Europe 86.1% | Western Europe 79.7% |
| Product family, CSAT | Devices 4.06 | Commerce 4.02 |

- Support path, channel, issue type and customer tier explain most differences. Region and product family explain little.
- Most activity: Orders & delivery (31% of tickets), live chat (47%), Western Europe (40%).
- Among tickets with CSAT, repeat contact (correlation −0.49), first-contact resolution (+0.45) and SLA met (+0.44) track satisfaction most.
- 56% of email tickets are human-only, against 22% in chat. Half of Business tickets use email, against about a quarter for other tiers.

## Q11, Q12: AI-only vs AI-to-human vs human-only

| Path | Tickets | Cost per ticket | CSAT | SLA | Avg resolution (h) | Repeat rate |
| --- | --- | --- | --- | --- | --- | --- |
| AI only | 4,724 | €1.46 | 4.40 | 95.6% | 0.7 | 10.9% |
| AI to human | 3,456 | €27.09 | 3.52 | 83.3% | 6.7 | 26.0% |
| Human only | 3,820 | €16.28 | 4.08 | 66.7% | 12.0 | 14.2% |

Same complexity, cost per ticket / CSAT / SLA:

| Complexity | AI only | AI to human | Human only |
| --- | --- | --- | --- |
| Low | €1.04 / 4.45 / 96.3% | €16.21 / 3.75 / 84.1% | €7.94 / 4.18 / 70.2% |
| Medium | €1.48 / 4.46 / 95.1% | €24.81 / 3.68 / 84.2% | €17.37 / 4.18 / 65.0% |
| High | €3.33 / 3.94 / 94.2% | €37.95 / 3.11 / 81.5% | €28.99 / 3.74 / 63.7% |

The handoff path takes the hardest tickets (33% high complexity, against 11% for AI-only and 22% for human-only), but is still the costliest path within every complexity level. AI-only tickets may also be easier in ways the data does not record.

Issue suitability (product family barely matters; route by issue type):

| Issue | Tickets | AI-only share | Handoff share | Cost per ticket | CSAT | Repeat rate |
| --- | --- | --- | --- | --- | --- | --- |
| Order tracking | 1,870 | 60.1% | 10.2% | €2.45 | 4.33 | 11.5% |
| Password reset | 1,233 | 58.6% | 10.4% | €2.05 | 4.25 | 10.6% |
| Invoice copy | 593 | 55.6% | 12.5% | €3.15 | 4.32 | 11.5% |
| Missing parcel | 650 | 21.1% | 49.1% | €22.07 | 3.73 | 18.9% |
| Cancellation | 413 | 20.3% | 50.6% | €20.05 | 3.93 | 19.1% |
| Billing dispute | 546 | 6.2% | 57.3% | €39.85 | 3.19 | 32.8% |
| Integration error | 753 | 10.6% | 57.9% | €48.78 | 3.51 | 29.9% |

## Q13: escalation reasons and handoff timing

AI-to-human tickets only (3,456).

| Handoff timing | Tickets | CSAT | Queue (h) | Resolution (h) | Repeat rate | Cost per ticket |
| --- | --- | --- | --- | --- | --- | --- |
| Up to 5 min | 753 | 3.72 | 2.4 | 5.3 | 22.4% | €20.46 |
| 5 to 12 min | 1,902 | 3.69 | 2.6 | 5.8 | 24.8% | €23.42 |
| Over 12 min | 801 | 2.92 | 3.0 | 10.1 | 32.1% | €42.06 |

| Escalation reason | Tickets | CSAT | Queue (h) | Resolution (h) | Cost per ticket | Handed off after 12+ min |
| --- | --- | --- | --- | --- | --- | --- |
| Missing technical knowledge | 436 | 3.12 | 2.5 | 3.9 | €55.98 | 73.9% |
| Policy restriction | 571 | 3.25 | 2.9 | 13.9 | €35.27 | 42.4% |
| Identity verification | 122 | 3.44 | 2.5 | 3.1 | €22.86 | 11.5% |
| Action unavailable | 675 | 3.61 | 2.6 | 5.5 | €19.72 | 9.5% |
| Customer requested human | 808 | 3.68 | 2.7 | 5.6 | €19.70 | 10.3% |
| Low answer confidence | 844 | 3.72 | 2.7 | 5.8 | €20.22 | 9.0% |

- Worst case: policy-restricted tickets handed off after 12 minutes (242 tickets) have CSAT 2.66, 22.1-hour resolution and 59% SLA.
- Longer queues stretch resolution and SLA but barely move CSAT: between queues under 1 hour and over 8 hours, resolution rises from 4.4 to 20.1 hours and SLA falls from 87% to 62%, while CSAT moves from 3.53 to 3.33.
- Minutes with the AI correlate with CSAT at −0.29, queue minutes at −0.06.
- Only about 40% of these tickets have a CSAT score.

## Q14: repeat contacts and their cost

Repeat contacts add €28.7K, 17.6% of total cost; €15.6K of it is on the AI-to-human path.

| Path | First-contact result | Tickets | Repeat rate | Repeat contacts | Repeat cost | Per ticket |
| --- | --- | --- | --- | --- | --- | --- |
| AI only | Resolved | 4,551 | 10.0% | 556 | €4,274 | €0.94 |
| AI only | Not resolved | 173 | 33.5% | 69 | €603 | €3.49 |
| AI to human | Resolved | 3,188 | 24.0% | 916 | €13,388 | €4.20 |
| AI to human | Not resolved | 268 | 49.6% | 154 | €2,166 | €8.08 |
| Human only | Resolved | 3,662 | 13.5% | 603 | €7,539 | €2.06 |
| Human only | Not resolved | 158 | 32.3% | 62 | €725 | €4.59 |

Resolved handoff tickets are the biggest pool (47% of repeat cost) because there are many of them. Unresolved tickets repeat two to three times as often but are only 599 tickets (about 12% of repeat cost).

## Q15: tiers, lifecycle, languages

| Tier | Tickets | SLA | CSAT | Cost per ticket | Avg resolution (h) |
| --- | --- | --- | --- | --- | --- |
| Business | 2,636 | 55.2% | 3.94 | €15.55 | 9.0 |
| Plus | 3,798 | 87.2% | 4.07 | €13.38 | 5.2 |
| Standard | 5,566 | 93.0% | 4.12 | €12.74 | 5.2 |

| Language | Tickets | SLA | CSAT | Cost per ticket | Repeat rate |
| --- | --- | --- | --- | --- | --- |
| English | 3,591 | 83.6% | 4.04 | €13.03 | 15.0% |
| German | 2,746 | 75.2% | 4.03 | €14.90 | 16.0% |
| French | 2,068 | 85.6% | 4.07 | €14.24 | 17.0% |
| Spanish | 1,537 | 86.8% | 4.10 | €10.36 | 16.2% |
| Italian | 1,101 | 85.2% | 4.02 | €13.79 | 19.3% |
| Swedish | 957 | 87.1% | 4.06 | €15.09 | 17.2% |

- Business tickets on email (1,306) reach only 24.0% SLA, against 86.2% on live chat.
- German's low SLA is mostly tier mix: 44% of German tickets are Business, against 10% to 17% for other languages.
- New customers cost €15.68 per ticket against €12.55 for established ones, with a 19.4% repeat rate against 14.8%. They get AI-only service less often (35% against 41%).
- CSAT varies little by tier, lifecycle or language (3.9 to 4.1), so gaps show up in SLA, cost and repeats.

## Q9, Q10: opportunities and recommendations

| Priority | Action | Evidence |
| --- | --- | --- |
| 1 | Hand off within 5 minutes for missing technical knowledge and policy restrictions; cap AI time at about 12 minutes | Handoffs after 12 min: CSAT 2.92, €42.06 per ticket |
| 2 | Send billing disputes and integration errors straight to a human | 57% to 58% handed off anyway, at €39.85 and €48.78 per ticket |
| 3 | Fix email service for Business customers | Business email SLA 24.0% |
| 4 | Reduce repeats after AI-to-human handoffs | €13.4K repeat cost on resolved handoff tickets |
| 5 | Keep growing AI-only for order tracking, password reset, invoice copy | 56% to 60% AI-only, €2 to €3 per ticket |
| 6 | Raise AI-only use for new customers | €15.68 per ticket against €12.55 |
| 7 | Investigate the Aug 2026 cost spike and plan for the Nov peak | €17.76 per ticket in Aug 2026; 840 tickets in Nov 2025 |

Urgent tickets also miss SLA most often (64.1%) at every complexity level. Test actions 1 and 3 on a subset before rolling them out.
