# Earthquake Data Engineering Pipeline

A comprehensive data engineering solution for processing USGS earthquake data using Databricks as the central platform with medallion architecture.

## Architecture Overview

This project implements a full ETL pipeline for earthquake data using Databricks as the primary platform for both processing and orchestration.

![Architecture Diagram](./images/architecture_diagram.png)
*Architecture diagram showing the full data flow from USGS source through Databricks processing layers to Azure Synapse Analytics and Power BI*

## Key Features

- **Unified Databricks Platform**: Entire ETL process runs within Databricks
- **Medallion Architecture Implementation**: 
  - Bronze layer (raw data)
  - Silver layer (cleaned data)
  - Gold layer (enriched data with business context)
- **Databricks Workflows**: Orchestration of all pipeline components
- **Azure Synapse Analytics Integration**: For advanced querying and data serving
- **Power BI Visualization**: For creating interactive dashboards

## Workflow Orchestration

The pipeline uses Databricks Workflows instead of Azure Data Factory, providing a more streamlined approach with all components in one platform.

![Databricks Workflows](./images/databricks_workflows.png)
*Screenshot showing the Databricks Workflows jobs executing the bronze, silver, and gold notebook tasks*

## Data Analysis

The processed data is made available for analysis through Azure Synapse Analytics, enabling SQL queries against the gold layer data.

![Synapse Analytics Query](./images/synapse_query.png)
*Example query in Azure Synapse Analytics showing earthquake counts by country and severity*

## Implementation Details

### Data Source
- USGS Earthquake API: `https://earthquake.usgs.gov/fdsnws/event/1/`

### Processing Layers
1. **Bronze Layer**: Raw ingestion from API
2. **Silver Layer**: Data cleaning and standardization
3. **Gold Layer**: Enrichment with geographical context (country codes)

### Technologies Used
- Azure Databricks
- Azure Data Lake Storage Gen2
- Azure Synapse Analytics
- Power BI

## Benefits of This Approach

- **Simplified Architecture**: Reduced complexity by centralizing ETL in Databricks
- **Enhanced Development Experience**: Single platform for both development and execution
- **Improved Data Quality**: Medallion architecture ensures proper data governance
- **Cost Efficiency**: Fewer Azure services required
- **Seamless Scaling**: Native Databricks scaling for handling varying data loads

## Getting Started

1. Clone this repository
2. Configure your Azure environment as specified in the setup guide
3. Deploy the Databricks notebooks from the `/notebooks` directory
4. Configure Databricks Workflows based on the provided examples
5. Connect to Azure Synapse Analytics for querying the processed data

## Requirements

- Azure subscription
- Databricks workspace
- Azure Storage Account with hierarchical namespace
- Azure Synapse workspace (for advanced querying)

## Setup Guide

For detailed setup instructions, refer to the [GUIDE.md](./GUIDE.md) file.

## License

[MIT](LICENSE)
