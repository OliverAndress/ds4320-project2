# DS 4320 Project 2: Predicting Carbon Offset Success Using Relational Risk Modeling

## Executive Summary

This repository presents a data-driven framework for modeling the physical constraints and scaling laws of global energy infrastructure. By migrating 29,910 power generation records into a document-oriented MongoDB Atlas cluster, we have synthesized a high-performance analytical pipeline covering 164 countries. Our Random Forest machine learning architecture analyzes the non-linear relationship between geospatial coordinates and Nameplate Capacity (MW), revealing that location is a dominant predictor of industrial scale. This project provides a scalable, automated solution for forecasting energy infrastructure footprints, enabling strategic stakeholders to conduct initial market sizing and infrastructure assessments through rigorous feature importance analysis and geospatial benchmarking.

**Author:** [Oliver Andress]  

**NetID:** [csg7su]  

**DOI:** [DOI]

**License:** [MIT License]

### Quick Links
- **Press Release:** [Press Release](PRESS_RELEASE.md)
- **Data:** [Data Folder](./data)
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

**Headline:** UVA Data Scientists Unveil AI Driven Global Grid Intelligence to Predict World Energy Infrastructure

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
This dataset originates from the **World Resources Institute (WRI)**, specifically the **Global Power Plant Database (v1.3.0)**. The WRI is a global research non-profit that established this database as a comprehensive, open-source inventory of the world’s power generation assets. The raw data was acquired as a standardized CSV on **March 15, 2026**, containing approximately 29,910 records across 164 countries. 

The dataset aggregates information from national sub-registries, government ministries, and utility reports. Key attributes include plant name, nameplate capacity (MW), primary fuel type, and high-precision geospatial coordinates. For this project, the data was migrated from its flat-file origin into a **MongoDB Atlas** cluster. This transition involved a custom ETL process to "documentize" the data, converting flat columns into nested BSON objects (e.g., `location` and `specs`) to better reflect the hierarchical nature of industrial infrastructure.

### Code Documentation

| File | Description | Location |
| :--- | :--- | :--- |
| **ingestion_pipeline.py** | Orchestrates the migration from raw CSV to MongoDB Atlas. Includes the transformation logic for nested document nesting, connection string handling via `.env`, and comprehensive error logging to `ingestion.log`. | [ingestion_pipeline.ipynb](code/ingestion_pipeline.ipynb) |
| **analysis_pipeline.ipynb** | The primary Jupyter Notebook containing the database query, data flattening, Random Forest modeling, and publication-quality visualization generation. | [analysis_pipeline.ipynb](code/analysis_pipeline.ipynb) |

### Bias Identification
Systematic biases within global energy reporting must be acknowledged to interpret the model's predictions accurately:
* **Reporting Bias:** Data quality varies significantly by national transparency standards. Countries with mature regulatory frameworks (Global North) provide granular, verified data, while data from developing regions may rely on older, non-verified estimates.
* **Size (Utility) Bias:** There is a clear bias toward utility-scale facilities. Smaller, decentralized power sources (e.g., private residential solar or micro-hydro) are frequently omitted from national registries, potentially skewing the model toward larger "Nameplate" averages.
* **Technological Lag:** Given the rapid expansion of renewables, the database may under-represent the most recent "distributed" energy projects (Wind/Solar) while maintaining a complete record of older, centralized fossil fuel assets.
* **Geospatial Precision Bias:** While coordinates are provided, "fuzzy" location data in certain sensitive regions can introduce noise into the model's ability to cluster plants by regional industrial hubs.

### Bias Mitigation
To account for these biases and ensure a robust predictive tool, the following strategies were implemented:
* **Logarithmic Scaling:** We applied a log-transformation to the capacity target to mitigate the extreme "Size Bias" between 1 MW and 20,000 MW plants. This ensures the model is not dominated by a few outlier "Mega-Plants."
* **One-Hot Feature Encoding:** By treating "Fuel Type" as a categorical feature, we allow the model to learn the specific scale-constraints of different technologies (e.g., knowing that "Nuclear" implies a large scale regardless of location).
* **Stratified Evaluation:** The use of **RMSE** alongside **R-squared** allows us to quantify the magnitude of our errors across different scales, identifying where the "Reporting Bias" may be creating higher variance.
* **Geospatial Feature Weighting:** The Random Forest model naturally identifies Feature Importance, allowing us to quantify how much "Longitude" (a proxy for regional development) overrides specific technological data, thus identifying geographic bias in the results.

### Rationale for Critical Decisions
* **NoSQL Document Architecture:** I chose to move from CSV to **MongoDB** to handle the "Implicit Schema" of power plants. Since different energy types often have different metadata (e.g., Dam height for Hydro vs. Panel type for Solar), a document store prevents the "Column Bloat" found in traditional SQL databases.
* **Nested BSON Objects:** I made the decision to group `latitude` and `longitude` into a single `location` sub-object. This enforces data integrity, ensuring that coordinates are treated as a single geospatial point rather than two independent numbers.
* **Pipeline-Based Preprocessing:** All transformations (One-Hot Encoding and Column Transformation) were handled within a **Scikit-Learn Pipeline**. This prevents **Data Leakage** by ensuring that the categorical mapping learned on the training set is the only information used to transform the test set.
* **Inclusion of "Unknown" Fuels:** Rather than dropping records with missing fuel data, I retained them to maintain a realistic "Global" sample size. The model is forced to rely on geospatial coordinates for these records, testing the robustness of the location-based features.
* **Uncertainty Sources:** Primary uncertainties include: (1) Temporal drift in plant decommissioning, (2) Subjectivity in "Primary Fuel" for multi-fuel plants, and (3) Inconsistencies in how "Nameplate Capacity" is measured across different national standards. These limitations are acknowledged as the primary drivers of the observed variance in the $R^2$ results.
  
## Metadata
---

### Implicit Schema Guidelines

To maximize the flexibility of the **MongoDB Atlas** document store, this project adheres to a "Document-First" design philosophy. Unlike a rigid SQL schema, the implicit schema for the `power_grid_db` was designed using the following guidelines:

1.  **Logical Grouping (Nesting):** High-level attributes are grouped into sub-objects based on their domain. For instance, `latitude` and `longitude` are nested within a `location` object, while technical metrics like `capacity_mw` are nested within a `specs` object. This ensures that a single document represents a cohesive physical entity.
2.  **Data Typing for Performance:** Numerical values that require mathematical operations (e.g., coordinates and capacity) are explicitly cast as `Double` or `Decimal128` during ingestion. This prevents the computational overhead of string-to-float conversion during the ML pipeline's query phase.
3.  **Handling Sparse Metadata:** The schema supports "Polymorphism," allowing renewable plants to store additional metadata (e.g., `solar_type` or `wind_turbine_count`) that fossil-fuel plants omit, without requiring the creation of thousands of NULL values in a flat table.

---

### Data Summary

| Attribute | Value |
| :--- | :--- |
| **Total Records** | 29,910 |
| **Unique Countries** | 164 |
| **Primary Target** | `capacity_mw` |
| **Database Type** | NoSQL (Document-Oriented) |
| **Storage Engine** | WiredTiger (MongoDB Atlas) |
| **Collection Name** | `plants` |

---

### Data Dictionary

| Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `plant_name` | String | The official name of the power generation facility. | "Three Gorges Dam" |
| `capacity_mw` | Float64 | The nameplate capacity of the plant in Megawatts. | 22500.0 |
| `primary_fuel` | String | The main energy source used for generation. | "Hydro" |
| `latitude` | Float64 | Precision coordinate of the plant's location. | 30.8239 |
| `longitude` | Float64 | Precision coordinate of the plant's location. | 111.0031 |
| `country_long` | String | The full name of the country where the plant is located. | "China" |

---

### Numerical Uncertainty Quantification

In industrial infrastructure modeling, numerical features are rarely "perfect." The following table quantifies the sources of uncertainty for the predictive features:

| Feature | Estimated Uncertainty | Primary Source of Noise |
| :--- | :--- | :--- |
| **capacity_mw** | ± 5-10% | **Reporting Standards:** Discrepancies between "Gross Capacity" (total output) and "Net Capacity" (output sent to grid) vary by country. Furthermore, historical decommissioning of individual units within a plant may not be reflected in real-time in the nameplate total. |
| **latitude** | ± 0.001 - 0.1° | **Precision Variance:** Some coordinates are high-precision GPS points at the plant's generator hall, while others (especially in sensitive regions) are "fuzzy" centroids representing a general administrative area. |
| **longitude** | ± 0.001 - 0.1° | **Geospatial Drift:** Similar to latitude, the precision depends on the source (e.g., satellite verification vs. self-reported government registries). |
