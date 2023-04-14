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

## Ingest Taxi Trip data and analyze  

1. In Synapse studio, on the left-side pane, select **Develop.**  
2. Under **KQL scripts**, Select + (Add new resource) > **KQL script**. On the right-side pane, you can name your script.
3. In the **Connect to** menu, select *adxpooltaxitrip*.  
4. In the **Use database** menu, select *TaxiTripDatabase*  
5. Paste in the following command, and select **Run** to create a TaxiTrip table.
   

