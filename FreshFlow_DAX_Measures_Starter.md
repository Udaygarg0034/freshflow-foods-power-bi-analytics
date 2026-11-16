# DAX Measures — FreshFlow Foods Starter Catalogue

Create the measures with **Modeling → New measure** or right-click the preferred table in the Fields/Data pane → **New measure**. The suggested home table is for organisation only; a measure can reference columns and measures from other related tables.

> These formulas use the exact column names in the supplied nine-table CSVs. Validate column names, relationships, filter context and business definitions in your PBIX before using them in a final report.

## Sales measures (suggested home table: FactSales)

```dax
Gross Sales =
SUMX(
    FactSales,
    FactSales[Quantity] * FactSales[UnitPrice]
)

Discount Amount =
SUMX(
    FactSales,
    FactSales[Quantity] * FactSales[UnitPrice] * FactSales[DiscountPct]
)

Net Sales =
[Gross Sales] - [Discount Amount]

COGS =
SUMX(
    FactSales,
    FactSales[Quantity] * FactSales[UnitCostAtSale]
)

Gross Profit =
[Net Sales] - [COGS]

Gross Margin % =
DIVIDE([Gross Profit], [Net Sales])

Units Sold =
SUM(FactSales[Quantity])

Orders =
DISTINCTCOUNT(FactSales[OrderID])

Sales Lines =
DISTINCTCOUNT(FactSales[SalesLineID])

Buying Customers =
DISTINCTCOUNT(FactSales[CustomerID])

Average Order Value =
DIVIDE([Net Sales], [Orders])
```

## Time intelligence measures (suggested home table: FactSales)

These require an active relationship from `DimDate[Date]` to `FactSales[OrderDate]`, a continuous date table marked as the date table, and suitable date context.

```dax
Net Sales MTD =
TOTALMTD([Net Sales], 'DimDate'[Date])

Net Sales QTD =
TOTALQTD([Net Sales], 'DimDate'[Date])

Net Sales YTD =
TOTALYTD([Net Sales], 'DimDate'[Date])

Net Sales Previous Year =
CALCULATE(
    [Net Sales],
    SAMEPERIODLASTYEAR('DimDate'[Date])
)

Net Sales YoY Change =
[Net Sales] - [Net Sales Previous Year]

Net Sales YoY % =
DIVIDE([Net Sales YoY Change], [Net Sales Previous Year])
```

## Returns measures (suggested home table: FactReturns)

```dax
Return Events =
DISTINCTCOUNT(FactReturns[ReturnID])

Returned Units =
SUM(FactReturns[ReturnQty])

Refund Amount =
SUM(FactReturns[RefundAmount])
```

Returns measures use `DimDate[Date]` related to `FactReturns[ReturnDate]` in the intended model. This is a return-date operational view. A sales-cohort return measure needs different logic and should be documented separately.

## Target measures (suggested home table: FactTargets)

```dax
Sales Target =
SUM(FactTargets[SalesTargetAmount])

Target Variance =
[Net Sales] - [Sales Target]

Target Achievement % =
DIVIDE([Net Sales], [Sales Target])

Units Target =
SUM(FactTargets[UnitsTarget])
```

Use target measures at matching month/product/geography grain. Targets are stored at month-start; a daily comparison can be misleading.

## Inventory measures (suggested home table: FactInventory)

Inventory is a month-end snapshot. Do not add ClosingStockQty across months to describe current stock. A simple latest-date measure is:

```dax
Latest Snapshot Date =
MAX(FactInventory[SnapshotDate])
```

A basic latest snapshot quantity measure for the current filter context is:

```dax
Closing Stock Qty (Latest Snapshot) =
VAR LatestDate = MAX(FactInventory[SnapshotDate])
RETURN
    CALCULATE(
        SUM(FactInventory[ClosingStockQty]),
        FactInventory[SnapshotDate] = LatestDate
    )
```

Validate this measure by product, geography and date context before using it as a company-wide closing-stock KPI.

## Concepts to explain in an interview

- `SUM` adds values from a column; `SUMX` iterates rows and evaluates an expression.
- `DISTINCTCOUNT` counts unique business identifiers; use it for OrderID because one order may have multiple lines.
- `DIVIDE` handles zero denominators safely.
- `CALCULATE` evaluates an expression in modified filter context.
- Time intelligence depends on a proper date table and relationship.
- A measure is evaluated in the current filter context; a calculated column is stored row by row.

## Validation before dashboard use

- Reconcile Gross Sales, Discount Amount, Net Sales and COGS to independent checks.
- Validate Orders as distinct OrderID, not row count.
- Check the measure responds to Region, Product, Customer and Date filters.
- Confirm the Year slicer filters sales correctly before relying on time intelligence.
- Confirm targets are compared at monthly product/geography grain.
- Confirm inventory uses latest snapshot logic.
