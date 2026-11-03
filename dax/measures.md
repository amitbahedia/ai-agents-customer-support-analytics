# DAX measures

Definitions as read from the model on 2 Nov 2026. Table and column names follow the model. Measure names may have been tidied since, so check them against the `.pbix`.

## Foundational Measures

```dax
Total Tickets = DISTINCTCOUNT(factsupporttickets[TicketID])
```

```dax
Initially Resolved Tickets =
CALCULATE([Total Tickets], factsupporttickets[InitialResolvedFlag] = 1)
```

```dax
Initial Resolution Rate =
DIVIDE([Initially Resolved Tickets], [Total Tickets], 0)
```

```dax
Tickets with Repeat Contacts =
CALCULATE(
    DISTINCTCOUNT(factsupporttickets[TicketID]),
    factsupporttickets[RepeatContactCount7d] > 0
)
```

```dax
Repeat Contact Rate =
DIVIDE(
    CALCULATE(
        DISTINCTCOUNT(factsupporttickets[TicketID]),
        factsupporttickets[RepeatContactCount7d] > 0
    ),
    [Total Tickets],
    0
)
```

```dax
Total Repeat Contacts with seven days =
SUM(factsupporttickets[RepeatContactCount7d])
```

```dax
-- Met the SLA target AND resolved at first contact, over all tickets.
-- The data dictionary says the SLA measure also requires InitialResolvedFlag = 1.
SLA Attainment =
DIVIDE(
    CALCULATE(
        [Total Tickets],
        factsupporttickets[SLAMetFlag] = 1,
        factsupporttickets[InitialResolvedFlag] = 1
    ),
    [Total Tickets],
    0
)
```

```dax
Total Support Cost =
SUM(factsupporttickets[AICostEUR])
    + SUM(factsupporttickets[HumanCostEUR])
    + SUM(factsupporttickets[RepeatCostEUR])
```

```dax
Cost Per Ticket = DIVIDE([Total Support Cost], [Total Tickets])
```

```dax
-- Recorded scores only; blanks are never imputed.
Avg CSAT = AVERAGE(factsupporttickets[CSATScore])
```

```dax
-- CSAT response rate
Tickets with CSAT Score =
DIVIDE(
    CALCULATE([Total Tickets], NOT(ISBLANK(factsupporttickets[CSATScore]))),
    [Total Tickets],
    0
)
```

```dax
High Complexity Share =
DIVIDE(
    CALCULATE([Total Tickets], factsupporttickets[Complexity] = "High"),
    [Total Tickets],
    0
)
```

```dax
High Priority Share =
DIVIDE(
    CALCULATE([Total Tickets], factsupporttickets[Priority] = "High"),
    [Total Tickets],
    0
)
```

```dax
Average Resolution Hours = AVERAGE(factsupporttickets[ResolutionHours])
```

```dax
-- Filter to SupportPath = "AI → human" for handoff queue analysis;
-- AI-only tickets have zero queue minutes.
Average Queue Hours = AVERAGE(factsupporttickets[QueueMinutes]) / 60
```

## AI Agent Effectiveness & Operational Efficiency Measures

```dax
AI Only Tickets =
CALCULATE([Total Tickets], dimsupportroute[SupportPath] = "AI only")
```

```dax
AI Only Tickets % =
VAR SelectedSupportPaths = VALUES(dimsupportroute[SupportPath])
VAR AIOnlyTickets =
    CALCULATE(
        DISTINCTCOUNT(factsupporttickets[TicketID]),
        KEEPFILTERS(dimsupportroute[SupportPath] = "AI only")
    )
VAR AllTickets = DISTINCTCOUNT(factsupporttickets[TicketID])
RETURN
    IF(
        NOT("AI only" IN SelectedSupportPaths),
        BLANK(),
        DIVIDE(AIOnlyTickets, AllTickets, 0)
    )
```

```dax
AI Initial Resolution =
CALCULATE([Initial Resolution Rate], dimsupportroute[SupportPath] = "AI only")
```

```dax
-- Handoffs over all AI-handled tickets (AI only + AI to human)
AI Handoff Rate =
DIVIDE(
    CALCULATE([Total Tickets], dimsupportroute[SupportPath] = "AI → human"),
    CALCULATE(
        [Total Tickets],
        dimsupportroute[SupportPath] IN {"AI only", "AI → human"}
    )
)
```

```dax
Cost per AI only ticket =
DIVIDE(
    CALCULATE([Total Support Cost], dimsupportroute[SupportPath] = "AI only"),
    CALCULATE([Total Tickets], dimsupportroute[SupportPath] = "AI only")
)
```

```dax
AI only average CSAT =
CALCULATE([Avg CSAT], dimsupportroute[SupportPath] = "AI only")
```

```dax
AI only Tickets with CSAT Score =
DIVIDE(
    CALCULATE([AI Only Tickets], NOT(ISBLANK(factsupporttickets[CSATScore]))),
    [AI Only Tickets],
    0
)
```

```dax
AI only SLA =
CALCULATE([SLA Attainment], dimsupportroute[SupportPath] = "AI only")
```

```dax
AI only SLA Attainment Tickets =
CALCULATE(
    [AI Only Tickets],
    factsupporttickets[SLAMetFlag] = 1,
    factsupporttickets[InitialResolvedFlag] = 1
)
```

```dax
High Complex Tickets Involving AI =
CALCULATE(
    [Total Tickets],
    factsupporttickets[Complexity] = "High",
    dimsupportroute[SupportPath] IN {"AI only", "AI → human"}
)
```

## Dynamic Text Measures

Short card captions built with `FORMAT`, for example:

```dax
Initial Resolution text =
FORMAT([Initially Resolved Tickets], "#,##0") & " tickets initially resolved"

Average CSAT Coverage text =
FORMAT([Tickets with CSAT Score], "0.0%") & " response coverage"

Support Cost Breakdown Text =
"AI " & FORMAT(SUM(factsupporttickets[AICostEUR]), "€#,##0")
    & " · Human " & FORMAT(SUM(factsupporttickets[HumanCostEUR]), "€#,##0")
    & " · Repeat " & FORMAT(SUM(factsupporttickets[RepeatCostEUR]), "€#,##0")
```

## Field parameters

- **Key Parameters:** Cost Per Ticket, Initial Resolution Rate, Repeat Contact Rate, Avg CSAT, SLA Attainment, Total Tickets.
- **Other Performance Parameters:** Average Queue Hours, Average Resolution Hours, Avg CSAT.

## Validated values (no filters)

| Measure | Value |
| --- | --- |
| Total Tickets | 12,000 |
| Initial Resolution Rate | 95.01% |
| Repeat Contact Rate | 16.30% |
| Total Repeat Contacts with seven days | 2,360 |
| SLA Attainment | 82.87% |
| Total Support Cost | €162,721.50 |
| Cost Per Ticket | €13.56 |
| Avg CSAT | 4.05 |
| CSAT response rate | 41.64% |
| Average Resolution Hours | 6.04 |
| AI Handoff Rate | 42.25% |
