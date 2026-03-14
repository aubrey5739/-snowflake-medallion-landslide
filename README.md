# Snowflake Medallion Analytics Platform for Global Landslide Intelligence

A cloud-based analytics project that transforms raw landslide event records into decision-ready reporting assets using Snowflake’s medallion architecture, automated data pipelines, and AI-enabled analytics.

## Project Summary
This project was built to show how a modern analytics team can move from raw data to reusable business insight quickly. Using the NASA Global Landslide Catalog, our team designed a Snowflake-based Bronze–Silver–Gold architecture, created reporting-ready datasets, automated incremental refresh logic, and added AI capabilities for text understanding, semantic search, and natural-language querying.

The result was a lightweight analytics prototype that combined data engineering, business intelligence, and AI-enabled workflows in a single end-to-end solution.

## Business / Analytical Goal
The goal was to create a scalable analytics environment that could help users answer practical risk and monitoring questions such as:

- Which countries experience the highest landslide fatalities?
- Which triggers are most frequent, and which are most severe?
- Are there seasonal patterns in landslide events and fatalities?
- How can unstructured event descriptions be made more useful for analysis?

Rather than limiting the project to one-time analysis, we focused on building a reusable reporting structure that could support repeated querying, dashboarding, and future extension.

## My Role
I worked as part of a three-person team and contributed to the design and implementation of the analytics workflow. My work included:

- Structuring medallion layers in Snowflake to separate raw, refined, and curated data assets
- Supporting the design of a star-schema-based Silver layer and reporting-oriented Gold layer
- Building SQL-based transformation logic for analytics-ready datasets
- Applying Snowflake Cortex capabilities for sentiment analysis, text summarization, semantic search, and natural-language-to-SQL exploration
- Contributing to dashboard development and presentation of key analytical findings
- Helping frame the project as an end-to-end analytics prototype rather than a one-time class exercise

## Solution Architecture
### Bronze Layer: Raw + AI-Enriched Source Data
The Bronze layer stores raw landslide records as ingested and serves as the entry point for incremental processing. We also enriched event descriptions with AI-generated sentiment and summaries to make unstructured text more usable downstream.

### Silver Layer: Cleaned, Structured Analytical Model
The Silver layer converts raw event data into a star schema centered on a landslide fact table and supporting dimensions such as date, country, trigger, category, size, setting, source, and geography. This layer supports consistent querying and easier downstream reporting.

### Gold Layer: Curated Reporting Outputs
The Gold layer contains aggregated, decision-ready tables designed for dashboarding and recurring analysis. These tables support country-level fatality analysis, trigger-level severity comparisons, and monthly/quarterly trend reporting.

## AI-Enabled Analytics Features
This project intentionally incorporated AI features into the analytical workflow rather than treating AI as a separate add-on.

### 1. Text Understanding with Snowflake Cortex
We used Cortex functions to summarize event descriptions and extract sentiment signals from unstructured text.

### 2. Semantic Search over Event Narratives
We created a semantic search service so users could retrieve relevant landslide events using natural language phrases instead of exact keyword matching.

### 3. Natural-Language-to-SQL Exploration
We designed a semantic model that supported business-style questions such as identifying high-fatality countries, common triggers, and seasonal patterns without requiring end users to write SQL directly.

These features helped demonstrate how AI can accelerate exploratory analysis and make data more accessible to business users.

## Data Pipeline and Reusability
A major goal of the project was to show repeatable analytics delivery, not just static analysis.

To support that, we implemented an incremental pipeline using Snowflake Streams and Stored Procedures:

- New records land in Bronze
- Stream logic captures only new rows
- Stored procedures load updates into the Silver fact table
- Gold reporting tables are refreshed automatically

This made the project closer to a practical analytics MVP by reducing manual refresh effort and making the reporting structure easier to sustain.

## Key Outputs
The project produced:

- A Snowflake medallion architecture with Bronze, Silver, and Gold layers
- Structured reporting datasets for recurring analysis
- AI-enriched text fields for unstructured event descriptions
- Semantic search and natural-language analytics capabilities
- Interactive dashboard views for country risk, trigger patterns, and seasonality
- A reproducible notebook-based workflow and project presentation

## Selected Findings
A few notable patterns identified through the project included:

- India, China, and Afghanistan ranked among the countries with the highest total fatalities
- Downpour was the most common trigger, while tropical cyclones and monsoon rainfall showed high fatality severity
- Landslide events and fatalities rose sharply during monsoon-linked seasonal periods, especially in Q2 and Q3

These outputs showed how curated data models and AI-assisted querying can turn a raw public dataset into a more decision-ready analytics asset.

## Tools and Technologies
- **Snowflake** for cloud data warehousing
- **SQL** for transformations, modeling, and reporting logic
- **Snowflake Streams** for incremental data capture
- **Stored Procedures** for refresh automation
- **Snowflake Cortex** for summarization, sentiment analysis, semantic search, and natural-language analytics
- **Streamlit in Snowflake** for dashboarding and interactive reporting

## Why This Project Matters
This project reflects the kind of work I enjoy most: building practical analytics solutions that combine clean data structure, business-facing outputs, and AI-enabled functionality.

It also demonstrates several capabilities relevant to analytics consulting roles:

- Turning raw data into reusable reporting assets
- Designing lightweight, scalable analytical frameworks
- Building prototype solutions quickly
- Integrating AI into analytical workflows
- Supporting decision-making through structured dashboards and accessible insights

## Repository Contents
- `Projectcode_ClawJawCoil-1.ipynb` — core project notebook and SQL workflow
- `Global_Landslide_Catalog_Export.csv` — raw source dataset
- `PresentationSlides_ClawJawCoil-1.pdf` — final project presentation
- `README.md` — project overview and documentation

## Data Source
NASA Global Landslide Catalog (GLC), maintained by NASA Goddard Space Flight Center and distributed via Kaggle.
