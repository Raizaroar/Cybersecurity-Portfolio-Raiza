# PROJECT OVERVIEW

In this lab, you will analyze real Windows Security logs to detect unauthorized access attempts. This is exactly the daily work of a Level 1 SOC Analyst: reviewing security events, identifying suspicious patterns, and documenting findings.

**Real-world scenario:** You are a SOC Analyst at a company. The alert system has detected multiple 4625 events (Failed Logon) on a critical server. Your job is to investigate whether this is:

   - A brute force attack in progress
   - A legitimate user with an incorrect password
   - Normal system activity

## TOOLS USED

| **Tool**              | **Purpose**                 | **Why This Tool?**                                      |
|-----------------------|-----------------------------|----------------------------------------------------------|
| PowerShell            | Log analysis and filtering  | Native to Windows, commonly used in enterprise SOCs      |
| Windows Event Viewer  | Visual log inspection       | Standard GUI for quick analysis                          |
| Excel/CSV             | Data correlation            | Pattern analysis across large data volumes               |
| MITRE ATT&CK          | Threat mapping              | Industry-standard framework                              |


## OBJECTIVE

   - Extract and analyze Windows Security Event Logs
   - Identify events 4625 (Failed Logon)
   - Detect brute force attack patterns
   - Correlate events by user, IP, and time
   - Document findings in professional SOC format
   - Map to MITRE ATT&CK Technique T1110 (Brute Force)

## LAB ENVIRONMENT

   - System: Windows 10/11 or Windows Server 
   - PowerShell: Version 5.1+ 
Tools: Event Viewer 
Sample Data: Security.evtx 
Analysis Location: 04-SIEM-Projects/Lab-1.1.2-Windows-Event-Analysis/


## PREREQUISITES

   - Lab 1.1.1 completed
   - PowerShell installed 
   - Administrator permissions 
   - 2-3 hours of dedicated time
   - Investigative mindset

## STEP BY STEP - LABORATORY 1.1.2

**STEP 1.1.2.1: Verify PowerShell and Permissions**

- Open PowerShell as Administrator Windows 10/11:

