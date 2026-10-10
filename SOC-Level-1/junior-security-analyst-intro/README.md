# Junior Security Analyst Intro

- **Platform:** TryHackMe
- **Difficulty:** Easy
- **Link:** [Junior Security Analyst Intro](https://tryhackme.com/room/jrsecanalystintrouxo)

## Overview

An introductory room to the SOC Level 1 path. It covers the role of a Junior Security Analyst, the structure of a Security Operations Center (SOC), and walks you through a hands-on exercise where you investigate alerts on a SIEM dashboard, look up a suspicious IP, escalate the incident, and block the attacker on a firewall.

## Task 1 - Junior Security Analyst Journey

This task introduces the role of a Junior Security Analyst (also called a SOC Level 1 Analyst). You work in a 24/7 SOC team reviewing security alerts alongside your colleagues. Your daily duties include:

- Monitor and investigate various security alerts
- Participate in SOC brainstorms and workshops
- Cooperate with other teams to keep the company safe
- Constantly learn and discover new attacks and defenses

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | Which team do you work with as a Junior Security Analyst? | `SOC` |

## Task 2 - Security Operations Center (SOC)

This task introduces the SOC team and its different roles. You meet your colleagues:

- **Will Griffin** - Senior Analyst: your closest colleague, helps junior analysts and handles complex cases after the initial analysis.
- **Corey Stevens** - SOC Engineer: maintains security tools and configures alerts. Doesn't analyze alerts directly.
- **Emily Conway** - SOC Manager: keeps everything under control, reports SOC results to top management.
- **Daniel Herrera** - Incident Responder: called on demand when something serious has happened, handles major incidents.

Your daily duties as a Junior Analyst may include:

- Detect and prevent a data stealer infection on a coworker's laptop
- Analyze and stop a phishing campaign targeting the finance team
- Participate in a bigger incident, such as a full-scale ransomware attack
- Team up with your teammates to build detection rules and automations
- Go beyond cyber and understand how companies operate from the inside

## Task 3 - A Day in the Life of a Security Analyst

This is the hands-on task. You open the attached lab and navigate to a simulated alert dashboard.

### SIEM Dashboard

The SIEM dashboard shows several alerts. Two **Critical** alerts stand out at the top:

- **17:44** - Successful SSH login from the suspicious IP address `221.181.185.159`
- **17:40** - Unauthorized login attempts from IP address `221.181.185.159` to port 22

Other alerts of lower severity are also present (failed logins, expired passwords), but the critical ones demand immediate attention.

<img src="images/siem-dashboard.png" width="75%">

### IP Hunter

Switching to the IP Hunter tab and looking up `221.181.185.159` confirms it is **malicious**, involved in 4 cyber attacks:

| Attribute | Value |
|-----------|-------|
| ISP | China Mobile Communications Corporation |
| Domain Name | chinamobile.thm |
| Country | China |
| Categories | Port Scan, C2 Server, PlugX |

<img src="images/ip-hunter.png" width="75%">

### Escalation

Since the IP is confirmed malicious and there was a successful SSH login, this needs to be escalated. Among the available staff, **Will Griffin** (SOC Team Lead) is the correct person to escalate to, as he is in charge of your team and handles complex cases.

<img src="images/escalation.png" width="75%">

### Firewall Block

After escalation, you block the malicious IP `221.181.185.159` on the firewall to stop the attack.

<img src="images/firewall-block.png" width="75%">

### Answers

| # | Question | Answer |
|---|----------|--------|
| 1 | What was the malicious IP address in the alerts? | `221.181.185.159` |
| 2 | To whom did you escalate the alert with the malicious IP? | `Will Griffin` |
| 3 | What message did you get after blocking the IP address on the firewall? | `THM{until-we-meet-again}` |
