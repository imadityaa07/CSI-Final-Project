# CSI-Final-Project
✅ Pipeline 1: pl_copy_customer_main
Purpose:
To copy customer data from an Azure SQL Database to ADLS, but only when a threshold count of 500 records is met.

Key Features:

Uses a Lookup activity to get the count of records from the Customer table.

Includes an If Condition to check if the count is greater than 500.

If the condition is met, it proceeds to copy the data to ADLS (/customer/ folder).

If the count is also greater than 600, it triggers a second pipeline (pl_copy_product_child), passing the record count as a parameter.

✅ Pipeline 2: pl_copy_product_child
Purpose:
To copy product data, but only when it is triggered by the parent pipeline and the customer count exceeds 600.

Key Features:

Accepts a parameter customerCount from the parent pipeline.

Evaluates if the value is >600.

If the condition is met, it copies data from the Product table into ADLS (/product/ folder).

# File & Execution Structure :
Both pipelines have been created using basic ADF activities: Lookup, If Condition, Copy, and Execute Pipeline.

The pipelines demonstrate dynamic decision making, parameter passing, and chained pipeline execution.

All data is securely moved from SQL to ADLS and logged via ADF Monitor.

✅ Pipeline: pl_fetch_country_data
Purpose:
To fetch data for 5 countries — India, US, UK, China, and Russia — from a REST API and save each country’s data as a separate JSON file in the data lake.

Key Components:

5 Copy Data Activities, each configured individually (Approach A).

Each activity makes a GET request to the same REST API endpoint but for a different country.

Output is written to ADLS in the format:
country-data/<country_name>.json (e.g., country-data/india.json).

Folder Structure in ADLS:

pgsql
Copy
Edit
adls-root/
└── country-data/
    ├── india.json
    ├── us.json
    ├── uk.json
    ├── china.json
    └── russia.json
🕓 Scheduling:
This pipeline is scheduled to run twice daily:

At 12:00 AM IST

At 12:00 PM IST

This ensures near real-time country data collection twice every day.
