---
published: true # Optional. Set to true to publish the workshop (default: false)
type: workshop # Required.
title: Building Fabric Real-Time Intelligence solution in a day # Required. Full title of the workshop
short_title: Fabric Real-Time Intelligence Tutorial # Optional. Short title displayed in the header
description: In this technical workshop, you will build a complete analytics platform with streaming data using Microsoft Fabric Real-Time Intelligence components and other features of Microsoft Fabric. This is a proctor led worksop in which each section is accompanied by a technical overview of Fabric RTI components. # Required.
level: intermediate # Required. Can be 'beginner', 'intermediate' or 'advanced'
authors: # Required. You can add as many authors as needed
  - Microsoft Fabric Real-Time Intelligence
contacts: # Required. Must match the number of authors
  - https://aka.ms/fabricblog
duration_minutes: 360 # Required. Estimated duration in minutes
audience: students, pro devs, analysts # Optional. Audience of the workshop (students, pro devs, etc.)
tags: fabric, kql, realtime, intelligence, event, stream, sql, data, analytics, kusto, medallion, dashboard, reflex, activator # Required. Tags for filtering and searching
---

# Introduction

Suppose you own an e-commerce website selling bike accessories. You have millions of visitors a month, you want to analyze the website traffic, consumer patterns and predict sales.

This workshop will walk you through the process of building an end-to-end [Real-Time Intelligence](https://blog.fabric.microsoft.com/en-us/blog/introducing-real-time-intelligence-in-microsoft-fabric) Solution in MS Fabric, using the medallion architecture, for your e-commerce website.

You will learn how to:

- Build a web traffic analytics solution using Fabric Real-Time Intelligence.
- Use Fabric shortcuts to query data without move or copy(with AdventureWorksLT sample data).
- Stream events into Fabric Eventhouse via Eventstream & leverage OneLake availability.
- Create real-time data transformations in Fabric Eventhouse through the power of Kusto Query Language (KQL) & Fabric Copilot.
- Create real-time visualizations using Real-Time Dashboards.
- Build Reflex actions and alerts on the streaming data.

See what real customers like [McLaren](https://www.linkedin.com/posts/shahdevang_if-you-missed-flavien-daussys-story-at-build-activity-7199013652681633792-3hdp), [Dener Motorsports](https://customers.microsoft.com/en-us/story/1751743814947802722-dener-motorsport-producose-ltd-azure-service-fabric-other-en-brazil), [Elcome](https://customers.microsoft.com/en-us/story/1770346240728000716-elcome-microsoft-copilot-consumer-goods-en-united-arab-emirates), [Seair Exim Solutions](https://customers.microsoft.com/en-us/story/1751967961979695913-seair-power-bi-professional-services-en-india) & [One NZ](https://customers.microsoft.com/en-us/story/1736247733970863057-onenz-powerbi-telecommunications-en-new-zealand) are saying.

All the **code** in this tutorial can be found here:  
[Build Fabric Real-Time Intelligence solution in a day](https://github.com/microsoft/FabConRTITutorial/)

## Duration

- Workshop 5-6 hours.
- Each section is accompanied with technical explanation of the Fabric Real-Time Intelligence component being used.
- Without the accompanied explanation, lab can be completed in 1-2 hours.
- **TO BE CHANGED** [pre-reqs](https://moaw.dev/workshop/?src=gh%3Amicrosoft%2FFabricRTIWorkshop%2Fmain%2Fdocs%2F&step=6) 30-45 minutes (section 7, recommend provisioning trial tenant prior if necessary).

## Original Creators

This workshop/tutorial was originally written by the following authors and is available at [Fabric-RTI-Workshop](https://aka.ms/fabricrtiworkshop)

- [Denise Schlesinger](https://github.com/denisa-ms), Microsoft, Principal CSA
- [Hiram Fleitas](https://aka.ms/hiram), Microsoft, Senior CSA
- Guy Yehudy, Microsoft, Principal PM

## Authors

- [Brian Bønk Rueløkke](https://www.linkedin.com/in/brianbonk/), Data Platform MVP
- [Devang Shah](https://www.linkedin.com/in/shahdevang/), Principal Program Manager, Microsoft
- [Frank Geisler](https://www.linkedin.com/in/frank-geisler/), Data Platform MVP
- [Johan Ludvig Brattås](https://www.linkedin.com/in/johanludvig/), Data Platform MVP
- [Matt Gordon](https://www.linkedin.com/in/sqlatspeed/), Data Platform MVP

## Contributing

- If you'd like to contribute to this lab, report a bug or issue, please feel free to submit a Pull-Request to the [GitHub repo](https://github.com/microsoft/FabConRTITutorial/) for us to review or [submit Issues](https://github.com/microsoft/FabConRTITutorial/issues) you encounter.

---

## Fabric Real-Time Intelligence

Let's cover the key-features of Real-Time Intelligence and how we plan to use them for our architecture.

### Eventstreams

- Eventstreams allow us to bring real-time events (including Kafka endpoints) into Fabric, transform them, and then route them to various destinations without writing any code (no-code).

- In this solution, Clicks and Impressions events are ingested from an Eventstream into the respective 'BronzeClicks' and 'BronzeImpressions' tables.

- Enhanced capabilities allows us to source data into Eventstreams from Azure Event Hubs, IoT Hubs, Azure SQL Database (CDC), PostgreSQL Database (CDC), MySQL Database (CDC), Azure Cosmos Database (CDC), Google Cloud Pub/Sub, Amazon Kinesis Data Streams, Confluent Cloud Kafka, Azure Blog Storage events, Fabric Workspace Item events, Sample data or Custom endpoint (Custom App).

- Feature [documentation](https://learn.microsoft.com/fabric/real-time-analytics/event-streams/overview).

### Shortcuts

- Shortcuts enable the creation of a live connections between OneLake and data sources, whether internal or external to Azure. This allows us to retrieve data from these locations as if they were seamlessly integrated into Microsoft Fabric.

- A shortcut is a schema entity that references data stored external to a KQL database in your cluster. In Lakehouse(s), Eventhouse(s), or KQL Databases it's possible to create shortcuts referencing internal locations within Microsoft Fabric, ADLS Gen2, Spark Notebooks, AWS S3 storage accounts, or Microsoft Dataverse.

- By enabling us to reference different storage locations, OneLake's Shortcuts provides a unified source of truth for all our data, within the Microsoft Fabric environment and ensures clarity regarding the origin of our data.

- In this solution, the `Product` and `ProductCategory` delta tables in OneLake are defined as external tables using shortcuts. Meaning the data is not copied but served from the OneLake itself. Shortcuts allow data to remain stored in outside of Fabric, yet presented via Fabric as a central location.

- Feature [documentation](https://learn.microsoft.com/fabric/real-time-analytics/onelake-shortcuts?tabs=onelake-shortcut).

### Eventhouse

- An Eventhouse can host multiple KQL Databases for easier management. It will store events data from the Eventstream, leverage shortcuts and automate transformations in real-time. Eventhouses are **specifically tailored** to time-based, streaming or batch events with structured, semi-structured, and unstructured data.

- An Eventhouse is the best place to store streaming data in Fabric. It provides a highly scalable analytics system with built-in Machine Learning capabilities for discrete analytics over highly-granular data. It's useful for any scenario that includes event-based data, for example, telemetry and log data, time series and IoT data, security and compliance logs, or financial records.

- Eventhouse's support Kusto Query Language (KQL) queries, T-SQL queries and Python. The data is automatically made available in delta-parquet format and can be easily accessed from Notebooks for more advanced transformations.

- Feature [documentation](https://learn.microsoft.com/fabric/real-time-intelligence/eventhouse).

### KQL Update policies

- This feature is also known as a mini-ETL. Update policies are automation mechanisms, triggered when new data is written to a table. They eliminate the need for external orchestration by automatically running a query to transform the ingested data and save the result to a destination table.

- Multiple update policies can be defined on a single table, allowing for different transformations and saving data to multiple tables simultaneously. **Target** tables can have a different schema, retention policy, and other policies than the **Source** table.

- In this solution, the data in derived silver layer tables (targets) of our medallion architecture is inserted upon ingestion into bronze tables (sources). Using Kusto's update policy feature, this appends transformed rows in real-time into the target table, as data is landing in a source table. This can also be set to run in as a transaction, meaning if the data from bronze fails to be transformed to silver, it will not be loaded to bronze either. By default, this is set to off allowing maximum throughput.

- Feature [documentation](https://learn.microsoft.com/azure/data-explorer/kusto/management/update-policy).

### KQL Materialized Views

- Materialized views expose an aggregation query over a source table, or over another materialized view. We will use materialized views to create the Gold Layer in our medallion architecture. Most common materialized views provide the current reading of a metric or statistics of metrics over time. They can also be backfilled with historical data; however, by default they are automatically populated by newly ingested data.

- Feature [documentation](https://learn.microsoft.com/azure/data-explorer/kusto/management/materialized-views/materialized-view-overview).

### One Logical Copy

- This feature creates a one logical copy of KQL Database data by turning on OneLake availability. Turning on OneLake availability for your KQL tables, database or Eventhouse means that you can query the data in your KQL database in Delta Lake format via other Fabric engines such as Direct Lake mode in Power BI, Warehouse, Lakehouse, Notebooks, and more. When activated, it will copy via mirroring the KQL data to your Fabric Datalake in delta-parquet format. Allowing you to shortcut tables from your KQL Database via OneLake to your Fabric Lakehouse, Data Warehouse, and also query the data in delta-parquet format using Spark Notebooks or the SQL-endpoint of the Lakehouse.

- Feature [documentation](https://learn.microsoft.com/fabric/real-time-analytics/one-logical-copy).

### KQL Dynamic fields

- Dynamic fields are a powerful feature of KQL database's that support evolving schema changes and object polymorphism, allowing the storage/querying of different event types that have a common denominator of base fields.

- Feature [documentation](https://learn.microsoft.com/azure/data-explorer/kusto/query/scalar-data-types/dynamic).

### Kusto Query Language (KQL)

- KQL is also known as the language of the cloud. It's available in many other services such as Microsoft Sentinel, Azure Monitor, Azure Resource Graph and Microsoft Defender. The code-name **Kusto** engine was invented by 4 engineers from the Power BI team over 10 years ago and has been implemented across all Microsoft services including Github Copilot, LinkedIn, Azure, Office 365, and XBOX.

- KQL queries are easy to write, read and edit. The language is most commonly used to analyze logs, sign-on events, application traces, diagnostics, signals, metrics and much more. Supports multi-statement queries, relational operators such as filters (where clauses), union, joins aggregations to produce a tabular output. It allows the ability to simply pipe (|) additional commands for ad-hoc analytics without needing to re-write entire queries. It has similarities to PowerShell, Excel functions, LINQ, function SQL, and OS Shell (Bash). It supports DML statements, DDL statements (referred to as Control Commands), built-in machine learning operators for forecasting & anomaly detection, plus more... including in-line Python & R-Lang.

- In this solution, KQL commands will be automatically created and executed by eventstream to ingest data when configuring the Eventhouse KQL Database destination in the Eventstream. These commands will create the respective 'bronze' tables. Secondly, the control commands will be issued in a database script that automate creation of additional schema items such as Tables, Shortcuts, Functions, Policies and Materialized-Views.
- Feature [documentation](https://learn.microsoft.com/azure/data-explorer/kusto/query/).

### Real-Time Dashboards

![Real-Time Dashboards](assets/RTAMenu.png)

- While similar to Power BI's dashboard functionality, Real-time Dashboards have a different use case. Real-time Dashboards are commonly used for operational decision making, rather than the business intelligence use cases Power BI targets. Power BI supports more advanced visualizations and provides a rich data-story capabilities. Real-time Dashboards refresh very fast and allow with ease to toggle between visuals, and analysts to pro-developer can explore/edit queries without needing to download a desktop tool. This makes the experience simpler for analysts to understand and visualize large volumes of highly-granular data.

- In this solution, the Real-time dashboard will contain a collection of visual tiles _Click Through Rate_ stat KPIs, _Impressions_ area chart, _Clicks_ area chart, _Impressions by Location_ map for geo-spatial analytics and _Average Page Load Time_ in a line chart. This feature supports filter parameters, additional pages, markdown tiles, including Plotly, multiple KQL datasources, base queries, embedding.

- Real-time Dashboard's also support sharing while retaining permission controls, setting of alerts via Data Activator, and automatic refresh with a minimum frequency of 30 seconds.

- Feature [documentation](https://learn.microsoft.com/fabric/real-time-intelligence/dashboard-real-time-create).

### Data Activator

- Data Activator (code-name Reflex) is a no-code experience in Microsoft Fabric for automatically taking actions when patterns or conditions are detected in changing data. It monitors data in Power BI reports, Eventstreams items and Real-time Dashboards, for when the data hits certain thresholds or matches other patterns. It then triggers the appropriate action, such as alerting users or kicking off Power Automate workflows.

- Some common use cases are:

  - Run Ads when same-store sales decline.
  - Alert store managers to move food from failing freezers before it spoils.
  - Retain customers who had a bad experience by tracking their journey through apps, websites etc.
  - Help logistics companies find lost shipments proactively by starting an investigation when package status isn't updated for a certain length of time.
  - Alert account teams when customers fall behind with conditional thresholds.
  - Track data pipeline quality, to either re-run jobs, alert for detected failures or anomalies.

- In this solution, we will set an alert in our Real-time Dashboard to **Message me in Teams** functionality.

- Feature [documentation](https://learn.microsoft.com/fabric/data-activator/data-activator-introduction).

### Copilot

- Copilot for Real-Time Intelligence is an advanced AI tool designed to help you explore your data and extract valuable insights. You can input questions about your data, which are then automatically translated into Kusto Query Language (KQL) queries. Copilot streamlines the process of analyzing data for both experienced KQL users and citizen data scientists.

- Feature [documentation](https://learn.microsoft.com/fabric/get-started/copilot-real-time-intelligence).

  ![Copilot](assets/Copilot.png "Fabric Copilot in KQL Queryset")

---

## The e-commerce store

The e-commerce store database entities are:

- **Product:** the product catalogue.
- **ProductCategory:** the product categories.
- **events:** a click or impression event.

  - An **impression event** is logged when a product appears in the search results.

    ![Impressions](assets/store1.png)

  - A **click event** is logged when the product is clicked and the customer has viewed the details.

    ![Clicks](assets/store2.png)

    Photo by [Himiway Bikes](https://unsplash.com/@himiwaybikes?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/black-and-gray-motorcycle-parked-beside-brown-wall-Gj5PXw1kM6U?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash).  
    Photo by [HEAD Accessories](https://unsplash.com/@headaccessories?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/silver-and-orange-head-lamp-9uISZprJdXU?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash).  
    Photo by [Jan Kopřiva](https://unsplash.com/@jxk?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-close-up-of-a-helmet-with-sunglasses-on-it-CT6AScSsQQM?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash).

---

## Components of Fabric's Real-Time Intelligence

![RTIComponents](assets/RTIComponents.png "Components of Fabric's Real-Time Intelligence")

Real-Time Intelligence allows organizations to ingest, process, analyze and, questions over your data using natural language, transform and automatically act on data. All with a central hub (Real-Time Hub) to easily access and visualize all internal and external, first- and third-party streaming data.

Using Real-Time Intelligence enables faster, more accurate decision-making and accelerated time to insight.

## Lab Architecture

![Architectural Diagram](assets/architecture.png "Architecture Diagram")

Now with Data Activator (Reflex), we can also set alerts on Real-time Dashboards to send a message in Teams with conditional thresholds or even more advanced actions.

---

## Data schema

### Data flow

![MRD](assets/mrd.png)

### Tables

| Table                 | Origin           | Description                                                                                                                                                                                                                                 |
| --------------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **BronzeClicks**      | Eventhouse table | Streaming events representing the product being seen or clicked by the customer. Will be streamed into Fabric Eventhouse from an eventstream. We'll use a Fabric Notebook to simulate and push synthetic data (fake data) into an endpoint. |
| **BronzeImpressions** | Eventhouse table | Streaming events representing the product being seen or clicked by the customer. Will be streamed into Fabric Eventhouse from an eventstream. We'll use a Fabric Notebook to simulate and push synthetic data (fake data) into an endpoint. |
| **SilverClicks**      | EventHouse table | Table created based on an update policy with **transformed data**.                                                                                                                                                                          |
| **SilverImpressions** | EventHouse table | Table created based on an update policy with **transformed data**.                                                                                                                                                                          |

### External Tables

| Table               | Origin                              | Description                                  |
| ------------------- | ----------------------------------- | -------------------------------------------- |
| **Product**         | **Shortcut** to OneLake delta table | Products, including descriptions and prices. |
| **ProductCategory** | **Shortcut** to OneLake delta table | Product category.                            |

### Functions

| Function                  | Description                                                                 |
| ------------------------- | --------------------------------------------------------------------------- |
| **expandClickpath**       | Expands JSON array of dictonaries to transform into strongly typed columns. |
| **expandRelatedProducts** | Expands JSON array of dictonaries to transform into strongly typed columns  |

### Materialized-Views

| View            | Origin                  | Description                                                                                                                   |
| --------------- | ----------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| **ToBeDefined** | Eventhouse silver table | Materialized view showing only the **latest** changes in the source table showing how to handle duplicate or updated records. |
| **ToBeDefined** | Eventhouse silver table | Materialized view showing only the **latest** changes in the source table showing how to handle duplicate or updated records. |

---

## Pre-requisites

- Recommended material to review (at least one) prior to this lab, however it's not required:
  - [Write your first query with Kusto](https://aka.ms/learn.kql)
  - [Implement a Real-Time Intelligence Solution Tutorial](https://learn.microsoft.com/fabric/real-time-intelligence/tutorial-introduction)

| :heavy_exclamation_mark: **Important**                                                                                                                                                |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **To complete the lab you **must** have access to a [Microsoft Fabric](https://www.microsoft.com/microsoft-fabric/getting-started) workspace with at least Contributor permissions.** |

### Fabric tenant and capacity for the tutorial

For the purpose of this tutorial, speakers/proctors will provide a tenant with capacity for you to build your solution.

---

## Building the platform

### 1. Login to Lab Environment

| :heavy_exclamation_mark: **Important**                                                                                  |
| :---------------------------------------------------------------------------------------------------------------------- |
| **Do **not** use an InPrivate browser window. Recommend using a Personal browser window to successfully run this lab.** |

1. Open [app.fabric.microsoft.com](https://app.fabric.microsoft.com/) in your browser.

   ![FabricURL](assets/image_task01_step01.png "Fabric URL")

2. Login with provided credentials, if a trial fabric tenant was previously setup (reference Pre-reqs). You may also choose to run the lab in your own Fabric Tenant if you already have one.

3. Click **Real-Time Intelligence**.

   ![Fabric Home](assets/image_task01_step03.png "Real-Time Intelligence")

### 2. Fabric Workspace

1. Click **Workspaces** on the left menu and open the Fabric Workspace **designated** to your login by the Fabric Trial Tenant.

| :notebook: **Note**                                                                  |
| :----------------------------------------------------------------------------------- |
| **(Optional) If using your own Fabric Tenant, create a new workspace for this lab.** |

1. To create a new Workspace click on **Workspaces** in the left pane and then click on **+ New Workspace** in the popup window.

   ![alt text](assets/image_task02_step01.png)

2. Enter _RTI Tutorial_ as name for the new Workspace. Then extend **Advanced**

   ![alt text](assets/image_task02_step02.png)

   | :notebook: **Note**                                                                                                                                                                         |
   | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
   | **If the name that you would like to use for your workspace is still available this will be shown below the input box for **Name**. Workspace Names have to be unique in a Fabric tenant.** |

3. Check if the option **Trial** is checked. If so click on **Apply**.

   ![alt text](assets/image_task02_step03.png)

   After you clicked on **Apply** the workspace will be created. This can take up to a minute.

### 3. Create a new Eventhouse

1. Create an Eventhouse called "WebEvents_EH".

<div class="info" data-title="Note">
  
> The [Eventhouse](<https://learn.microsoft.com/en-us/fabric/real-time-intelligence/eventhouse>) is designed to handle real-time data streams efficiently, which lets organizations ingest, process, and analyze data in near real-time. Eventhouses are particularly useful for scenarios where **timely insights are crucial**. Eventhouses are **specifically tailored** to time-based, streaming events with multiple data formats. This data is automatically indexed and partitioned based on ingestion time.
</div>
2. Enable OneLake Availability

This feature is also called "one logical copy" and it automatically allows KQL Database tables to be accessed from a Lakehouse, Notebooks, etc in delta-parquet format via OneLake.

- When activated it will constantly copy the KQL data to your Fabric OneLake in delta format. It allows you to query KQL Database tables as delta tables using Spark or SQL endpoint on the Lakehouse. We recommend enabling this feature "before" we load the more data into our KQL Database. Also, consider this feature can be enabled/disabled per table if necessary. You can read more about this feature here: [Announcing Delta Lake support in Real-Time Intelligence KQL Database](https://support.fabric.microsoft.com/blog/announcing-delta-support-in-real-time-analytics-kql-db?ft=All).

![alt text](assets/fabrta70.png)

### Here's how to set this up

1. Open your Eventhouse.
2. Select your KQL Database.
3. Click on the pencil icon next to OneLake availability in the Database details pane. Click the toggle to activate it and click Done.
   ![alt text](assets/fabrta61.png)
   ![alt text](assets/fabrta62.png)

<div class="info" data-title="Note">
  
>   Newly created tables will automatically inherit the "OneLake availability" setting from the Database level. 
</div>

## 4. Create a new Eventstream

In this section we will be streaming events (impressions and clicks events) generated by a notebook. The events will be streamed into an eventstream and consumed by our Eventhouse KQL Database.

![alt text](assets/fabrta73.png)

1. Create an Eventstream called "WebEventsStream_ES". Check the box "Enhanced Capabilities".

2. Click on "Use Custom Endpoint". This will create an event hub connected to the Eventstream. Provide the source name as "WebEventsCustomSource" or as you prefer.
3. Click on "Add".
   ![alt text](assets/fabrta74.png)
4. Click on "Publish".
5. Click on the Eventstream source - Custom Endpoint to get the "Event hub name" and "Connection string-primary key". We need these values to send the events from our Notebook.
6. Click on "Keys".
7. Click the copy icon next to the **Event hub name** to copy it to a notepad.
   ![alt text](assets/fabrta8.png)
8. Click the view icon at the end of the **Connection string** (primary or secondary) to see it.
9. Then, click the copy icon at the end of **Connection string** to copy it to a notepad. It must be visible in order to copy it.

<div class="info" data-title="Note">
  
>   Eventstreams Custom-Endpoint/Custom-App sources also provide **Kafka** endpoints where data can be pushed to.
</div>

## 5. Import Data Generator Notebook

1. Import the notebook file [Generate_synthetic_web_events.ipynb](https://github.com/microsoft/FabConRTITutorial/blob/461275d54917670ee996bf85c780290061a35ff2/notebook/Generate_synthetic_web_events.ipynb) to generate events using streaming.
2. From GitHub, click the "Download raw file" icon on the top right.
3. Then proceed to import the notebook file to your Fabric workspace.
   ![alt text](assets/fabrta8.1.png)
   ![alt text](assets/fabrta8.2.png)

## 6. Run the notebook

1. DO NOT use an InPrivate browser window. Recommend using a Personal browser window for the Notebook session to connect & run successfully.
2. Open the "Generate_synthetic_web_events" notebook in your Fabric Workspace.
3. Paste your `Event hub name` value and `Event hub Connection String Primary Key` value using the values your copied from the previous step - Eventstream keys.
   ![alt text](assets/fabrta9.png)
4. Click **Run all** at the top left to start generating streaming events.
5. Wait a few minutes for the first code cell to finish and it will proceed to next code cells automatically.
6. Scroll down to the last code cell and it should begin to print the generated synthetic events in JSON format.
   ![Notebook Success](assets/NotebookSuccess.png)

## 7. Define Eventstream topology

1. Open the Eventstream in your Fabric Workspace.
2. Click on "Edit".
   ![alt text](assets/fabrta75.png)
3. Select "Transform events or add Destination" - Filter.
4. Click on the pencil icon the Filter node.
5. Provide "ClickEventsFilter" as the Operation name.
6. Choose "eventType" in the drop down for Select a field to filter on.
7. Choose "equals" as the condition
8. Type "CLICK" in the taxt box. Note: "CLICK" is in ALL CAPS.
9. Click on "Save".
10. Click on "+" sign next to the ClickEventsFilter node and choose "Stream".
11. Enter the name as "ClickEventsStream".
12. Click on "+" sign next to the ClickEventsStream node and choose "Eventhouse".
13. Provide "ClickEventStore" as the Destination name.
14. Select your workspace, Eventhouse that we created called "WebEvents_EH" and KQL Database of the same name.
15. Create a new table in our KQL Database called `BronzeClicks`. Click "Save".

16. Click on "+" sign next to the WebEventsStream_ES node and choose "Filter".
17. Delete the connection between this new filter node and ClickEventsFilter node.
18. Connect the output of WebEventsStream_ES node to the input of ClickEventsFilter node.
19. Click on the pencil icon of the new Filter node.
20. Provide "ImpressionEventsFilter" as the Operation name.
21. Choose "eventType" in the drop down for Select a field to filter on.
22. Choose "equals" as the condition
23. Type "IMPRESSION" in the taxt box. Note: "IMPRESSION" is in ALL CAPS.
24. Click on "Save".
25. Click on "+" sign next to the ImpressionEventsFilter node and choose "Stream".
26. Enter the name as "ImpressionEventsStream".
27. Click on "+" sign next to the ImpressionEventsStream node and choose "Eventhouse".
28. Provide "ImpressionEventStore" as the Destination name.
29. Select your workspace, Eventhouse that we created called "WebEvents_EH" and KQL Database of the same name.
30. Create a new table in our KQL Database called `BronzeImpressions`. Click "Save".
31. Click on "Publish".
32. After a few minutes, you should see the ClickEventStore and ImpressionEventStore node changing to mode "Streaming".

![alt text](assets/fabrta77.png)

In the end your Eventstream toplogy should appear as shown in the image below.

![alt text](assets/fabrta76.png)

## 8. Setting up the Lakehouse

1. Go to "ref_data" folder in the Github repo and download the products.csv and productcategory.csv files on your computer.

2. Create new Lakehouse called "WebSalesData_LH" in your workspace.

### Uploading reference data files and creating delta tables

3. Click "Get data" and choose "Upload Files"
4. Upload the 2 files that you downloaded in the previous steps.
5. After the files have been uploaded, browse the "Files" folder.
6. Select the "..." context menu for each file and choose "Load to tables".
7. Retain all the default values and click "Load".

Ensure that both the files products.csv and productcategory.csv are available as delta tables in your lakehouse. Eventually, your lakehouse should appear as follows.

![alt text](assets/fabrta78.png)

### Accessing Eventhouse data from the lakehouse

8. If your Lakehouse is using the Schemas then expand Tables, right-click dbo schema & select "New table shortcut". If Schemas do not appear under Tables, then click on "Get data" drop down, choose **New shortcut**.
9. ![alt text](assets/fabrta65.png)
10. Select Microsoft OneLake.
    ![alt text](assets/fabrta66.png)
11. Select the "BronzeClicks" and "BronzeImpressions" tables in our Eventhouse KQL Database and click "Next".

<div class="info" data-title="Note">
  
> You may return to this step to create additional shortcuts, after running the [createAll.kql](<https://github.com/microsoft/FabricRTIWorkshop/blob/main/kql/createAll.kql>) database script which will create the additional tables. For now, you may proceed by selecting just the "BronzeClicks" and "BronzeImpressions" tables.
</div>

![alt text](assets/LakeShortcut1.png)

12. Click "Create".
    ![alt text](assets/LakeShortcut2.png)

13. Now you will have the Eventhouse KQL Database tables available in your Lakehouse. This also works across workspaces. You can query them like any other Lakehouse table.
    ![alt text](assets/fabrta69.png)

## 9. Build the KQL DB schema

In this section we will create all the silver tables, functions, materialized-views, and enable update policies and in our Eventhouse KQL Database. Two of the tables (product and productCategory) are shortcuts to the lakehouse and the data is NOT being copied into our KQL Database.
![alt text](assets/fabrta71.png)

1. Open the WebEvents_EH KQL Database in the Eventhouse of your Fabric Workspace.
2. Click on "New" and choose "OneLake shortcut".
3. Select "Microsoft OneLake".
4. Select "WebSalesData_LH" and click on "Next".
5. Expand Tables, select productcategory table and click on "Create". This will create a shortcut to the table productcategory in your Lakehouse without copying the data from the Lakehouse to Eventhouse.
6. Repeat the above steps to create a similar shortcut to the products table.
7. Expand the Shortcuts branch in the WebEvents_EH tree to verify if the 2 shortcuts have been correctly created.
8. Click on "Explore your Data".  
   ![alt text](assets/fabrta25.png)
9. Open the [createAll.kql](https://github.com/microsoft/FabConRTITutorial/blob/2452f81b0bf2561ff382988de96f47482d903523/kql/createAll.kql) file in GitHub and click copy icon at the top right to copy the entire file content.
10. Replace all on the "Explore your data" by deleting lines 1-19 and paste the contents of the createAll.kql file.
11. Click Run
    ![alt text](assets/fabrta27.png)
12. Click Save as KQL queryset, name it "createAll".
13. You can add additional tabs in the KQL Queryset to add new queries.
14. Your tables, functions, and materialized views should appear on the database pane on the left.

![alt text](assets/fabrta28.png) 15. (Optional) While on the KQL Database details screen you may explore additional **Real-Time Intelligence Samples** by clicking the **drop-drop next to Get data** and selecting a desired sample. These samples give you the ability to learn more.
![EventhouseSamples](assets/EventhouseSamples.png "Real-Time Intelligence Samples")

# 10. Real-Time Dashboard

In this section, we will build a real-time dashboard to visualize the streaming data and set it to refresh every 30 seconds. (Optionally) A pre-built version of the dashboard is available to download [here](<https://github.com/microsoft/FabricRTIWorkshop/blob/main/dashboards/RTA%20dashboard/dashboard-RTA Dashboard.json>), which can be imported and configured to your KQL Database data source.

- The Proctor Guide covers this process.
  ![Real-Time Dashboard](assets/RealTimeDashboard.png "Real-Time Dashboard")

1. Click + Create (button is located at top left Menu underneath Home).
2. Current workspace should be the same one.
3. Scroll down and choose **Real-Time Dashboard**.
4. Name it "Web Events Dashboard".
5. Click **+ Add tile**.
6. Click **+ Data source**.
7. Set the **Database** to "WebEvents_EH" & click Create.
8. Proceed to paste each query below, add a visual, and apply changes. (Optionally) All queries are available in this script file [dashboard-RTA.kql](https://github.com/microsoft/FabricRTIWorkshop/blob/main/dashboards/RTA%20dashboard/dashboard-RTA.kql).

### Clicks by hour

```
//Clicks by hour
SilverClicks
| where eventDate between (_startTime.._endTime)
| summarize date_count = count() by bin(eventDate, 1h)
| render timechart
| top 30 by date_count
```

1. Set Time rage parameter at the top left to **Last 7 days**. This parameter is referenced by the query in the `where` clause by using fields `_startTime` and `_endTime`.
2. Click **Run**.
3. Click **+ Add visual**.
4. Format the visual.
5. Set Tile name to "Click by hour".
6. Set Visual type to **Area chart**.
7. Click **Apply Changes**.

![ClicksByHour](assets/ClicksByHour.png "Clicks by hour")

9. While editing the dashboard, click **Manage** on the top left, and click **Parameters**.
10. Edit the "Time range" parameter by setting the Default value to **Last 7 Days**, click Close & Done.

![TimeRangeParameter](assets/TimeRangeParameter.png "Parameter Default Value")

11. Click **+ Add tile** again to proceed with the next visuals.

### Impressions by hour

12. Visual type: **Area chart**.

```
//Impressions by hour
SilverImpressions
| where eventDate between (_startTime.._endTime)
| summarize date_count = count() by bin(eventDate, 1h)
| render timechart
| top 30 by date_count
```

![alt text](assets/fabrta53.png)

### Impressions by location

12. Visual type: **Map**.

```
//Impressions by location
SilverImpressions
| where eventDate  between (_startTime.._endTime)
| join external_table('products') on $left.productId == $right.ProductID
| project lon = toreal(geo_info_from_ip_address(ip_address).longitude), lat = toreal(geo_info_from_ip_address(ip_address).latitude), Name
| render scatterchart with (kind = map) //, xcolumn=lon, ycolumns=lat)
```

![alt text](assets/fabrta54.png)

### Average Page Load time

13. Visual type: **Timechart**.

```
//Average Page Load time
SilverImpressions
| where eventDate   between (_startTime.._endTime)
//| summarize average_loadtime = avg(page_loading_seconds) by bin(eventDate, 1h)
| make-series average_loadtime = avg(page_loading_seconds) on eventDate from _startTime to _endTime+4h step 1h
| extend forecast = series_decompose_forecast(average_loadtime, 4)
| render timechart
```

![alt text](assets/AvgPageLoadTime.png)

### Impressions, Clicks & CTR

14. Add a tile & paste the query below once. Note, this is a multi-statement query that uses multiple let statements & a query combined by semicolons.
15. Set Tile name: **Impressions**.
16. Visual type: **Stat**.
17. Data Value column to `impressions`.
18. Click **Apply changes**.
19. Click the 3-dots (...) at the top right of the tile you just created to **Duplicate** it two more times.
20. Name the 2nd one **Clicks**, set the Data value column to `clicks`, then Apply changes.
21. Name the 3rd **Click Through Rate**, set the Data value column to `CTR`, then Apply changes.
22. (Optional) On the "Visual formatting" pane, scroll down and adjust the "Conditional formatting" as desired by clicking "+ Add rule".

```
//Clicks, Impressions, CTR
let imp =  SilverImpressions
| where eventDate  between (_startTime.._endTime)
| extend dateOnly = substring(todatetime(eventDate).tostring(), 0, 10)
| summarize imp_count = count() by dateOnly;
let clck = SilverClicks
| where eventDate  between (_startTime.._endTime)
| extend dateOnly = substring(todatetime(eventDate).tostring(), 0, 10)
| summarize clck_count = count() by dateOnly;
imp
| join clck on $left.dateOnly == $right.dateOnly
| project selected_date = dateOnly , impressions = imp_count , clicks = clck_count, CTR = clck_count * 100 / imp_count
```

![alt text](assets/fabrta56.png)
![alt text](assets/fabrta57.png)
![alt text](assets/fabrta58.png)

### Average Page Load Time Anomalies

```
//Avg Page Load Time Anomalies
SilverImpressions
| where eventDate   between (_startTime.._endTime)
| make-series average_loadtime = avg(page_loading_seconds) on eventDate from _startTime to _endTime+4h step 1h
| extend anomalies = series_decompose_anomalies(average_loadtime)
| render anomalychart
```

### Strong Anomalies

```
//Strong Anomalies
SilverImpressions
| where eventDate between (_startTime.._endTime)
| make-series average_loadtime = avg(page_loading_seconds) on eventDate from _startTime to _endTime+4h step 1h
| extend anomalies = series_decompose_anomalies(average_loadtime,2.5)
| mv-expand eventDate, average_loadtime, anomalies
| where anomalies <> 0
| project-away anomalies
```

### Logo (Markdown Text Tile)

```
//Logo (Markdown Text Tile)
![AdventureWorks](https://vikasrajput.github.io/resources/PBIRptDev/AdventureWorksLogo.jpg "AdventureWorks")
```

> The title can be resized on the dashboard canvas directly, rather than writing code.

### Auto-refresh

22. While editing the dashboard, click **Manage** > **Auto refresh**.
23. Set it to **Enabled**, and **Default** refresh rate to **30 seconds**, click Apply.
24. Click **Home** and then **Save**.

## 11. Reflex

1. While editing the dashboard, click **Manage** > Set Alert.
2. Choose "Clicks by hour".
3. Select Condition "Becomes greater than".
4. Set Value to 250.
5. Action choose **Message me in Teams**.
6. Click Create.

<div class="info" data-title="Note">
  
> The Reflex item will appear in your workspace and you can edit the Reflex trigger action. The same Reflex item can also trigger multiple actions. 
</div>

## 12. Stop the notebook

At this point you've completed the lab, so you may stop running the notebook.

1. Open the notebook "Generate synthetic events" from your workspace and click **Stop** on the last code cell if its still running.
2. (Optionally) You can click **Cancel All** on the top menu or click the stop red-square button to Stop session. These only appear when your session is active or the notebook is running.
   ![alt text](assets/fabrta60.png)
3. (Optionally) "LabAdmin" can click **Monitor** on the left Menu, search for "generate", click the 3-dots (...) next to the notebook "In progress" status and click **Cancel**.
   ![StopAllNotebooks](assets/StopAllNotebooks.png "Monitor - click 3-dots per item to Cancel")

## THAT's ALL FOLKS!!

---

# Continue your learning

- [Implement a Real-Time Intelligence Solution with Microsoft Fabric](https://aka.ms/realtimeskill/)
- [Real-Time Intelligence documentation in Microsoft Fabric](https://aka.ms/fabric-docs-rta)
- [Microsoft Fabric Updates Blog](https://aka.ms/fabricblog)
- [Get started with Microsoft Fabric](https://aka.ms/fabric-learn)
- [The mechanics of Real-Time Intelligence in Microsoft Fabric](https://youtube.com/watch?v=4h6Wnc294gA)
- [Real-Time Intelligence in Microsoft Fabric](https://youtube.com/watch?v=ugb_w3BTZGE)

### Thank you!

![logo](https://img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/RE1Mu3b?ver=5c31)
