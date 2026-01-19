#  SOC Analyst 30-Day Intensive Training Journey

- **Start Date:** 16-01-2026 

- **Target Completion:** pending  

- **Current Status:** Day 2 -  Windows Security Event Log Analysis

---

##  Progress Overview

| Week | Focus Area | Labs Completed | Status |
|------|-----------|----------------|--------|
| Week 1 | Fundamentals & Reconnaissance | 2/7 | 🔄 In Progress |
| Week 2 | Threat Detection | 0/7 | ⏳ Pending |
| Week 3 | Incident Response | 0/7 | ⏳ Pending |
| Week 4 | Advanced Cases & Portfolio | 0/9 | ⏳ Pending |

**Overall Progress:** 2/30 Labs (0.6%)

---

##  Daily Log

### Day 1 - 16-01-2026
**Lab:** 1.1.1 - Environment Setup  
**Status:** Completed  
**Time Invested:** 2 hours  
**Key Learnings:**
- Integrated training plan with existing portfolio structure
- Configured professional Git workflow
- Created progress tracking system

**Challenges:**
- None

**Portfolio Updates:**
- Created `00-SOC-Analyst-30Day-Journey` folder
- Established tracking mechanism

---

### Day 2 - 17-01-2026
**Lab:** 1.1.2 - Windows Security Event Log Analysis  
**Status:**  Completed
**Time Invested:** 2 hours

**Planned Activities:**
- Analyze failed login attempts (Event ID 4625)
- Detect brute force patterns
- Document findings in `04-SIEM-Projects` [![Abrir Proyecto](https://img.shields.io/badge/GitHub-Proyecto-blue?logo=github)](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/tree/main/labs/04-SIEM-Projects/Lab-1.1.2-Windows-Event-Analysis)

**Key Learnings:**
- Analyzed Windows Event ID 4625 (Failed Logon) for brute force detection
- Implemented temporal analysis to detect attack velocity
- Correlated failed attempts with successful logins (compromise detection)
- Mapped findings to MITRE ATT&CK T1110.001
- Created professional incident report with executive summary

**Challenges:**

***Permission and Access Control Issues***

- **Challenge**: Encountered `UnauthorizedAccessException` when attempting to read Windows Security event logs

- **Root Cause**: PowerShell terminal running without elevated privileges

- **Resolution**: Executed VS Code as Administrator to gain necessary permissions for accessing Security event logs

- **Learning**: Understanding Windows security contexts and the importance of proper privilege escalation for security analysis tasks


**Skills Developed:**
- PowerShell log analysis
- Pattern recognition in security events
- Incident documentation
- MITRE ATT&CK framework


### Day 3 - 18/01/25

**Lab:** 1.1.3 - Network Traffic Analysis with Wireshark  
**Status:**  Completed 
**Time Invested:** 2 hours
**Location:** `01-Network-Security` [![Abrir Proyecto](https://img.shields.io/badge/GitHub-Proyecto-blue?logo=github)](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/tree/main/labs/01-Network-Security/wireshark-analysis/Lab-1.1.3-Wireshark-Analysis)

**Planned Activities:**

- Capture and analyze network packets
- Identify suspicious traffic patterns
- Detect common network attacks (ARP spoofing, port scanning)

**Key Learnings:**

- Installed and configured Wireshark with Npcap
- Captured and analyzed live network traffic (ICMP, DNS, HTTP, TLS)
- Mastered display filters for traffic isolation
- Detected port scanning patterns using TCP flag analysis
- Followed TCP streams to reconstruct communications
- Established network baseline for anomaly detection

**Challenges:**

**Challenge 1:** ***No Network Interfaces Visible***

- Issue: Wireshark showed no interfaces on first launch
- Root Cause: Npcap not installed or service not running
- Solution: Reinstalled Npcap with "WinPcap API-compatible mode"
- Lesson: Packet capture requires kernel-level drivers

**Challenge 2:** ***Capture Permission Denied (Linux)***

- Issue: "You don't have permission to capture on that device"
- Root Cause: User not in wireshark group
- Solution: sudo usermod -a -G wireshark $USER + logout/login
- Lesson: Non-root capture requires proper group membership

**Skills Developed:**

-  Packet capture and analysis
-  Display filter creation (15+ filters)
-  Protocol analysis (OSI layers 2-7)
-  Attack pattern recognition
-  Network baseline establishment


---

### Day 4 - [Tomorrow]
**Lab:** 1.1.4 - Malware Detection with VirusTotal  
**Status:**  Pending  
**Planned Activities:**
- Hash-based file identification (MD5, SHA256)
- VirusTotal API integration
- Static malware analysis basics
- YARA rule introduction







---

##  Skills Development Tracker

| Skill Category | Day 1 | Day 7 | Day 14 | Day 21 | Day 30 |
|---------------|-------|-------|--------|--------|--------|
| Log Analysis | 🟡 | ⬜ | ⬜ | ⬜ | ⬜ |
| Network Traffic Analysis | 🟡 | ⬜ | ⬜ | ⬜ | ⬜ |
| Malware Analysis | ⬜ | ⬜ | ⬜ | ⬜ | ⬜ |
| Incident Response | ⬜ | ⬜ | ⬜ | ⬜ | ⬜ |
| Threat Intelligence | ⬜ | ⬜ | ⬜ | ⬜ | ⬜ |
| Automation/Scripting | ⬜ | ⬜ | ⬜ | ⬜ | ⬜ |

**Legend:**  
⬜ Not Started | 🟡 Learning | 🟢 Competent | 🔵 Proficient

---

##  Certification Readiness

- [ ] CompTIA Security+ concepts covered
- [ ] CySA+ practical skills demonstrated
- [ ] Blue Team fundamentals mastered
- [ ] SOC Analyst interview-ready

---

## Resources Utilized

- [ ] MITRE ATT&CK Framework
- [ ] NIST Cybersecurity Framework
- [ ] Cyber Kill Chain
- [ ] SANS Reading Room
- [ ] Wireshark Documentation
- [ ] PowerShell for Security

---

##  Achievements Unlocked

- [/\] Day 1: Environment configured like a professional SOC
- [ ] Day 7: First week milestone - Fundamentals mastered
- [ ] Day 14: Mid-point - Detection capabilities demonstrated
- [ ] Day 21: Week 3 - Incident response skills proven
- [ ] Day 30: COMPLETE - Ready for SOC Analyst roles

---

**What makes this training valuable:**
- Hands-on, practical labs 
- Documented evidence of every skill
- Industry-standard tools and frameworks
- Real-world scenarios and case studies
- Professional documentation standards

**Portfolio Structure Benefits:**
- Organized by cybersecurity domain
- Each lab includes executive summaries
- Screenshots and evidence included 
- GitHub activity shows consistent learning

---

**Last Updated:** [19-01-2026]



> [!NOTE]
> This roadmap has been developed with guidance from Claude, an AI assistant by Anthropic.