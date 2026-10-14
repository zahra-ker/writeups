# SOC Workbooks & Lookups

- **Platform:** TryHackMe
- **Difficulty:** Easy
- **Link:** [SOC Workbooks & Lookups](https://tryhackme.com/room/socworkbookslookups)

## Overview

This room explores SOC workbooks designed to streamline alert triage and explains various lookup methods to quickly retrieve user and system context. You learn about identity inventory, asset inventory, and network diagrams as key sources of information during an investigation. The room also covers workbook theory (enrichment, investigation, escalation stages) and ends with a hands-on exercise where you build three investigation workbooks.

## Task 1 - Introduction

Alert triage is a complex process that often requires analysts to gather additional information about affected employees or servers. This room explores SOC workbooks designed to streamline alert triage and explains various lookup methods to quickly retrieve user and system context.

Learning objectives:

- Familiarise yourself with SOC investigation workbooks
- Learn where to find and how to use asset inventory in SOC
- Understand the importance of corporate network diagrams
- Practice workflow building inside an interactive interface

## Task 2 - Assets & Identities

Imagine having a night shift and looking into an alert saying that G.Baker logged into the HQ-FINFS-02 server. The user downloaded the "Financial Report US 2024.xlsx" file and shared it with R.Lund. To correctly triage the alert, you need answers to several questions: Who is G.Baker? What is HQ-FINFS-02? Why could R.Lund need access to corporate financial records?

### Identity Inventory

Identity inventory is a catalogue of corporate employees (user accounts), services (machine accounts), and their details like privileges, contacts, and roles within the company. For the scenario above, identity inventory would help you get context about G.Baker and R.Lund, and make it simpler to decide if the activity was expected or not.

| Full Name | Username | Email | Role | Location | Access |
|-----------|----------|-------|------|----------|--------|
| Gregory Baker | G.Baker | g.baker@tryhatme.thm | Chief Financial Officer | Europe, UK | VPN, HQ, FINANCE |
| Raymond Lund | R.Lund | r.lund@tryhatme.thm | US Financial Adviser | US, Texas | VPN, FINANCE |
| Kate Danner | K.Danner | k.danner@tryhatme.thm | Chief Technology Officer | Europe, UK | VPN, DA, HQ, AWS |
| svc-veeam-06 | svc-veeam-06 | N/A | Backup Service Account | N/A | VEEAM, DMZ, HQ |
| svc-nginx-pp | svc-nginx-pp | N/A | Web App Service Account | N/A | DMZ |

#### Sources of Identities

| Solution | Examples | Description |
|----------|----------|-------------|
| Active Directory | On-prem AD, Entra ID | AD itself is an identity database, commonly used by SOC |
| SSO Providers | Okta, Google Workspace | Cloud alternative for AD, an easy way to manage and search the users |
| HR Systems | BambooHR, SAP, HiBob | Limited to employees only, but usually provides full employee data |
| Custom Solution | CSV or Excel Sheets | It is common for IT or security teams to maintain their own solutions |

### Asset Inventory

Asset inventory, also called asset lookup, is a list of all computing resources within an organisation's IT environment. While "asset" is a vague term and can refer to software, hardware, or employees, this room emphasises servers and workstations only. For the scenario above, asset inventory would help you get context about the HQ-FINFS-02 server.

| Hostname | Location | IP Address | OS | Owner | Purpose |
|----------|----------|------------|-----|-------|---------|
| HQ-FINFS-02 | UK Datacenter | 172.16.15.89 | Windows Server 2022 | Central IT | File server for financial records |
| HQ-ADDC-01 | UK Datacenter | 172.16.15.10 | Windows Server 2019 | Central IT | Primary AD domain controller |
| PC-891D | London Office | 192.168.5.13 | Windows 11 Pro | Tech Support | Stationary PC for accountants |
| L007694 | Remote | N/A | MacOS 13 | A.Kelly, DevOps | Corporate laptop |
| L005325 | Remote | N/A | MacOS 13 | J.Eldridge, HR | Corporate laptop |

#### Sources of Assets

| Solution | Examples | Description |
|----------|----------|-------------|
| Active Directory | On-prem AD, Entra ID | Not only an identity but also a solid asset inventory database |
| SIEM or EDR | Elastic, CrowdStrike | Some SIEM or EDR agents collect information about the monitored hosts |
| MDM Solution | MS Intune, Jamf MDM | A dedicated class of solutions created to list and manage assets |
| Custom Solution | CSV or Excel Sheets | Same as with the identity inventory, custom solutions are common |

With this context, the scenario makes sense: G.Baker (CFO) shared a financial report from the finance file server with R.Lund (US Financial Adviser), both of whom have legitimate FINANCE access. The activity looks expected.

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Looking at the identity inventory, what is the role of R.Lund at the company? | `US Financial Adviser` |
| 2 | Checking the asset inventory, what data does the HQ-FINFS-02 server store? | `Financial records` |
| 3 | Finally, does the file sharing from the scenario look legitimate and expected? (Yea/Nay) | `Yea` |

## Task 3 - Network Diagrams

Continuing identity and asset inventory topics, you might also need to look at the alert from a network point of view, especially in bigger companies. Consider the following scenario where you are investigating a chain of related alerts based on firewall logs:

1. **08:00**: An IP 103.61.240.174 is repeatedly connecting to a corporate firewall via port TCP/10443
2. **08:23**: Firewall logs show that the IP 103.61.240.174 was translated to an internal 10.10.0.53 IP
3. **08:25**: The IP 10.10.0.53 is scanning the 172.16.15.0/24 network range but does not find open ports
4. **08:32**: The same IP is now scanning the 172.16.23.0/24 network range, and the attack seems to be ongoing

A **network diagram** is a visual schema presenting existing locations, subnets, and their connections. The diagram for this scenario shows three subnets: Database Subnet (172.16.15.0/24), VPN Subnet (10.10.0.0/16), and Office Subnet (172.16.23.0/24). The exposed ports on the corporate firewall are www.tryhatme.thm:80, www.tryhatme.thm:443, and vpn.tryhatme.thm:10443.

By referring to the network diagram, you can reconstruct the attack path:

1. Threat actor behind the 103.61.240.174 IP performed **VPN brute force**, targeting vpn.tryhatme.thm
2. After a successful brute force and VPN login, the threat actor was assigned an IP from the **VPN Subnet**
3. Then, the adversary tried to scan the **Database Subnet**, but was likely blocked by the firewall rules
4. Seeing no success, the threat actor switches to the **Office Subnet**, looking for their next target

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | According to the network diagram, which service is exposed on the TCP/10443 port? | `VPN` |
| 2 | Now, which subnet would the server behind 172.16.15.99 IP belong to? | `Database subnet` |
| 3 | Finally, does the scenario look like a True Positive (TP) or False Positive (FP)? | `TP` |

## Task 4 - Workbooks Theory

With asset inventory and network diagrams, you can get enough context about the user, host, or IP address. Next, your task is to make a verdict on whether the activity you see is safe or not. For some alerts, the analysis is simple, but some may require dozens of essential steps. How can you ensure the analysis steps are always followed?

### SOC Workbooks

A **SOC workbook**, also called playbook, runbook, or workflow, is a structured document that defines the steps required to investigate and remediate specific threats efficiently and consistently. Since L1 analysts are considered junior specialists and are not expected to triage every possible attack scenario perfectly, senior analysts often prepare workbooks to support their less experienced teammates. L1 analysts are recommended and sometimes even required to triage the alerts precisely according to workbooks to avoid mistakes and streamline the analysis.

### Workbook Example

The "Unusual Login Location Workbook" is a typical example. It is divided into three logical stages:

1. **Enrichment**: Use Threat Intelligence and identity inventory to get information about the affected user. This includes looking up the user's expected location in BambooHR, checking the login IP in Threat Intelligence services, and looking it up in anonymisation detection services.

2. **Investigation**: Using the gathered data and SIEM logs, make your verdict if the login is expected. Check if the login IP is confirmed malicious, look for suspicious actions like MFA reset, review login history via Splunk's "Login Timeline" dashboard, and determine if the login location or time is atypical for the user.

3. **Escalation**: Escalate the alert to L2 or communicate the login with the user if necessary. Write an alert report and attach the evidence for L2 review.

By following the steps in the correct order, you can guarantee high-quality alert triage and eliminate cases where the verdict is made without enough evidence.

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Which SOC role would use workbooks the most (e.g. SOC Manager)? | `SOC L1 Analyst` |
| 2 | What is the process of gathering user, host, or IP context using TI and lookups? | `Enrichment` |
| 3 | Looking at the workbook example, what platform is used as an identity inventory source? | `BambooHR` |

## Task 5 - Workbooks Practice

Different teams have different approaches to workbook building. Some teams may have hundreds of complex workbooks for every possible detection rule, more like a SOAR automation playbook than human guides. Other teams may prepare just a few high-level workbooks for the most common attack vectors and rely more on the experience and decision-making of L1 analysts. In any case, you as an L1 analyst should know how to divide your investigation into modular blocks and build simple workbooks around them.

In this exercise, you drag and drop the missing workbook steps from the options to their respective positions. Three workbooks cover the most common SOC alert categories.

### Workbook 1: Phishing Email

<img src="images/workbook-phishing.png" width="65%">

The phishing email workbook follows these steps: take ownership of the alert and use identity inventory to get context of the email recipients, investigate the email using an EML analyser (SPF/DKIM, content, sender domain reputation), then continue to attachment analysis using sandbox for binaries and manual code review for scripts. If the attachment is malicious, the origin is faked, or the email is unexpected for the recipients' roles, gather triage evidence and escalate to L2. Otherwise, close as False Positive.

### Workbook 2: Suspicious Download & PowerShell

<img src="images/workbook-powershell.png" width="65%">

This workbook starts by assigning the alert and getting context of the affected machine using asset inventory. Next, use threat intel to analyse the URL and perform static analysis of the downloaded executable. Build a process tree to find the parent process and account used to start PowerShell. If the file is malicious, downloaded from an untrusted resource, or originates from a suspicious process, save SIEM search results and escalate to L2. Otherwise, close as False Positive and reach SOC engineers to tune the detection rule.

### Workbook 3: Network Scanning

<img src="images/workbook-scanning.png" width="65%">

This workbook begins by assigning the alert and getting the IPs context from the network map and asset inventory. List ports scanned by the IP and find out which services could be running on them. If the source is a corporate Nessus/Zabbix scanner, verify if the scanning pattern is expected. If the source IP is unlisted or the scanning pattern looks like a malicious Discovery attempt, collect evidence and escalate to L2. Otherwise, close as False Positive and reach out to SOC engineers to tune the rule.

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | What flag did you receive after completing the first workbook? | `THM{the_most_common_soc_workbook}` |
| 2 | What flag did you receive after completing the second workbook? | `THM{be_vigilant_with_powershell}` |
| 3 | What flag did you receive after completing the third workbook? | `THM{asset_inventory_is_essential}` |
