# Azure Synapse Proof-of-Concept

![Synapse Analytics](https://raw.githubusercontent.com/Azure/azure-synapse-consumption-play/main/images/synapse1.png)

## Table Of Contents

1. Introduction
2. Purpose
3. Prerequisites
4. Deployment
5. Post Deployment

## Introduction

This template deploys necessary resources to run an Azure Synapse Proof-of-Concept. 
Following resources are deployed with this template along with some RBAC role assignments:

- An Azure Synapse Workspace with batch data pipeline and other required resources
- An Azure Synapse SQL Pool
- An optional Apache Spark Pool
- Azure Data Lake Storage Gen2 account
- A new File System inside the Storage Account to be used by Azure Synapse
- A Logic App to Pause the SQL Pool at defined schedule
- A Logic App to Resume the SQL Pool at defined schedule
- A key vault to store the secrets

The data pipeline inside the Synapse Workspace gets Newyork Taxi trip and fare data, joins them and perform aggregations on them to give the final aggregated results. Other resources include datasets, linked services and dataflows. All resources are completely parameterized and all the secrets are stored in the key vault. These secrets are fetched inside the linked services using key vault linked service. The Logic App will check for Active Queries. If there are active queries, it will wait 5 minutes and check again until there are none before pausing

## Purpose

This template allows the Administrator to deploy a Proof-of-Concept environment of Azure Synapse Analytics with some pre-set parameters. This allows more time to focus on the Proof-of-Concept at hand and test the service.

## Prerequisites

Owner role (otherwise Contributor + User Access Administrator roles) for the Azure Subscription the template being deployed in. This is for creation of a separate Proof-of-Concept Resource Group and to delegate roles necessary for this proof of concept. Refer to this [official documentation](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-steps) for RBAC role-assignments.

## Deployment

- Fork out [this github repository](https://github.com/Azure/azure-synapse-consumption-play) into your github account.

![Fork](https://raw.githubusercontent.com/Azure/azure-synapse-consumption-play/main/images/4.gif)
 
- Click 'Deploy To Azure' button given below to deploy all the resources. 

  [![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-synapse-consumption-play%2Fmain%2Fazuredeploy.json)

- Provide the values for:

   - Resource group (create new)
   - Region
   - Company Tla
   - Option (true or false) for Allow All Connections
   - Option (true or false) for Spark Deployment
   - Spark Node Size (Small, Medium, large) if Spark Deployment is set to true
   - Sql Administrator Login
   - Sql Administrator Login Password
   - Sku
   - Option (true or false) for Metadata Sync
   - Frequency
   - Time Zone
   - Resume Time
   - Pause Time
   - Option (Enabled or Disabled) for Transparent Data Encryption
   - Github Username (username for the account where [this github repository](https://github.com/Azure/azure-synapse-consumption-play) was forked out into)

- Click 'Review + Create'.

- On successfull validation, click 'Create'.

## Post Deployment
- Current Azure user needs to have "Storage Blob Data Contributor" role access to recently created Azure Data Lake Storage Gen2 account to avoid 403 type permission errors.
- After the deployment is complete, click 'Go to resource group'.
- You'll see all the resources deployed in the resource group.
- Click on the newly deployed Synapse workspace.
- Click on link 'Open' inside the box labelled as 'Open Synapse Studio'.
- Click on 'Log into Github' after workspace is opened. Provide your credentials for the github account holding the forked out repository.
- After logging in into your github account, click on 'Integrate' icon in the left panel. A blade will appear from right side of the screen.
- Make sure that 'main' branch is selected as 'Working branch' and click 'Save'.

![PostDeployment-1](https://raw.githubusercontent.com/Azure/azure-synapse-consumption-play/main/images/1.gif)

- Now open the pipeline named 'TripFaresDataPipeline'.
- Click on 'Parameters' tab at bottom of the window.
- Update the parameters' values. You can copy the resources' names from the resource group recently deployed.
- Make sure the SQL login username is correct and the workspace name is fully qualified domain name, i.e. workspaceName.database.windows.net

![PostDeployment-2](https://raw.githubusercontent.com/Azure/azure-synapse-consumption-play/main/images/2.gif)

- After the parameters are updated, click on 'Commit all'.
- After successful commit, click 'Publish'. A blade will appear from right side of the window.
- Click 'Ok'.

![PostDeployment-3](https://raw.githubusercontent.com/Azure/azure-synapse-consumption-play/main/images/3.gif)
