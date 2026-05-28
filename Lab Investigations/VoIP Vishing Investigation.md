# VoIP Vishing Investigation

## Overview

This investigation focused on analyzing suspicious VoIP traffic after a user named James received a suspicious phone call from an individual impersonating a bank representative in an apparent vishing (voice phishing) attack.

The objective was to:

- Identify the source of the suspicious call
- Analyze VoIP signaling and RTP traffic
- Reconstruct the phone conversation timeline
- Identify the spoofed bank identity
- Extract attacker-provided caller information
- Recover sensitive information disclosed during the call

The investigation was performed using:

- Wireshark
- RTP Stream Analysis
- SIP Protocol Inspection
- VoIP Call Reconstruction
- Packet Capture Analysis

---

## Scenario Summary

A suspicious phone call was received by James from a caller claiming to represent a bank. During the conversation, the caller attempted to socially engineer James into disclosing sensitive personal information.

The investigation involved analyzing SIP signaling traffic and RTP audio streams contained within a packet capture file to reconstruct the call and identify attacker activity.

### Observed Suspicious Activity

- Suspicious inbound VoIP call
- Caller impersonating a financial institution
- Social engineering attempt targeting sensitive information
- RTP voice traffic containing disclosed personal data
- SIP signaling identifying spoofed caller information

---

## Investigation Objectives

- Identify the source phone number of the caller
- Determine the targeted recipient number
- Analyze SIP and RTP traffic
- Recover VoIP audio streams
- Identify the spoofed bank identity
- Reconstruct the attack timeline
- Extract sensitive information disclosed during the call

---

## Tools & Technologies Used

| Tool / Technology | Purpose |
|---|---|
| Wireshark | Packet and protocol analysis |
| SIP Analysis | VoIP signaling inspection |
| RTP Stream Analysis | Voice traffic reconstruction |
| RTP Player | Audio playback and analysis |
| PCAP Analysis | Network traffic investigation |
| MITRE ATT&CK Framework | Attack classification |

---

# Investigation Process

## 1. Initial Investigation

The investigation began after reviewing a packet capture file containing suspicious VoIP traffic associated with a potential vishing attack.

Initial analysis focused on identifying VoIP-related protocols and reconstructing the phone conversation between the attacker and the victim.

Using Wireshark’s VoIP analysis features, SIP signaling traffic and RTP streams were identified and analyzed.

### Initial Indicators

| Indicator | Details |
|---|---|
| Protocol | SIP / RTP |
| Victim Extension | 7001 |
| Suspicious Caller | 01326947697 |
| Attack Type | Vishing (Voice Phishing) |
| Spoofed Identity | Bank of Wealth |

---

## 2. Log & Artifact Analysis

SIP signaling traffic was analyzed to identify call metadata, including caller identifiers, timestamps, call duration, and VoIP session details.

RTP streams were reconstructed and played back using Wireshark’s RTP Player functionality to recover the full audio conversation.

### Important Evidence

| Evidence | Description |
|---|---|
| SIP INVITE | Initial call setup request |
| RTP Streams | Audio communication traffic |
| Caller ID | Spoofed bank phone number |
| RTP Audio Playback | Recovery of spoken conversation |
| VoIP Session Metadata | Call timing and duration |

### Key Findings

- The attacker initiated a SIP-based VoIP call impersonating a bank representative.
- James’s extension number was identified as `7001`.
- The spoofed caller number was identified as `01326947697`.
- The call duration was approximately `00:01:35`.
- RTP stream reconstruction successfully recovered the phone conversation audio.
- The attacker impersonated a financial institution identified as `Bank of Wealth`.
- Sensitive information, including James’s social number (`5678`), was disclosed during the conversation.

---

## 3. Threat Intelligence & IOC Analysis

Analysis focused on identifying attacker-controlled phone numbers and indicators associated with the vishing attempt.

### Indicators of Compromise (IOCs)

| Type | Value |
|---|---|
| Phone Number | 01326947697 |
| Victim Extension | 7001 |
| Protocol | SIP |
| RTP Packet Count | 18348 |
| Spoofed Organization | Bank of Wealth |

---

## 4. Attack Analysis

The attacker leveraged VoIP infrastructure to perform a social engineering attack against the victim.

Using SIP signaling and RTP audio streams, the attacker impersonated a legitimate financial institution and attempted to gain trust in order to collect sensitive information.

### Indicators Observed

- Social engineering through voice impersonation
- VoIP-based phishing (vishing)
- SIP call establishment activity
- RTP audio stream transmission
- Disclosure of sensitive information during live conversation

---

# Key Findings

| Finding | Details |
|---|---|
| Attack Type | Vishing |
| Victim Extension | 7001 |
| Spoofed Caller Number | 01326947697 |
| Impersonated Organization | Bank of Wealth |
| RTP Packet Count | 18348 |
| Call Duration | 00:01:35 |
| Sensitive Information Exposed | Social Number: 5678 |

---

# MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---|---|---|
| Initial Access | Phishing: Voice Phishing | `T1566.004` |
| Credential Access | Steal or Forge Authentication Information | `T1649` |
| Collection | Audio Capture | `T1123` |
| Reconnaissance | Gather Victim Identity Information | `T1589` |
| Social Engineering | Trusted Relationship Exploitation | `T1199` |

---

# Lessons Learned

This investigation demonstrated how VoIP traffic analysis and RTP stream reconstruction can be used to identify and investigate vishing attacks targeting users through social engineering techniques.

## Key Takeaways

- Monitor suspicious VoIP activity and SIP signaling traffic
- Analyze RTP streams during voice-based investigations
- Validate suspicious caller identities carefully
- Train users to recognize social engineering attempts
- Protect sensitive personal information during phone interactions
- Use packet analysis tools to reconstruct attacker activity

---

# Conclusion

The investigation successfully identified and analyzed a vishing attack involving a spoofed bank representative attempting to socially engineer sensitive information from the victim.

Through SIP signaling analysis and RTP audio reconstruction, the attacker’s phone number, impersonated organization, and call details were successfully identified.

The investigation highlighted the effectiveness of packet capture analysis and VoIP traffic reconstruction for investigating voice phishing incidents and social engineering attacks.

This investigation emphasizes the importance of:

- VoIP traffic monitoring
- user awareness training
- SIP and RTP analysis
- proactive threat detection
- social engineering defense strategies
- incident response and forensic analysis

---
```
