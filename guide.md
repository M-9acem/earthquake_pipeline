# Earthquake Data Engineering Pipeline: Databricks-Centered Implementation

## Overview and Architecture

### Business Case

Earthquake data is incredibly valuable for understanding seismic events and mitigating risks. Government agencies, research institutions, and insurance companies rely on up-to-date information to plan emergency responses and assess risks. With this automated pipeline, we ensure these stakeholders get the latest data in a way that's easy to understand and ready to use, saving time and improving decision-making.

### Architecture Overview

This pipeline follows a modern, unified architecture centered on Databricks, ensuring scalability, reliability, and efficiency. The architecture includes:

1. **Data Ingestion**: Direct ingestion of earthquake data from the USGS Earthquake API into Databricks.
2. **Data Processing**: Implementation of the medallion architecture (bronze, silver, gold) within Databricks.
3. **Data Storage**: Databricks File System (DBFS) and external storage via Azure Data Lake Storage.
4. **Workflow Orchestration**: Databricks Workflows for managing the entire ETL process.
5. **Data Analysis**: Azure Synapse Analytics for querying processed data.
6. **Visualization**: Power BI integration for interactive dashboards.

### Data Modeling: Medallion Architecture

The implementation follows the medallion architecture pattern:

1. **Bronze Layer**: Raw data ingested directly from the API, stored in Parquet format.
2. **Silver Layer**: Cleaned and normalized data with improved quality and structure.
3. **Gold Layer**: Enriched data with business value (e.g., added country codes).

### Understanding the API

- The earthquake API provides detailed seismic event data for a specified start and end date.
- **API URL**: `https://earthquake.usgs.gov/fdsnws/event/1/`

### Key Benefits

- **Unified Platform**: All ETL operations within Databricks, eliminating tool switching.
- **Simplified Orchestration**: Databricks Workflows instead of Azure Data Factory.
- **Improved Collaboration**: Common environment for data engineers and data scientists.
- **Enhanced Monitoring**: Built-in monitoring and logging capabilities.

---

## Step 1: Create an Azure Account
1. Sign up for an Azure account if you do not already have one.

---

## Step 2: Create a Databricks Workspace
1. Create a Databricks workspace in Azure.
2. Select the **Standard LTS (Long Term Support)** tier.

---

## Step 3: Set Up a Storage Account
1. Create a Storage Account with **hierarchical namespaces** enabled.
2. Create three containers: `bronze`, `silver`, and `gold`.
3. Configure appropriate access permissions for Databricks.

---

## Step 4: Configure Databricks for External Storage
1. Open the Databricks workspace.
2. Set up credentials and external locations for the storage containers.
3. Ensure proper permissions are in place for read/write operations.

---

## Step 5: Create Databricks Notebooks
1. Create separate notebooks for each data layer:
   - `bronze_notebook.py`: Handles API data ingestion and raw storage
   - `silver_notebook.py`: Performs data cleaning and normalization
   - `gold_notebook.py`: Enriches data with business context

2. Implement code from the provided templates or from this repository.

---

## Step 6: Install Required Libraries
1. Install necessary libraries at the cluster level:
   - Navigate to **Compute > Cluster > Libraries > + Install New Library**
   - Install `reverse_geocoder` for location enrichment

---

## Step 7: Performance Optimization
1. Implement performance improvements for the gold layer:
   - Use batch processing for geocoding operations
   - Consider caching for frequently accessed data
   - Implement appropriate partitioning strategies

---

## Step 8: Set Up Databricks Workflows
1. Create a Databricks Workflow to orchestrate the entire pipeline:
   - Add job tasks for each notebook (`bronze`, `silver`, `gold`)
   - Configure dependencies between tasks
   - Set parameters for dynamic execution (e.g., date ranges)

2. Schedule the workflow to run at desired intervals (e.g., daily).

---

## Step 9: Integrate Azure Synapse Analytics
1. Create a Synapse Workspace linked to your storage.
2. Query processed data using Serverless SQL:
   ```sql
   SELECT
       country_code,
       COUNT(CASE WHEN LOWER(sig_class) = 'low' THEN 1 END) AS low_count,
       COUNT(CASE WHEN LOWER(sig_class) IN ('medium', 'moderate') THEN 1 END) AS medium_count,
       COUNT(CASE WHEN LOWER(sig_class) = 'high' THEN 1 END) AS high_count
   FROM
       OPENROWSET(
           BULK 'https://<storage_account>.dfs.core.windows.net/gold/earthquake_events_gold/**',
           FORMAT = 'PARQUET'
       ) AS [result]
   GROUP BY
       country_code;
   ```

---

## Step 10: Connect Power BI for Visualization
1. Connect Power BI to Azure Synapse or directly to the gold layer data.
2. Design dashboards for key stakeholders.

---

## Key Advantages of This Implementation

1. **Simplified Architecture**: Reduced complexity by centralizing ETL in Databricks.
2. **Enhanced Development Experience**: Single platform for both development and execution.
3. **Cost Efficiency**: Fewer Azure services required, potentially reducing overall costs.
4. **Improved Maintainability**: Centralized code management and version control.
5. **Seamless Scaling**: Databricks' native scaling capabilities for handling varying loads.

This implementation demonstrates a modern approach to data engineering, leveraging Databricks as a unified platform for the entire ETL process while maintaining integration with Azure's analytics ecosystem.
