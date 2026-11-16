# Business Problem Statement — FreshFlow Foods

## 1. Background

FreshFlow Foods is a fictional Indian FMCG and dairy distribution business with a multi-product portfolio, a network of customers and sales representatives, and operations across several sales territories. For this portfolio exercise, the company data covers 1 January 2024 through 31 December 2025.

The organisation needs a consistent view of its commercial and operational performance. Sales, returns, inventory and monthly targets are represented in separate datasets and must be combined through a reliable analytical model.

## 2. Problem

Management does not have one trusted, interactive view to answer basic but important questions such as:

- How much revenue and gross profit are being generated?
- Which products, categories, customers and channels contribute to performance?
- Which territories and sales representatives achieve or miss their targets?
- How frequently are products returned, how much is refunded, and why?
- Where is stock below reorder levels, and where might inventory be excessive?
- How do results change over time?

Without standard definitions, clean source data and a consistent model, different teams may calculate the same KPI differently or draw conclusions from incomplete and mismatched data.

## 3. Project goal

Develop a Power BI analytics solution that consolidates the synthetic sales and operations data, cleans it in Power Query, models it using dimensions and fact tables, defines reusable DAX measures, and presents interactive reports that support management decisions.

## 4. Stakeholders and decisions

| Stakeholder | Decisions supported |
|---|---|
| CEO / General Management | Overall performance, profitability and priority areas |
| Sales Head | Revenue, target attainment, territory and representative performance |
| Category / Product Manager | Product mix, SKU performance, discounts and margin |
| Customer / Channel Manager | Customer segments, channel contribution and account performance |
| Quality / Returns Team | Return volume, refund value, reasons and disposition |
| Supply Chain / Inventory Team | Closing stock, stock status and replenishment priorities |

## 5. Analytical questions

1. What are Gross Sales, Discount Amount, Net Sales, COGS, Gross Profit and Gross Margin %?
2. Which categories, brands and SKUs generate the highest Net Sales and Gross Profit?
3. How does sales performance vary by region, territory, customer channel and sales representative?
4. How does actual Net Sales compare with approved sales targets at the same month/product/geography grain?
5. What are returned units, refund value, return reasons and disposition patterns?
6. Which product-geography combinations have closing stock below reorder levels?
7. How do revenue and margin trend by month, quarter and year?
8. How do discounts relate to sales and margin?

## 6. Scope

### In scope

- Import and profile nine CSV tables.
- Clean headers, data types, duplicates, blanks, errors and inconsistent text in Power Query.
- Document table grain and create a star-schema-style model.
- Create DAX measures for sales, profitability, target attainment, returns and inventory.
- Build an executive overview and supporting analytical report pages.
- Validate measures, relationships, filters and report interactions.
- Produce findings and recommendations based on the synthetic data.
- Publish the PBIX, documentation and screenshots on GitHub.

### Out of scope

- Connecting to a real company's ERP, CRM or warehouse system.
- Real-time streaming or production refresh infrastructure.
- Actual business recommendations for a real company.
- Claims of real customer behaviour or real financial performance.

## 7. Success criteria

The project is successful when:

- All nine tables load with correct headers and data types.
- The 120 intentional duplicate sales rows are removed using `SalesLineID`, resulting in 18,000 sales rows.
- Dimension keys are unique and fact keys match the dimensions as expected.
- Relationships follow the documented grain and filtering design.
- DAX measures reconcile to independent checks and behave correctly under slicers.
- Date, region, product and customer filters work as intended.
- Report pages answer the defined business questions clearly.
- Findings are quantified and limitations are stated.
- The repository contains the PBIX, source data, documentation and final screenshots.

## 8. Data disclaimer

All company, product, customer, territory, transaction and financial data is synthetic and generated for practice. It must not be presented as actual FreshFlow Foods business information.
