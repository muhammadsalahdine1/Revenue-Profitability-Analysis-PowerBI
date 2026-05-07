# Revenue and Profitability Analysis - Power BI Dashboard

This Power BI dashboard provides comprehensive financial analytics for business performance tracking. It focuses on revenue, profit, and profitability margins across different dimensions including time periods, countries, products, and customer segments.

## 📊 Dashboard Overview

### Key Metrics
- **Total Revenue**: $92.31M
- **Profit Margin %**: Dynamic calculation based on revenue and COGS
- **Time Period**: 2014 (Full year data)

### Core Visualizations

#### 1. Profit & Revenue by Time
- Breakdown by **Year**, **Quarter**, and **Month**
- Monthly trends from January to December 2014
- Comparative view of Profit vs. Revenue over time

#### 2. Profit & Revenue by Country
- Geographic performance analysis across:
  - France
  - Canada
  - Germany
  - United States
  - Mexico

#### 3. Profit & Revenue by Product
- Product-level profitability analysis
- Products tracked:
  - Paseo
  - VTT
  - Amarilla
  - Carretera
  - Velo
  - Montana

#### 4. Profit & Revenue by Segment
- Customer segment breakdown:
  - Government
  - Small Business
  - Channel Partners
  - Midmarket
  - Enterprise

## 🛠️ Data Model

### Tables

#### financials Table
| Column | Description |
|--------|-------------|
| COGS | Cost of Goods Sold |
| Country | Sales country |
| Date | Transaction date |
| DateKey | Unique date identifier |
| Discount Band | Discount category |
| Discounts | Discount amount |
| Gross Sales | Total sales before adjustments |
| Manufacturing Price | Production cost |
| Product | Product name |
| Profit | Calculated profit |
| Sale Price | Final sale price |
| Sales | Revenue amount |
| Segment | Customer segment |
| Units Sold | Quantity sold |
| Year | Transaction year |

#### Dim Date Table
A comprehensive date dimension with 30+ columns including:
- Date, DateKey
- Year, Quarter, Month
- Day, Day Name, Week Number
- Start/End of Month, Quarter, Year
- Is Weekday, Is Weekend
- Month Year Label

## 📐 DAX Measures Library

### Core Measures

| Measure | Formula Description |
|---------|---------------------|
| `Revenue` | SUM(financials[Sales]) |
| `Profit` | SUM(financials[Profit]) |
| `Profit Margin %` | DIVIDE([Profit], [Revenue], 0) |
| `COGS` | SUM(financials[COGS]) |

### Time Intelligence Measures

| Measure | Purpose |
|---------|---------|
| `Revenue MTD` | Month-to-Date revenue |
| `Revenue YTD` | Year-to-Date revenue |
| `Revenue MOM %` | Month-over-Month growth |
| `Revenue YOY %` | Year-over-Year growth |
| `RevenueLast3Months` | Rolling 3-month revenue |
| `Profit MTD` | Month-to-Date profit |
| `Profit YTD` | Year-to-Date profit |
| `Profit MOM %` | Month-over-Month profit growth |
| `Profit YOY %` | Year-over-Year profit growth |

### Sample DAX Formulas

```dax
// Revenue Month-over-Month %
Revenue MOM % =
VAR _currentmonthrevenue = [Revenue]
VAR _previousmonthrevenue = CALCULATE([Revenue], PREVIOUSMONTH('Dim Date'[Date]))
VAR _change = _currentmonthrevenue - _previousmonthrevenue
RETURN
DIVIDE(_change, _previousmonthrevenue, 0)

// Profit Margin %
Profit Margin % = DIVIDE([Profit], [Revenue], 0)

// Last 3 Months Revenue
RevenueLast3Months =
VAR startdate = EOMONTH(MAX('Dim Date'[Date]), -4)
VAR enddate = EOMONTH(MAX('Dim Date'[Date]), -1)
RETURN
CALCULATE([Revenue], 'Dim Date'[Date] >= startdate, 'Dim Date'[Date] <= enddate)
