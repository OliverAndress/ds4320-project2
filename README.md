# DS 4320 Project 2: Predicting Carbon Offset Success Using Relational Risk Modeling

## Executive Summary

This repository presents a data science-driven solution to the "trust gap" in global carbon offset markets. By deconstructing fragmented voluntary carbon market data into a 3rd Normal Form (3NF) Star Schema, we have synthesized a high-integrity analytical dataset ($D_1$) covering 11,245 projects across 92 countries. Our Random Forest machine learning architecture achieves an 81% accuracy rate in predicting the "Retirement Ratio"—the primary KPI for institutional market trust. This project provides a scalable, automated framework for identifying high-integrity carbon credits, enabling investors to mitigate "greenwashing" risk through rigorous feature importance analysis and historical developer benchmarking.

**Author:** [Oliver Andress]  

**NetID:** [csg7su]  

**DOI:** [[DOI]


**License:** [MIT License]

### Quick Links
- **Press Release:** [Press Release]
- **Data:** [OneDrive Data Folder](
- **Pipeline:** [Analysis Pipeline](./code/pipeline.ipynb)



---

## Problem Definition

**General Problem:**


**Refined Specific Problem:**

The primary challenge in carbon market analysis is the "Selection Bias" inherent in registry data, as the dataset only includes projects that successfully cleared the rigorous initial registration process. Consequently, this project defines success not as **Market Utilization**.

The specific problem is to predict whether a registered reforestation initiative will be **Effective** (demonstrating high credit retirement rates and consistent issuance) or **Underperforming** (functioning as a "Ghost Project" with issued credits that fail to reach the market). By leveraging registry affiliation, geographic region, and project type, the model identifies the risk factors that lead a registered project to stall or fail in its operational phase.

**Rationale:**

<>

**Motivation Paragraph**

<>

**Press Release**

**Headline:** UVA Data Scientists Launch ‘Carbon Integrity’ AI to Unmask the ‘Ghost Credits’ Threatening Climate Goals

**Full Press Release:** [Press Release](PRESS_RELEASE.md)

---

## Domain Context: The Voluntary Carbon Market (VCM)

### Terminology & KPIs

<Insert Markdown Table>


---

<Insert which context and domains this is situated in>

---

### Background Reading Summary

Background Reading
All background reading materials are available in this folder: [Background Readings Folder] <insert link> 

The background_reading folder contains five high-impact...


<Insert Markdown table> 

---

## Data Creation

### Data Provenance

<Insert later> 

### Code Documentation


<Insert Markdown> 


### Bias Identification

Several sources of systematic bias exist in this  data that must be acknowledged:

<Insert Later> 

### Bias Mitigation

To address these identified biases and ensure a robust risk model, we implement the following strategies:

<Insert Later> 

### Rationale for Critical Decisions

<insert Later> 
  
## Metadata
---

Insert later
---

### Data Dictionary

Complete data dictionary documenting all features across the established Secondary Data Set ($D_1$) used for Machine Learning.

#### Table 1: PROJECTS (Central Fact Table)

| Column | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| **project_id** | string | Unique identifier for each carbon offset project (Primary Key). | "VCS123" |
| **developer_id** | int64 | Reference to the entity responsible (Foreign Key → developers.developer_id). | 1042 |
| **location_id** | int64 | Reference to the geographic site (Foreign Key → locations.location_id). | 505 |
| **method_id** | int64 | Reference to the verification rules (Foreign Key → methodology.method_id). | 88 |
| **project_name** | string | The official name of the project as listed in the registry. | "Southern Cardamom REDD+" |
| **broad_category**| string | Engineered feature binning 60+ specific types into 8 industrial sectors. | "Nature-Based" |

#### Table 2: DEVELOPERS (Dimension)

| Column | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| **developer_id** | int64 | Unique numerical identifier for the developer (Primary Key). | 1042 |
| **developer** | string | The specific entity or firm name. | "South Pole" |

#### Table 3: LOCATIONS (Dimension)

| Column | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| **location_id** | int64 | Unique identifier for a country/region pair (Primary Key). | 505 |
| **country** | string | The nation where the project site is physically located. | "Cambodia" |
| **region** | string | The global geographic theater of operation. | "Latin America" |

#### Table 4: METHODOLOGY (Dimension)

| Column | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| **method_id** | int64 | Unique identifier for a registry/protocol pair (Primary Key). | 88 |
| **registry** | string | The official body responsible for issuing and verifying credits. | "Verra (VCS)" |
| **protocol** | string | The specific methodology ruleset used during the audit. | "VM0009" |

#### Table 5: PERFORMANCE (Fact Extension)

| Column | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| **project_id** | string | Project identifier (Primary Key, Foreign Key → projects.project_id). | "VCS123" |
| **issued_num** | float64 | Total metric tonnes of $CO_2$ credits created. | 1540200.0 |
| **retired_num** | float64 | Total metric tonnes of $CO_2$ credits permanently removed. | 850000.0 |
| **retirement_ratio**| float64 | **Target Variable**: The ratio of retired credits to total issued credits. | 0.55 |
---

### Quantitative Uncertainty of Numerical Features

* **Measurement Error ($\pm 3\%$):** `issued_num` and `retired_num` are subject to registry reporting lags (Vintage Lag). Data for the most recent 12 months is considered "preliminary" and may be updated as final audits close.
* **Calculation Stability:** `retirement_ratio` uncertainty increases for projects with `issued_num` < 1,000, as small transactions create high ratio volatility.
* **Type Transformation:** The conversion from raw strings (e.g., "1,200,500") to `float64` was validated against a random sample to ensure no data loss during the SQL `CAST` operation.
