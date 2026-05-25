# Golden Ticket — Active Directory Attack Investigation

## Overview

This investigation analyzed suspicious authentication activity within a Windows Active Directory environment after alerts indicated possible lateral movement toward the Domain Controller (DC).

The objective was to:

- Trace attacker behavior through Windows Security Event Logs
- Identify compromised accounts
- Investigate Kerberos-related abuse
- Determine whether a Golden Ticket attack had been attempted

The investigation was performed using:

- Windows Event Viewer
- `security.evtx` logs
- The LetsDefend SOC training environment

---

# Scenario Summary

Security monitoring systems detected suspicious authentication attempts and lateral movement activity originating from compromised internal workstations.

Indicators suggested the attacker was attempting to:

1. Escalate privileges within the domain
2. Move laterally across systems
3. Target the Domain Controller
4. Abuse Kerberos authentication mechanisms

The investigation uncovered evidence of:

- AS-REP Roasting
- Service account compromise
- Kerberos ticket abuse
- Potential Golden Ticket activity

---

# Investigation Objectives

- Identify the compromised service account
- Determine the attacker source IP address
- Detect evidence of AS-REP Roasting
- Identify targeted Kerberos accounts
- Investigate suspicious Kerberos ticket activity
- Confirm attempted Golden Ticket authentication
- Establish a timeline of attacker actions

---

# Tools & Technologies

| Tool / Technology | Purpose |
|---|---|
| Windows Event Viewer | Log analysis |
| Windows Security Logs (`security.evtx`) | Authentication investigation |
| Kerberos Analysis | Ticket activity investigation |
| Active Directory Monitoring | Domain activity tracking |
| MITRE ATT&CK Framework | Attack classification |

---

# Investigation Process

## 1. Security Log Review

The investigation began by loading the provided `security.evtx` file into Windows Event Viewer for analysis of Windows Security Events.

### Primary Event IDs Investigated

| Event ID | Description |
|---|---|
| `4624` | Successful Logon |
| `4768` | Kerberos TGT Request |
| `4769` | Kerberos Service Ticket Request |
| `4672` | Special Privileges Assigned |

---

## 2. Identifying the Compromised Service Account

The investigation focused on Kerberos authentication activity and successful network logons originating from suspicious internal IP addresses.

Analysis of Event ID `4624` revealed successful NTLM-based network authentication attempts involving the account:

```text
SQLService
