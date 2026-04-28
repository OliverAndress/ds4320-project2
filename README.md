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

**General Problem Statement:** 11. Forecasting energy demand

**Refined Specific Problem Statement:** This project focuses on predicting the **Nameplate Capacity (MW)** of global power facilities by utilizing geographic coordinates (latitude and longitude) and primary fuel sources as high-dimensional features. By shifting from a macro-view of energy demand to a micro-level prediction of facility output, we can model the physical infrastructure required to meet that demand.

---

### Project Motivation
With more and more international attention on energy demands with rising AI energy costs and more consumption in general I was curious to see and look into something related to energy. When I was looking online for a data source to use I came across one in Kaggle that gave tens of thousands of different global power facilities. 

### Rationale for Refinement
The refinement from the broad domain of "forecasting energy demand" to the specific prediction of "nameplate capacity" was driven by the need for technical rigor and model interpretability. While demand is influenced by thousands of volatile socio-economic variables, capacity is a function of fixed engineering constraints and geographic suitability. By narrowing the scope to these intrinsic features, the project utilizes supervised learning techniques—such as Random Forest regression and pipeline-based feature encoding—to provide a defensible, metric-driven analysis. This specific focus allows for a deeper evaluation of how much information "location" and "technology" actually carry, revealing the latent variables—like regional economic maturity—that drive the scale of global energy infrastructure.

**Press Release**

**Headline:** UVA Data Scientists Launch ‘Carbon Integrity’ AI to Unmask the ‘Ghost Credits’ Threatening Climate Goals

**Full Press Release:** [Press Release](PRESS_RELEASE.md)

---

## Domain Exposition

### Terminology & KPIs

| Term | Definition | Why it matters in this project |
| :--- | :--- | :--- |
| **Nameplate Capacity** | The maximum intended full-load sustained output of a facility. | The primary target variable ($y$) for our predictive model. |
| **Megawatt (MW)** | A unit of power equal to one million watts. | The standard unit of measurement for our capacity predictions. |
| **Primary Fuel** | The main energy source used by a plant to generate electricity. | A key categorical feature used to differentiate power scales (e.g., Nuclear vs. Solar). |
| **Baseload Power** | The minimum amount of electric power delivered to a grid at a steady rate. | Explains why certain fuels (Nuclear/Coal) dominate high-capacity predictions. |
| **Capacity Factor** | The ratio of actual electrical energy produced to the maximum possible energy. | Distinguishes between theoretical output and actual performance in demand forecasting. |
| **Grid Interconnectivity** | The physical linking of various energy networks. | A latent variable that influences whether a plant is built for local or national scale. |
| **Geospatial Coordinates** | The latitude and longitude identifying a specific point on Earth. | The primary features used to identify regional industrial clusters. |
| **NoSQL (Document Store)** | A non-tabular database that stores data in flexible, JSON-like documents. | The architecture used in MongoDB Atlas to handle our nested power plant data. |
| **BSON** | A binary-encoded serialization of JSON-like documents. | The format MongoDB uses to store our "Implicit Schema" in the cloud. |
| **Implicit Schema** | A data structure defined by the data itself rather than a rigid table. | Allows us to store nested objects like `location` and `specs` without pre-defined SQL columns. |
| **Random Forest** | An ensemble learning method that uses multiple decision trees. | Our chosen model for capturing non-linear geospatial patterns in energy infrastructure. |
| **R-squared ($R^2$)** | A statistical measure of how close the data are to the fitted regression line. | Used to evaluate how much of the variance in capacity is explained by our features. |
| **RMSE** | Root Mean Square Error; measures the average magnitude of prediction error. | KPI used to determine how many Megawatts our model is "off" by on average. |
| **One-Hot Encoding** | Converting categorical text into a binary numerical format. | Necessary for the model to process "Fuel Type" as a mathematical input. |
| **Log-Log Scaling** | Applying a logarithmic transformation to both the $x$ and $y$ axes. | Critical for visualizing power plants that span four orders of magnitude in size. |
| **Feature Importance** | A technique that assigns scores to input features based on their predictive power. | Used to prove that Longitude and Latitude are the dominant drivers of plant scale. |

--- 

### Domain Context: Global Energy Infrastructure

This project exists within the domain of **Global Energy Infrastructure and Utility Management**. It addresses the physical supply side of the "Forecasting Energy Demand" problem by analyzing the distribution and scale of the world's power generation assets. In a globalized economy, energy capacity is not just a technical metric; it is a proxy for regional industrial maturity, economic development, and carbon intensity. 

While energy demand is often studied through the lens of consumption, this analysis focuses on the structural reality of the power Grid. By utilizing a document-oriented database to store 29,910 facilities across 164 countries, the project seeks to understand the "Scaling Laws" of infrastructure. It investigates how geographic placement and technological choice (fuel type) dictate the size of a plant. In a professional strategic context, this modeling is vital for "Market Sizing" and identifying where the physical limitations of a regional grid may hinder future industrial growth or transition to renewable energy.



---

### Background Reading Summary

Background Reading
All background reading materials are available in this folder: [Background Readings Folder](https://myuva-my.sharepoint.com/:f:/g/personal/csg7su_virginia_edu/IgCCNLVdFR9XRbgJ_3TWUNmOAaQX6Hq8oM8zLB38e8zEydw?e=OkBdaw) 

The background_reading folder contains five high-impact articles and strategic reports that provide the domain-specific context for this energy infrastructure pipeline. These sources cover the technical definitions of capacity, the global shift toward renewables, and the emerging demand drivers like AI and data centers.

| Title | Brief Description | Link to OneDrive File |
| :--- | :--- | :--- |
| **The Biggest Power Plants: Nameplate vs. Generation** | Explores the critical distinction between Nameplate Capacity and actual generation, defining the "Capacity Factor" as the bridge between theoretical output and grid reality. It provides the technical justification for the variance in predictive models across different fuel types. | [View PDF](https://myuva-my.sharepoint.com/:b:/g/personal/csg7su_virginia_edu/IQDk839zB_QbTZvwmRfvrVHlAeLiEW2_kMBtYoHtW_hJSFc?e=NEOtmx) |
| **100% Renewable Energy Roadmap for 139 Countries** | A strategic research summary detailing the technical and economic pathways for a global transition to zero-carbon energy by 2050. It analyzes the role of market forces in displacing fossil fuels and the infrastructure scaling required for 99% of global emissions coverage. | [View PDF](https://myuva-my.sharepoint.com/:b:/g/personal/csg7su_virginia_edu/IQCnCmtxH7oQS6od040FS9TjASnfz-QAa95XmSeD8L_0VZ8?e=gF1Kvu) |
| **IEA: The Impact of AI on Global Energy Demand** | Analyzes the emergence of AI and data centers as primary drivers of new electricity consumption. This report highlights the geospatial clustering of demand in the U.S. and China, providing the "why" behind the significance of location features in modern infrastructure forecasting. | [View PDF](https://myuva-my.sharepoint.com/:b:/g/personal/csg7su_virginia_edu/IQCKGKAgoToJQLx9rvq9obFfAdLQRh5yurMO6DcUXCKZaWg?e=MhD1rT) |
| **China’s Structural Shift: From Coal to Clean Foundations** | Provides a deep dive into the energy transition of the world’s largest electricity consumer. It documents the first absolute decline in coal generation and the exponential rise of renewables, offering essential context for regional variance in global energy datasets. | [View PDF](https://myuva-my.sharepoint.com/:b:/g/personal/csg7su_virginia_edu/IQAtHSyVEghZQ5WqdJFsuTUyAY4HltBxa_IaNg0oOD8eLVI?e=BNx8iW) |
| **IEA: World Energy Supply and Transformation** | A macro-level overview of global energy balances, transformation efficiencies, and final consumption sectors. It establishes the baseline for the current fossil-fuel-dominated grid and the industrial sectors dictating the scale of required capacity. | [View PDF](https://myuva-my.sharepoint.com/:b:/g/personal/csg7su_virginia_edu/IQD23tmq83TWSrxtc5vvDwSEARWC18Q-CeK3MZuEyAHrqSA?e=vLSNZb) |

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
