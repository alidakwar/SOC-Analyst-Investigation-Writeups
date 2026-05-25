# Golden Ticket — Active Directory Attack Investigation

## Overview

This investigation focused on analyzing suspicious authentication activity within a Windows Active Directory environment after alerts indicated possible lateral movement toward the Domain Controller (DC).

The objective was to:

- Trace attacker behavior through Windows Security Event Logs
- Identify compromised accounts
- Investigate Kerberos-related abuse
- Determine whether a Golden Ticket attack had been attempted

The investigation was performed using:

- Windows Event Viewer
- Analysis of `security.evtx` logs
- The LetsDefend SOC training environment

---

## Scenario Summary

Security monitoring systems detected suspicious authentication attempts and lateral movement activity originating from compromised internal workstations.

Indicators suggested the attacker was attempting to:

1. Escalate privileges within the domain
2. Move laterally across systems
3. Target the Domain Controller
4. Abuse Kerberos authentication mechanisms

During the investigation, evidence of the following activity was identified:

- AS-REP Roasting
- Service account compromise
- Kerberos ticket abuse
- Potential Golden Ticket authentication attempts

---

## Investigation Objectives

- Identify the compromised service account
- Determine the attacker source IP address
- Detect evidence of AS-REP Roasting
- Identify targeted Kerberos accounts
- Investigate suspicious Kerberos ticket activity
- Confirm attempted Golden Ticket authentication
- Establish a timeline of attacker actions

---

## Tools & Technologies Used

| Tool / Technology | Purpose |
|---|---|
| Windows Event Viewer | Log analysis |
| Windows Security Event Logs (`security.evtx`) | Authentication investigation |
| Kerberos Authentication Analysis | Ticket activity analysis |
| Active Directory Security Monitoring | Domain activity tracking |
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
```

### Associated Source IP

```text
192.168.110.129
```

This activity strongly suggested compromise of the SQL service account and subsequent lateral movement within the environment.

### Indicators Observed

- Logon Type: `3` (Network Logon)
- Authentication Package: `NTLM`
- Repeated authentication activity from the same host

---

## 3. Detecting AS-REP Roasting Activity

Further investigation into Kerberos Ticket Granting Ticket (TGT) requests using Event ID `4768` revealed evidence of AS-REP Roasting.

### Indicators Identified

- `PreAuthType: 0`
- `ServiceName: krbtgt`
- Suspicious TGT requests originating from the attacker IP

### Targeted Account

```text
Corrado
```

### Suspicious Encryption Type

```text
0x17
```

This encryption type is commonly associated with RC4-based Kerberos roasting attacks.

The activity indicated the attacker attempted to retrieve crackable Kerberos authentication material for offline password attacks.

---

## 4. Golden Ticket Investigation

Following compromise of the service account, additional Kerberos activity was identified involving the `krbtgt` service and privileged domain authentication attempts.

### Suspicious Event Activity

Event ID `4769` activity involving:

```text
Administrator@SOPRANOS.local
```

suggested the attacker attempted to forge Kerberos authentication tickets consistent with Golden Ticket abuse techniques.

Subsequent Event ID `4624` logon activity confirmed Kerberos-based authentication attempts involving the privileged domain account:

```text
Administrator
```

### Indicators of Golden Ticket Abuse

- Forged Kerberos ticket activity
- Privileged authentication attempts
- `krbtgt`-related anomalies
- Domain persistence behavior

This behavior was consistent with post-compromise privilege escalation and domain persistence attempts.

---

# Key Findings

| Finding | Details |
|---|---|
| Compromised Service Account | `SQLService` |
| Attacker Source IP | `192.168.110.129` |
| AS-REP Roasting Target | `Corrado` |
| Privileged Account Targeted | `Administrator` |
| Attack Technique | Kerberos Ticket Abuse / Golden Ticket Attempt |

---

# MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---|---|---|
| Credential Access | Steal or Forge Kerberos Tickets | `T1558` |
| Credential Access | AS-REP Roasting | `T1558.004` |
| Lateral Movement | Remote Services | `T1021` |
| Privilege Escalation | Valid Accounts | `T1078` |
| Defense Evasion | Golden Ticket | `T1558.001` |

---

# Lessons Learned

This investigation demonstrated how Kerberos authentication logs can reveal multiple stages of attacker activity within Active Directory environments.

## Key Takeaways

- Monitor Event IDs `4768`, `4769`, and `4624`
- Detect AS-REP Roasting through `PreAuthType: 0`
- Investigate suspicious Kerberos encryption types such as `0x17`
- Correlate lateral movement with privileged authentication activity
- Monitor `krbtgt` activity for Golden Ticket indicators

---

# Conclusion

The investigation successfully identified a multi-stage Active Directory attack involving:

- Kerberos abuse
- Service account compromise
- Lateral movement
- Attempted Golden Ticket authentication

Through structured log analysis and event correlation, the attacker’s progression from initial Kerberos abuse to privileged domain authentication was reconstructed successfully.

This investigation demonstrates the importance of:

- Centralized logging
- Kerberos monitoring
- Active Directory threat detection
- Event correlation within SOC operations
