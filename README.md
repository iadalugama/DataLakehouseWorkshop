# Data Lakehouse Workshop

The data lakehouse is a data architecture pattern that combines functional aspects of the data warehouse, with the data lake, on one platform. This workshop will walk through an example of how the data lakehouse pattern can be facilitated in Azure with Azure Synapse Analytics.

![Data Lakehouse with Azure Synapse Analytics](./images/readme/001.png)

## :page_with_curl: Agenda

![Agenda](./images/readme/agenda.png)

## :thinking: Prerequisites

* An [Azure account](https://azure.microsoft.com/free/) with an active subscription.
  * If you are working through this content as part of a proctored workshop, your proctor may be able to provide you with an Azure Pass. Instructions on how to activate an Azure Pass can be [found here](./azurepass.md).
  * Alternatively, if you don't have access to an Azure subscription, you may be able to sign up for a [free account](https://www.azure.com/free).
* You must have the necessary privileges within your Azure subscription to create resources, perform role assignments, register resource providers (if required), etc.

## :test_tube: Lab Environment Setup

* [Azure Pass Activation](./azurepass.md)
* [Resource Provider Registration](./providers.md)
* [Lab Environment](./modules/module00.md)

## :books: Learning Modules

1. Customers
    * [1A. Incremental Copy to Raw (using Change Data Capture)](./modules/module01a.md)
    * [1B. Dimension Table - Initial Load](./modules/module01b.md)
    * [1C. Dimension Table - Incremental Load (SCD Type 2)](./modules/module01c.md)
    * [1D. Automation using Triggers](./modules/module01d.md)
2. Orders
    * [2A. Incremental Copy to Raw (using High Watermark)](./modules/module02a.md)
    * [2B. Fact Table - Incremental Load](./modules/module02b.md)
    * [2C. Automation using Triggers](./modules/module02c.md)
3. [Logical Data Warehouse](./modules/module03.md)
4. [Data Visualization](./modules/module04.md)

<div align="right"><a href="#data-lakehouse-workshop">↥ back to top</a></div>

## :link: Workshop URL

[aka.ms/lakehouselab](https://aka.ms/lakehouselab)
