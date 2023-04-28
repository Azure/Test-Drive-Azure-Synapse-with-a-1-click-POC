## Module 2: Azure Synapse Data Explorer Pool  
Module 2 will be focused on the basic steps to load and analyze the Trip Data (time-series data) with Data Explorer Pool for Azure Synapse.  

## Create a Data Explorer Pool  
1. In Synapse studio, on the left-side pane, select **Manage > Data Explorer pools**
2. Select **New**, and then enter the following details on the **Basics** tab:  
   | Setting | Value | Description |
   |:------|:------|:------
   | Data Explorer Pool Name | adxpooltaxitrip | This is the name the Data Explorer pool will have |
   | Workload | Compute Optimized | This workload provides a higher CPU to SSD storage ratio. |
   | Node Size | Small(4 cores) | Set this to the smallest size to reduce costs for this quickstart |  
 3. Select **Review + Create > Create.** Your data explorer will start the provisioning process. Once it is complete move on to the next step.

![Creating ADX pool](https://github.com/Azure/Test-Drive-Azure-Synapse-with-a-1-click-POC/raw/nataliarodri906-patch-1/images/gif1.gif)

## Create a Data Explorer Database  
1. In Synapse Studio, on the left-side pane, Select **Data**.  
2. Select + (Add new resource) > **Data Explorer Database** and paste the following information:  
   | Setting | Value | Description |
   |:------|:------|:------
   | Data Explorer Pool Name | adxpooltaxitrip | The name of the Data Explorer pool to use |
   | Name | taxitripdatabase | This database name must be unique within the cluster. |
   | Default retention period | 365 | The time span (in days) for which it's guaranteed that the data is kept available to query. The time span is measured from the time that data is ingested. |   
   |Default cache period | 31 | The time span (in days) for which to keep frequently queried data available in SSD storage or RAM, rather than in longer-term storage  
3. Select **Create** to create the database. Creation typically takes less than a minute.  

![Creating Data Explorer Pool](https://github.com/Azure/Test-Drive-Azure-Synapse-with-a-1-click-POC/raw/nataliarodri906-patch-1/images/gif2.gif)

## Ingesting Taxi Trip data  

1. In Synapse studio, on the left-side pane, select **Data** 
2. Right-click ADX database and click on **Open in Azure Data Explorer**. This opens the Azure Data Explorer web UI. 
3. Once in the web UI click on the **Data** tab on the left. This opens the ADX "One-Click UI", where you can quickly ingest data, create database tables, and automatically map the table schema.  
4. Click on **Ingest data**, and then enter the following details:
   | Setting | Value | Description |
   |:------|:------|:------
   | Cluster | adxpooltaxitrip | Enter name of Data Explorer pool created |
   | Database | taxitripdatabase | Enter name of database created |
   | New Table | taxitriptable | Enter the name for the table that will hold the taxi trip data | 
6. Select **Next**, and then enter the following information for **Source**:
   - Under *Source type* choose **File**.
   - Under *Upload Files -> Browse Files -> File Name* paste the following Github URL: https://github.com/Azure/Test-Drive-Azure-Synapse-with-a-1-click-POC/raw/main/tripDataAndFaresCSV/trip-data.csv
   - Ensure the **trip-data.csv** file was properly uploaded by seeing a green checkmark under the **status** column.
7.  Select **Next: Schema** and leave all the information as default. This page displays the schema and a partial data preview of the **taxitriptable** that will be created.
8.  Select **Next: Start Ingestion** and this will begin the ingestion process for the data. It is complete once all the files display a green checkmark. Click **Close** to complete.
9.  Validate ingestion by selecting **Query** on the left-side pane, clicking on the taxitripdatabase -> taxitriptable and you will see all of the data within the table. The same will be found in Synapse studio. 

![Ingesting Data](https://github.com/Azure/Test-Drive-Azure-Synapse-with-a-1-click-POC/raw/nataliarodri906-patch-1/images/gif3.gif)
   

