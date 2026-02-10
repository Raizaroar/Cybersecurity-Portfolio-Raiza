# Detection Maturity Matrix

## MITRE ATT&CK Coverage

| Tactic | Techniques Covered | Detection Methods | Maturity Level |
|--------|-------------------|-------------------|----------------|
| **Initial Access** | T1566.001, T1190 | Email analysis, web logs | High |
| **Execution** | T1059.001, T1204.002 | Process creation, YARA |  Very High |
| **Persistence** | T1547.001, T1136.001 | Registry, account creation |  Medium |
| **Privilege Escalation** | T1078.002 | Admin group changes |  Medium |
| **Defense Evasion** | T1036.005, T1027 | Masquerading, obfuscation |  Low |
| **Credential Access** | T1110.001, T1003.001 | Brute force, Mimikatz | High |
| **Discovery** | T1046, T1018 | Network scans |  Medium |
| **Lateral Movement** | - | None | Very Low |
| **Collection** | T1005, T1039 | File access |  Low |
| **C2** | T1071.001 | Beaconing analysis |  High |
| **Exfiltration** | T1041 | Large uploads | Medium |

---

## Detection Types

| Detection Type | Count | Examples |
|---------------|-------|----------|
| **Signature-based** | 7 | YARA rules, hash matching |
| **Behavior-based** | 5 | Process graphs, beaconing |
| **Anomaly-based** | 3 | Rare processes, off-hours |
| **Threat Intel** | 2 | Known bad IPs, domains |
| **Correlation** | 4 | Multi-stage attack chains |

**Total Detections:** 21

---

## Maturity Levels Defined

**Very Low** - No/minimal detection  
 **Low** - Basic detection, high false positives  
 **Medium** - Moderate coverage, tuned for environment  
 **High** - Good coverage, automated response  
 **Very High** - Comprehensive, behavior-based, tested  

---

## Improvement Roadmap

### Q1 Priorities (Next 30 days)
1. Add lateral movement detections (RDP, SMB)
2. Improve defense evasion coverage
3. Deploy EDR for better process telemetry

### Q2 Goals
1. Achieve 80%+ MITRE coverage
2. Reduce false positive rate to <5%
3. Implement automated response actions

### Long-term Vision
1. Machine learning anomaly detection
2. User and Entity Behavior Analytics (UEBA)
3. Threat intelligence automation