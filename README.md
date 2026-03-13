# The Moving Earth: Understanding Landslide Events

> MIS 381N: Information Management | Team ClawJawCoil  
> Melissa Cai Shi · Cassandre Korvink · **Mikyung Oh**  
> University of Texas at Austin · Fall 2025

---

## Overview

Built a **Medallion Architecture (Bronze → Silver → Gold)** data warehouse on Snowflake using NASA's Global Landslide Catalog (GLC), and applied Snowflake Cortex AI features for unstructured text analysis and natural language querying.

- **Dataset:** NASA/Kaggle Global Landslide Catalog — 11,000+ events, 31 attributes
- **Platform:** Snowflake (Data Warehouse + Cortex AI)
- **Goal:** Identify high-risk regions, trigger patterns, and fatality distributions

---

## Architecture

### Medallion Model (3-Layer)

```
Bronze (Raw)
└── LANDSLIDE_RAW — Raw CSV ingested as-is, no transformations
    └── + AI columns: DESCRIPTION_SENTIMENT, DESCRIPTION_SUMMARY

Silver (Refined)
└── Star Schema — cleaned and normalized data
    ├── FACT_LANDSLIDE (fact table)
    └── 10 Dimension tables
        DIM_DATE, DIM_COUNTRY, DIM_LANDSLIDE_TRIGGER,
        DIM_LANDSLIDE_SIZE, DIM_LANDSLIDE_CATEGORY,
        DIM_LANDSLIDE_SETTING, DIM_SOURCE,
        DIM_ADMIN_DIVISION, DIM_STORM, DIM_GAZETTEER

Gold (Curated)
└── Aggregated tables for business reporting
    ├── COUNTRY_FATALITIES
    ├── TRIGGER_SIZE_FATALITIES
    ├── MONTHLY_LANDSLIDES_FATALITIES
    └── QUARTERLY_LANDSLIDE_FATALITIES
```

---

## Business Use Cases & Key Findings

### 1. Fatalities by Country
| Rank | Country | Total Fatalities |
|------|---------|-----------------|
| 1 | India | 7,069 |
| 2 | China | 4,945 |
| 3 | Afghanistan | 2,294 |
| 4 | Philippines | 1,859 |
| 5 | Brazil | 1,743 |

### 2. Trigger Types & Severity
- **Downpour** is the most frequent trigger (4,681 events) with 17,338 total fatalities
- Tropical cyclones are the deadliest per event (avg. 5.15 fatalities/event)
- Monsoon rainfall shows high lethality (avg. 5.6 fatalities/event)

### 3. Seasonal Patterns
- Events and fatalities spike sharply in **June–August** (summer monsoon season)
- Q3 (Jul–Sep) sees the highest event count (3,332 events)
- Q2 (Apr–Jun) has the highest fatalities (12,737) due to early-summer heavy rainfall

---

## AI & Advanced Analytics

### Snowflake Cortex AI Functions (Bronze Layer)
```sql
-- Sentiment analysis
AI_SENTIMENT(EVENT_DESCRIPTION) → DESCRIPTION_SENTIMENT

-- Text summarization
SNOWFLAKE.CORTEX.SUMMARIZE(EVENT_DESCRIPTION) → DESCRIPTION_SUMMARY
```

### Cortex Search (Semantic Search)
Built `BRONZE.LANDSLIDE_DESC_SEARCH_SVC` for semantic search over unstructured event descriptions:
- `"heavy rain monsoon"` → rainfall-related events
- `"killed buried houses destroyed homeless"` → high-casualty events
- `"earthquake rockfall road collapse"` → seismic-triggered landslides

### Cortex Analyst (Natural Language → SQL)
Semantic model on Silver layer enabling natural language business queries:
- "Which countries have the highest landslide fatalities over time?"
- "What are the most common triggers and their severity?"
- "What seasonal patterns exist in landslide occurrences?"

---

## Incremental Loading Pipeline

Automated Bronze → Silver → Gold pipeline using Snowflake Streams & Stored Procedures:

```
New data arrives (simulated CSV)
    ↓
BRONZE.LANDSLIDE_STREAM (captures new rows only)
    ↓
SP_LOAD_FACT_LANDSLIDE (Stored Procedure)
    ├── Step A: Silver Load — consume stream, append to FACT_LANDSLIDE
    └── Step B: Gold Refresh — recompute aggregation tables
```

**Validation:** Bronze 11,033 → 11,038 rows; Silver & Gold updated correctly ✅

---

## Streamlit Dashboard

3 interactive visualizations built on Gold Layer data:
- Fatalities by country (bar chart)
- Event count and fatality distribution by trigger type
- Monthly/quarterly landslide trend analysis

---

## Tech Stack

```
Snowflake
├── SQL                   — Bronze/Silver/Gold layer design & ETL
├── Snowflake Streams     — incremental data capture
├── Stored Procedures     — automated pipeline
├── Cortex AI Functions   — sentiment analysis, text summarization
├── Cortex Search         — semantic search service
├── Cortex Analyst        — natural language querying
└── Streamlit in Snowflake — dashboard visualization
```

---

## Repository Structure

```
├── Projectcode_ClawJawCoil-1.ipynb     # Full SQL code (Snowflake Notebook)
├── Global_Landslide_Catalog_Export.csv # Raw dataset
├── PresentationSlides_ClawJawCoil.pdf  # Final presentation slides
└── README.md
```

---

## Data Source

**NASA Global Landslide Catalog (GLC)**  
Maintained by NASA Goddard Space Flight Center  
Available via [Kaggle](https://www.kaggle.com/datasets/nasa/landslide-events)

---

*MIS 381N: Information Management | University of Texas at Austin · Fall 2025*
