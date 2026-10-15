# Introduction to EDRs

- **Platform:** TryHackMe
- **Difficulty:** Easy
- **Link:** [Introduction to EDRs](https://tryhackme.com/room/introductiontoedrs)

## Overview

This room covers Endpoint Detection and Response (EDR) solutions, explaining how they differ from traditional antivirus software, their architecture, the types of telemetry they collect, and their detection and response capabilities. The room ends with a hands-on exercise where you investigate realistic alerts on an EDR dashboard.

## Task 1 - Introduction

Endpoint Detection and Response (EDR) is a security solution designed to monitor, detect, and respond to advanced threats at the endpoint level. As a SOC analyst, it is essential to understand how EDR works since it is a widely adopted solution in organizations to protect their endpoints.

Learning objectives:

- Understand the basics of EDR and how it works
- Differentiate EDR from traditional Antivirus solutions
- Examine the architecture of an EDR solution
- Analyze the types of telemetry it collects from endpoints
- Understand the detection and response capabilities of an EDR
- Investigate a realistic alert in the EDR

## Task 2 - What is an EDR?

The increase in the use of digital devices is undeniable, and cyber threats are increasing day by day. To protect these devices, organizations implement several security measures, most of which are to protect the network. However, the fast adoption of Remote Work has exposed these devices as they are out of the perimeter protection deployed on the organization's network.

EDR is a security solution that offers deep-level protection for endpoints. No matter where the endpoints are, the EDR will make sure they are monitored constantly and threats are detected. Some EDR solutions in the market include CrowdStrike Falcon, Symantec EDR, SentinelOne EDR, and Microsoft Defender for Endpoint.

### Features of EDR

There are three main features of an EDR, which can also be referred to as the three pillars:

- **Visibility**: The level of visibility EDR provides is impressive. It collects detailed data from the endpoints, including process modifications, registry modifications, file and folder modifications, user actions, and much more. It presents this information in a structured format to the analyst, who can see the whole process tree with a complete activity timeline. Any detections in the EDR land with a whole context.

- **Detection**: The detection feature wins over traditional detection capabilities. It incorporates signature-based as well as behavior-based detections, such as unexpected user activities. With modern machine learning capabilities, it identifies any deviation from the baseline behavior and instantly flags it. It can also detect fileless malware that resides in memory and allows feeding custom IOCs for threat detections.

- **Response**: EDR empowers analysts to take action on detected threats from within the central EDR console. As an analyst, you may decide to isolate a complete endpoint, terminate a process, or quarantine some files. You can also connect to the host remotely and execute actions independently.

It is important to remember that an EDR is a host-only security solution and does not detect network-level threats.

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Which feature of EDR provides a complete context for all the detections? | `Visibility` |
| 2 | Which process spawned sc.exe? | `cmd.exe` |

## Task 3 - Beyond the Antivirus

Both AV and EDR have the same motive of protecting the endpoint they are installed on. However, they differ in the level of protection they provide.

### Airport Analogy

Think of an airport as an endpoint that needs protection. The immigration check (AV) monitors incoming people and matches their passports with a list of known criminals. If there is a match, the entry is blocked. But what happens if somebody who has never been identified as a criminal tries to come in? The immigration check will let them in. The airport has an undetected threat inside.

An EDR would be the security officers stationed inside the airport. They constantly monitor the security cameras and motion sensors, watching for suspicious behaviour: roaming close to restricted areas, leaving bags unattended. Even if somebody manages to evade the immigration check, the security officers will constantly be monitoring their actions.

### Scenario Breakdown

A phishing email with a malicious Word document walks through the differences between AV and EDR:

1. User receives a phishing email with a Word document embedded with a malicious macro (VBA script)
2. The user downloads the document and opens it
3. The malicious macro is silently executed, and it spawns PowerShell
4. The malicious macro runs an obfuscated PowerShell command to download a sophisticated second-stage payload
5. The payload is injected into a legitimate svchost.exe
6. The attacker gains remote access to the system

| Attack Step | AV's Response | EDR's Response |
|-------------|---------------|----------------|
| Step #1 | Does nothing if the downloaded file has no previous signature in the database | Logs the file download activity and monitors it |
| Step #2 | Does nothing upon the opening of the document since winword.exe is a legitimate utility | Records the execution of winword.exe and keeps monitoring |
| Step #3 | Does nothing if the executed macro has no previous signature | Detects and flags the macro execution due to the unusual parent-child relationship of winword.exe and PowerShell.exe processes |
| Step #4 | Typically, AVs will not detect obfuscated PowerShell scripts | Flags the obfuscated script execution |
| Step #5 | Will not flag malicious injection into svchost.exe since it does not monitor the memory injections | Detects Process Injection in svchost.exe |
| Step #6 | Lacks Network Level Visibility | Flags the unexpected behaviour of svchost.exe, making an outbound connection |
| Final Action | May be marked as clean | Generates an alert with the full attack chain and enables the analyst to take actions from within the EDR |

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | In the given analogy, what presents an AV? | `immigration check` |
| 2 | Which legitimate process was hijacked by the attacker in the scenario? | `svchost.exe` |
| 3 | Which security solution might mark this activity as clean? | `Antivirus` |

## Task 4 - How an EDR works?

### Agents

EDR agents (also known as sensors) are deployed inside the endpoints. They are the eyes and ears of the EDR. Their job is to sit at the endpoint and monitor all the activities. The information about these activities is sent in detail to the EDR central console in real time. The EDR agents can do some basic signature-based and behavior-based detections by themselves and send them to the EDR console, which triggers alerts.

### EDR Console

All the detailed data sent by the EDR agents is correlated and analyzed through complex logic and machine learning algorithms. The threat intelligence information is matched with the collected data. The EDR connects all the dots to form a detection, often called an alert. The console dashboard gives a holistic view of the current status of detections in all the endpoints.

### What happens after Detection?

When a detection comes, it's a SOC analyst's responsibility to acknowledge the alert and prioritize it. The EDR gives severities to all the alerts (Critical, High, Medium, Low, Informational). The alert with the highest severity is investigated as a priority. Once the alert is clicked, the analyst can see all the details of the detection, including files executed, processes executed, network connection attempts, registry modifications, and much more. The analyst's job is to determine if the alert is a false positive or a true positive. In case of a true positive, the analyst can take actions from within the EDR console.

### EDR with Other Tools

Although a standalone EDR provides enough information to detect and respond to threats in an endpoint, it works alongside other security solutions to form a larger security ecosystem. Within a network, you will see Firewalls, DLPs, Email Security Gateways, IAMs, EDRs, and other solutions protecting the different components. To minimize the effort and maximize the efficiency, all these security solutions are integrated with a SIEM solution that becomes the central point of investigation for the analysts.

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Which component of the EDR is responsible for collecting telemetry from the endpoints? | `Agent` |
| 2 | An EDR agent is also known as a? | `sensor` |

## Task 5 - EDR Telemetry

EDR agents collect different data from their endpoints and push it to the EDR console. This data is known as Telemetry. Telemetry is the black box of an endpoint with everything necessary for detection and investigation.

### Collected Telemetry

- **Process Executions and Terminations**: Keeps track of all running and idle processes, which helps identify suspicious child-parent process relationships, suspicious executables initiating a process, malware payload, etc.

- **Network Connections**: All the endpoints' network connections are monitored, which helps identify any connection to a C2 server, unusual port usage, data exfiltration, or lateral movement within the network.

- **Command Line Activity**: Captures all commands executed on the endpoints in CMD, PowerShell, etc., which helps identify malicious command execution, obfuscated PowerShell script executions, which are often missed by a traditional antivirus.

- **Files and Folders Modifications**: Threat actors modify different files and folders during data staging, ransomware executions, and malicious file dropping. The EDR tracks this.

- **Registry Modifications**: The registry is a goldmine of information about the configurations in a Windows system. There are many registry modifications that occur during a malicious activity, and most of these are monitored by the EDR.

EDR uses complex logic and machine learning algorithms to assess the activities. Advanced threats keep most of their activities stealthy, using legitimate utilities during execution. Individually, their activities may seem harmless, but when observed through detailed telemetry, they tell a different story. The analysts can understand the full chain of events, identify the root cause, and reconstruct the attack timeline.

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Which telemetry data helps in detecting C2 communications? | `Network Connections` |
| 2 | Where are the configuration settings of a Windows system primarily stored? | `registry` |

## Task 6 - Detection And Response Capabilities

### Detection

Based on the telemetry received from the endpoints, some advanced detection techniques are applied. These include:

- **Behavioral Detection**: Instead of just matching signatures with known threats, it observes the complete behavior of a file. Advanced threats craft their malware to look clean and use legitimate processes. EDR catches this behavior. For example, a process winword.exe spawning PowerShell.exe will be flagged due to the unusual parent-child relationship.

- **Anomaly Detection**: With time, EDR understands the baseline behavior of the endpoints. Any activity that deviates from this behavior will be flagged. Sometimes this can generate false positives as well, but with the full context it gives, the analyst can identify its legitimacy. For example, a process modifying an auto-start registry key which is not common behavior on the endpoint.

- **IOC Matching**: EDRs have strong threat intelligence field integrations. Except for zero-day attacks, most of the attacks have indicators published in threat intelligence feeds. EDR flags any activity that matches any known IOC. For example, a user downloads a file that drops an executable, and the hash of this executable is matched with the threat intelligence feed and instantly flagged.

- **MITRE ATT&CK Mapping**: Any activity flagged by the EDR is not only marked as malicious or suspicious but also mapped with the MITRE Tactic and Technique (attack stage). For example, the creation of a scheduled task will likely map to Tactic: Persistence, Technique: Scheduled Task/Job.

- **Machine Learning Algorithms**: Advanced threat actors try to evade defenses as much as possible. Modern EDRs have machine learning models trained by a large dataset of normal and malicious behaviors. This can detect complex patterns of an attack. Fileless attacks and multi-staged intrusions are often detected through this.

### Response

EDR offers both automated and manual responses. You can make policies to block known malicious behaviors automatically. Manual response capabilities include:

- **Isolate Host**: Isolate an endpoint from the network to contain malicious activity and prevent lateral movement
- **Terminate Process**: Neutralize the malicious activity by terminating a specific process without full host isolation
- **Quarantine**: Move a malicious file to an isolated location where it can not be executed
- **Remote Access**: Remotely access the shell of any endpoint to gain deeper visibility or take custom actions, such as running scripts or collecting data
- **Artefacts Collection**: Extract important artefacts from endpoints without physically accessing the device, including memory dumps, event logs, specific folder contents, and registry hives

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Which feature of the EDR helps you identify threats based on known malicious behaviours? | `IOC Matching` |

## Task 7 - Investigate an alert on EDR

You are a SOC analyst at TECH THM with access to the EDR console, having multiple medium and high-severity detections. Your task is to perform triage on each detection using the available information in the EDR and answer a series of questions related to these detections.

### DESKTOP-HR01: Initial Access via Malicious Office Document

The process chain shows how a malicious Office document led to malware being downloaded onto the system. WINWORD.EXE spawned CMD.EXE, which launched cURL.EXE to download a payload from ayebd.thm and save it as install.exe in the Public folder:

<img src="images/edr-process-chain.png" width="65%">

Clicking on INSTALL.EXE in the process chain reveals more details: the file was unsigned, flagged as "Known in malware campaigns" by Threat Intel, and quarantined by the EDR before it could be launched:

<img src="images/edr-install-exe.png" width="65%">

### WIN-ENG-LAPTOP03: Credential Dumping via LSASS Memory Access

On this machine, syncsvc.exe (located in haris.khan's Temp folder) accessed the memory of lsass.exe and wrote a dump to disk. The tool then attempted to exfiltrate the dump to files-wetransfer.com, but the connection was blocked by the EDR. Threat Intel identified it as a known credential dumping tool (ATT&CK ID: T1003.001):

<img src="images/edr-credential-dumping.png" width="65%">

### DESKTOP-DEV01: Execution from AppData Directory

UpdateAgent.exe was launched from daniel.richards' AppData\Roaming folder. It initiated an outbound connection to an internal IP (10.10.20.5) on port 8080. Despite the suspicious location and behavior, Threat Intel labelled it as a known internal IT utility tool:

<img src="images/edr-updateagent.png" width="65%">

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Which tool was launched by CMD.exe to download the payload on DESKTOP-HR01? | `CURL.exe` |
| 2 | What is the absolute path to the downloaded malware on the DESKTOP-HR01 machine? | `C:\Users\Public\install.exe` |
| 3 | What is the absolute path to the suspicious syncsvc.exe on the WIN-ENG-LAPTOP03 machine? | `C:\Users\haris.khan\AppData\Local\Temp\syncsvc.exe` |
| 4 | On which URL was the exfiltration attempt being made on WIN-ENG-LAPTOP03? | `https://files-wetransfer.com/upload/session/ab12cd34ef56/dump_2025.dmp` |
| 5 | What was UpdateAgent.exe labelled by Threat Intel on DESKTOP-DEV01? | `Known internal IT utility tool` |
