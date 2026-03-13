# The Moving Earth: Understanding Landslide Events

> MIS 381N: Information Management 수업 팀 프로젝트 | Team ClawJawCoil  
> Melissa Cai Shi · Cassandre Korvink · **Mikyung Oh**  
> University of Texas at Austin · Fall 2025

---

## 🌍 Overview

NASA의 Global Landslide Catalog(GLC) 데이터를 활용해 Snowflake 위에 **Medallion Architecture(Bronze → Silver → Gold)** 기반 데이터 웨어하우스를 구축하고, Snowflake Cortex AI 기능으로 비정형 텍스트를 분석한 프로젝트.

- **Dataset:** NASA/Kaggle Global Landslide Catalog — 11,000+ 이벤트, 31개 속성
- **Platform:** Snowflake (Data Warehouse + Cortex AI)
- **Goal:** 고위험 지역 및 트리거 패턴 식별, 사망자 분석

---

## 🏗 Architecture

### Medallion Model (3-Layer)

```
Bronze (Raw)
└── LANDSLIDE_RAW — Kaggle CSV 원본 그대로 적재. 변환 없음.
    └── + AI 컬럼: DESCRIPTION_SENTIMENT, DESCRIPTION_SUMMARY

Silver (Refined)
└── Star Schema — 정제·정규화된 데이터
    ├── FACT_LANDSLIDE (팩트 테이블)
    └── 10개 Dimension 테이블
        DIM_DATE, DIM_COUNTRY, DIM_LANDSLIDE_TRIGGER,
        DIM_LANDSLIDE_SIZE, DIM_LANDSLIDE_CATEGORY,
        DIM_LANDSLIDE_SETTING, DIM_SOURCE,
        DIM_ADMIN_DIVISION, DIM_STORM, DIM_GAZETTEER

Gold (Curated)
└── 비즈니스 보고용 집계 테이블
    ├── COUNTRY_FATALITIES
    ├── TRIGGER_SIZE_FATALITIES
    ├── MONTHLY_LANDSLIDES_FATALITIES
    └── QUARTERLY_LANDSLIDE_FATALITIES
```

---

## 📊 Business Use Cases & Key Findings

### 1. 국가별 사망자 분포
| 순위 | 국가 | 총 사망자 |
|------|------|-----------|
| 1 | India | 7,069 |
| 2 | China | 4,945 |
| 3 | Afghanistan | 2,294 |
| 4 | Philippines | 1,859 |
| 5 | Brazil | 1,743 |

### 2. 주요 트리거 및 심각도
- **Downpour(폭우)** 가 가장 빈번 (4,681건) + 총 사망 17,338명
- 트로피컬 사이클론은 건당 평균 사망자 5.15명으로 가장 치명적
- Monsoon(계절풍)도 건당 평균 5.6명으로 높은 치사율

### 3. 계절적 패턴
- **6~8월(여름 몬순 시즌)** 에 이벤트 및 사망자 급증
- Q3(7~9월)에 가장 많은 이벤트(3,332건) 발생
- Q2(4~6월)에 사망자 가장 많음(12,737명) → 초여름 집중호우 영향

---

## 🤖 AI & Advanced Analytics

### Snowflake Cortex AI Functions (Bronze Layer)
```sql
-- 감성 분석
AI_SENTIMENT(EVENT_DESCRIPTION) → DESCRIPTION_SENTIMENT

-- 텍스트 요약
SNOWFLAKE.CORTEX.SUMMARIZE(EVENT_DESCRIPTION) → DESCRIPTION_SUMMARY
```

### Cortex Search (Semantic Search)
`BRONZE.LANDSLIDE_DESC_SEARCH_SVC` 서비스로 비정형 이벤트 설명 시맨틱 검색:
- `"heavy rain monsoon"` → 강우 관련 이벤트 검색
- `"killed buried houses destroyed homeless"` → 인명 피해 이벤트 필터링
- `"earthquake rockfall road collapse"` → 지진 관련 산사태 검색

### Cortex Analyst (NL → SQL)
Silver layer 시맨틱 모델 기반 자연어 비즈니스 질의 응답:
- "Which countries have the highest landslide fatalities over time?"
- "What are the most common triggers and their severity?"
- "What seasonal patterns exist in landslide occurrences?"

---

## 🔄 Incremental Loading Pipeline

Snowflake Streams & Stored Procedures로 자동화된 Bronze → Silver → Gold 파이프라인 구축:

```
새 데이터 도착 (Global_Landslide_Data_New_Dummy.csv)
    ↓
BRONZE.LANDSLIDE_STREAM (새 행만 캡처)
    ↓
SP_LOAD_FACT_LANDSLIDE (Stored Procedure)
    ├── Step A: Silver Load — Stream 소비 후 FACT_LANDSLIDE에 Append
    └── Step B: Gold Refresh — 집계 테이블 재계산
```

**검증 결과:** Bronze 11,033 → 11,038행, Silver/Gold 동일하게 반영 ✅

---

## 📈 Streamlit Dashboard

Gold Layer 데이터 기반 3가지 인터랙티브 시각화:
- 국가별 사망자 수 Bar Chart
- 트리거 유형별 이벤트 수 및 사망자 분포
- 월별/분기별 랜드슬라이드 트렌드

---

## 🛠 Tech Stack

```
Snowflake
├── SQL                  — Bronze/Silver/Gold 레이어 설계 및 ETL
├── Snowflake Streams    — 증분 데이터 캡처
├── Stored Procedures    — 자동화 파이프라인
├── Cortex AI Functions  — 감성분석, 텍스트 요약
├── Cortex Search        — 시맨틱 검색 서비스
├── Cortex Analyst       — 자연어 쿼리
└── Streamlit in Snowflake — 대시보드 시각화
```

---

## 📁 Repository Structure

```
├── Projectcode_ClawJawCoil-1.ipynb    # 전체 SQL 코드 (Snowflake Notebook)
├── Global_Landslide_Catalog_Export.csv # 원본 데이터셋
├── PresentationSlides_ClawJawCoil.pdf  # 최종 발표 슬라이드
└── README.md
```

---

## 📖 Data Source

**NASA Global Landslide Catalog (GLC)**  
Maintained by NASA Goddard Space Flight Center  
Available via [Kaggle](https://www.kaggle.com/datasets/nasa/landslide-events)

---

*MIS 381N: Information Management | University of Texas at Austin · Fall 2025*
