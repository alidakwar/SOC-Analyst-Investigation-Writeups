Golden Ticket — Active Directory Attack Investigation
Overview

This investigation focused on analyzing suspicious authentication activity within a Windows Active Directory environment after alerts indicated possible lateral movement toward the Domain Controller (DC). The objective was to trace attacker behavior through Windows Security Event Logs, identify compromised accounts, investigate Kerberos-related abuse, and determine whether a Golden Ticket attack had been attempted.

The investigation was performed using Windows Event Viewer and analysis of security.evtx logs provided within the LetsDefend SOC training environment.

Scenario Summary

Security monitoring systems detected suspicious authentication attempts and lateral movement activity originating from compromised internal workstations. Indicators suggested the attacker was attempting to escalate privileges within the domain and ultimately target the Domain Controller.

During the investigation, evidence of:

AS-REP Roasting activity
Service account compromise
Kerberos ticket abuse
Potential Golden Ticket authentication attempts

was identified through analysis of Windows Security logs.

Investigation Objectives
Identify the compromised service account
Determine the attacker source IP address
Detect evidence of AS-REP Roasting
Identify targeted Kerberos accounts
Investigate suspicious Kerberos ticket activity
Confirm attempted Golden Ticket authentication
Establish a timeline of attacker actions
Tools & Technologies Used
Windows Event Viewer
Windows Security Event Logs (security.evtx)
Kerberos Authentication Analysis
Active Directory Security Monitoring
MITRE ATT&CK Framework
Investigation Process
1. Security Log Review

The investigation began by loading the provided security.evtx file into Windows Event Viewer for analysis of Windows Security Events.

Primary event IDs investigated included:

Event ID	Description
4624	Successful Logon
4768	Kerberos TGT Request
4769	Kerberos Service Ticket Request
4672	Special Privileges Assigned
2. Identifying the Compromised Service Account

The investigation focused on Kerberos authentication activity and successful network logons originating from suspicious internal IP addresses.

Analysis of Event ID 4624 revealed successful NTLM-based network authentication attempts involving the account:

SQLService

The associated source IP address was identified as:

192.168.110.129

This activity strongly suggested compromise of the SQL service account and subsequent lateral movement within the environment.

Relevant indicators included:

Logon Type: 3 (Network Logon)
Authentication Package: NTLM
Repeated authentication activity from the same host
3. Detecting AS-REP Roasting Activity

Further investigation into Kerberos Ticket Granting Ticket (TGT) requests using Event ID 4768 revealed evidence of AS-REP Roasting.

The following indicators were identified:

PreAuthType: 0
ServiceName: krbtgt
Suspicious TGT requests originating from the attacker IP

The targeted account was identified as:

Corrado

Additional analysis identified Kerberos ticket activity using encryption type:

0x17

which is commonly associated with RC4-based Kerberos roasting attacks.

This activity indicated the attacker attempted to retrieve crackable Kerberos authentication material for offline password attacks.

4. Golden Ticket Investigation

Following compromise of the service account, additional Kerberos activity was identified involving the krbtgt service and privileged domain authentication attempts.

Suspicious Event ID 4769 activity involving:

Administrator@SOPRANOS.local

suggested the attacker attempted to forge Kerberos authentication tickets consistent with Golden Ticket abuse techniques.

Subsequent Event ID 4624 logon activity confirmed Kerberos-based authentication attempts involving the privileged domain account:

Administrator

This behavior was consistent with post-compromise privilege escalation and domain persistence attempts.

Key Findings
Finding	Details
Compromised Service Account	SQLService
Attacker Source IP	192.168.110.129
AS-REP Roasting Target	Corrado
Privileged Account Targeted	Administrator
Attack Technique	Kerberos Ticket Abuse / Golden Ticket Attempt
MITRE ATT&CK Mapping
Tactic	Technique	ID
Credential Access	Steal or Forge Kerberos Tickets	T1558
Credential Access	AS-REP Roasting	T1558.004
Lateral Movement	Remote Services	T1021
Privilege Escalation	Valid Accounts	T1078
Defense Evasion	Golden Ticket	T1558.001
Lessons Learned

This investigation demonstrated how Kerberos authentication logs can reveal multiple stages of attacker activity within Active Directory environments.

Key takeaways included:

The importance of monitoring Event IDs 4768, 4769, and 4624
Detection opportunities for AS-REP Roasting through PreAuthType: 0
Identification of suspicious Kerberos encryption types such as 0x17
Correlation of lateral movement and privileged authentication activity
Recognition of Golden Ticket indicators involving krbtgt and privileged domain accounts
Conclusion

The investigation successfully identified a multi-stage Active Directory attack involving Kerberos abuse, service account compromise, lateral movement, and attempted Golden Ticket authentication.

Through structured log analysis and event correlation, the attacker’s progression from initial Kerberos abuse to privileged domain authentication was reconstructed, demonstrating the importance of centralized logging, Kerberos monitoring, and Active Directory threat detection within SOC operations.
