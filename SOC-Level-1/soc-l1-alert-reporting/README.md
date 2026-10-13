# SOC L1 Alert Reporting

- **Platform:** TryHackMe
- **Difficulty:** Easy
- **Link:** [SOC L1 Alert Reporting](https://tryhackme.com/room/socl1alertreporting)

## Overview

This room builds on alert triage by introducing three essential SOC skills: alert reporting, escalation, and communication. You learn how to write structured alert reports using the Five Ws approach, when and how to escalate alerts to L2 analysts, and how to handle crisis communication scenarios. The room includes hands-on practice on the SOC dashboard where you investigate new alerts, write reports, and escalate them.

## Task 1 - Introduction

After triaging alerts, L1 analysts may be uncertain about how to classify them, requiring senior support or information from the system owner. L1 analysts also deal with real cyberattacks and breaches that need immediate attention and remediation actions. This room covers these cases by introducing three new terms: alert reporting, escalation, and communication.

Learning objectives:

- Understand the need for SOC alert reporting and escalation
- Learn how to write alert comments or case reports properly
- Explore escalation methods and communication best practices
- Apply the knowledge to triage alerts in a simulated environment
- Feel more confident in SOC Simulator and during SAL1 certification

## Task 2 - Alert Funnel

In the previous room, you learned how to classify and triage alerts. The next step is understanding what happens after triage: how does your work prevent threats and stop breaches?

L1 analysts receive alerts in a SIEM, EDR, or ticket management platform. Most alerts are closed as False Positives or handled at L1 level, but complex and threatening ones are sent to L2. Out of 100 SIEM alerts, roughly 10 make it through as True Positives to L2, and from those, maybe 1 becomes an incident handled by the DFIR team. The ultimate goal is to protect corporate data.

To send alerts further, you need three skills:

### Alert Reporting

Before closing or passing an alert to L2, you might have to report it. Depending on team standards and alert severity, you can be required to document your investigation in detail, ensuring all relevant evidence is included. This is especially important for True Positives, which require escalation.

### Alert Escalation

If the True Positive alert requires additional actions or deeper investigation, escalate it to the L2 analyst for further review following the agreed procedures. Your alert report comes in handy since L2 will use it to get the initial context and spend less time on the analysis from scratch.

### Communication

You may also need to communicate with other departments during or after the analysis. For example, ask the IT team if they confirm granting administrative privileges to some users or contact HR to get more information about a newly hired employee.

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | What is the process of passing suspicious alerts to an L2 analyst for review? | `Alert Escalation` |
| 2 | What is the process of formally describing alert details and findings? | `Alert Reporting` |

## Task 3 - Reporting Guide

Having L1 analysts write alert reports in addition to marking them as True or False Positives is essential. Alert reports serve several key purposes:

| Alert Report Purpose | Explanation |
|---------------------|-------------|
| Provide context for escalation | A well-written report saves lots of time for L2 analysts and helps them quickly understand what happened |
| Save findings for the records | Raw SIEM logs are stored for 3-12 months, but alerts are kept indefinitely. It's better to keep all the context inside the alert, just in case |
| Improve investigation skills | If you can't explain it simply, you don't understand it well enough. Report writing is a great way to boost L1 skills by summarising alerts |

### Report Format

Imagine yourself as an L2 analyst, a DFIR team member, or an IT professional who needs to understand the alert. Follow the **Five Ws** approach and include at least these items in the report:

- **Who**: Which user logs in, runs the command, or downloads the file
- **What**: What exact action or event sequence was performed
- **When**: When exactly did the suspicious activity start and end
- **Where**: Which device, IP, or website was involved in the alert
- **Why**: The most important W, the reasoning for your final verdict

On the SOC dashboard, the "Sensitive Document Share to External" alert reveals that m.boslan@tryhackme.thm shared an internal HR spreadsheet ("Employee Records (Updated)") to an external Protonmail address. The DLP solution had already blocked an attempt to download the whole HR folder:

<img src="images/sensitive-document-alert.png" width="65%">

A new "Email Marked as Phishing after Delivery" alert also appears, showing an email from support@microsoft.com to the IT Manager with a suspicious REPORT.rar attachment. Both SPF and DKIM checks failed:

<img src="images/phishing-email-alert.png" width="65%">

After investigating the phishing alert and writing a proper report using the Five Ws template, you receive the flag.

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | According to the SOC dashboard, which user email leaked the sensitive document? | `m.boslan@tryhackme.thm` |
| 2 | Looking at the new alerts, who is the "sender" of the suspicious, likely phishing email? | `support@microsoft.com` |
| 3 | Using the Five Ws template, what flag did you receive after writing a good report? | `THM{nice_attempt_faking_microsoft_support}` |

## Task 4 - Escalation Guide

After making a verdict and writing your alert report, you must choose whether to escalate the alert to L2. You should escalate when:

1. The alert is an indicator of a major cyberattack requiring deeper investigation or DFIR
2. Remediation actions like malware removal, host isolation, or password reset are required
3. Communication with customers, partners, management, or law enforcement agencies is required
4. You just do not fully understand the alert and need some help from more senior analysts

### Escalation Steps

To escalate an alert, reassign it to the L2 on shift and ping them in corporate chat or in person. In some teams, you may be required to create a formal written escalation request with dozens of required fields.

### Requesting L2 Support

It is generally fine for L1 to request senior support if something is unclear. Especially in your first months, it's always better to discuss the alert and clarify SOC procedures than to blindly close the alert you don't understand yourself.

### SOC Dashboard Escalation

On the dashboard, the escalation workflow is:

1. Move the alert to **In Progress** status and do the analysis
2. Write an alert report and set your **verdict**, such as True Positive
3. If escalation is required, assign the alert **to your L2** on shift
4. L2 will receive a notification and start from your alert report

The assignee dropdown shows the available team members, with E.Fleming as the L2 analyst on shift:

<img src="images/assignee-dropdown.png" width="65%">

After correctly escalating both new alerts to L2, the dashboard shows all alerts have been processed:

<img src="images/dashboard-escalated.png" width="65%">

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Who is your current L2 in the SOC dashboard that you can assign (escalate) the alerts to? | `E.Fleming` |
| 2 | What flag did you receive after correctly escalating the alert from the previous task to L2? | `THM{good_job_escalating_your_first_alert}` |
| 3 | What flag did you get after escalating the second new alert according to the workflow? | `THM{looks_like_webshell_via_old_exchange}` |

## Task 5 - SOC Communication

The escalation and reporting topics should sound straightforward. But you should be prepared for unexpected scenarios and know what to do in critical cases. The SOC team has its own **Crisis Communication** procedures, with guides and processes to help you and your teammates resolve issues.

### Communication Cases

- **L2 is unavailable for 30 minutes during a critical alert**: First try to call L2, then L3, and finally your manager. Ensure you know where to find emergency contacts.
- **Account compromise requires validating the login with the affected user**: Do not contact the user through the breached chat. Use alternative contact methods like a phone call.
- **Overwhelming number of alerts, some critical**: Prioritise alerts according to the workflow, but inform your L2 on shift about the situation.
- **You misclassified an alert and likely missed a malicious action**: Immediately reach out to your L2 explaining your concerns. Threat actors can be silent for weeks before impact.
- **SIEM logs are not parsed correctly or not searchable**: Do not skip the alert. Investigate what you can and report the issue to your L2 on shift or SOC engineer.

### Communication By L2

No matter what the agreements are, L2 will eventually receive the ticket from you, read your report, and contact you in case of any questions. Once everything is clear, the L2 analyst will typically research the alert details further, validate if the alert is indeed a True Positive, communicate with other departments if needed, and for major incidents, start a formal Incident Response process.

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Should you first try to contact your manager in case of a critical threat (Yea/Nay)? | `Nay` |
| 2 | Should you immediately contact your L2 if you think you missed the attack (Yea/Nay)? | `Yea` |
