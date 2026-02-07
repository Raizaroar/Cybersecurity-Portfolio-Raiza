# Lab 2.1.2: Advanced Splunk - Detection Engineering & Threat Hunting

**Date:** 28-01-2026
**Analyst:** Raiza  
**Status:** Completed  
**Category:** SIEM Operations / Threat Hunting  
**Difficulty:** Advanced  
**Time Invested:** 5


## Executive Summary

Conducted proactive threat hunt for macro-based malware delivery via Office documents. Hunt focused on detecting unusual parent-child process relationships where Microsoft Office applications spawn scripting interpreters—a key indicator of malicious macro execution.

## OBJECTIVE

- Master advanced SPL (regex, eval, macros, subsearches)
- Create multi-log correlation detections
- Implement Sigma rules in Splunk
- Conduct hypothesis-driven threat hunt
- Build threat hunting dashboard
- Document hunt findings professionally
- Create detection engineering playbook


# TOOLS USED

| Tool                      | Purpose                     | Why This Tool?                   |
|---------------------------|-----------------------------|----------------------------------|
| Splunk Enterprise         | SIEM platform               | From yesterday's setup           |
| Advanced SPL              | Complex queries             | Correlation, regex, eval         |
| Sigma Rules               | Universal detection format  | Convert to SPL                   |
| MITRE ATT&CK Navigator    | Coverage visualization      | Map detection gaps               |
| Threat Hunting Playbooks | Structured hunting          | Hypothesis-driven                |

## Step-by-Step Implementation

1. Step: Advanced SPL Techniques
2. Step: Multi-Log Correlation Detections
3. Step: Sigma Rules Integration
4. Step: Threat Hunting Methodology
5. Step: Build Threat Hunting Dashboard
6. Step:  Document Threat Hunt

**Key Findings:**

- 3 instances of Word spawning PowerShell
- 1 confirmed malicious macro detected
- 2 false positives (legitimate admin scripts)

## Skills Demonstrated:

-  Advanced SIEM operations
-  Detection engineering
-  Threat hunting methodology
-  Sigma rule implementation
-  Professional hunt documentation

**Key Learnings:**
- Mastered 5 advanced SPL techniques (regex, eval, macros, transactions, subsearches)
- Created 3 multi-log correlation detections
- Implemented 2 Sigma rules with SPL conversion
- Conducted hypothesis-driven threat hunt (5 hypotheses)
- Detected 1 real malicious macro attack
- Built threat hunting dashboard (5 panels)
- Created detection engineering playbook

**Advanced SPL:**
- regex (pattern extraction)
- eval (field calculations, case statements)
- macros (reusable search fragments)
- transaction (event grouping)
- advanced subsearches and joins

**Correlations Created:**
1. Phishing → Malware → C2 (3-stage)
2. Brute Force → Success → Privilege Escalation
3. Port Scan → Exploitation → Exfiltration

**Threat Hunt:**
- Hypothesis: Office macros spawning PowerShell
- Finding: 1 malicious, 2 false positives
- Detection rule created and deployed