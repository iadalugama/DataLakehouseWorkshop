# Module 03 - Logical Data Warehouse

[< Previous Module](../modules/module02c.md) - **[Home](../README.md)** - [Next Module >](../modules/module04.md)

## :stopwatch: Estimated Duration

10 minutes

## :thinking: Prerequisites

- [x] Lab environment deployed
- [x] Module 1D complete
- [x] Module 2C complete

## :loudspeaker: Introduction

In this module, we will setup a Logical Data Warehouse (LDW), a relational layer, on top of the data files residing in the data lake. This will enable a broad range of business intelligence tools to query data from the Delta Lake tables using the serverless SQL endpoint.

```mermaid
flowchart LR
ds-.->v1
ds-.->v2
ds[(Data Lake\ncurated)]
bi["SQL Tools\n(e.g. Power BI)"]
subgraph db["Database (ldw)"]
subgraph schema["Schema (wwi)"]
v1[View\nwwi.customers]
v2[View\nwwi.orders]
end
end
v1-.->bi
v2-.->bi
```

## :dart: Objectives

- Create a database.
- Create a schema.
- Create views on top of Delta Lake tables.

## Table of Contents

1. [Create a Database](#1-Create-a-Database)
2. [Create a Schema](#2-Create-a-Schema)
3. [Create Views](#3-Create-Views)
4. [Explore your data](#4-Explore-your-data)

## 1. Create a Database

A [serverless SQL database](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azure-sqldw-latest&tabs=sqlod) is a logical container where users can create utility objects such as data sources, file formats and schemas, simplifying access to files placed in Azure storage (e.g. CSV, Parquet, Delta) through external tables and views. Note: The master database in serverless SQL pool does not support the creation of:

- External tables
- External data sources
- Database scoped credentials
- External file formats

In this step, we will create a user database with the name `ldw` and collation `Latin1_General_100_BIN2_UTF8`.

1. Navigate to the **Develop** hub

    ![ALT](../images/module03/001.png)

2. Click the **[+]** icon to add a new resource and select **SQL script**

    ![ALT](../images/module03/002.png)

3. Copy and paste the code snippet below and click **Run**

    ```sql
    CREATE DATABASE ldw COLLATE Latin1_General_100_BIN2_UTF8;
    ```

    ![ALT](../images/module03/003.png)

4. To the right of the **Use database** drop-down menu, click the **Refresh** icon

    ![ALT](../images/module03/004.png)

5. Set the **Use database** property to `ldw`

    ![ALT](../images/module03/005.png)

<div align="right"><a href="#module-03---logical-data-warehouse">↥ back to top</a></div>

## 2. Create a Schema

A [schema](https://docs.microsoft.com/sql/t-sql/statements/create-schema-transact-sql?view=azure-sqldw-latest) can be used to logically group metadata such as tables and views within a user database. In this step, we will create a schema called `wwi`.

1. Copy and paste the code snippet below and click **Run**

```sql
CREATE SCHEMA wwi;
```

![ALT](../images/module03/009.png)

<div align="right"><a href="#module-03---logical-data-warehouse">↥ back to top</a></div>

## 3. Create Views

[Views](https://docs.microsoft.com/sql/t-sql/statements/create-view-transact-sql?view=azure-sqldw-latest) are virtual tables which encapsulate and enable reuse of serverless SQL pool queries. Once created, views can be consumed by SQL compatible tools such as Power BI. In this step, we will create views on top of our Delta Lake tables, `customers` and `orders`.

1. Copy and paste the code snippet below, replace `YOUR_DATA_LAKE_ACCOUNT` with the name of your Azure Data Lake Storage Gen2 account, and click **Run**

    ```sql
    CREATE VIEW wwi.customers
    AS SELECT *
    FROM
        OPENROWSET(
            BULK 'https://YOUR_DATA_LAKE_ACCOUNT.dfs.core.windows.net/03-curated/wwi/customers',
            FORMAT = 'DELTA'
        ) AS [result];
    ```

    ![ALT](../images/module03/010.png)

2. Copy and paste the code snippet below, replace `YOUR_DATA_LAKE_ACCOUNT` with the name of your Azure Data Lake Storage Gen2 account, and click **Run**

    ```sql
    CREATE VIEW wwi.orders
    AS SELECT *
    FROM
        OPENROWSET(
            BULK 'https://YOUR_DATA_LAKE_ACCOUNT.dfs.core.windows.net/03-curated/wwi/orders',
            FORMAT = 'DELTA'
        ) AS [result];
    ```

    ![ALT](../images/module03/011.png)

<div align="right"><a href="#module-03---logical-data-warehouse">↥ back to top</a></div>

## 4. Explore your data

The serverless SQL query service enables you to [read data](https://docs.microsoft.com/azure/synapse-analytics/sql/query-delta-lake-format) stored in the Delta Lake format using the `OPENROWSET` function. Since we have created virtual tables which wrap our serverless SQL queries utilizing the OPENROWSET function, we can simply refer to the views (e.g. `SELECT * FROM wwi.customers`).

1. Navigate to the **Data** hub

    ![ALT](../images/module03/012.png)

2. Click the web browser **Refresh** button to refresh the entire page

    ![ALT](../images/module03/013.png)

3. Under the **Workspace** tab, you should see a new **SQL database** called `ldw`

    ![ALT](../images/module03/014.png)

4. Expand the `ldw` database, navigate to **Views**

    ![ALT](../images/module03/015.png)

5. Right-click on `wwi.customers` and select **New SQL script > Select TOP 100 rows**

    ![ALT](../images/module03/016.png)

6. Click **Run**

    ![ALT](../images/module03/017.png)

7. Replace the existing SQL by copying and pasting the below code snippet, and click **Run**

```sql
SELECT
    orders.CustomerKey,
    customers.CustomerAddress,
    SUM(orders.Quantity) AS Quantity
FROM
    wwi.orders AS orders,
    wwi.customers AS customers
WHERE
    orders.CustomerKey = customers.CustomerSK
GROUP BY
    orders.CustomerKey,
    customers.CustomerAddress
ORDER BY
    orders.CustomerKey ASC
```

![ALT](../images/module03/018.png)

<div align="right"><a href="#module-03---logical-data-warehouse">↥ back to top</a></div>

## :tada: Summary

You have successfully created a relational layer on top of Delta Lake tables residing in your Azure Data Lake Storage Gen2 account.

## :white_check_mark: Results

Azure Synapse Analytics

- [x] 1 x Database (ldw)
- [x] 1 x Schema (wwi)
- [x] 2 x Views (wwi.customers, wwi.orders)

[Continue >](../modules/module04.md)
