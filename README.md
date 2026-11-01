# Delivery - On The Go: Hub Staffing Analysis

Data Analyst technical assessment. Delivery - On The Go runs six delivery hubs in Metro City, grouped into three clusters. Order volume rose sharply in some hubs in the second week of the period, and hub managers asked for more delivery agents. This project uses 14 days of operational data, the company cost sheet and a 4-week volume forecast to decide where hiring is actually needed.

**Period analysed:** 13 to 26 October 2026 (Week 1: 13-19 Oct, Week 2: 20-26 Oct). **Forecast:** 27 October to 23 November 2026.

## Recommendation

**Do not hire 8 agents. Move 2, hire 2, and fix Riverside's dispatch.**

| Hub | Gap at forecast peak | Action | Cost (BDT) |
|---|---|---|---|
| Greenfield | +2 | Receive 2 agents from Lakeview (same cluster) | 4,000 |
| Lakeview | -3 | Donor; keeps 9 agents against a peak need of 8 | 0 |
| Riverside | +2 | Hold reduction programme; no hire | 15,000 |
| Hillcrest | +2 | Hire 2 Fixed agents (no donor in its cluster) | 46,400 |
| Brookside | -1 | Coach 3 high-return agents; no hire | 0 |
| Westwood | 0 | Monitor | 0 |
| **Total** | | | **65,400** of the 70,000 budget |

The 8-hire request would cost BDT 185,600, which is 2.65 times the available budget.

## Key findings

- **Volume rose 19% in Week 2** (11,689 to 13,906 attempts). Greenfield (88.6%), Riverside (90.8%) and Hillcrest (95.9%) are above the 85% utilisation target. Lakeview (56.2%) and Brookside (71.5%) are under 75%.
- **Headcount is nearly enough.** At the forecast peak the company needs 64 agents against 62 active. The gaps are uneven, so moving agents is cheaper than hiring: a move costs BDT 2,000, a hire about BDT 23,200 for the month.
- **Riverside is a dispatch problem.** It had 874 holds in Week 2, 64.4% agent-side, with Late Dispatch the top reason (312). It accounts for 563 of the company's 740 agent-side holds, and its cost per delivered order is BDT 45.67 against 26 to 28 at the efficient hubs.
- **Brookside's returns come from three agents** (A1045, A1046, A1047), who made 257 of its 457 returns. This is a coaching issue, not a hiring one.

## Repository contents

| Path | What it contains |
|---|---|
| `Datasets/` | The source data: `hubs.csv`, `agents.csv`, `orders.csv`, `order_logs.csv`, `volume_forecast.csv`, `costs.csv` and `cost_sheet.xlsx` (tabs: `agent_salary`, `costs`, `rules`) |
| `Dashboard/Delivery on the go.pbix` | The Power BI source file |
| `Dashboard/1 Hub Overview` | Dashboard page 1: utilisation by hub against the 85% target |
| `Dashboard/2 Holds & Returns` | Dashboard page 2: holds by reason, attempt outcomes, high-return agents |
| `Dashboard/3 Cost` | Dashboard page 3: cost per delivered order and its components |
| `Dashboard/4 Staffing & Plan` | Dashboard page 4: staffing gap with and without the plan, and the budget |
| `Hub Staffing Recommendation - Delivery On The Go` | The 8-slide presentation for the Head of Operations |

## Live dashboard

**[INSERT DASHBOARD LINK]** (opens without a login)

To use the source file instead, open `Dashboard/Delivery on the go.pbix` in Power BI Desktop (Windows). The model reads the CSV files in `Datasets/`; if Power BI cannot find them, update the file paths under Transform data > Data source settings.

## Method

- **Utilisation** = Assigned attempts / (available agents x 7 days x 40 standard attempts). The target is 85%, or 34 attempts per agent per day. Every active agent counts as available every day. Week 1 includes the 2 agents who left at the end of that week.
- **Attempts and outcomes.** Every `Assigned` event is one attempt with exactly one outcome (Delivered, Hold or Returned). This reconciles exactly: 25,595 = 21,078 + 3,042 + 1,475.
- **Agent-side holds** are Late Dispatch, Not Attempted and Agent Time Shortage. All other hold reasons are customer-side.
- **Cost per delivered order** = (agent pay + BDT 30 per hold + BDT 55 per return) / delivered orders. Fixed agents cost BDT 650 per working day; Variable agents cost BDT 300 plus BDT 14 per delivery.
- **Required agents for the forecast** = forecast orders x each hub's Week 2 attempts per order / 238, rounded up, where 238 = 7 days x 40 attempts x 85%. The forecast is in orders but staffing is in attempts, because held orders are attempted again.
- **Company rules applied:** agents move only within the same cluster, and only out of hubs whose Week 2 utilisation is below 75%. The hold reduction programme removes 50% of agent-side holds and takes 14 days to reach full effect. New agents work at 60% speed in their first 7 days.

## Main assumptions

1. A new Fixed hire works all 28 days of the forecast period, costing BDT 5,000 once plus BDT 650 a day (BDT 23,200 in total).
2. The hold programme starts at the beginning of the forecast period and ramps up linearly over 14 days.
3. Each avoided agent-side hold saves one retry attempt.
4. Week 2 behaviour (attempts per order) continues through the forecast.
5. An agent works on any day on which they have at least one event in `order_logs`.

The full list of assumptions, all SQL queries (Q1 to Q7) with their results, and the strategic answers (Q8 to Q10) are in the assessment report. **[INSERT LINK TO REPORT PDF, or add the file to this repository]**

## Tools

PostgreSQL for the analysis, Power BI for the dashboard.
