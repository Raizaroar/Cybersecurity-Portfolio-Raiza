# Lab 2.1.2: Advanced Splunk - Detection Engineering & Threat Hunting

**Date:** 28-01-2026
**Analyst:** Raiza  
**Status:** Completed  
**Category:** SIEM Operations / Threat Hunting  
**Difficulty:** Advanced  
**Time Invested:** 5


## Executive Summary

Conducted proactive threat hunt for macro-based malware delivery via Office documents. Hunt focused on detecting unusual parent-child process relationships where Microsoft Office applications spawn scripting interpreters—a key indicator of malicious macro execution.

# TOOLS USED

| Tool                      | Purpose                     | Why This Tool?                   |
|---------------------------|-----------------------------|----------------------------------|
| Splunk Enterprise         | SIEM platform               | From yesterday's setup           |
| Advanced SPL              | Complex queries             | Correlation, regex, eval         |
| Sigma Rules               | Universal detection format  | Convert to SPL                   |
| MITRE ATT&CK Navigator    | Coverage visualization      | Map detection gaps               |
| Threat Hunting Playbooks | Structured hunting          | Hypothesis-driven                |

## OBJECTIVE

- Master advanced SPL (regex, eval, macros, subsearches)
- Create multi-log correlation detections
- Implement Sigma rules in Splunk
- Conduct hypothesis-driven threat hunt
- Build threat hunting dashboard
- Document hunt findings professionally
- Create detection engineering playbook


**Key Findings:**

- 3 instances of Word spawning PowerShell
- 1 confirmed malicious macro detected
- 2 false positives (legitimate admin scripts)

## Step-by-Step Implementation

1. Step: Advanced SPL Techniques
2. Step: Multi-Log Correlation Detections
3. Step: Sigma Rules Integration
4. Step: Threat Hunting Methodology
5. Step: Build Threat Hunting Dashboard
6. Step:  Document Threat Hunt
