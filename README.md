# NYC_Yellow_Taxi_Data_Pipeline_Analysis_on_GCP

## Overview
This project showcases an end-to-end data pipeline on **Google Cloud Platform (GCP)** for the **NYC Yellow Taxi** dataset. Raw Parquet files are uploaded to **Cloud Storage**, modeled and transformed with **Dataform**, and served in a clean, analytics-ready format. Insights are presented via an interactive **Looker Studio** dashboard.

## Architecture
**Source → Cloud Storage (raw) → Dataform (transform/model) → BigQuery (warehouse) → Looker Studio (viz)**

**GCP services:** Cloud Storage, BigQuery, Dataform (plus Looker Studio for visualization)

## Dataset
- **NYC TLC Trip Record Data** (multiple years; large-scale real-world trips)
- Typical fields: pickup/dropoff timestamps, locations, trip distance, fare, passenger count.

## Pipeline Steps
1. **Ingest**: Manually upload Parquet files to a designated Cloud Storage bucket.
2. **Transform/Model**: Use **Dataform** to clean, standardize schemas, and build partitioned/clustered BigQuery tables.
3. **Serve**: Publish curated datasets in BigQuery for downstream analysis.
4. **Visualize**: Connect BigQuery to **Looker Studio** to deliver interactive dashboards.

## Tech Stack
- **Cloud**: GCP (Cloud Storage, BigQuery), Dataform
- **Languages**: SQL
- **Visualization**: Looker Studio

## Dataform Project Structure

- **Loading Area** → Ingests data from Cloud Storage and creates raw base tables in BigQuery.  
- **Staging Layer** → Cleans and deduplicates data, builds incremental tables with upsert logic for efficient updates.  
- **Serving Layer** → Models data into a star schema for high-performance analytics and sharing across departments.  
- **BI Dashboards** → Provides views and aggregated tables optimized for BI tools, reducing cost and improving dashboard speed.

## Data Model

The project follows a **star schema** design to optimize query performance in BigQuery and support analytics use cases.

- **fact_trip_date**: Central fact table containing detailed trip-level records (pickup/dropoff times, distance, fares, surcharges, payment type, etc.).  
- **dim_date**: Calendar dimension for temporal analysis (year, quarter, month, weekday, weekend flag).  
- **dim_vendor**: Vendor dimension mapping Vendor IDs to vendor names.  
- **dim_location**: Location dimension containing LocationID, borough, zone, service zone, and geometry for geospatial analysis.  
- **dim_payment**: Payment dimension describing payment types and methods.

This structure enables:
- Efficient **joins** for BI dashboards (fact → dimension tables).  
- Flexible analysis by **time, vendor, location, or payment type**.  
- Support for reusable filters in Looker Studio (e.g., Zone filter can be applied globally via dim_location.Zone).

<img width="534" height="624" alt="Screenshot 2025-09-10 at 11 33 27" src="https://github.com/user-attachments/assets/d17b0d00-2f18-4990-ae74-557d9a64b03e" />

  

## Dashboard Insights

The final data is visualized in an interactive [Looker Studio Dashboard](https://lookerstudio.google.com/reporting/b3caf304-18ff-41cd-9ee9-392a3d21c7e3).

### Page 1: Overall Performance
- Key KPIs: total fare revenue, total trips, average trip revenue, and average trip distance.  
- Vendor comparison by revenue, trip count, and average fare.  
- Breakdown of revenue by rate type and payment method.  

### Page 2: Trip Behavior Analysis
- Top 10 pickup zones and hotspot mapping by vendor.  
- Distribution of trips by weekday and by hour of the day.  
- Trends over time, including year-over-year comparisons of trip volumes.  

## Data Source & License
This project uses the **NYC TLC Trip Record Data**, made publicly available by the  
[New York City Taxi and Limousine Commission](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page).  

The dataset is part of [NYC Open Data](https://opendata.cityofnewyork.us/),  
licensed under the [Open Data Commons Public Domain Dedication and License (PDDL)](https://opendatacommons.org/licenses/pddl/).
