## One-Click Deployment of Rapid Synapse POC Accelerator 
This is a guide for One-click deployment of a pre-built POC (Test Drive) for Synapse into customers’ subscription. 

[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-synapse-consumption-play%2Fmain%2Fazuredeploy.json) 

![Synapse Analytics](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-synapse-poc/images/synapse1.png)

## How should a CSA use this technical asset?
### Step 1

### Step 2

...

This template deploys necessary resources to run an Azure Synapse Proof-of-Concept

This template deploys the following:

- An Azure Synapse Workspace
  - (OPTIONAL) Allows All connections in by default (Firewall IP Addresses)
  - Allows Azure Services to access the workspace by default
  - Managed Virtual Network is Enabled
- An Azure Synapse SQL Pool
- (OPTIONAL) Apache Spark Pool
  - Auto-paused set to 15 minutes of idling
- Azure Data Lake Storage Gen2 account
  - Azure Synapse Workspace identity given Storage Blob Data Contributor to the Storage Account
    - A new File System inside the Storage Account to be used by Azure Synapse
- A Logic App to Pause the SQL Pool at defined schedule
  - The Logic App will check for Active Queries. If there are active queries, it will wait 5 minutes and check again until there are none before pausing
- A Logic App to Resume the SQL Pool at defined schedule
- Both Logic App managed identities are given Contributor rights to the Resource Group
- Grants the Workspace identity CONTROL to all SQL pools and SQL on-demand pool


## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft 
trademarks or logos is subject to and must follow 
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.
