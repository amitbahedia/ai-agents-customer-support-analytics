# Data dictionary

Source: `AI_Agents_Customer_Support_Challenge_clean.xlsx` (FP20 Analytics Challenge 42), sheet `Data Dictionary`, with observed values added where noted.

## FactSupportTickets (12,000 rows, one per ticket)

| Column | Type | Meaning |
| --- | --- | --- |
| TicketID | Text | Unique root episode identifier; no repeated follow-up rows |
| CreatedDate | Date | Date-only creation cohort; sole calendar relationship |
| IssueKey | Whole number | Foreign key to `DimIssue` |
| ProfileKey | Whole number | Foreign key to `DimCustomerProfile` |
| ProductKey | Whole number | Foreign key to `DimProduct` |
| RouteKey | Whole number | Foreign key to `DimSupportRoute` |
| Complexity | Text | Ticket-level categorisation. Observed values: Low, Medium, High |
| Priority | Text | Ticket-level categorisation. Observed values: Normal, High, Urgent |
| InitialResolvedFlag | Whole number | 1 = initially solved; 0 = closed without solution |
| RepeatContactCount7d | Whole number | 0, 1 or 2 linked same-issue repeat contacts within 7 days of initial closure |
| CSATScore | Whole number | 1 to 5 rating; null means survey non-response |
| FirstResponseMinutes | Decimal | Initial-stage duration in minutes; zero when that stage does not occur |
| AIConversationMinutes | Decimal | Initial-stage duration in minutes; zero when that stage does not occur |
| HumanHandlingMinutes | Decimal | Initial-stage duration in minutes; zero when that stage does not occur |
| QueueMinutes | Decimal | Initial-stage duration in minutes; zero when that stage does not occur |
| ResolutionHours | Decimal | Elapsed hours to initial closure; includes asynchronous wait beyond explicitly recorded queues |
| SLATargetHours | Whole number (4, 8, 24, 48) | Priority/tier elapsed-time commitment in hours. Listed as decimal in the source dictionary |
| SLAMetFlag | Whole number | 1 if initial closure elapsed time <= target; the SLA measure also requires `InitialResolvedFlag = 1` |
| AICostEUR | Decimal | Observed synthetic initial AI usage cost; zero for Human only |
| HumanCostEUR | Decimal | Observed synthetic initial human labour cost; zero for AI only |
| RepeatCostEUR | Decimal | Additional direct support costs of linked repeats; zero when no repeat |
| EscalationReason | Text | Handoff reason; "Not applicable" on non-handoff paths |
| HandoffBand | Text | AI time before handoff: Up to 5 min, 5–12 min, Over 12 min; otherwise "Not applicable" |

## Dimensions

| Table | Columns |
| --- | --- |
| DimDate | Date, Year, Quarter, MonthNo, MonthName, YearMonth, YearMonthSort (730 days, Jan 2025 to Dec 2026) |
| DimIssue | IssueKey, IssueCategory (5), IssueSubcategory (15), SupportTeam |
| DimProduct | ProductKey, ProductFamily (Commerce, Productivity, Devices), ProductName (6) |
| DimCustomerProfile | ProfileKey, Region, Country, Language (6), CustomerTier (Standard, Plus, Business), Lifecycle (New, Established). A profile is a segment combination, not a unique customer |
| DimSupportRoute | RouteKey, Channel (Live chat, Email, Messaging), SupportPath (AI only, AI → human, Human only), AIAgent, PathColor |

## Relationships

All are one-to-many from the dimension to `FactSupportTickets`, active, single-direction.

| Dimension key | Fact column |
| --- | --- |
| DimDate[Date] | CreatedDate |
| DimIssue[IssueKey] | IssueKey |
| DimCustomerProfile[ProfileKey] | ProfileKey |
| DimProduct[ProductKey] | ProductKey |
| DimSupportRoute[RouteKey] | RouteKey |

## Data checks performed

- 12,000 rows and 12,000 distinct `TicketID` values; no blank IDs.
- Only `CSATScore` has nulls (7,003 rows, 58.4%). Every other column is complete.
- `SLAMetFlag` equals `ResolutionHours <= SLATargetHours` for all rows. Of the 599 unresolved tickets, 514 still carry `SLAMetFlag = 1`, so the SLA measure also requires `InitialResolvedFlag = 1`.
- Duration fields are zero where the stage does not occur: AI-only tickets have zero human and queue minutes, and human-only tickets have zero AI minutes.
- `RepeatCostEUR` is positive exactly when `RepeatContactCount7d > 0`.
- Escalation reason and handoff band are populated only on the AI-to-human path.

