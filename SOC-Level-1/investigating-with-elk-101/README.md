# Investigating with ELK 101

- **Platform:** TryHackMe
- **Difficulty:** Easy
- **Link:** [Investigating with ELK 101](https://tryhackme.com/room/investigatingwithelk101)

## Overview

This room covers how the Elastic Stack (ELK) is used for log analysis and investigations. Although ELK is not a traditional SIEM, many SOC teams use it like one because of its data searching and visualizing capability. The room explores the components of ELK, how to search and filter data in Kibana using KQL, and how to create visualizations and dashboards while investigating VPN logs.

## Task 1 - Introduction

This room covers how the Elastic Stack (ELK) is used for log analysis and investigations. It explores the components of ELK, how log analysis is performed through it, and how to create visualizations and dashboards.

Learning objectives:

- Understand the components of ELK and their use in a SOC
- Explore the different features of ELK
- Learn to search and filter data in ELK
- Investigate VPN logs to identify anomalies
- Get familiar with creating visualizations and dashboards in ELK

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | I am all set! | No answer needed |

## Task 2 - Elastic Stack Overview

The Elastic Stack (ELK) was originally developed to store, search, and visualize large amounts of data. Organizations used it to monitor application performance and perform searches on large datasets. Over time, its features made it popular in security operations as well, and many SOC teams now use ELK almost as a SIEM solution.

Elastic Stack is a collection of open-source components that work together to collect data from any source, store and search it, and visualize it in real time. The four components are the following.

### 1. Elasticsearch

A full-text search and analytics engine for JSON-formatted documents. It stores, analyzes, and correlates data, and supports a RESTful API for interacting with it.

### 2. Logstash

A data processing engine that takes data from different sources, filters or normalizes it, and then sends it to the destination, which could be Kibana or a listening port. A Logstash configuration file is divided into three parts:

1. **Input**: defines the source from which the data is ingested
2. **Filter**: specifies the filter options to normalize the ingested log
3. **Output**: defines where the filtered data should be sent (a listening port, Kibana interface, Elasticsearch database, or file)

### 3. Beats

Host-based agents known as data shippers that transfer data from the endpoints to Elasticsearch. Each beat is a single-purpose agent that sends specific data:

- **Filebeat**: lightweight shipper for logs and other data
- **Metricbeat**: lightweight shipper for metric data
- **Packetbeat**: lightweight shipper for network data
- **Winlogbeat**: lightweight shipper for Windows event logs
- **Auditbeat**: lightweight shipper for audit data
- **Heartbeat**: lightweight shipper for uptime monitoring
- **Functionbeat**: serverless shipper for cloud data

### 4. Kibana

A web-based data visualization tool that works with Elasticsearch to analyze, investigate, and visualize data streams in real time. It supports creating multiple visualizations and dashboards for better visibility.

### How They Work Together

The components work together in a pipeline:

1. **Beats** collect data from multiple agents (for example, Winlogbeat collects Windows event logs and Packetbeat collects network traffic flows)
2. **Logstash** collects data from beats, ports, or files, parses and normalizes it into field-value pairs, and stores it in Elasticsearch
3. **Elasticsearch** acts as a database used to search and analyze data
4. **Kibana** displays and visualizes the data stored in Elasticsearch

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Logstash is used to visualize the data. (yay/nay) | `nay` |
| 2 | Elasticstash supports all data formats apart from JSON. (yay/nay) | `nay` |

## Task 3 - Lab Connection

The attached lab machine must be started before proceeding. Elastic needs about five minutes to start before the Kibana dashboard can be accessed with the provided credentials.

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Move to the next task! | No answer needed |

## Task 4 - Discover Tab

From this task onward, the focus is on ELK's front-end interface. Kibana is the component that supports interactions with the front end. The Discover tab is where SOC analysts spend most of their time. It shows the ingested logs, the search bar, normalized fields, and more.

### Key Elements of the Discover Tab

1. **Logs**: each row shows a single log containing information about the event, along with the fields and values found in that log
2. **Fields Pane**: the left panel shows the list of fields parsed from the logs. Clicking any field adds it to the filter or removes it from the search
3. **Index Pattern**: each type of log is stored in a different index pattern. This selects which index pattern to explore (for example, `vpn_connections` for VPN logs)
4. **Search Bar**: where search queries and filters are applied to narrow down results
5. **Time Filter**: narrows down results based on a specific time duration
6. **Time Interval**: chart showing event counts over time
7. **TOP Bar**: options to save, open, or share the search
8. **Discover Tab**: the main workspace in Kibana for exploring, searching, and analyzing raw data
9. **Add Filter**: applies filters to specific fields to narrow down results without manually typing entire queries

### Index Pattern

Kibana requires an index pattern to access the data stored in Elasticsearch. The index pattern tells Kibana which data to explore. Each log source has a different log structure, so when logs are ingested, they are normalized into corresponding fields and values by creating a dedicated index pattern. In the lab, the `vpn_connections` index pattern contains the VPN logs.

### Fields Pane

The left panel shows the list of normalized fields in the available logs. Clicking any field reveals the top 5 values and the percentage of occurrence. The + button adds a filter to show logs containing that value, and the - button adds a filter to exclude it.

### Time Filter

The time filter applies a log filter based on time. It supports Absolute, Relative, and Now options.

### Timeline

The timeline pane provides an overview of event counts over time, which is helpful for identifying spikes in the logs. For example, an unusual log spike can be observed on 11th January 2022.

### Create Table

By default, logs are shown in raw form. Clicking a log and selecting important fields creates a table showing only those fields, reducing noise and making the data more presentable. The table format can be saved so it shows the same fields every time.

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Select the index vpn_connections and filter from 31st December 2021 to 2nd Feb 2022. How many hits are returned? | `2861` |
| 2 | Which IP address has the maximum number of connections? | `238.163.231.224` |
| 3 | Which user is responsible for the overall maximum traffic? | `James` |
| 4 | Apply Filter on UserName Emanda; which SourceIP has max hits? | `107.14.1.247` |
| 5 | On 11th Jan, which IP caused the spike observed in the time chart? | `172.201.60.191` |
| 6 | How many connections were observed from IP 238.163.231.224, excluding the New York state? | `48` |
| 7 | Create a table with the fields IP, UserName, Source_Country and save. | No answer needed |

## Task 5 - KQL (Kibana Query Language)

The Search Bar in the Discover tab uses KQL (Kibana Query Language), a search query language for searching the ingested logs/documents in Elasticsearch. KQL supports two different ways of searching.

### Free Text Search

Free text search looks for logs based on text only. A search of the term `security` returns all documents that contain this term, regardless of the field. For example, searching for `"United States"` returns 2304 hits.

KQL looks for the whole term or word in the documents, so searching for just `United` returns no results. The wildcard `*` matches parts of a word, so `United*` returns all results containing "United" and any term after it.

### Field-Based Search

Field-based search provides the field name and the value being searched for. The syntax is `Field: Value`, with a colon as the separator. For example:

```
Source_ip : 238.163.231.224 AND UserName : Suleman
```

Clicking the search bar presents all available fields for use in the query.

### Logical Operators (AND | OR | NOT)

KQL supports logical operators:

- **AND**: returns logs containing both terms. `"United States" AND "Virginia"` returns 304 hits, all from Virginia
- **OR**: returns logs containing either term. `"United States" OR "England"` shows logs from either country
- **NOT**: excludes a particular term. `"United States" AND NOT ("Florida")` returns 1,902 hits, showing all US states except Florida

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Create a search query to filter the logs where Source_Country is the United States and show logs from User James or Albert. How many records were returned? | `161` |
| 2 | A user Johny Brown was terminated on the 1st of January, 2022. Create a search query to determine how many times a VPN connection was observed after his termination. | `1` |

## Task 6 - Creating Visualizations

The visualization tab presents the data in different forms such as tables, pie charts, and bar charts.

### Create Visualization

There are a few ways to reach the visualization tab. One way is to click any field in the Discover tab and choose the visualization option. Multiple visualizations can be created by selecting options like tables, pie charts, and so on.

### Correlation Option

Correlations between multiple fields are often needed. Dragging the required field into the middle creates a correlation tab in the visualization. For example, selecting `Source_Country` as the second field alongside `Source_IP` shows a pie chart of IPs broken down by country.

A table can also show the values of selected fields as columns, displaying the top values of Source_ip, Source_Country, and the count of records.

### Saving Visualizations

The most important step is saving the visualizations. Click the Save button in the top right corner, add a title and description, and click Save and add to the library.

### Failed Connection Attempts Visualization

For the failed connection visualization, use the `vpn_connections` data view, set the time picker to include January 2022, then filter for `action: failed`. Using `UserName` and `Source_ip` as the table fields shows the users and IPs involved in failed attempts.

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Which user was observed with the greatest number of failed attempts? | `Simon` |
| 2 | How many wrong VPN connection attempts were observed in January? | `274` |

## Task 7 - Creating Dashboards

Dashboards provide good visibility into log collection, and multiple dashboards can be created to fulfill a specific need. This task combines different saved searches and visualizations into a custom dashboard for VPN log visibility.

### Creating a Custom Dashboard

With a few searches saved from the Discover tab and some visualizations created, the steps to build a dashboard are:

1. Go to the Dashboard tab and click Create dashboard
2. Click Add from Library
3. Click the visualizations and saved searches to add them to the dashboard
4. Once the items are added, adjust them accordingly
5. Save the dashboard after completing it

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Create the dashboard containing the available visualizations. | No answer needed |
