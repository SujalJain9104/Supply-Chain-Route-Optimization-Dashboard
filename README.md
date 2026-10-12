# Supply-Chain-Route-Optimization-Dashboard
An interactive Power BI dashboard for comparing supply-chain routes and transportation modes using SKU performance, defect rates, shipment quantity, cost, location, and profit. The dashboard brings operational and financial indicators into one view to support clearer, evidence-based route decisions.

> **Project focus:** Choose a route and transportation mode that best balances product quality, quantity requirements, cost, delivery speed, location, and profitability—not simply the lowest shipping cost.

## Dashboard at a Glance

The report brings together operational KPIs and route comparisons to help users evaluate performance across Route A, Route B, and Route C, and across Road, Rail, Air, and Sea transportation modes.

### Key dashboard views

- **Supply chain KPIs:** SKU count, quantity sold, average defect rate, and total cost.
- **Route comparison:** Compare routes by volume, cost, quality indicators, shipping time, and estimated profit.
- **Transportation mode analysis:** Review quantity, average shipping time, shipping cost, and defect-rate patterns by mode.
- **Location analysis:** Explore cost patterns by location.
- **Product analysis:** Compare defect rates across product types.
- **Inspection status:** Review Pass, Fail, and Pending inspection records.
- **Interactive filtering:** Explore the report by route, supplier, location, product type, transportation mode, and other available attributes.

## Route Decision Framework

Route decisions should be evaluated against seven core parameters. Each parameter contributes a different perspective to the decision.

| Parameter | What to evaluate | Decision significance |
|---|---|---|
| **SKUs** | SKU identity, product type, and number of distinct SKUs | Helps identify product-specific handling needs and the mix of items assigned to a route. |
| **Defect** | Average defect rate and inspection outcomes | Supports quality-focused decisions and helps identify where further inspection or corrective action may be needed. |
| **Quantity** | Quantity sold, order quantity, stock, and availability where relevant | Helps assess shipment demand and prioritise routes for larger or more urgent quantities. |
| **Cost** | Recorded costs and shipping costs | Supports cost comparison, cost-per-unit analysis, and evaluation of potential savings. |
| **Location** | Location and available route options | Keeps the decision relevant to the destination or operating area and the route options being compared. |
| **Mode** | Road, Rail, Air, and Sea | Supports trade-offs between shipping time, shipping cost, and product requirements. |
| **Profit** | Revenue less recorded costs; profit margin | Helps identify options that support stronger financial outcomes rather than focusing on sales volume or low freight cost alone. |

### Decision sequence

1. **Establish the shipment requirement.** Identify the SKU or product type, quantity, location, and service urgency.
2. **Review quality.** Check defect-rate patterns and inspection outcomes. Prioritise investigation of failed inspections and keep pending inspections visible.
3. **Compare feasible routes and modes.** Evaluate available options against the shipment's location, product needs, and delivery requirements.
4. **Compare cost and profitability.** Review total cost, shipping cost, estimated profit, and profit margin together.
5. **Select the best-fit option.** Choose the route and mode that provide the most suitable balance for the specific shipment or business priority.

A suitable route can vary by SKU, quantity, location, urgency, and quality requirements. The dashboard is designed to support a conditional decision rather than declaring one route best for every shipment.

## Key Findings from the Current Dataset

The following descriptive comparisons are based on the 100 records in the accompanying supply-chain dataset. Monetary values are shown in the units recorded in the source.

### Route comparison

| Route | Distinct SKUs / Records | Quantity Sold | Average Defect Rate | Total Costs | Estimated Profit | Average Shipping Time | Average Shipping Cost |
|---|---:|---:|---:|---:|---:|---:|---:|
| **Route A** | 43 | 19,778 | 2.342 | 20,875.77 | 232,323.08 | 6.02 | 5.38 |
| **Route B** | 37 | 18,370 | 2.322 | 22,039.38 | 182,444.62 | 5.70 | 5.55 |
| **Route C** | 20 | 7,951 | 2.055 | 10,009.42 | 109,912.54 | 5.25 | 5.90 |

**What the comparison suggests**

- **Route A** has the highest recorded quantity and estimated total profit, along with a lower average shipping cost than Routes B and C. Its average shipping time is longer than the other two routes.
- **Route B** has a shorter average shipping time than Route A and a slightly lower average defect rate than Route A. Its estimated total profit is lower than Route A's.
- **Route C** has the lowest average defect rate and shortest average shipping time, while its average shipping cost is the highest of the three routes and its recorded quantity is the lowest.

These patterns support different choices for different priorities: Route A is a candidate when volume and estimated total profit are important; Route C merits consideration when the recorded defect-rate and shipping-time indicators are priorities; and Route B offers a middle-ground profile on several measures.

### Transportation mode comparison

| Transportation Mode | Average Shipping Time | Average Shipping Cost | Average Defect Rate | Typical trade-off shown in the data |
|---|---:|---:|---:|---|
| **Road** | 4.72 | 5.54 | 2.621 | Shortest average recorded shipping time, with the highest average defect rate among the four modes. |
| **Air** | 5.12 | 6.02 | 1.824 | Lowest average defect rate, with the highest average shipping cost. |
| **Rail** | 6.57 | 5.47 | 2.319 | Intermediate average shipping cost, with a longer shipping time than Road and Air. |
| **Sea** | 7.12 | 4.97 | 2.315 | Lowest average shipping cost, with the longest average shipping time. |

The comparison highlights a clear cost-versus-speed trade-off in the observed records. Road has the shortest average shipping time, while Sea has the lowest average shipping cost. Air has the lowest recorded average defect rate. The preferred mode depends on the shipment's service needs and commercial priorities.

### Inspection outcomes by route

| Route | Failed Records | Passed Records | Pending Records |
|---|---:|---:|---:|
| **Route A** | 18 | 6 | 19 |
| **Route B** | 11 | 12 | 14 |
| **Route C** | 7 | 5 | 8 |

Inspection counts add a quality view to the route comparison. They should be considered alongside defect rates, product mix, and the number of records for each route.

## Core DAX Measures

The formulas below use the model table name `supply_chain_data` and the column names shown in the dashboard. They can be added as measures in Power BI.

### Operational KPIs

```dax
Total SKUs =
DISTINCTCOUNT('supply_chain_data'[SKU])
```

```dax
Total Quantity Sold =
SUM('supply_chain_data'[Number of products sold])
```

```dax
Average Defect Rate =
AVERAGE('supply_chain_data'[Defect rates])
```

```dax
Total Cost =
SUM('supply_chain_data'[Costs])
```

```dax
Total Shipping Cost =
SUM('supply_chain_data'[Shipping costs])
```

```dax
Average Shipping Time =
AVERAGE('supply_chain_data'[Shipping times])
```

```dax
Average Shipping Cost =
AVERAGE('supply_chain_data'[Shipping costs])
```

### Profitability

```dax
Total Revenue =
SUM('supply_chain_data'[Revenue generated])
```

```dax
Estimated Profit =
[Total Revenue] - [Total Cost]
```

```dax
Estimated Profit Margin % =
DIVIDE(
    [Estimated Profit],
    [Total Revenue],
    0
)
```

### Inspection outcomes

```dax
Failed Inspections =
CALCULATE(
    COUNTROWS('supply_chain_data'),
    'supply_chain_data'[Inspection results] = "Fail"
)
```

```dax
Passed Inspections =
CALCULATE(
    COUNTROWS('supply_chain_data'),
    'supply_chain_data'[Inspection results] = "Pass"
)
```

```dax
Pending Inspections =
CALCULATE(
    COUNTROWS('supply_chain_data'),
    'supply_chain_data'[Inspection results] = "Pending"
)
```

```dax
Completed Inspections =
[Failed Inspections] + [Passed Inspections]
```

```dax
Inspection Failure Rate % =
DIVIDE(
    [Failed Inspections],
    [Completed Inspections],
    0
)
```

### Profit calculation

`Estimated Profit` is defined in this project as **Revenue Generated − Costs**. `Estimated Profit Margin %` is expressed as estimated profit divided by revenue. These measures provide a consistent basis for comparing route-level profitability in the dashboard.

## Business Value

The dashboard helps bring key supply-chain trade-offs into one decision view:

- **Quality-led decisions:** Surface defect and inspection patterns while comparing routes and product types.
- **Cost-aware transportation:** Compare transportation modes using both shipping time and shipping cost.
- **Quantity-aware planning:** Review demand and SKU mix before prioritising a route.
- **Location-specific comparisons:** Explore cost patterns and route options for the selected location.
- **Profit-focused decisions:** Include estimated profit and margin alongside cost, volume, quality, and speed.
- **Interactive analysis:** Use report filters to investigate specific suppliers, SKUs, locations, product types, routes, and modes.

## Technology Stack

- **Microsoft Power BI Desktop** — dashboard development and interactive visualisation
- **DAX (Data Analysis Expressions)** — KPI and route-comparison calculations
- **Microsoft Excel** — tabular source data

## Repository Contents

```text
supply-chain-route-optimization-dashboard/
├── README_Route_Decision.md
├── Supply Chain Dashboard.pbix
└── Supply chain data.xlsx
```

## Summary

The Supply Chain Route Optimization Dashboard is a multi-criteria decision-support project that combines SKU performance, defects, quantity, cost, location, transportation mode, and estimated profit. It makes route and mode trade-offs easier to compare and provides a practical foundation for selecting shipment options that align with operational requirements and financial objectives.
