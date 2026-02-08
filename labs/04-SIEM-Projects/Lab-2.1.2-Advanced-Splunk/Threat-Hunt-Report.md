# Threat Hunt Report: Macro Malware Detection

**Hunt ID:** HUNT-2025-001  
**Date:** 07-02-2026  
**Hunter:** Raiza  
**Status:** Completed  
**Duration:** 2 hours  

---

## Executive Summary

Conducted proactive threat hunt for macro-based malware delivery via Office documents. Hunt focused on detecting unusual parent-child process relationships where Microsoft Office applications spawn scripting interpreters—a key indicator of malicious macro execution.

**Key Findings:**
- 3 instances of Word spawning PowerShell
- 1 confirmed malicious macro detected
- 2 false positives (legitimate admin scripts)

---

## Hypothesis

**Statement:** "Attackers are using malicious Office macros to deliver malware. Office applications should rarely spawn PowerShell or cmd.exe under normal usage."

**MITRE ATT&CK Reference:** T1566.001 (Spearphishing Attachment), T1059.001 (PowerShell)

**Risk Assessment:** HIGH - Office macros are common initial access vector

---

## Data Sources

- `index="windows_security"` - Event ID 4688 (Process Creation)
- Time Range: Last 7 days
- Total Events Analyzed: 15,234

---

## Hunt Execution

### Query 1: Office → Scripting Engine Relationships

```spl
index="windows_security" EventCode=4688
| eval parent=lower(ParentProcessName)
| eval child=lower(NewProcessName)
| search parent IN ("*winword.exe", "*excel.exe", "*powerpnt.exe")
      AND child IN ("*powershell.exe", "*cmd.exe", "*wscript.exe")
| stats count by _time, parent, child, User, ComputerName, CommandLine
```

**Results:** 3 events

---

### Finding 1: Suspicious Word → PowerShell

```
Time: 2025-01-28 14:32:15
User: jsmith
Host: WORKSTATION-01
Parent: C:\Program Files\Microsoft Office\WINWORD.EXE
Child: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
CommandLine: powershell.exe -WindowStyle Hidden -enc JABzAD0ATgBlAHcALQBPAGIAagBlAGMAdAAgAEkATwAuAE0AZQBtAG8AcgB5AFMAdAByAGUAYQBtACgALABbAEMAbwBuAHYAZQByAHQAXQA6ADoARgByAG8AbQBCAGEAcwBlADYANABTAHQAcgBpAG4AZwAoACIASAA0AHMASQBBAEEAQQBBAEEAQQBBAEEAQQBLADEAVQB5AEIAegBRAGcAQQBnAGQARwBoAGwAIABiAEcARgA2AGUAUwBCAGsAYgAyAGMAZwBxAGEAdQBtAHAAcwBpAEEAZwBvAHYAZQByACAAdABoAGUAKwBmAGUAbgBjAGUAIgApACkAOwAkAHMALgBXAHIAaQB0AGUAKAAkAHMALgBUAG8AQQByAHIAYQB5ACgAKQAsADAALAAkAHMALgBMAGUAbgBnAHQAaAApAA==
```

**Analysis:**
- Encoded PowerShell command (`-enc`)
- WindowStyle Hidden (stealth)
- Decoded content: Downloads and executes payload

**Verdict:**  MALICIOUS

**IOCs:**
- User: jsmith
- Host: WORKSTATION-01
- Process: Encoded PowerShell from Word

---

### Finding 2: Excel → CMD (False Positive)

```
Time: 2025-01-27 09:15:00
User: admin
Host: IT-ADMIN-01
Parent: C:\Program Files\Microsoft Office\EXCEL.EXE
Child: C:\Windows\System32\cmd.exe
CommandLine: cmd.exe /c "echo Backup complete"
```

**Analysis:**
- Admin user on IT system
- Simple echo command (no download/execution)
- Correlates with scheduled Excel macro for backups

**Verdict:** BENIGN (Documented admin script)

---

### Finding 3: Word → PowerShell (False Positive)

```
Time: 2025-01-26 16:45:00
User: developer
Host: DEV-WORKSTATION
Parent: WINWORD.EXE
Child: powershell.exe
CommandLine: powershell.exe Get-Date
```

**Analysis:**
- Developer account
- Simple Get-Date command
- Known legitimate use case (Word plugin)

**Verdict:** BENIGN (Documented developer tool)

---

## Confirmed Threats

### Threat 1: Malicious Macro Execution

**Summary:** User jsmith executed Word document with malicious macro that downloaded and executed PowerShell payload.

**Timeline:**
1. 14:30:00 - User opens email attachment (email logs)
2. 14:32:15 - Word spawns encoded PowerShell
3. 14:32:20 - Outbound connection to 185.220.101.45:443 (firewall logs)
4. 14:35:00 - Additional malware downloaded

**Impact:** Single workstation compromised, C2 established

**Status:** Contained (host isolated, malware removed)

---

## Detection Rule Created

**Rule Name:** Office Macro - Suspicious Child Process

```spl
index="windows_security" EventCode=4688
| eval parent=lower(ParentProcessName)
| eval child=lower(NewProcessName)
| search parent IN ("*winword.exe", "*excel.exe", "*powerpnt.exe")
      AND child IN ("*powershell.exe", "*cmd.exe", "*wscript.exe")
| search CommandLine="*-enc*" OR CommandLine="*IEX*" OR CommandLine="*DownloadString*"
| eval severity="HIGH"
| table _time, User, ComputerName, parent, child, CommandLine, severity
```

**Alert Configuration:**

- Trigger: Any result
- Action: Email SOC, create ticket
- Schedule: Real-time

**Expected False Positive Rate:** <5% (filtered for encoding/download keywords)

---

## Recommendations

### Immediate Actions
- [x] Isolate WORKSTATION-01 (completed)
- [x] Reset jsmith credentials (completed)
- [ ] Scan all hosts for same malware hash
- [ ] Block macro-enabled documents at email gateway

### Short-Term
- [ ] Deploy new detection rule to production SIEM
- [ ] User awareness training (macro risks)
- [ ] Enable ASR rules (Attack Surface Reduction) on all endpoints

### Long-Term
- [ ] Disable macros by default (Group Policy)
- [ ] Implement application whitelisting
- [ ] Regular macro-based phishing simulations

---

## Lessons Learned

**What Worked:**
- Hypothesis correctly identified attack vector
- Parent-child relationship analysis effective
- Encoded PowerShell stood out clearly

**What Could Improve:**
- Need more context on legitimate uses
- Whitelist known admin scripts to reduce false positives
- Correlate with email logs earlier in hunt

**Hunt Efficiency:**
- Time to Finding: 15 minutes
- False Positive Rate: 67% (2 of 3)
- Confirmed Threats: 1

---

**Status:** Hunt successful, threat neutralized

![Dashboard_Lab2.1.2](/assets/screenshots/04-SIEM-Projects/Lab-2.1.2-Advanced-Splunk/Lab-2.1.2-AdvancedSplunk13.png)