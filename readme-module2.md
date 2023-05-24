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
   
## Analyze Taxi Trip Data using KQL

1. In Synpase studio, on the left-side pane, select **Develop**  
2. Under 'Notebooks' dropdown on the left side of the screen, click on the KQL notebook named 'Analyze Taxi Trip Data using KQL'  
3. Once in the notebook, ensure you are connected to your ADX pool **'adxpooltaxitrip'** and database **'taxitripdatabase'** and then run through each of the sections of the script (a-i) separately and observe the results:  
   a. Counts the number of records in the *'taxitriptable'* table. 
      ![count](
   b. Summarizes the minimum and maximum values of the 'pickup_datetime' columns in the'taxitriptable' table.  
   c. Summarizes the count of records in the 'taxitriptable' table by grouping them into daily intervals based on the 'pickup_datetime' column. 
   d. Filters the 'taxitriptable' table to select records with pickup datetime between January 1, 2013, 01:04:00 (UTC) and January 19th, 2013, 19:49:49 (UTC) (which are the min and max found in step b), then counts the number of rides in 15 minute intervals within that time range, and finally visualizes the results as a time chart.   
   e. Now we are trimming the tails of the dataset by filtering the 'taxitriptable' table to select records with a pickup datetime between January 13, 2013, 00:00:00 and January 13, 2013, 16:15:00. Similarly it then counts the number of rides in 15 minute intervals within that time range, and finally visualizes the results as a time chart.  
   f. Again, it filters the 'taxitriptable' table to select records with a pickup datetime between January 13, 2013, 00:00:00 and January 13, 2013, 16:15:00. It then counts the number of rides in 15 min intervals within that time range. After that, it uses the "series_decompose_anomalies" function to identify anomalies in the ride count data. Finally, it visualizes the anomalies as an anomaly chart titled "Anomalies on NYC taxi rides". Anomalies can be seen as red dots on the chart.  
   g. Creates a series of ride counts in 15-minute intervals from the "taxitriptable" table for the time range between January 13, 2013, 00:00:00 and January 13, 2013, 16:15:00. It then uses the "series_decompose_anomalies" function to identify anomalies in the ride count data and extends the table with an "anomalies" column. The "mv-expand" function is used to expand the table to separate rows for each ride count and its corresponding anomaly value and pickup datetime. The code then filters the table to only include rows where the anomaly value is not equal to zero. The table is sorted by pickup datetime and the first 10 rows are selected for further analysis. Ultimately,  listing the anomalies found. 
   h. Similarly, it creates a series of ride counts in 15-minute intervals from the "taxitriptable" table for the time range between January 13, 2013, 00:00:00 and January 13, 2013, 16:15:00. It then uses the "series_decompose_anomalies" function to decompose the ride count data into anomalies, score, and baseline values. The table is expanded to separate rows for each anomaly, pickup datetime, ride count, score, and baseline. Finally, the table is projected to include the anomalies, pickup datetime, ride count, score, and baseline columns, where the anomalies column is set to null if the anomaly value is 0.
   i. This code retrieves data from the "FaresData" table using a SQL request in Azure Synapse Data Explorer. It then projects specific columns from the retrieved data, including converting some columns to specific data types. Finally, it performs a left outer join with the "taxitriptable" table based on matching values in the "medallion", "hack_license", "vendor_id", and "pickup_datetime" columns. Overall, it joined the synapse SQL pool and data explorer pool to visualize later in PowerBI.  
   
## Steps for PowerBI integration and visualization


