# ShopSphere Sales Dataset README

This README provides instructions for using the `ShopSphere_Sales.csv` dataset, designed for the "KQL Choose-Your-Own-Adventure: Interactive Query Learning" blog post. The dataset represents e-commerce sales data for practicing Kusto Query Language (KQL) queries.

## Dataset Overview

The `ShopSphere_Sales.csv` file contains synthetic e-commerce sales data for *ShopSphere*, an online store. The dataset includes the following columns:

- **Timestamp**: Date and time of the sale (ISO 8601 format, e.g., `2025-06-25T10:15:00Z`).
- **UserId**: Unique customer identifier (e.g., `U12345`).
- **Category**: Product category (e.g., `Electronics`, `Clothing`, `Home`).
- **Item**: Specific product sold (e.g., `Smartphone`, `T-Shirt`, `Lamp`).
- **Price**: Sale amount in USD (e.g., `699.99`).
- **Region**: Customer's region (e.g., `NorthAmerica`, `Europe`, `Asia`).

The dataset spans sales from June 25, 2025, to July 2, 2025, and includes 15 sample records.

## Prerequisites

To use the dataset with KQL, you need access to a KQL-compatible platform, such as:

- **Azure Data Explorer** (recommended for running KQL queries).
- **Azure Synapse Analytics** or other Kusto-compatible services.

Ensure you have:

- A valid account and access to one of the above platforms.
- Basic familiarity with KQL or the accompanying blog post for query guidance.

## Setup Instructions

1. **Download the Dataset**
   - Obtain the `ShopSphere_Sales.csv` file from the provided source (e.g., repository or blog post attachment).

2. **Ingest the Dataset into Azure Data Explorer**
   - Log in to your Azure Data Explorer cluster.
   - Create a database if you don’t already have one:
     ```kql
     .create database ShopSphereDB
     ```
   - Create a table to hold the sales data:
     ```kql
     .create table Sales (
         Timestamp: datetime,
         UserId: string,
         Category: string,
         Item: string,
         Price: real,
         Region: string
     )
     ```
   - Ingest the CSV file into the `Sales` table:
     ```kql
     .ingest inline into table Sales <|
     [Paste the contents of ShopSphere_Sales.csv here]
     ```
     Alternatively, upload the CSV file to a storage location (e.g., Azure Blob Storage) and use:
     ```kql
     .ingest into table Sales 'https://yourstorage.blob.core.windows.net/path/ShopSphere_Sales.csv' with (format='csv', ignoreFirstRecord=true)
     ```
     Note: Replace the URL with your storage path and ensure `ignoreFirstRecord=true` to skip the CSV header.

3. **Verify the Data**
   - Run a quick query to confirm the data loaded correctly:
     ```kql
     Sales | take 5
     ```
   - This should display the first 5 rows of the dataset.

## Using the Dataset

The dataset is designed to work with the KQL queries in the "KQL Choose-Your-Own-Adventure" blog post. Follow these steps:

1. **Explore the Blog Post**
   - Refer to the blog post for an interactive guide to building KQL queries.
   - The post walks you through choices (e.g., filtering by time or category) to create queries like:
     ```kql
     Sales
     | where Timestamp > ago(7d)
     | summarize SaleCount = count(), AvgPrice = avg(Price) by Category
     ```

2. **Run Queries**
   - Use the Azure Data Explorer web UI or client tools to execute queries.
   - Copy and paste queries from the blog post into the query editor.
   - Modify queries to experiment with different filters, aggregations, or sorting.

3. **Example Queries**
   - Get total revenue for Electronics:
     ```kql
     Sales
     | where Category == "Electronics"
     | summarize TotalRevenue = sum(Price)
     ```
   - Count sales by region:
     ```kql
     Sales
     | summarize SaleCount = count() by Region
     ```

## Notes

- **Data Scope**: The dataset is small (15 rows) for simplicity. For larger-scale practice, you can duplicate rows or generate additional synthetic data.
- **Timestamp**: All timestamps are in UTC (denoted by `Z`). Adjust queries (e.g., using `ago()`) based on your analysis needs.
- **Extending the Dataset**: Add more columns (e.g., `Quantity`) or rows to explore advanced KQL features like `join` or `pivot`.

## Troubleshooting

- **Ingestion Errors**: Ensure the CSV format matches the table schema. Check for missing commas or incorrect data types.
- **Query Issues**: Verify column names and data types in your queries. Use `Sales | getschema` to inspect the table structure.
- **Access Problems**: Confirm your Azure Data Explorer permissions for database and table operations.

## Resources

- [KQL Documentation](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/)
- [Azure Data Explorer Web UI](https://dataexplorer.azure.com/)
- The "KQL Choose-Your-Own-Adventure" blog post for guided query-building.

Happy querying, and enjoy your KQL adventure with ShopSphere!
