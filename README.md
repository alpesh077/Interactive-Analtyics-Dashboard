# Raw Business Transaction Data

This directory is the designated location for storing unmodified, raw transaction datasets for the Business Analytics Dashboard.

## Candidate / Expected Fields
When raw sales transaction datasets are ingested, they typically contain some or all of the following attributes:

- `Order ID`: Unique transaction identifier
- `Order Date`: Date/timestamp of order placement
- `Customer ID`: Unique customer identifier
- `Customer Name`: Customer name or business entity
- `Region`: Geographic sales territory / market
- `Sales Channel`: Route to market (e.g., In-Store, Online, Wholesale, Distributor)
- `Product`: Product description or name
- `Category`: Product categorization or grouping
- `Quantity`: Number of units sold
- `Unit Price`: Price per single unit
- `Discount`: Discount percentage or amount applied
- `Cost`: Unit cost or total goods cost
- `Revenue`: Gross/net sales generated
- `Profit`: Operating profit generated

> **Note**: Do not assume every field is present or named identically across files. In Step 02 (Dataset Understanding and Data Profiling), the actual schema and nullability will be systematically inspected.
