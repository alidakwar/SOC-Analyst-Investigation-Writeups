# APT35 HYPERSCRAPE Email Exfiltration Investigation Report

## Alert Overview

| Field | Details |
|---|---|
| Alert Name | SOC250 - APT35 HYPERSCRAPE Data Exfiltration Tool Detected |
| Severity | Medium (Escalated to High During Investigation) |
| Category | Data Leakage / Advanced Persistent Threat |
| Date Investigated | 2023-12-27 |
| Status | True Positive |
| Analyst | Ali Dakwar |

---

# Executive Summary

A security alert was triggered after the execution of `EmailDownloader.exe` on the internal endpoint `Arthur` (`172.16.17.72`). Initial analysis identified the executable as suspicious due to its execution path, unusual behavior, and a SHA-256 hash matching known indicators associated with APT35 (Charming Kitten) and the HYPERSCRAPE email extraction malware documented by Google Threat Analysis Group (TAG).

Further investigation confirmed successful mailbox download activity through Outlook Web Access (OWA), outbound communication to known APT35 infrastructure, and a successful remote interactive logon from a suspicious external IP address. The evidence strongly indicates successful email collection and likely data exfiltration activity originating from the compromised host.

At the time of investigation, no evidence of lateral movement or compromise of additional internal endpoints was identified. The attack appears limited to a single compromised internal endpoint.

---

# Investigation Objectives

- Determine whether the alert represented malicious activity
- Identify affected systems and users
- Validate indicators of compromise (IOCs)
- Analyze attacker behavior and execution methods
- Determine whether data exfiltration occurred
- Assess the scope of compromise
- Recommend containment and remediation actions

---

# Tools & Data Sources

- SIEM Platform
- Windows Event Logs
- Mailbox Audit Logs
- Firewall Logs
- EDR Telemetry
- Threat Intelligence Platforms
- VirusTotal
- Google Threat Analysis Group (TAG)

---

# Indicators of Compromise (IOCs)

| Type | Value |
|---|---|
| Internal IP Address | 172.16.17.72 |
| External IP Address | 136.243.108.14 |
| External IP Address | 173.209.51.54 |
| Hostname | Arthur |
| Username | Arthur |
| Mailbox User | arthur@letsdefend.io |
| Process Name | EmailDownloader.exe |
| File Path | C:\Users\LetsDefend\Downloads\EmailDownloader.exe |
| SHA-256 Hash | cd2ba296828660ecd07a36e8931b851dda0802069ed926b3161745aae9aa6daa |

---

# Investigation Process

## 1. Initial Alert Review

The investigation began after the SIEM generated an alert indicating suspicious execution of `EmailDownloader.exe` on the host `Arthur`.

### Alert Details

| Field | Value |
|---|---|
| Hostname | Arthur |
| IP Address | 172.16.17.72 |
| Process Name | EmailDownloader.exe |
| Parent Process | C:\Windows\Explorer.EXE |
| Command Line | C:\Users\LetsDefend\Downloads\EmailDownloader.exe |
| Device Action | Allowed |

Initial observations identified several suspicious indicators:
- The executable was launched from the user's Downloads directory
- The process name implied email collection capability
- The executable hash matched known malware indicators associated with APT35
- The process was allowed to execute successfully

The host IP address (`172.16.17.72`) belongs to a private RFC1918 internal address range, confirming the malicious activity originated from an internal endpoint.

---

## 2. Process Execution Analysis

Process telemetry confirmed the malicious executable was launched through Windows Explorer.

```text
Process Name: EmailDownloader.exe
Parent Process: C:\Windows\Explorer.EXE
Command Line:
C:\Users\LetsDefend\Downloads\EmailDownloader.exe
```

### Findings

- The executable was manually launched from the Downloads directory
- Downloads directories are commonly abused for malware staging
- No legitimate software associated with the executable was identified
- The executable naming convention suggested mailbox extraction functionality

---

## 3. Mailbox Audit Log Analysis

Mailbox audit logs confirmed successful mailbox download activity associated with the compromised user account.

```text
Event MailboxGuid: 6d4fbdae-e3ae-4530-8d0b-f62a14687939

Owner: EC2AMAZ-ILGVOIN\Arthur
LastAccessed: 2023-12-27T11:21:48.140625-00:00
Operation: Download
OperationResult: Succeeded
LogonType: User
FolderPathName: \Mails\Inbox
ClientInfoString: Client:OWA;Action:ViaProxy
ClientIPAddress: 172.16.17.72
InternalLogonType: Owner
MailboxOwnerUPN: arthur@letsdefend.io
Subject: Notification of Multiple Mail Download
```

### Findings

- Successful email download activity occurred
- Access originated from the compromised internal endpoint
- Outlook Web Access (OWA) was used to access mailbox contents
- Multiple mailbox download operations were observed
- The Inbox folder was specifically targeted

The behavior strongly aligns with documented HYPERSCRAPE functionality involving automated mailbox collection and extraction.

---

## 4. Network Traffic Analysis

Firewall telemetry identified outbound communication initiated by `EmailDownloader.exe` to known APT35 infrastructure.

```text
Source IP: 172.16.17.72

Destination IP: 136.243.108.14
Destination Port: 80
Source Process: EmailDownloader.exe
Firewall Action: SUCCESS
```

### Findings

- The malicious process successfully established outbound communication
- The destination IP is associated with known APT35 infrastructure
- Communication occurred over HTTP (Port 80)
- The firewall permitted the outbound connection

This activity is consistent with:
- Command-and-control (C2) communication
- Exfiltration traffic
- Malware beaconing behavior

---

## 5. Authentication Log Analysis

Windows authentication logs revealed successful remote interactive access to the endpoint.

```text
Username: Arthur

EventID: 4624
Logon Type: 10
Source IP: 173.209.51.54
```

### Findings

- Event ID 4624 confirmed successful authentication
- Logon Type 10 indicates RemoteInteractive access (RDP/Terminal Services)
- Source IP matched known suspicious infrastructure associated with APT35

This suggests the attacker established remote access to the endpoint prior to or during malware execution.

---

## 6. Threat Intelligence Validation

Threat intelligence enrichment confirmed the SHA-256 hash matched known HYPERSCRAPE malware associated with APT35 (Charming Kitten).

### Threat Intelligence Reference

Google Threat Analysis Group (TAG):
https://blog.google/threat-analysis-group/new-iranian-apt-data-extraction-tool/

### Related Known HYPERSCRAPE Hashes

```text
03d0e7ad4c12273a42e4c95d854408b98b0cf5ecf5f8c5ce05b24729b6f4e369
35a485972282b7e0e8e3a7a9cbf86ad93856378fd96cc8e230be5099c4b89208
5afc59cd2b39f988733eba427c8cf6e48bd2e9dc3d48a4db550655efe0dca798
6dc0600de00ba6574488472d5c48aa2a7b23a74ff1378d8aee6a93ea0ee7364f
767bd025c8e7d36f64dbd636ce0f29e873d1e3ca415d5ad49053a68918fe89f4
977f0053690684eb509da27d5eec2a560311c084a4a133191ef387e110e8b85f
ac8e59e8abeacf0885b451833726be3e8e2d9c88d21f27b16ebe00f00c1409e6
cd2ba296828660ecd07a36e8931b851dda0802069ed926b3161745aae9aa6daa
```

### Known HYPERSCRAPE Capabilities

- Automated mailbox extraction
- Outlook Web Access abuse
- Email collection and downloading
- `.eml` file extraction
- Potential deletion of security notification emails
- Credential and session abuse

---

## 7. Scope Assessment

The investigation included review of:
- Internal network connections
- Authentication events
- Endpoint telemetry
- Firewall activity
- Process execution logs

No evidence was identified for:
- SMB lateral movement
- PsExec execution
- WMI execution
- Internal scanning activity
- Additional compromised internal systems

### Scope Conclusion

The incident currently appears limited to:
- One compromised internal endpoint:
  - `Arthur (172.16.17.72)`

External IP addresses identified were associated with attacker infrastructure rather than additional victim systems.

---

# MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---|---|---|
| Reconnaissance | Gather Victim Host Information | T1592 |
| Initial Access | Valid Accounts | T1078 |
| Collection | Email Collection | T1114 |
| Command and Control | Application Layer Protocol | T1071 |
| Exfiltration | Exfiltration Over C2 Channel | T1041 |

---

# Findings

- `EmailDownloader.exe` was executed on the endpoint `Arthur`
- The executable hash matched known APT35 HYPERSCRAPE malware
- Successful mailbox download activity was confirmed through OWA
- Outbound communication to known APT35 infrastructure succeeded
- Remote interactive access was identified from suspicious infrastructure
- Evidence strongly supports email collection and likely data exfiltration
- No evidence of lateral movement or additional compromised endpoints was identified

---

# Verdict

## True Positive

The investigation confirmed malicious activity associated with APT35 (Charming Kitten) and the HYPERSCRAPE email exfiltration malware.

### Justification

- The SHA-256 hash matched publicly documented HYPERSCRAPE malware
- The host communicated with known APT35 infrastructure
- Successful mailbox download operations were confirmed
- Remote interactive access from suspicious infrastructure was identified
- Malware behavior aligned with documented APT35 tactics, techniques, and procedures (TTPs)

---

# Remediation Recommendations

## Immediate Containment

- Isolate the endpoint `Arthur`
- Block the following malicious IP addresses:
  - 136.243.108.14
  - 173.209.51.54
- Block known malicious file hashes organization-wide
- Terminate active malicious sessions

## Credential Security

- Reset credentials associated with Arthur
- Revoke active OWA/O365 sessions and authentication tokens
- Enforce MFA validation for affected accounts

## Threat Hunting

Conduct organization-wide hunting for:
- `EmailDownloader.exe`
- Known HYPERSCRAPE hashes
- Event ID 4624 Logon Type 10
- Unusual OWA mailbox download activity
- Suspicious outbound HTTP traffic
- Similar downloads directory executions

## Security Improvements

- Improve monitoring for mailbox download events
- Enhance endpoint monitoring for suspicious executable launches
- Increase detection coverage for APT35 indicators
- Review firewall policies for suspicious outbound traffic
- Improve alerting for remote interactive logons from external infrastructure

---

# Key Takeaways

- Threat intelligence enrichment was critical in confirming APT35 attribution
- Mailbox audit logs provided strong evidence of successful email collection
- Monitoring outbound traffic enabled rapid identification of attacker infrastructure
- Downloads directory execution remains a high-risk malware vector
- Remote interactive logons from suspicious infrastructure should be prioritized for investigation
- Early containment likely prevented broader organizational compromise

---
