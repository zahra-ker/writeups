# Splunk: Basics

- **Platform:** TryHackMe
- **Difficulty:** Easy
- **Link:** [Splunk: Basics](https://tryhackme.com/room/splunk101)

## Overview

This room explores the basics of Splunk, one of the leading SIEM solutions in the market. It covers Splunk's three main components (Forwarder, Indexer, Search Head), walks through the Splunk interface, and ends with a hands-on exercise where you ingest VPN logs and query them using SPL (Search Processing Language).

## Task 1 - Introduction

Splunk is one of the leading SIEM solutions in the market. It allows users to collect, analyze, and correlate network and machine logs in real time. In this room, we explore the basics of Splunk and its functionalities, and how it provides better visibility of network activities and helps speed up detection.

Learning objectives:

- Understanding the components of Splunk
- Exploring some available options in Splunk
- Understanding log ingestion in Splunk
- Practically ingesting some logs in Splunk and analyzing them

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Continue with the next task. | No answer needed |

## Task 2 - Connect with the Lab

Before proceeding with the following tasks, start the attached lab machine. After the machine starts, the Splunk instance can be accessed directly from your web browser.

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Connect with the lab. | No answer needed |

## Task 3 - Splunk Components

Splunk has three main components: Forwarder, Indexer, and Search Head. These components work together to help us search and analyze the data.

### Splunk Forwarder

Splunk Forwarder is a lightweight agent installed on the endpoint intended to be monitored, and its main task is to collect the data and send it to the Splunk instance. It does not affect the endpoint's performance as it takes a few resources to process. Some of the key data sources are:

- Web server generating web traffic
- Windows machine generating Windows Event Logs, PowerShell, and Sysmon data
- Linux host generating host-centric logs
- Database generating DB connection requests, responses, and errors

The forwarder collects the data from the log sources and sends it to the Splunk Indexer.

### Splunk Indexer

Splunk Indexer plays the main role in processing the data it receives from forwarders. It parses and normalizes the data into field-value pairs, categorizes it, and stores the results as events, making the processed data easy to search and analyze.

### Search Head

Splunk Search Head is the place within the Search & Reporting App where users can search the indexed logs. The searches are done using SPL (Search Processing Language), a powerful query language for searching indexed data. When the user performs a search, the request is sent to the indexer, and the relevant events are returned as field-value pairs.

The Search Head also allows you to transform results into presentable tables and visualizations such as pie, bar, and column charts.

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Which component is used to collect and send data over the Splunk instance? | `Forwarder` |

## Task 4 - Navigating Splunk

When you access Splunk, you will see the default home screen with several sections:

### Splunk Bar

The top panel with the following options:

- **Messages**: View system-level notifications and messages
- **Settings**: Configure Splunk instance settings
- **Activity**: Review the progress of search jobs and processes
- **Help**: View tutorials and documentation
- **Find**: Search across the App

The Splunk Bar also allows users to switch between installed Splunk apps.

### Apps Panel

This panel shows the apps installed for the Splunk instance. The default app for every Splunk installation is **Search & Reporting**.

### Explore Splunk

This panel contains quick links to add data to the Splunk instance, add new Splunk apps, and access the Splunk documentation.

### Splunk Dashboard

The last section is the Home Dashboard. By default, no dashboards are displayed. You can choose from a range of dashboards readily available within your Splunk instance, select one from the dropdown menu, or visit the dashboards listing page. Custom dashboards you create can be viewed separately by clicking on the **Yours** tab.

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | In the Add Data tab, which option is used to collect data from files and ports? | `Monitor` |

## Task 5 - Adding Data

Splunk can ingest any data. When data is added to Splunk, the data is processed and transformed into a series of individual events. The data sources can be event logs, website logs, firewall logs, etc. The data sources are grouped into categories including Files and directories, Network events, IT Operations, Cloud services, Database services, Security services, Virtualization services, Application servers, Windows sources, and others.

### Practical: Ingesting VPN Logs

In this task, we focus on VPN logs. The Add Data screen provides three methods to get data in: Upload (files from your computer), Monitor (files and ports on the Splunk platform instance), and Forward (data from a Splunk forwarder).

To upload the VPN_logs file, follow five steps:

1. **Select Source**: Choose the log file and the data source
2. **Select Source Type**: Select the type of logs being ingested (e.g. JSON, syslog)
3. **Input Settings**: Select the index where logs will be dumped and the hostname to associate with the logs
4. **Review**: Review all the configurations
5. **Done**: Complete the upload

The downloaded `VPN_logs` file is newline-delimited JSON. To import it:

1. Open Add Data and choose Upload
2. Select the downloaded `VPN_logs` file
3. Keep the JSON source type detected by Splunk
4. On Input Settings, create or select the index `VPN_Logs`
5. After the upload completes, open Search & Reporting and set the time picker to All time

If the field names do not appear in search results, add `| spath` after the base search to parse the JSON fields from each event.

### Useful SPL Queries

Total event count:
```
index=VPN_Logs
| stats count
```

Events by a specific user:
```
index=VPN_Logs
| spath
| search UserName="Maleena"
| stats count
```

Username associated with a specific IP:
```
index=VPN_Logs
| spath
| search Source_ip="107.14.182.38"
| stats values(UserName) as UserName count
```

Events from all countries except one:
```
index=VPN_Logs
| spath
| search Source_Country!="France"
| stats count
```

Events associated with a specific IP:
```
index=VPN_Logs
| spath
| search Source_ip="107.3.206.58"
| stats count
```

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Upload the data attached to this task and create an index "VPN_Logs". How many events are present in the log file? | `2862` |
| 2 | How many log events are captured by the user Maleena? | `60` |
| 3 | What is the username associated with IP 107.14.182.38? | `Smith` |
| 4 | What is the number of events that originated from all countries except France? | `2814` |
| 5 | How many VPN events were associated with the IP 107.3.206.58? | `14` |

## Task 6 - Conclusion

In this room, we learned about Splunk's core components, explored the Splunk interface, and practiced uploading data to Splunk. This provides the foundational knowledge of Splunk SIEM.

For further learning, TryHackMe recommends these follow-up rooms:

- Splunk: Exploring SPL
- Incident Handling with Splunk
- Investigating With Splunk
- Benign - Challenge
- PoshEclipse - Challenge

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Join the next room. | No answer needed |
