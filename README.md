# Azure Data Factory COVID-19 Reporting

### Concept of the Project 💡
- This project involves the acquisition of several Covid-19 Datasets from the ECDC website. These datasets are subsequently processed through diverse ADF components to effect transformations. These transformations are executed using ADF and Databricks. The resultant data is then loaded into Data Lake with the intention of enabling the Analytics team to draw meaningful and practical insights from these datasets. The primary objective is to comprehensively understand the workings of ADF.

### Task 🎯
- This project's mission is to ingest data from multiple data sources, clean it up, and alter it using Data Flows and Databricks. Once the data has been cleaned, it should be imported into a processed Datalake Gen2 container.
  
### Source Data: 📤
- ECDC (https://www.ecdc.europa.eu/en/covid-19)
- Population Data From Azure Files (eurostat_data)
- [Download these Datasets](https://github.com/ayush9892/Azure_Data_Factory_COVID_19_Reporting/tree/main/Datasets)

### Destination: 📥📍
- Azure Data Lake Gen2 Storage

## Tools ⚙

### Data Integration/Ingestion

- ADF Pipelines

### Transformation

- Data Flows within the Data Factory
- Databricks


## Approach

### Environment Setup
- Azure Subscription
- Data Factory
- Data Lake Storage Gen2
- Azure Databricks Cluster


### DATA EXTRACTION/ INGESTION
Four different datasets were ingested from both the ECDC website and Azure files into Datalake Gen2. They are - 

- Cases and Deaths Data (ECDC)
- Hospital Admissions Data (ECDC)
- Test Conducted Data (ECDC)
- Population Data



### DATA TRANSFORMATION

The below datasets was transformed using ADF Data flows  -
- Cases and Deaths Data 
- Hospital Admissions Data

  #### Steps for Cases and Death transformation: -
  1. Cases And Deaths Source (Azure Data Lake Storage Gen2 )
  2. Filter Europe-Only Data
  3. Select only the required columns
  4. PivotCounts using indicator Columns(confirmed cases, deaths) and get the sum of daily cases count
  5. Lookup Country to get country_code_2_digit,country_code_3_digit columns
  6. Select Only the required columns for the Sink
  7. Create a Sink dataset (Azure Data Lake Storage Gen2)
  ![image](https://github.com/ayush9892/Azure_Data_Factory_COVID_19_Reporting/assets/85745368/77163a70-46e6-4ac2-b9a5-a599e707ba37)

  #### Steps for Hospital Admissions transformation: -
  1. Hospital Admissions Source (Azure Data Lake Storage Gen2 )
  2. Select only the required columns
  3. Lookup Country to get country_code_2_digit,country_code_3_digit columns
  4. Select only the required columns
  5. Condition to Split Weekly and Daily
    - indicator=='Weekly new hospital admissions per 100k' || indicator=='Weekly new ICU admissions per 100k'
    - indicator== "Daily hospital occupancy" || indicator=="Daily ICU occupancy"
  6. For Weekly Path
    - Join with Date to get ecdc_Year_week, week_start_date, week_End_date
    - Piovt Counts using indicator Columns(confirmed cases, deaths) and get the sum of daily cases count
    - Sort data using reported_year_week ASC and Country DESC
    - Select only required columns for sink
    - Create a sink dataset (Azure Data Lake Storage Gen2)
  7. For Daily Path
    - Pivot Counts using indicator Columns(confirmed cases, deaths) and get the sum of daily cases count
    - Sort Data using reported_year_week ASC and Country DESC
    - Select only required columns for sink
    - Create a sink dataset (Azure Data Lake Storage Gen2)
  ![image](https://github.com/ayush9892/Azure_Data_Factory_COVID_19_Reporting/assets/85745368/01280c7f-22a4-4d9c-85b6-0b1ff8ae68ef)


The below datasets was transformed using Azure Databricks -
- Population Data
- Test Conducted Data



# Used Technologies
- Azure DataFactory
- Azure Databricks (Pyspark)
- Azure Storage Account
- Azure Data Lake Gen2





