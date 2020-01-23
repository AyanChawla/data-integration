---
title: Creating computed entities in dataflows
description: Learn how to create computed entities in dataflows
author: bensack
manager: kfile
ms.reviewer: ''

ms.service: dataflows
ms.topic: conceptual
ms.date: 08/15/2019
ms.author: bensack

LocalizationGroup: Data from files
---
# Creating computed entities in dataflows

You can perform **in-storage computations** when using **dataflows** with a Power BI Premium subscription. This lets you perform calculations on your existing dataflows, and return results that enable you to focus on report creation and analytics. 

![Computed entities in Power BI Premium](media/dataflows-computed-entities/computed-entities-premium-00.png)

To perform **in-storage computations**, you first must create the dataflow and bring data into that Power BI dataflow storage. Once you have a dataflow that contains data, you can create **computed entities**, which are entities that perform in-storage computations. 

There are two ways you can connect dataflow data to Power BI:

* [Using self-service authoring of a dataflow](https://docs.microsoft.com/power-bi/service-dataflows-create-use)
* Using an external dataflow

The following sections describe how to create computed entities on your dataflow data.

## How to create computed entities 

Once you have a dataflow with a list of entities, you can perform calculations on those entities.

In the dataflow authoring tool in the Power BI service, select **Edit entities**, then right-click on the entity you want to use as the basis for your computed entity and on which you want to perform calculations. In the context menu, choose **Reference**.

For the entity to be eligible as a computed entity, the **Enable load** selection must be checked, as shown in the following image. Right-click on the entity to display this context menu.

![Check the Enable load in the right-click context menu](media/dataflows-computed-entities/computed-entities-premium-01.png)

By selecting **Enable load**, you create a new entity for which its source is the referenced entity. The icon changes, and displays the **computed** icon, as shown in the following image.

![Computed entities in Power BI Premium](media/dataflows-computed-entities/computed-entities-premium-00.png)

Any transformation you perform on this newly created entity will be run on the data that already resides in Power BI dataflow storage. That means that the query will not run against the external data source from which the data was imported (for example, the SQL database from which the data was pulled), but rather, is performed on the data that resides in the dataflow storage.

### Example use cases
What kind of transformations can be performed with computed entities? Any transformation that you usually specify using the transformation user interface in Power BI, or the M editor, are all supported when performing in-storage computation. 

Consider the following example: you have an *Account* entity that contains the raw data for all the customers from your Dynamics 365 subscription. You also have *ServiceCalls* raw data from the Service Center, with data from the support calls that were performed from the different account in each day of the year.

Imagine you want to enrich the *Account* entity with data from the *ServiceCalls*. 

First you would need to aggregate the data from the ServiceCalls to calculate the number of support calls that were done for each account in the last year. 

![Example of a computed entity in Power BI Premium](media/dataflows-computed-entities/computed-entities-premium-02.png)

Next, you would want to merge the *Account* entity with the *ServiceCallsAggregated* entity to calculate the enriched **Account** table.

![Example of a computed entity in Power BI Premium](media/dataflows-computed-entities/computed-entities-premium-03.png)

And then you can see the results, shown as *EnrichedAccount* in the following image.

![Results of a computed entity in Power BI Premium](media/dataflows-computed-entities/computed-entities-premium-04.png)

And that's it&mdash;the transformation is performed on the data in the dataflow that resides in your Power BI Premium subscription, not on the source data.

## Considerations and limitations

It's important to note that if you remove the workspace from Power BI Premium capacity, the associated dataflow will no longer refresh. 

When working with dataflows specifically created in an organization's Azure Data Lake Storage Gen2 account, linked entities and computed entities only work properly when the entities reside in the same storage account. For more information, see [Connect Azure Data Lake Storage Gen2 for dataflow storage)](https://docs.microsoft.com/power-bi/service-dataflows-connect-azure-data-lake-storage-gen2).

Linked entities are only available for dataflows created in Power BI and Power Apps portals. As a best practice, when doing computations on data joined by on-premises and cloud data, create a new entity to perform such computations. This provides a better experience than using an existing entity for computations, such as an entity that is also querying data from both sources and doing in-lake transformations.

## Next Steps

This article described computed entities and dataflows. Here are some more articles that might be useful.

* [Self-service data prep in Power BI](dataflows-integration-overview.md)
* [Using incremental refresh with dataflows](dataflows-incremental-refresh.md)
* [Connect to data sources for dataflows](dataflows-data-sources.md)
* [Link entities between dataflows](dataflows-linked-entities.md)

The following links provide additional information about dataflows in Power BI, and other resources: 
* [Create and use dataflows in Power BI](https://docs.microsoft.com/power-bi/service-dataflows-create-use)
* [Using dataflows with on-premises data sources](https://docs.microsoft.com/power-bi/service-dataflows-on-premises-gateways)
* [Developer resources for Power BI dataflows](https://docs.microsoft.com/power-bi/service-dataflows-developer-resources)
* [Configure workspace dataflow settings (Preview)](https://docs.microsoft.com/power-bi/service-dataflows-configure-workspace-storage-settings)
* [Add a CDM folder to Power BI as a dataflow (Preview)](https://docs.microsoft.com/power-bi/service-dataflows-add-cdm-folder)
* [Connect Azure Data Lake Storage Gen2 for dataflow storage (Preview)](https://docs.microsoft.com/power-bi/service-dataflows-connect-azure-data-lake-storage-gen2)

For more information about Power Query and scheduled refresh, you can read these articles:
* [Query overview in Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-query-overview)
* [Configuring scheduled refresh](https://docs.microsoft.com/power-bi/refresh-scheduled-refresh)

For more information about the Common Data Model, you can read its overview article:
* [Common Data Model - overview ](https://docs.microsoft.com/powerapps/common-data-model/overview)

